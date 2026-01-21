# Story 1.3: Keychain Token Storage

Status: review

## Story

As a **user**,
I want **my authentication token stored securely**,
so that **my session persists across app launches** (NFR-S1).

## Acceptance Criteria

1. **AC1:** Given a successful Sign in with Apple, when authentication completes, then the user identifier is stored in iOS Keychain
2. **AC2:** The token is stored using raw Keychain APIs (Security framework)
3. **AC3:** No external Keychain library is used (KeychainAccess, etc.)
4. **AC4:** The token is retrievable for future sessions
5. **AC5:** The token can be deleted (for sign-out in Story 1.4)
6. **AC6:** Keychain errors are handled gracefully without crashing

## Tasks / Subtasks

- [ ] **Task 1: Create KeychainService** (AC: #1, #2, #3)
  - [ ] 1.1 Create `Core/Services/KeychainService.swift`
  - [ ] 1.2 Import Security framework
  - [ ] 1.3 Define Keychain keys as constants (e.g., `userIdentifierKey`)
  - [ ] 1.4 Implement `save(key:value:)` method using SecItemAdd
  - [ ] 1.5 Implement `read(key:)` method using SecItemCopyMatching
  - [ ] 1.6 Implement `delete(key:)` method using SecItemDelete

- [ ] **Task 2: Define KeychainError enum** (AC: #6)
  - [ ] 2.1 Create error cases: `duplicateItem`, `itemNotFound`, `unexpectedStatus`
  - [ ] 2.2 Map OSStatus codes to meaningful errors
  - [ ] 2.3 Implement LocalizedError for user-facing messages

- [ ] **Task 3: Integrate with AuthViewModel** (AC: #1, #4)
  - [ ] 3.1 Inject KeychainService into AuthViewModel
  - [ ] 3.2 Save userIdentifier to Keychain after successful Sign in
  - [ ] 3.3 Add method to check if token exists in Keychain
  - [ ] 3.4 Prepare `deleteToken()` for Story 1.4 (sign-out)

- [ ] **Task 4: Test Keychain Operations** (AC: all)
  - [ ] 4.1 Verify save works after Sign in
  - [ ] 4.2 Verify read returns saved value
  - [ ] 4.3 Verify delete removes the item
  - [ ] 4.4 Verify graceful handling of errors

## Dev Notes

### 🚨 CRITICAL: Previous Story Learnings (Story 1.2)

**From Story 1.2 Implementation:**
- Auth flow working: SignInView → Sign in with Apple → ContentView
- `AuthViewModel` exists at `Features/Auth/ViewModels/AuthViewModel.swift`
- `AppleAuthService` exists at `Features/Auth/Services/AppleAuthService.swift`
- User identifier is captured in `AuthViewModel.userIdentifier`
- Currently NOT persisted - resets on app restart

**Current File Locations:**
```
MySubGuard/MySubGuard/
├── Features/
│   └── Auth/
│       ├── Services/
│       │   └── AppleAuthService.swift
│       └── ViewModels/
│           └── AuthViewModel.swift  ← MODIFY (add Keychain)
└── Core/
    ├── Persistence/
    │   └── ModelContainer+Shared.swift
    └── Services/                     ← NEW folder
        └── KeychainService.swift     ← CREATE
```

### Architecture Compliance

**From Architecture Document:**

| Decision | Implementation |
|----------|----------------|
| Token Storage | Raw Keychain APIs (Security framework) |
| No External Libraries | DO NOT use KeychainAccess, KeychainSwift, etc. |
| User ID | Store Apple userIdentifier string |
| Service Key | `com.steeven.mysubguard.userIdentifier` |

### KeychainService Implementation Pattern

```swift
// Core/Services/KeychainService.swift
import Foundation
import Security

enum KeychainError: LocalizedError {
    case duplicateItem
    case itemNotFound
    case unexpectedStatus(OSStatus)

    var errorDescription: String? {
        switch self {
        case .duplicateItem:
            return "Item already exists in Keychain"
        case .itemNotFound:
            return "Item not found in Keychain"
        case .unexpectedStatus(let status):
            return "Keychain error: \(status)"
        }
    }
}

final class KeychainService {
    static let shared = KeychainService()

    private let service = "com.steeven.mysubguard"

    enum Key: String {
        case userIdentifier = "userIdentifier"
    }

    private init() {}

    func save(_ value: String, for key: Key) throws {
        let data = Data(value.utf8)

        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrService as String: service,
            kSecAttrAccount as String: key.rawValue,
            kSecValueData as String: data
        ]

        // Delete existing item first
        SecItemDelete(query as CFDictionary)

        let status = SecItemAdd(query as CFDictionary, nil)

        guard status == errSecSuccess else {
            throw KeychainError.unexpectedStatus(status)
        }
    }

    func read(for key: Key) -> String? {
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrService as String: service,
            kSecAttrAccount as String: key.rawValue,
            kSecReturnData as String: true,
            kSecMatchLimit as String: kSecMatchLimitOne
        ]

        var result: AnyObject?
        let status = SecItemCopyMatching(query as CFDictionary, &result)

        guard status == errSecSuccess,
              let data = result as? Data,
              let value = String(data: data, encoding: .utf8) else {
            return nil
        }

        return value
    }

    func delete(for key: Key) throws {
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrService as String: service,
            kSecAttrAccount as String: key.rawValue
        ]

        let status = SecItemDelete(query as CFDictionary)

        guard status == errSecSuccess || status == errSecItemNotFound else {
            throw KeychainError.unexpectedStatus(status)
        }
    }

    func exists(for key: Key) -> Bool {
        return read(for: key) != nil
    }
}
```

### AuthViewModel Integration

```swift
// Update AuthViewModel.swift
@Observable
final class AuthViewModel {
    var isAuthenticated = false
    var isLoading = false
    var errorMessage: String?

    private(set) var userIdentifier: String?

    private let authService = AppleAuthService()
    private let keychainService = KeychainService.shared

    @MainActor
    func signInWithApple() async {
        isLoading = true
        errorMessage = nil

        do {
            let credential = try await authService.signIn()
            userIdentifier = credential.user

            // Save to Keychain (NEW)
            try keychainService.save(credential.user, for: .userIdentifier)

            isAuthenticated = true
        } catch {
            // Handle error...
        }

        isLoading = false
    }

    // For Story 1.4
    @MainActor
    func signOut() {
        try? keychainService.delete(for: .userIdentifier)
        userIdentifier = nil
        isAuthenticated = false
    }

    // Check if user has saved credentials (for session restore in Story 1.4)
    func hasStoredCredentials() -> Bool {
        return keychainService.exists(for: .userIdentifier)
    }

    func loadStoredCredentials() -> String? {
        return keychainService.read(for: .userIdentifier)
    }
}
```

### Technical Requirements

| Requirement | Value |
|-------------|-------|
| Framework | Security (native iOS) |
| Keychain Service ID | `com.steeven.mysubguard` |
| Keychain Class | `kSecClassGenericPassword` |
| External Libraries | NONE (raw APIs only) |

### Security Considerations (NFR-S1)

- Keychain data is encrypted at rest by iOS
- Data persists across app reinstalls (tied to device)
- Accessible only to this app (sandboxed)
- Not backed up to iCloud by default (secure)

### Testing Checklist

- [ ] Sign in saves userIdentifier to Keychain
- [ ] App restart can read userIdentifier (verify in next story)
- [ ] No crashes on Keychain errors
- [ ] Console shows successful save operation
- [ ] No external Keychain libraries in project

### References

- [Source: architecture.md#Authentication-Security] - Raw Keychain APIs decision
- [Source: epics.md#Story-1.3] - Acceptance criteria
- [Source: 1-2-sign-in-with-apple-integration.md] - Auth flow implementation
- [Apple Docs: Keychain Services](https://developer.apple.com/documentation/security/keychain_services)

---

## Dev Agent Record

### Agent Model Used

_To be filled by dev agent_

### Debug Log References

_To be filled during implementation_

### Completion Notes List

_To be filled after implementation_

### File List

Files to create:
1. `MySubGuard/Core/Services/KeychainService.swift` - NEW

Files to modify:
1. `MySubGuard/Features/Auth/ViewModels/AuthViewModel.swift` - Add Keychain integration
