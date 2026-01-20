# Story 1.2: Sign in with Apple Integration

Status: review

## Story

As a **user**,
I want **to sign in using my Apple ID**,
so that **I can use the app without creating a new account** (FR1, FR3).

## Acceptance Criteria

1. **AC1:** Given the user is on the sign-in screen, when they tap "Sign in with Apple", then the Apple authentication sheet appears
2. **AC2:** Given successful authentication, when the user completes Sign in with Apple, then the user's identifier is received and stored
3. **AC3:** Given successful authentication, then no personal data beyond Apple ID is required (anonymous mode supported)
4. **AC4:** Given successful authentication, then the user is redirected to the main app interface (TabView)
5. **AC5:** Given authentication fails or is cancelled, when the sheet is dismissed, then the user remains on the sign-in screen
6. **AC6:** Given authentication fails, then an appropriate error message is shown to the user

## Tasks / Subtasks

- [ ] **Task 1: Create Auth Feature Structure** (AC: all)
  - [ ] 1.1 Create `Features/Auth/` folder structure
  - [ ] 1.2 Create `Features/Auth/Views/SignInView.swift`
  - [ ] 1.3 Create `Features/Auth/ViewModels/AuthViewModel.swift`
  - [ ] 1.4 Create `Features/Auth/Services/AppleAuthService.swift`

- [ ] **Task 2: Implement AppleAuthService** (AC: #1, #2, #3, #5, #6)
  - [ ] 2.1 Import AuthenticationServices framework
  - [ ] 2.2 Create `AppleAuthService` class conforming to `ASAuthorizationControllerDelegate`
  - [ ] 2.3 Implement `signIn()` async throws method
  - [ ] 2.4 Handle `ASAuthorizationAppleIDCredential` response
  - [ ] 2.5 Extract user identifier (anonymous - no email/name required)
  - [ ] 2.6 Handle errors gracefully (cancelled, failed, etc.)

- [ ] **Task 3: Implement AuthViewModel** (AC: all)
  - [ ] 3.1 Create `@Observable class AuthViewModel`
  - [ ] 3.2 Add `isAuthenticated: Bool` state property
  - [ ] 3.3 Add `isLoading: Bool` for sign-in progress
  - [ ] 3.4 Add `errorMessage: String?` for error display
  - [ ] 3.5 Implement `signInWithApple()` method calling AppleAuthService
  - [ ] 3.6 Update `isAuthenticated` on success

- [ ] **Task 4: Create SignInView** (AC: #1, #5, #6)
  - [ ] 4.1 Create SwiftUI view with app branding/logo
  - [ ] 4.2 Add `SignInWithAppleButton` from AuthenticationServices
  - [ ] 4.3 Style button with `.black` style for light mode
  - [ ] 4.4 Add loading indicator during authentication
  - [ ] 4.5 Display error messages when authentication fails
  - [ ] 4.6 Add accessibility labels (VoiceOver: "Se connecter avec Apple")

- [ ] **Task 5: Integrate Authentication Flow** (AC: #4)
  - [ ] 5.1 Modify `MySubguardApp.swift` to inject AuthViewModel via @Environment
  - [ ] 5.2 Create conditional navigation: SignInView vs ContentView based on auth state
  - [ ] 5.3 Test full flow: launch → sign in → main interface

- [ ] **Task 6: Add Sign in with Apple Capability** (AC: all)
  - [ ] 6.1 In Xcode: Target → Signing & Capabilities → + Capability → Sign in with Apple
  - [ ] 6.2 Verify entitlement is added to MySubguard.entitlements

## Dev Notes

### 🚨 CRITICAL: Previous Story Learnings (Story 1.1)

**From Story 1.1 Implementation:**
- Project uses **Xcode 26.2** which may have different UI for capabilities
- App Groups already configured: `group.com.steeven.monAppMobile`
- ModelContainer with fallback pattern already established
- Tab navigation exists at `ContentView.swift`
- Settings placeholder at `Features/Settings/Views/SettingsView.swift`

**File Locations Established:**
```
MySubGuard/
├── MySubguard/
│   ├── MySubguardApp.swift          ← MODIFY (add auth check)
│   ├── ContentView.swift            ← Already has TabView
│   ├── Core/
│   │   └── Persistence/
│   │       └── ModelContainer+Shared.swift
│   └── Features/
│       ├── Auth/                    ← NEW folder
│       │   ├── Views/
│       │   │   └── SignInView.swift
│       │   ├── ViewModels/
│       │   │   └── AuthViewModel.swift
│       │   └── Services/
│       │       └── AppleAuthService.swift
│       ├── Subscriptions/Views/
│       ├── Acknowledgment/Views/
│       └── Settings/Views/
```

### Architecture Compliance

**From Architecture Document:**

| Decision | Implementation |
|----------|----------------|
| Auth Method | Sign in with Apple (privacy-first, anonymous) |
| Token Storage | Raw Keychain APIs (Story 1.3) |
| User ID Server | SHA256(appleUserID) for anonymization |
| State Management | `@Observable` macro for ViewModels |
| DI Approach | `@Environment` for service injection |

### Sign in with Apple Implementation Pattern

```swift
// Features/Auth/Services/AppleAuthService.swift
import AuthenticationServices
import Foundation

@Observable
final class AppleAuthService: NSObject {
    private var currentNonce: String?
    private var continuation: CheckedContinuation<ASAuthorizationAppleIDCredential, Error>?

    func signIn() async throws -> ASAuthorizationAppleIDCredential {
        return try await withCheckedThrowingContinuation { continuation in
            self.continuation = continuation

            let appleIDProvider = ASAuthorizationAppleIDProvider()
            let request = appleIDProvider.createRequest()
            request.requestedScopes = [] // Anonymous - no email/name

            let authorizationController = ASAuthorizationController(authorizationRequests: [request])
            authorizationController.delegate = self
            authorizationController.performRequests()
        }
    }
}

extension AppleAuthService: ASAuthorizationControllerDelegate {
    func authorizationController(controller: ASAuthorizationController,
                                didCompleteWithAuthorization authorization: ASAuthorization) {
        if let credential = authorization.credential as? ASAuthorizationAppleIDCredential {
            continuation?.resume(returning: credential)
        }
    }

    func authorizationController(controller: ASAuthorizationController,
                                didCompleteWithError error: Error) {
        continuation?.resume(throwing: error)
    }
}
```

### AuthViewModel Pattern

```swift
// Features/Auth/ViewModels/AuthViewModel.swift
import Foundation
import AuthenticationServices

@Observable
final class AuthViewModel {
    var isAuthenticated = false
    var isLoading = false
    var errorMessage: String?

    private let authService = AppleAuthService()

    @MainActor
    func signInWithApple() async {
        isLoading = true
        errorMessage = nil

        do {
            let credential = try await authService.signIn()
            // Store user identifier (will be saved to Keychain in Story 1.3)
            let userIdentifier = credential.user
            print("User signed in: \(userIdentifier)")

            isAuthenticated = true
        } catch let error as ASAuthorizationError {
            switch error.code {
            case .canceled:
                // User cancelled - no error message needed
                break
            case .failed:
                errorMessage = "Échec de la connexion. Réessayez."
            case .invalidResponse:
                errorMessage = "Réponse invalide d'Apple."
            case .notHandled:
                errorMessage = "Demande non gérée."
            case .unknown:
                errorMessage = "Erreur inconnue."
            @unknown default:
                errorMessage = "Erreur inattendue."
            }
        } catch {
            errorMessage = "Erreur: \(error.localizedDescription)"
        }

        isLoading = false
    }
}
```

### SignInView Pattern

```swift
// Features/Auth/Views/SignInView.swift
import SwiftUI
import AuthenticationServices

struct SignInView: View {
    @Environment(AuthViewModel.self) private var authViewModel

    var body: some View {
        VStack(spacing: 40) {
            Spacer()

            // App Logo/Branding
            VStack(spacing: 16) {
                Image(systemName: "creditcard.and.123")
                    .font(.system(size: 80))
                    .foregroundStyle(.blue)

                Text("MySubGuard")
                    .font(.largeTitle)
                    .fontWeight(.bold)

                Text("Gardez le contrôle de vos abonnements")
                    .font(.subheadline)
                    .foregroundStyle(.secondary)
                    .multilineTextAlignment(.center)
            }

            Spacer()

            // Sign in Button
            VStack(spacing: 16) {
                if authViewModel.isLoading {
                    ProgressView()
                        .progressViewStyle(.circular)
                } else {
                    SignInWithAppleButton(.signIn, onRequest: { request in
                        request.requestedScopes = []
                    }, onCompletion: { result in
                        Task {
                            await authViewModel.signInWithApple()
                        }
                    })
                    .signInWithAppleButtonStyle(.black)
                    .frame(height: 50)
                    .accessibilityLabel("Se connecter avec Apple")
                }

                if let error = authViewModel.errorMessage {
                    Text(error)
                        .font(.caption)
                        .foregroundStyle(.red)
                        .multilineTextAlignment(.center)
                }
            }
            .padding(.horizontal, 40)

            Spacer()
        }
        .padding()
    }
}
```

### App Entry Point Modification

```swift
// MySubguardApp.swift
import SwiftUI
import SwiftData

@main
struct MySubguardApp: App {
    @State private var authViewModel = AuthViewModel()

    var body: some Scene {
        WindowGroup {
            Group {
                if authViewModel.isAuthenticated {
                    ContentView()
                } else {
                    SignInView()
                }
            }
            .environment(authViewModel)
        }
        .modelContainer(ModelContainer.shared)
    }
}
```

### Technical Requirements

| Requirement | Value |
|-------------|-------|
| Framework | AuthenticationServices |
| Min iOS Version | iOS 17.0 |
| Entitlement | Sign in with Apple |
| Scopes Required | None (anonymous mode) |
| Token Persistence | Story 1.3 (Keychain) |

### UX Requirements (from UX Design)

- Haptic feedback: Not required for sign-in button (system handled)
- Loading state: `ProgressView` during authentication
- Error display: Inline text below button, red color
- VoiceOver: "Se connecter avec Apple" accessibility label
- Dark mode: SignInWithAppleButton adapts automatically

### Testing Checklist

- [ ] Sign in with Apple capability added in Xcode
- [ ] Tapping button shows Apple auth sheet
- [ ] Successful auth redirects to TabView
- [ ] Cancelled auth keeps user on SignInView (no error)
- [ ] Failed auth shows appropriate error message
- [ ] Loading indicator appears during auth
- [ ] VoiceOver reads button correctly
- [ ] Dark mode displays correctly

### References

- [Source: architecture.md#Authentication-Security] - Auth decisions
- [Source: architecture.md#Frontend-Architecture-iOS] - @Observable pattern
- [Source: epics.md#Story-1.2] - Acceptance criteria
- [Source: 1-1-project-setup-navigation-structure.md] - File structure established
- [Apple Docs: Sign in with Apple](https://developer.apple.com/documentation/authenticationservices/implementing_user_authentication_with_sign_in_with_apple)

### ⚠️ Important Notes

1. **Token NOT persisted yet** - Story 1.3 will add Keychain storage
2. **Session restore NOT implemented** - Story 1.4 will handle this
3. **For now**: `isAuthenticated` resets on app restart (expected)
4. **Anonymous mode**: We request NO scopes (no email, no name)

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
1. `MySubguard/Features/Auth/Views/SignInView.swift` - NEW
2. `MySubguard/Features/Auth/ViewModels/AuthViewModel.swift` - NEW
3. `MySubguard/Features/Auth/Services/AppleAuthService.swift` - NEW

Files to modify:
1. `MySubguard/MySubguardApp.swift` - Add auth flow
2. `MySubguard/MySubguard.entitlements` - Add Sign in with Apple capability
