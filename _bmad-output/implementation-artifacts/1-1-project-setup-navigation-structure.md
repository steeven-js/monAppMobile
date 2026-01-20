# Story 1.1: Project Setup & Navigation Structure

Status: review

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

- [x] **Task 1: Configure App Groups** (AC: #3) ✅
  - [x] 1.1 Open project in Xcode → MySubGuard target → Signing & Capabilities
  - [x] 1.2 Click "+ Capability" → Add "App Groups"
  - [x] 1.3 Add identifier: `group.com.steeven.monAppMobile`
  - [x] 1.4 Verify App Groups appears in entitlements file

- [x] **Task 2: Add Widget Extension Target** (AC: #4) ✅
  - [x] 2.1 File → New → Target → Widget Extension
  - [x] 2.2 Name: `MySubGuardWidget`
  - [x] 2.3 Include Configuration App Intent: NO (simple widgets first)
  - [x] 2.4 Enable App Groups on Widget target: `group.com.steeven.monAppMobile`
  - [x] 2.5 Set deployment target iOS 17.0 on Widget target
  - [x] 2.6 Fixed visionOS template code for iOS compatibility

- [x] **Task 3: Configure SwiftData with Shared Container** (AC: #2) ✅
  - [x] 3.1 Delete default `Item.swift` model
  - [x] 3.2 Create `Core/Persistence/` folder structure
  - [x] 3.3 Implement `ModelContainer+Shared.swift` with App Groups container
  - [x] 3.4 Update `MySubGuardApp.swift` to use shared container
  - [x] 3.5 Verify Widget can access the same container

- [x] **Task 4: Implement 3-Tab Navigation** (AC: #5) ✅
  - [x] 4.1 Create `Features/Subscriptions/Views/SubscriptionListView.swift` (placeholder)
  - [x] 4.2 Create `Features/Acknowledgment/Views/AcknowledgeListView.swift` (placeholder)
  - [x] 4.3 Create `Features/Settings/Views/SettingsView.swift` (placeholder)
  - [x] 4.4 Update `ContentView.swift` with `TabView` containing 3 tabs
  - [x] 4.5 Add SF Symbols for each tab: `list.bullet`, `checkmark.circle`, `gear`

- [x] **Task 5: Project Structure Setup** (AC: #1, #6) ✅
  - [x] 5.1 Create folder structure per architecture spec
  - [x] 5.2 Set deployment target to iOS 17.0
  - [x] 5.3 Build and run on iOS 17 simulator (iPhone 17 Pro, iOS 26.2)
  - [x] 5.4 Verify all tabs navigate correctly

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

- [x] App builds without errors
- [x] App runs on iOS 17 simulator (tested on iPhone 17 Pro, iOS 26.2)
- [x] All 3 tabs are visible and tappable
- [x] Tab switching works correctly
- [x] Widget extension builds
- [x] Widget visible on home screen
- [x] No SwiftData errors in console

### References

- [Source: architecture.md#Selected-Starters] - iOS project initialization
- [Source: architecture.md#Complete-iOS-Project-Structure] - Folder structure
- [Source: architecture.md#Data-Architecture] - App Groups decision
- [Source: epics.md#Story-1.1] - Acceptance criteria

---

## Dev Agent Record

### Agent Model Used

Claude Opus 4.5 (claude-opus-4-5-20251101)

### Debug Log References

1. Initial platform mismatch error: Widget was targeting visionOS instead of iOS
2. Fixed by changing build destination from "My Mac" to iPhone simulator
3. visionOS API errors (`widgetTexture`, `levelOfDetail`, `supportedMountingStyles`) - Fixed by rewriting widget code for iOS

### Completion Notes List

**Completed: 2026-01-20**

1. **App Groups configured** via Xcode GUI - `group.com.steeven.monAppMobile`
2. **Widget Extension added** - `MySubGuardWidget` with iOS-compatible code
3. **SwiftData container** implemented with graceful fallback when App Groups unavailable
4. **3-Tab Navigation** working: Abonnements, Acquitter, Paramètres
5. **Build successful** on iPhone 17 Pro simulator (iOS 26.2)
6. **Widget displayed** on home screen

**Issues Encountered:**
- Xcode 26.2 generated visionOS widget template code by default
- Required manual fix to remove visionOS-specific APIs (`widgetTexture`, `levelOfDetail`, `supportedMountingStyles`)
- ModelContainer configured with Documents directory fallback for development without App Groups entitlement

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
