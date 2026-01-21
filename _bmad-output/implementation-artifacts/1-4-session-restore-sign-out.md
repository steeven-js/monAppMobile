# Story 1.4: Session Restore & Sign Out

Status: review

## Story

As a **user**,
I want **my session to restore automatically when I open the app**,
so that **I don't have to sign in every time** (FR4).

As a **user**,
I want **to sign out from the app**,
so that **I can switch accounts or protect my privacy** (FR2).

## Acceptance Criteria

1. **AC1:** Given a previously signed-in user, when they launch the app, then the session is restored from Keychain
2. **AC2:** Session restore completes in < 2 seconds (NFR-P3)
3. **AC3:** User is taken directly to main interface (no sign-in screen)
4. **AC4:** Given a signed-in user in Settings, when they tap "Sign out", then the Keychain token is deleted
5. **AC5:** After sign out, user is returned to the sign-in screen
6. **AC6:** Local data is NOT deleted on sign out (data belongs to device, not account)
7. **AC7:** Connection status is displayed in Settings (Connecte / Non connecte)

## Tasks / Subtasks

- [ ] **Task 1: Implement Session Restore on App Launch** (AC: #1, #2, #3)
  - [ ] 1.1 Call `restoreSessionIfAvailable()` in MySubGuardApp.swift on launch
  - [ ] 1.2 Show loading state while checking session
  - [ ] 1.3 Verify credential state with Apple (already implemented in AuthViewModel)
  - [ ] 1.4 Navigate to main interface if valid session

- [ ] **Task 2: Add Sign Out Button in Settings** (AC: #4, #5, #6)
  - [ ] 2.1 Create SettingsView.swift in Features/Settings/Views/
  - [ ] 2.2 Add "Se deconnecter" button with red styling
  - [ ] 2.3 Show confirmation dialog before sign out
  - [ ] 2.4 Call authViewModel.signOut() on confirmation
  - [ ] 2.5 Ensure local data (SwiftData) is NOT deleted

- [ ] **Task 3: Display Connection Status in Settings** (AC: #7)
  - [ ] 3.1 Show "Connecte" with green checkmark when authenticated
  - [ ] 3.2 Show "Non connecte" when not authenticated
  - [ ] 3.3 Display user identifier (masked) if available

- [ ] **Task 4: Test Session Flow** (AC: all)
  - [ ] 4.1 Sign in → close app → reopen → verify auto-restore
  - [ ] 4.2 Sign out → verify return to sign-in screen
  - [ ] 4.3 Verify local data persists after sign out
  - [ ] 4.4 Test invalid credential handling (revoked Apple ID)

## Dev Notes

### Previous Story Learnings (Story 1.3)

**From Story 1.3 Implementation:**
- KeychainService exists at `Core/Services/KeychainService.swift`
- AuthViewModel already has:
  - `restoreSessionIfAvailable()` - checks Keychain and validates with Apple
  - `signOut()` - deletes from Keychain and resets state
  - `hasStoredCredentials()` - checks if token exists
  - `loadStoredCredentials()` - reads token from Keychain

**Current File Locations:**
```
MySubGuard/MySubGuard/
├── Features/
│   ├── Auth/
│   │   ├── Services/AppleAuthService.swift
│   │   ├── ViewModels/AuthViewModel.swift
│   │   └── Views/SignInView.swift
│   └── Settings/
│       └── Views/
│           └── SettingsView.swift  ← MODIFY (add sign out + status)
├── Core/
│   └── Services/KeychainService.swift
└── MySubGuardApp.swift              ← MODIFY (add session restore)
```

### App Launch Flow

```swift
// MySubGuardApp.swift
@main
struct MySubGuardApp: App {
    @State private var authViewModel = AuthViewModel()
    @State private var isCheckingSession = true

    var body: some Scene {
        WindowGroup {
            Group {
                if isCheckingSession {
                    // Loading state
                    ProgressView()
                } else if authViewModel.isAuthenticated {
                    ContentView()
                } else {
                    SignInView()
                }
            }
            .environment(authViewModel)
            .task {
                await authViewModel.restoreSessionIfAvailable()
                isCheckingSession = false
            }
        }
    }
}
```

### SettingsView Updates

```swift
// Features/Settings/Views/SettingsView.swift
struct SettingsView: View {
    @Environment(AuthViewModel.self) private var authViewModel
    @State private var showSignOutConfirmation = false

    var body: some View {
        NavigationStack {
            List {
                // Connection Status Section
                Section {
                    HStack {
                        Text("Statut")
                        Spacer()
                        if authViewModel.isAuthenticated {
                            Label("Connecte", systemImage: "checkmark.circle.fill")
                                .foregroundStyle(.green)
                        } else {
                            Text("Non connecte")
                                .foregroundStyle(.secondary)
                        }
                    }
                }

                // Sign Out Section (only if authenticated)
                if authViewModel.isAuthenticated {
                    Section {
                        Button(role: .destructive) {
                            showSignOutConfirmation = true
                        } label: {
                            Label("Se deconnecter", systemImage: "rectangle.portrait.and.arrow.right")
                        }
                    }
                }
            }
            .navigationTitle("Parametres")
            .confirmationDialog(
                "Se deconnecter ?",
                isPresented: $showSignOutConfirmation,
                titleVisibility: .visible
            ) {
                Button("Se deconnecter", role: .destructive) {
                    authViewModel.signOut()
                }
                Button("Annuler", role: .cancel) {}
            } message: {
                Text("Vos donnees locales seront conservees.")
            }
        }
    }
}
```

### Technical Requirements

| Requirement | Value |
|-------------|-------|
| Session restore time | < 2 seconds (NFR-P3) |
| Local data on sign out | Preserved (not deleted) |
| Credential validation | Apple `getCredentialState` API |

### Security Considerations (NFR-S6)

- Credential state is verified with Apple on each restore
- If Apple revokes the credential, user must sign in again
- Keychain token is securely deleted on sign out

### Testing Checklist

- [ ] App launch restores session automatically
- [ ] No sign-in screen shown for returning users
- [ ] Sign out button visible in Settings
- [ ] Sign out deletes Keychain token
- [ ] Sign out returns to sign-in screen
- [ ] Local SwiftData remains after sign out
- [ ] Invalid credentials handled gracefully

### References

- [Source: epics.md#Story-1.4] - Acceptance criteria
- [Source: 1-3-keychain-token-storage.md] - Keychain implementation
- [Apple Docs: ASAuthorizationAppleIDProvider](https://developer.apple.com/documentation/authenticationservices/asauthorizationappleidprovider)

---

## Dev Agent Record

### Agent Model Used

_To be filled by dev agent_

### Debug Log References

_To be filled during implementation_

### Completion Notes List

_To be filled after implementation_

### File List

Files to modify:
1. `MySubGuard/MySubGuardApp.swift` - Add session restore on launch
2. `MySubGuard/Features/Settings/Views/SettingsView.swift` - Add sign out + status
