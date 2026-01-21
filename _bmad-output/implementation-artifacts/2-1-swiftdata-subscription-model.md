# Story 2.1: SwiftData Subscription Model

Status: ready-for-dev

## Story

As a **developer**,
I want **a SwiftData model for subscriptions**,
so that **user data persists locally and offline** (FR25).

## Acceptance Criteria

1. **AC1:** Subscription model includes: id, name, amount, frequency (enum), nextBillingDate, category (Pro/Personal), createdAt
2. **AC2:** Model uses @Model macro for SwiftData
3. **AC3:** ModelContainer is shared via App Groups (already configured in Story 1.1)
4. **AC4:** Data persists across app launches
5. **AC5:** All operations complete in < 100ms (NFR-P5)

## Tasks / Subtasks

- [ ] **Task 1: Create Subscription Model** (AC: #1, #2)
  - [ ] 1.1 Create `Core/Models/Subscription.swift`
  - [ ] 1.2 Add @Model macro
  - [ ] 1.3 Define properties: id (UUID), name, amount, frequency, nextBillingDate, category, createdAt
  - [ ] 1.4 Add computed property for annual amount

- [ ] **Task 2: Create Supporting Enums** (AC: #1)
  - [ ] 2.1 Create `BillingFrequency` enum (mensuel, annuel, hebdomadaire, trimestriel)
  - [ ] 2.2 Create `SubscriptionCategory` enum (pro, personal)
  - [ ] 2.3 Add display names in French

- [ ] **Task 3: Register Model with Container** (AC: #3)
  - [ ] 3.1 Add Subscription to ModelContainer schema
  - [ ] 3.2 Verify App Groups sharing works

- [ ] **Task 4: Test Persistence** (AC: #4, #5)
  - [ ] 4.1 Create test subscription
  - [ ] 4.2 Restart app and verify data persists
  - [ ] 4.3 Verify operations are fast (< 100ms)

## Dev Notes

### Model Definition

```swift
// Core/Models/Subscription.swift
import Foundation
import SwiftData

enum BillingFrequency: String, Codable, CaseIterable {
    case weekly = "hebdomadaire"
    case monthly = "mensuel"
    case quarterly = "trimestriel"
    case yearly = "annuel"

    var displayName: String {
        switch self {
        case .weekly: return "Hebdomadaire"
        case .monthly: return "Mensuel"
        case .quarterly: return "Trimestriel"
        case .yearly: return "Annuel"
        }
    }

    var monthlyMultiplier: Double {
        switch self {
        case .weekly: return 52.0 / 12.0
        case .monthly: return 1.0
        case .quarterly: return 1.0 / 3.0
        case .yearly: return 1.0 / 12.0
        }
    }
}

enum SubscriptionCategory: String, Codable, CaseIterable {
    case pro = "pro"
    case personal = "personal"

    var displayName: String {
        switch self {
        case .pro: return "Pro"
        case .personal: return "Personnel"
        }
    }
}

@Model
final class Subscription {
    var id: UUID
    var name: String
    var amount: Double
    var frequency: BillingFrequency
    var nextBillingDate: Date
    var category: SubscriptionCategory
    var createdAt: Date

    init(
        id: UUID = UUID(),
        name: String,
        amount: Double,
        frequency: BillingFrequency,
        nextBillingDate: Date,
        category: SubscriptionCategory = .personal,
        createdAt: Date = Date()
    ) {
        self.id = id
        self.name = name
        self.amount = amount
        self.frequency = frequency
        self.nextBillingDate = nextBillingDate
        self.category = category
        self.createdAt = createdAt
    }

    /// Converts amount to annual equivalent
    var annualAmount: Double {
        switch frequency {
        case .weekly: return amount * 52
        case .monthly: return amount * 12
        case .quarterly: return amount * 4
        case .yearly: return amount
        }
    }
}
```

### ModelContainer Update

```swift
// Update ModelContainer+Shared.swift
extension ModelContainer {
    static var shared: ModelContainer = {
        let schema = Schema([
            Subscription.self
        ])
        let config = ModelConfiguration(
            "MySubGuard",
            schema: schema,
            groupContainer: .identifier("group.com.steeven.mysubguard")
        )
        return try! ModelContainer(for: schema, configurations: config)
    }()
}
```

### Technical Requirements

| Requirement | Value |
|-------------|-------|
| Framework | SwiftData (iOS 17+) |
| Storage | App Groups shared container |
| Performance | < 100ms operations (NFR-P5) |

### References

- [Source: epics.md#Story-2.1] - Acceptance criteria
- [Source: architecture.md] - SwiftData decision
- [Apple Docs: SwiftData](https://developer.apple.com/documentation/swiftdata)

---

## Dev Agent Record

### Agent Model Used

_To be filled by dev agent_

### Completion Notes List

_To be filled after implementation_

### File List

Files to create:
1. `MySubGuard/Core/Models/Subscription.swift` - NEW

Files to modify:
1. `MySubGuard/Core/Persistence/ModelContainer+Shared.swift` - Add Subscription to schema
