# Story 1.1: Project Setup & Navigation Structure

Status: ready-for-dev

## Story

As a **developer**,
I want **a properly configured iOS project with SwiftUI, SwiftData, and App Groups**,
so that **I have the foundation for all future development**.

## Acceptance Criteria

1. **AC1:** App uses SwiftUI App lifecycle with `@main` attribute
2. **AC2:** SwiftData is configured with a shared `ModelContainer` accessible via App Groups
3. **AC3:** App Groups capability is enabled with identifier `group.com.steeven.monAppMobile`
4. **AC4:** Widget Extension target is added (named `MySubGuardWidget`)
5. **AC5:** 3-tab navigation is implemented with tabs: Abonnements, Acquitter, Paramètres
6. **AC6:** App builds and runs successfully on iOS 17+ simulator

## Tasks / Subtasks

- [ ] **Task 1: Configure App Groups** (AC: #3)
  - [ ] 1.1 Open project in Xcode → MySubGuard target → Signing & Capabilities
  - [ ] 1.2 Click "+ Capability" → Add "App Groups"
  - [ ] 1.3 Add identifier: `group.com.steeven.monAppMobile`
  - [ ] 1.4 Verify App Groups appears in entitlements file

- [ ] **Task 2: Add Widget Extension Target** (AC: #4)
  - [ ] 2.1 File → New → Target → Widget Extension
  - [ ] 2.2 Name: `MySubGuardWidget`
  - [ ] 2.3 Include Configuration App Intent: NO (simple widgets first)
  - [ ] 2.4 Enable App Groups on Widget target: `group.com.steeven.monAppMobile`
  - [ ] 2.5 Set deployment target iOS 17.0 on Widget target

- [ ] **Task 3: Configure SwiftData with Shared Container** (AC: #2)
  - [ ] 3.1 Delete default `Item.swift` model
  - [ ] 3.2 Create `Core/Persistence/` folder structure
  - [ ] 3.3 Implement `ModelContainer+Shared.swift` with App Groups container
  - [ ] 3.4 Update `MySubGuardApp.swift` to use shared container
  - [ ] 3.5 Verify Widget can access the same container

- [ ] **Task 4: Implement 3-Tab Navigation** (AC: #5)
  - [ ] 4.1 Create `Features/Subscriptions/Views/SubscriptionListView.swift` (placeholder)
  - [ ] 4.2 Create `Features/Acknowledgment/Views/AcknowledgeListView.swift` (placeholder)
  - [ ] 4.3 Create `Features/Settings/Views/SettingsView.swift` (placeholder)
  - [ ] 4.4 Update `ContentView.swift` with `TabView` containing 3 tabs
  - [ ] 4.5 Add SF Symbols for each tab: `list.bullet`, `checkmark.circle`, `gear`

- [ ] **Task 5: Project Structure Setup** (AC: #1, #6)
  - [ ] 5.1 Create folder structure per architecture spec
  - [ ] 5.2 Set deployment target to iOS 17.0
  - [ ] 5.3 Build and run on iOS 17 simulator
  - [ ] 5.4 Verify all tabs navigate correctly

## Dev Notes

### 🚨 CRITICAL: Existing Project Adaptation

The project **MySubGuard** already exists at `/MySubGuard/`. This story adapts the existing Xcode project rather than creating a new one.

**Current State:**
```
MySubGuard/
├── MySubGuard.xcodeproj/
├── MySubguard/
│   ├── MySubguardApp.swift    ← Modify
│   ├── ContentView.swift       ← Modify
│   └── Item.swift              ← DELETE (replace with our models)
├── MySubguardTests/
└── MySubguardUITests/
```

**Target State:**
```
MySubGuard/
├── MySubGuard.xcodeproj/
├── MySubguard/
│   ├── App/
│   │   └── MySubGuardApp.swift
│   ├── Features/
│   │   ├── Subscriptions/Views/SubscriptionListView.swift
│   │   ├── Acknowledgment/Views/AcknowledgeListView.swift
│   │   └── Settings/Views/SettingsView.swift
│   ├── Core/
│   │   └── Persistence/ModelContainer+Shared.swift
│   └── ContentView.swift
├── MySubGuardWidget/
│   ├── MySubGuardWidget.swift
│   └── MySubGuardWidgetBundle.swift
├── MySubguardTests/
└── MySubguardUITests/
```

### Architecture Compliance

**From Architecture Document:**

| Decision | Implementation |
|----------|----------------|
| SwiftUI App lifecycle | `@main struct MySubGuardApp: App` |
| SwiftData iOS 17 | `import SwiftData`, `@Model` macro |
| App Groups | `group.com.steeven.monAppMobile` |
| MVVM + @Observable | ViewModels use `@Observable` macro |
| Navigation | `TabView` with 3 tabs |

### SwiftData Shared Container Pattern

```swift
// Core/Persistence/ModelContainer+Shared.swift
import SwiftData
import Foundation

extension ModelContainer {
    static let shared: ModelContainer = {
        let schema = Schema([
            // Models will be added in future stories
        ])

        let config = ModelConfiguration(
            schema: schema,
            url: containerURL,
            allowsSave: true
        )

        do {
            return try ModelContainer(for: schema, configurations: config)
        } catch {
            fatalError("Failed to create ModelContainer: \(error)")
        }
    }()

    private static var containerURL: URL {
        guard let containerURL = FileManager.default
            .containerURL(forSecurityApplicationGroupIdentifier: "group.com.steeven.monAppMobile") else {
            fatalError("App Groups not configured correctly")
        }
        return containerURL.appendingPathComponent("MySubGuard.store")
    }
}
```

### Tab Navigation Implementation

```swift
// ContentView.swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        TabView {
            SubscriptionListView()
                .tabItem {
                    Label("Abonnements", systemImage: "list.bullet")
                }

            AcknowledgeListView()
                .tabItem {
                    Label("Acquitter", systemImage: "checkmark.circle")
                }

            SettingsView()
                .tabItem {
                    Label("Paramètres", systemImage: "gear")
                }
        }
    }
}
```

### Placeholder Views Pattern

```swift
// Features/Subscriptions/Views/SubscriptionListView.swift
import SwiftUI

struct SubscriptionListView: View {
    var body: some View {
        NavigationStack {
            Text("Abonnements - Coming in Story 2.2")
                .navigationTitle("Abonnements")
        }
    }
}
```

### Project Structure Notes

**Alignment with Architecture:**
- Uses recommended folder structure from architecture.md
- `Features/` organized by domain (Subscriptions, Acknowledgment, Settings)
- `Core/` for shared utilities and persistence

**Widget Extension:**
- Named `MySubGuardWidget` to match app naming
- Shares App Groups for data access
- Empty implementation for now (placeholder)

### Technical Requirements

| Requirement | Value |
|-------------|-------|
| Deployment Target | iOS 17.0 |
| Swift Version | 5.9+ |
| Xcode Version | 15.0+ |
| App Groups ID | `group.com.steeven.monAppMobile` |

### Testing Checklist

- [ ] App builds without errors
- [ ] App runs on iOS 17 simulator
- [ ] All 3 tabs are visible and tappable
- [ ] Tab switching works correctly
- [ ] Widget extension builds
- [ ] No SwiftData errors in console

### References

- [Source: architecture.md#Selected-Starters] - iOS project initialization
- [Source: architecture.md#Complete-iOS-Project-Structure] - Folder structure
- [Source: architecture.md#Data-Architecture] - App Groups decision
- [Source: epics.md#Story-1.1] - Acceptance criteria

---

## Dev Agent Record

### Agent Model Used

_To be filled by dev agent_

### Debug Log References

_To be filled during implementation_

### Completion Notes List

_To be filled after implementation_

### File List

Files to create/modify:
1. `MySubguard/App/MySubGuardApp.swift` - Modified with shared container
2. `MySubguard/ContentView.swift` - TabView implementation
3. `MySubguard/Core/Persistence/ModelContainer+Shared.swift` - NEW
4. `MySubguard/Features/Subscriptions/Views/SubscriptionListView.swift` - NEW
5. `MySubguard/Features/Acknowledgment/Views/AcknowledgeListView.swift` - NEW
6. `MySubguard/Features/Settings/Views/SettingsView.swift` - NEW
7. `MySubGuardWidget/MySubGuardWidget.swift` - NEW (via Xcode template)
8. `MySubGuardWidget/MySubGuardWidgetBundle.swift` - NEW (via Xcode template)

Files to delete:
1. `MySubguard/Item.swift` - Default Xcode SwiftData model
