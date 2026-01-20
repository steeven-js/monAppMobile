---
stepsCompleted: ['step-01-validate-prerequisites', 'step-02-design-epics', 'step-03-create-stories', 'step-04-final-validation']
status: complete
completedAt: '2026-01-19'
inputDocuments:
  - '_bmad-output/planning-artifacts/prd.md'
  - '_bmad-output/planning-artifacts/architecture.md'
  - '_bmad-output/planning-artifacts/ux-design-specification.md'
metrics:
  totalEpics: 9
  totalStories: 53
  frCoverage: '51/51 (100%)'
  nfrIntegration: 24
---

# monAppMobile - Epic Breakdown

## Overview

This document provides the complete epic and story breakdown for monAppMobile, decomposing the requirements from the PRD, UX Design, and Architecture into implementable stories.

## Requirements Inventory

### Functional Requirements

**1. User Authentication (FR1-4)**
- FR1: User can sign in using Sign in with Apple
- FR2: User can sign out from the app
- FR3: User can use the app anonymously (no personal data required beyond Apple ID)
- FR4: System can restore user session automatically on app launch

**2. Subscription Management (FR5-14)**
- FR5: User can add a new subscription manually
- FR6: User can specify subscription name, amount, frequency, and next billing date
- FR7: User can categorize subscription as Pro or Personal
- FR8: User can edit an existing subscription
- FR9: User can delete a subscription
- FR10: User can view all subscriptions in a list
- FR11: User can see subscription amounts displayed in annual format by default
- FR12: User can see the total annual cost of all subscriptions
- FR13: User can see total cost split by category (Pro vs Personal)
- FR14: System can auto-complete subscription name and logo from catalogue

**3. Acquittement & Control (FR15-19)**
- FR15: User can acknowledge an upcoming payment (acquittement conscient)
- FR16: User can see which payments are pending acknowledgment
- FR17: User can see their Control Score (percentage of acknowledged payments)
- FR18: System can calculate Control Score based on timely acknowledgments
- FR19: User can see payments that passed without acknowledgment

**4. Widget Experience (FR20-24)**
- FR20: User can add a widget to iOS home screen
- FR21: Widget can display upcoming payments (next 3-5 days)
- FR22: Widget can show payment amount and subscription name
- FR23: User can acknowledge payment directly from widget (swipe action)
- FR24: Widget can refresh automatically to show current data

**5. Data & Sync (FR25-30)**
- FR25: User can use the app fully offline (local data storage)
- FR26: System can sync subscription catalogue on first launch
- FR27: System can cache catalogue logos with 24h TTL
- FR28: User can have data backed up via iCloud
- FR29: User can restore data when changing devices
- FR30: System can queue community suggestions for upload when online

**6. Admin & Catalogue (FR31-40)**
- FR31: Admin can view dashboard with key metrics
- FR32: Admin can see funnel visualization (Downloads → Activation → Engagement → Retention)
- FR33: Admin can add a new subscription to the catalogue
- FR34: Admin can edit subscription details in catalogue
- FR35: Admin can upload and manage subscription logos
- FR36: Admin can view community-suggested subscriptions queue
- FR37: Admin can validate or reject community suggestions
- FR38: Admin can merge duplicate subscriptions (typos)
- FR39: Admin can see most-added subscriptions ranking
- FR40: Admin can receive alerts when thresholds are reached (1K, 5K, 10K users)

**7. Analytics & Metrics (FR41-47)**
- FR41: System can track app download event
- FR42: System can track first subscription added event
- FR43: System can track 5+ subscriptions reached event
- FR44: System can track retention events (D1, D7, D30)
- FR45: System can track premium conversion event
- FR46: System can report crash events
- FR47: Admin can view retention cohorts by signup week

**8. Premium & Monetization (FR48-51)**
- FR48: User can subscribe to premium tier via In-App Purchase
- FR49: User can restore previous purchases
- FR50: User can manage subscription via iOS subscription settings
- FR51: System can verify premium status via StoreKit

### NonFunctional Requirements

**Performance (NFR-P1 to P6)**
- NFR-P1: Widget refresh completes in < 1 second
- NFR-P2: Widget displays content in < 100ms
- NFR-P3: App launches to usable state in < 2 seconds
- NFR-P4: Catalogue sync completes in < 2 seconds
- NFR-P5: Local data operations complete in < 100ms
- NFR-P6: App remains responsive during background sync

**Security (NFR-S1 to S6)**
- NFR-S1: All authentication tokens stored in iOS Keychain
- NFR-S2: Local data encrypted at rest (iOS default)
- NFR-S3: All API communications over HTTPS
- NFR-S4: No sensitive data logged in crash reports
- NFR-S5: User financial data never transmitted to server
- NFR-S6: Sign in with Apple token refresh handled gracefully

**Reliability (NFR-R1 to R5)**
- NFR-R1: Crash-free session rate > 99.9%
- NFR-R2: App functions 100% offline for core features
- NFR-R3: Data persists across app updates
- NFR-R4: iCloud backup restores all user data
- NFR-R5: Widget updates reliably via WidgetKit timeline

**Scalability (NFR-SC1 to SC3)**
- NFR-SC1: Backend handles 10K concurrent users
- NFR-SC2: Catalogue API response < 500ms at peak
- NFR-SC3: Analytics ingestion handles 100K events/day

**Accessibility (NFR-A1 to A4)**
- NFR-A1: Full VoiceOver support for all screens
- NFR-A2: Dynamic Type support for text scaling
- NFR-A3: Minimum touch target size 44x44 points
- NFR-A4: Color contrast meets WCAG AA standard

**Integration (NFR-I1 to I4)**
- NFR-I1: StoreKit 2 integration handles all purchase states
- NFR-I2: Sign in with Apple supports account recovery
- NFR-I3: API gracefully handles offline/online transitions
- NFR-I4: iCloud sync handles merge conflicts

### Additional Requirements

**From Architecture - Project Setup:**
- iOS Project: Xcode New Project (SwiftUI, Swift, SwiftData) + Widget Extension target
- Laravel Backend: Laravel 12 + Filament v4 + Spatie Permission
- App Groups enabled for Widget ↔ App data sharing
- Sentry integration for crash reporting
- Xcode Cloud CI/CD pipeline setup

**From Architecture - Technical Decisions:**
- SwiftData iOS 17 baseline (not iOS 18)
- Raw Keychain APIs (no external library)
- URLSession + async/await for networking
- @Observable + @Environment for state management
- REST API versioned (/api/v1/)
- JSON fields in snake_case

**From UX Design - Custom Components:**
- SubscriptionCard (list item with swipe actions)
- AcknowledgeSwipeView (swipe-to-acknowledge interaction)
- ControlScoreRing (Apple Fitness-style progress ring)
- AnnualTotalHeader (sticky header with counter animation)
- WidgetSubscriptionView (compact widget format)

**From UX Design - Interaction Patterns:**
- Haptic feedback on all actions (Medium for acknowledge, Light for tap)
- Skeleton shimmer for loading (not spinners)
- 3-tab navigation (Abonnements, Acquitter, Paramètres)
- Sheet modal for add subscription (not fullscreen)
- Deep links from widget to specific subscription

**From UX Design - Accessibility:**
- VoiceOver labels on all interactive elements
- Dynamic Type support (no fixed font sizes)
- Reduce Motion support for animations
- Dark mode via semantic colors
- 44x44pt minimum touch targets

### FR Coverage Map

| FR | Epic | Description |
|----|------|-------------|
| FR1-4 | Epic 1 | Authentication (Sign in Apple, Sign out, Anonymous, Session) |
| FR5-13 | Epic 2 | Subscription CRUD & Display |
| FR14 | Epic 3 | Auto-complete from catalogue |
| FR15-19 | Epic 4 | Acquittement & Control Score |
| FR20-24 | Epic 5 | Widget Experience |
| FR25 | Epic 2 | Offline local storage |
| FR26-27 | Epic 3 | Catalogue sync & cache |
| FR28-29 | Epic 6 | iCloud Backup & Restore |
| FR30 | Epic 3 | Community suggestions queue |
| FR31-40 | Epic 7 | Admin Panel & Catalogue Management |
| FR41-47 | Epic 8 | Analytics & Metrics |
| FR48-51 | Epic 9 | Premium & Monetization |

**Coverage: 51/51 FRs (100%)**

---

## Epic List

### Epic 1: Foundation & Authentication
**Goal:** Users can install, sign in, and have a functional app shell

**User Outcome:** A working iOS app where users can authenticate via Sign in with Apple and maintain their session across app launches.

**FRs Covered:** FR1, FR2, FR3, FR4

**Technical Foundation:**
- iOS project setup (SwiftUI, SwiftData, App Groups)
- Sign in with Apple integration
- Raw Keychain token storage
- Session restore on launch
- Basic app navigation structure (3-tab)

---

### Epic 2: Subscription Core
**Goal:** Users can fully manage their subscriptions offline

**User Outcome:** Users can add, edit, delete subscriptions and see their annual totals organized by category, all working 100% offline.

**FRs Covered:** FR5, FR6, FR7, FR8, FR9, FR10, FR11, FR12, FR13, FR25

**Key Components:**
- SubscriptionCard component
- AnnualTotalHeader (sticky with counter animation)
- Add/Edit subscription form (sheet modal)
- SwiftData local persistence
- Pro/Personal categorization

---

### Epic 3: Catalogue Integration
**Goal:** Enhanced subscription entry with pre-filled data and logos

**User Outcome:** Users get auto-complete suggestions when adding subscriptions, with logos and pre-filled data from a synced catalogue.

**FRs Covered:** FR14, FR26, FR27, FR30

**Key Features:**
- Catalogue sync from Laravel API
- Logo caching with 24h TTL
- Auto-complete search
- Community suggestion queue (offline-first)

---

### Epic 4: Acquittement Conscient
**Goal:** Core differentiating feature - conscious payment control

**User Outcome:** Users can acknowledge upcoming payments with a satisfying swipe gesture and track their "Control Score" showing their payment awareness.

**FRs Covered:** FR15, FR16, FR17, FR18, FR19

**Key Components:**
- AcknowledgeSwipeView (swipe gesture)
- Pending payments list
- ControlScoreRing (Apple Fitness-style)
- Missed acknowledgments tracking
- Haptic feedback (medium)

---

### Epic 5: Widget Experience
**Goal:** Daily engagement touch point without opening app

**User Outcome:** Users can add iOS widgets showing upcoming payments and acknowledge directly from their home screen.

**FRs Covered:** FR20, FR21, FR22, FR23, FR24

**Key Features:**
- WidgetKit integration (Small/Medium/Large)
- WidgetSubscriptionView component
- Timeline provider with auto-refresh
- Deep links to specific subscriptions
- App Groups data sharing

---

### Epic 6: Data Backup & Recovery
**Goal:** Users never lose their data

**User Outcome:** User data is automatically backed up to iCloud and can be restored when changing devices.

**FRs Covered:** FR28, FR29

**Key Features:**
- iCloud backup integration
- Cross-device data restore
- Merge conflict handling

---

### Epic 7: Backend Admin Panel
**Goal:** Admin can manage catalogue and view metrics

**User Outcome:** Administrators have a full dashboard to manage the subscription catalogue, review community suggestions, and monitor key metrics.

**FRs Covered:** FR31, FR32, FR33, FR34, FR35, FR36, FR37, FR38, FR39, FR40

**Technical Stack:**
- Laravel 12 + Filament v4
- Dashboard with funnel visualization
- Catalogue CRUD with logo upload
- Suggestions queue workflow
- User threshold alerts

---

### Epic 8: Analytics & Insights
**Goal:** Data-driven understanding of user behavior

**User Outcome:** System tracks key events for funnel analysis, retention metrics, and crash reporting.

**FRs Covered:** FR41, FR42, FR43, FR44, FR45, FR46, FR47

**Key Features:**
- Event tracking (download, activation, engagement, retention)
- Sentry crash reporting
- Retention cohorts by signup week
- Anonymous analytics (privacy-first)

---

### Epic 9: Premium Monetization
**Goal:** Revenue generation via premium tier

**User Outcome:** Users can upgrade to premium for additional features via In-App Purchase, with easy restore functionality.

**FRs Covered:** FR48, FR49, FR50, FR51

**Key Features:**
- StoreKit 2 integration
- Premium purchase flow
- Restore purchases
- Server-side receipt validation
- Premium feature gating

---

# Story Details

## Epic 1: Foundation & Authentication

### Story 1.1: Project Setup & Navigation Structure

As a **developer**,
I want **a properly configured iOS project with SwiftUI, SwiftData, and App Groups**,
So that **I have the foundation for all future development**.

**Acceptance Criteria:**

**Given** a new Xcode project
**When** the project is created
**Then** it uses SwiftUI App lifecycle with @main
**And** SwiftData is configured with a ModelContainer
**And** App Groups capability is enabled (group.com.steeven.monAppMobile)
**And** Widget Extension target is added
**And** 3-tab navigation is implemented (Abonnements, Acquitter, Paramètres)
**And** the app builds and runs on iOS 17+ simulator

---

### Story 1.2: Sign in with Apple Integration

As a **user**,
I want **to sign in using my Apple ID**,
So that **I can use the app without creating a new account** (FR1, FR3).

**Acceptance Criteria:**

**Given** the user is on the sign-in screen
**When** they tap "Sign in with Apple"
**Then** the Apple authentication sheet appears
**And** upon successful authentication, the user's identifier is received
**And** no personal data beyond Apple ID is required (anonymous mode)
**And** the user is redirected to the main app interface

**Given** the authentication fails or is cancelled
**When** the sheet is dismissed
**Then** the user remains on the sign-in screen
**And** an appropriate error message is shown (if failure)

---

### Story 1.3: Keychain Token Storage

As a **user**,
I want **my authentication token stored securely**,
So that **my session persists across app launches** (NFR-S1).

**Acceptance Criteria:**

**Given** a successful Sign in with Apple
**When** the authentication completes
**Then** the token is stored in iOS Keychain using raw Keychain APIs
**And** the token is retrievable for future sessions
**And** no external Keychain library is used

---

### Story 1.4: Session Restore & Sign Out

As a **user**,
I want **my session to restore automatically when I open the app**,
So that **I don't have to sign in every time** (FR4).

As a **user**,
I want **to sign out from the app**,
So that **I can switch accounts or protect my privacy** (FR2).

**Acceptance Criteria:**

**Given** the user has previously signed in
**When** they launch the app
**Then** the session is restored from Keychain
**And** they are taken directly to the main interface
**And** session restore completes in < 2 seconds (NFR-P3)

**Given** the user is signed in
**When** they tap "Sign out" in Settings
**Then** the Keychain token is deleted
**And** they are returned to the sign-in screen
**And** local data is NOT deleted (data belongs to device, not account)

---

## Epic 2: Subscription Core

### Story 2.1: SwiftData Subscription Model

As a **developer**,
I want **a SwiftData model for subscriptions**,
So that **user data persists locally and offline** (FR25).

**Acceptance Criteria:**

**Given** the app needs to store subscriptions
**When** the Subscription model is created
**Then** it includes: id, name, amount, frequency (enum), nextBillingDate, category (Pro/Personal), createdAt
**And** it uses @Model macro for SwiftData
**And** the ModelContainer is shared via App Groups
**And** data persists across app launches
**And** all operations complete in < 100ms (NFR-P5)

---

### Story 2.2: Subscription List View

As a **user**,
I want **to view all my subscriptions in a list**,
So that **I can see what I'm paying for** (FR10).

**Acceptance Criteria:**

**Given** the user has subscriptions
**When** they open the Abonnements tab
**Then** all subscriptions are displayed in a scrollable list
**And** each subscription shows name, logo placeholder, and annual amount
**And** the SubscriptionCard component is used
**And** VoiceOver reads "{name}, {amount} par an" (NFR-A1)
**And** skeleton shimmer is shown while loading

**Given** the user has no subscriptions
**When** they open the Abonnements tab
**Then** an empty state is shown with "Ajoute ton premier abo" message
**And** a prominent "+" button is displayed

---

### Story 2.3: Annual Total Header

As a **user**,
I want **to see my total annual subscription cost**,
So that **I understand my yearly spending at a glance** (FR11, FR12).

**Acceptance Criteria:**

**Given** the user has subscriptions
**When** the list view loads
**Then** AnnualTotalHeader is displayed at the top
**And** it shows the total annual cost with counter animation on first display
**And** all amounts are converted to annual format (monthly × 12, etc.)
**And** the header is sticky when scrolling
**And** Dynamic Type is supported (NFR-A2)

---

### Story 2.4: Add Subscription

As a **user**,
I want **to add a new subscription manually**,
So that **I can track a new service I'm paying for** (FR5, FR6, FR7).

**Acceptance Criteria:**

**Given** the user taps the "+" button
**When** the add form opens
**Then** it appears as a sheet modal (not fullscreen)
**And** fields include: name (required), amount (required), frequency picker, next billing date, category toggle (Pro/Personal)
**And** frequency options are: Mensuel, Annuel, Hebdomadaire, Trimestriel
**And** default category is Personal

**Given** the user fills all required fields
**When** they tap "Ajouter"
**Then** the subscription is saved to SwiftData
**And** the list refreshes with the new subscription
**And** haptic feedback (success) is triggered
**And** the sheet dismisses

**Given** required fields are missing
**When** the user taps "Ajouter"
**Then** inline validation errors are shown
**And** the form is not submitted

---

### Story 2.5: Edit Subscription

As a **user**,
I want **to edit an existing subscription**,
So that **I can update details when prices change** (FR8).

**Acceptance Criteria:**

**Given** the user taps on a subscription
**When** the detail view opens
**Then** an "Edit" button is available
**And** tapping Edit opens the edit form (same as add form, pre-filled)
**And** changes are saved to SwiftData
**And** the list reflects updates immediately

---

### Story 2.6: Delete Subscription

As a **user**,
I want **to delete a subscription I no longer have**,
So that **my list stays accurate** (FR9).

**Acceptance Criteria:**

**Given** the user is viewing a subscription
**When** they swipe left on the SubscriptionCard
**Then** a red "Supprimer" action appears
**And** tapping it shows a confirmation dialog

**Given** the user confirms deletion
**When** they tap "Supprimer" in the dialog
**Then** the subscription is removed from SwiftData
**And** the list animates the removal
**And** haptic feedback (warning) is triggered

---

### Story 2.7: Category Breakdown

As a **user**,
I want **to see my costs split by Pro vs Personal**,
So that **I know how much is work-related vs personal** (FR13).

**Acceptance Criteria:**

**Given** the user has subscriptions in both categories
**When** they tap on the AnnualTotalHeader
**Then** a breakdown expands showing Pro total and Personal total
**And** each shows count of subscriptions and annual amount
**And** the breakdown is collapsible

---

## Epic 3: Catalogue Integration

### Story 3.1: Network Service & API Client

As a **developer**,
I want **a network service for API communication**,
So that **the app can fetch data from the Laravel backend**.

**Acceptance Criteria:**

**Given** the app needs to communicate with the server
**When** NetworkService is implemented
**Then** it uses URLSession with async/await
**And** all requests use HTTPS (NFR-S3)
**And** JSON decoding uses snake_case keyDecodingStrategy
**And** base URL is configurable via environment
**And** offline/online transitions are handled gracefully (NFR-I3)

---

### Story 3.2: Catalogue Sync on First Launch

As a **user**,
I want **the subscription catalogue to sync when I first open the app**,
So that **I can use auto-complete immediately** (FR26).

**Acceptance Criteria:**

**Given** the user launches the app for the first time
**When** authentication is complete
**Then** the catalogue syncs automatically in the background
**And** sync completes in < 2 seconds (NFR-P4)
**And** a discreet progress indicator is shown
**And** the app remains responsive during sync (NFR-P6)

**Given** the sync fails (no network)
**When** the app launches
**Then** the user can still use the app (offline mode)
**And** sync is retried on next launch or connectivity change

---

### Story 3.3: Catalogue Cache with 24h TTL

As a **user**,
I want **catalogue data and logos to be cached**,
So that **the app works fast without constant downloads** (FR27).

**Acceptance Criteria:**

**Given** the catalogue has been synced
**When** the user opens the app within 24 hours
**Then** cached data is used (no network request)
**And** logos are loaded from local cache

**Given** the cache is older than 24 hours
**When** the user opens the app with network
**Then** the catalogue is refreshed in background
**And** stale data is shown until refresh completes

---

### Story 3.4: Auto-complete Subscription Name & Logo

As a **user**,
I want **subscription names and logos to auto-complete when I type**,
So that **adding subscriptions is faster and more accurate** (FR14).

**Acceptance Criteria:**

**Given** the user is adding a subscription
**When** they start typing in the name field
**Then** matching catalogue items appear as suggestions
**And** suggestions show name and logo
**And** tapping a suggestion fills name, logo, and suggests typical amount

**Given** no matching catalogue item exists
**When** the user types a name
**Then** they can continue with manual entry
**And** an option to "Suggérer au catalogue" appears

---

### Story 3.5: Community Suggestion Queue

As a **user**,
I want **to suggest new subscriptions to the catalogue**,
So that **others can benefit from my additions** (FR30).

**Acceptance Criteria:**

**Given** the user adds a subscription not in the catalogue
**When** they tap "Suggérer au catalogue"
**Then** the suggestion is queued locally (offline-first)
**And** the queue syncs when online
**And** no personal data is sent (only subscription name)

**Given** the device is offline
**When** the user makes a suggestion
**Then** it's stored in the local queue
**And** synced automatically when connectivity returns

---

## Epic 4: Acquittement Conscient

### Story 4.1: Acknowledgment Data Model

As a **developer**,
I want **a SwiftData model for acknowledgments**,
So that **we can track which payments have been acknowledged**.

**Acceptance Criteria:**

**Given** the app needs to track acknowledgments
**When** the Acknowledgment model is created
**Then** it includes: id, subscriptionId, billingDate, acknowledgedAt (optional), status (pending/acknowledged/missed)
**And** it uses @Model macro for SwiftData
**And** it links to Subscription via relationship

---

### Story 4.2: Pending Payments List

As a **user**,
I want **to see which payments are coming up and need acknowledgment**,
So that **I know what to expect on my bank statement** (FR16).

**Acceptance Criteria:**

**Given** the user has subscriptions with upcoming billing dates
**When** they open the "Acquitter" tab
**Then** pending payments for the next 7 days are displayed
**And** each shows subscription name, amount, and billing date
**And** payments are sorted by date (soonest first)
**And** VoiceOver reads "{name}, {amount}, échéance {date}"

**Given** no payments are pending
**When** the user opens the Acquitter tab
**Then** "Bravo ! 🎉 Rien à acquitter" is displayed

---

### Story 4.3: Swipe to Acknowledge

As a **user**,
I want **to acknowledge a payment with a swipe gesture**,
So that **I consciously note the upcoming charge** (FR15).

**Acceptance Criteria:**

**Given** the user sees a pending payment
**When** they swipe right on the AcknowledgeSwipeView
**Then** the payment is marked as acknowledged
**And** acknowledgedAt timestamp is recorded
**And** haptic feedback (medium) is triggered
**And** a green checkmark animation plays
**And** the item animates out of the pending list

**Given** the swipe is incomplete (< 50% threshold)
**When** the user releases
**Then** the item snaps back
**And** no acknowledgment is recorded

---

### Story 4.4: Control Score Calculation

As a **user**,
I want **the system to calculate my Control Score**,
So that **I can see how aware I am of my payments** (FR18).

**Acceptance Criteria:**

**Given** the user has payment history
**When** the Control Score is calculated
**Then** score = (acknowledged payments / total payments due) × 100
**And** only payments from the last 90 days are counted
**And** score updates in real-time after each acknowledgment
**And** score is stored locally (not sent to server)

---

### Story 4.5: Control Score Display

As a **user**,
I want **to see my Control Score prominently**,
So that **I'm motivated to stay aware of my finances** (FR17).

**Acceptance Criteria:**

**Given** the user opens the Acquitter tab
**When** the view loads
**Then** ControlScoreRing is displayed at the top
**And** it shows the percentage in Apple Fitness ring style
**And** animation fills the ring on first display
**And** color is green (>80%), orange (50-80%), red (<50%)
**And** VoiceOver reads "Score de contrôle : {%}"

---

### Story 4.6: Missed Payments Tracking

As a **user**,
I want **to see payments that passed without my acknowledgment**,
So that **I can be more vigilant next time** (FR19).

**Acceptance Criteria:**

**Given** a billing date has passed
**When** the payment was not acknowledged
**Then** it's marked as "missed"
**And** it appears in a "Manqués" section below pending
**And** it shows with a subtle red indicator
**And** user can still acknowledge late (affects score less)

**Given** the user acknowledges a missed payment
**When** they swipe right
**Then** it's marked as "acknowledged late"
**And** score impact is reduced (0.5 instead of 1.0)

---

## Epic 5: Widget Experience

### Story 5.1: Widget Extension Setup

As a **developer**,
I want **a properly configured WidgetKit extension**,
So that **users can add widgets to their home screen** (FR20).

**Acceptance Criteria:**

**Given** the Widget Extension target exists
**When** the widget is configured
**Then** it supports Small, Medium, and Large sizes
**And** it uses App Groups to access shared data
**And** the widget appears in the iOS widget gallery
**And** it has a descriptive name and preview

---

### Story 5.2: Widget Timeline Provider

As a **user**,
I want **my widget to refresh automatically**,
So that **I always see current upcoming payments** (FR24).

**Acceptance Criteria:**

**Given** the widget is on the home screen
**When** WidgetKit requests a timeline
**Then** the provider generates entries for the next 24 hours
**And** timeline refreshes at midnight and after each acknowledgment
**And** widget refresh completes in < 1 second (NFR-P1)
**And** widget displays content in < 100ms (NFR-P2)

---

### Story 5.3: Widget Display - Upcoming Payments

As a **user**,
I want **my widget to show upcoming payments**,
So that **I see what's due without opening the app** (FR21, FR22).

**Acceptance Criteria:**

**Given** the user has pending payments
**When** the widget displays
**Then** Small widget shows next 1 payment (name + amount)
**And** Medium widget shows next 2-3 payments
**And** Large widget shows next 3-5 payments with dates
**And** WidgetSubscriptionView component is used
**And** amounts are displayed in annual format
**And** VoiceOver reads payment details

**Given** no payments are pending
**When** the widget displays
**Then** "Tout est acquitté ✓" is shown

---

### Story 5.4: Widget Deep Links

As a **user**,
I want **to tap a payment in the widget to open the app**,
So that **I can quickly acknowledge or see details**.

**Acceptance Criteria:**

**Given** the user taps a payment in the widget
**When** the deep link is triggered
**Then** the app opens to the Acquitter tab
**And** the specific subscription is highlighted
**And** the user can acknowledge immediately

**Given** the user taps the widget header/empty area
**When** the deep link is triggered
**Then** the app opens to the main view

---

### Story 5.5: Widget Acknowledge Action

As a **user**,
I want **to acknowledge a payment directly from the widget**,
So that **I don't have to open the app** (FR23).

**Acceptance Criteria:**

**Given** the user long-presses the widget (Medium/Large)
**When** they select "Acquitter prochain"
**Then** the next pending payment is acknowledged
**And** the widget refreshes immediately
**And** haptic feedback is triggered
**And** a brief confirmation appears

**Given** there are no pending payments
**When** the user tries to acknowledge
**Then** nothing happens (action is disabled)

---

## Epic 6: Data Backup & Recovery

### Story 6.1: iCloud Backup Configuration

As a **user**,
I want **my subscription data to be backed up to iCloud automatically**,
So that **I don't lose my data if I lose my phone** (FR28).

**Acceptance Criteria:**

**Given** the app uses SwiftData in the Documents directory
**When** iOS performs iCloud backup
**Then** all subscription and acknowledgment data is included
**And** no additional configuration is needed (iOS default behavior)
**And** user financial data stays local (never synced to server - NFR-S5)

**Given** the user has iCloud backup disabled
**When** they use the app
**Then** data persists locally only
**And** a subtle reminder appears in Settings

---

### Story 6.2: Data Restore on New Device

As a **user**,
I want **to restore my data when I get a new device**,
So that **I don't have to re-enter all my subscriptions** (FR29).

**Acceptance Criteria:**

**Given** the user sets up a new device from iCloud backup
**When** the app is installed
**Then** all subscriptions are restored
**And** all acknowledgment history is restored
**And** Control Score is preserved
**And** data restore completes automatically (NFR-R4)

**Given** the user signs in with the same Apple ID
**When** the app launches on the new device
**Then** local data from backup is used
**And** no server sync of personal data occurs

---

### Story 6.3: Merge Conflict Handling

As a **user**,
I want **data conflicts to be handled gracefully**,
So that **I don't lose data if I use multiple devices** (NFR-I4).

**Acceptance Criteria:**

**Given** the same subscription exists on two devices
**When** backups merge
**Then** the most recently modified version wins
**And** no data is silently lost
**And** user is not prompted for manual conflict resolution

**Given** a subscription exists on one device but not another
**When** backups merge
**Then** the subscription is added (not deleted)

---

## Epic 7: Backend Admin Panel

### Story 7.1: Laravel Project Setup

As a **developer**,
I want **a Laravel 12 + Filament v4 backend**,
So that **we have an admin panel foundation**.

**Acceptance Criteria:**

**Given** the backend project needs to be created
**When** the project is initialized
**Then** Laravel 12 is installed
**And** Filament v4 is configured with admin panel
**And** Spatie Permission is installed for RBAC
**And** Supabase PostgreSQL connection is configured
**And** API routes are versioned (/api/v1/)

---

### Story 7.2: Catalogue Item Model & Resource

As an **admin**,
I want **to manage subscription catalogue items**,
So that **users have accurate auto-complete data** (FR33, FR34).

**Acceptance Criteria:**

**Given** the admin opens the Filament panel
**When** they navigate to Catalogue
**Then** all catalogue items are listed with search/filter
**And** they can create new items (name, typical_amount, logo_url, category)
**And** they can edit existing items
**And** changes are reflected in the API immediately

---

### Story 7.3: Logo Upload & Management

As an **admin**,
I want **to upload and manage subscription logos**,
So that **the app displays recognizable brand images** (FR35).

**Acceptance Criteria:**

**Given** the admin is editing a catalogue item
**When** they upload a logo
**Then** the image is stored in storage/app/logos/
**And** the logo URL is saved to the database
**And** logos are served via public URL
**And** image validation ensures reasonable file size (< 500KB)

---

### Story 7.4: Admin Dashboard

As an **admin**,
I want **to see key metrics on the dashboard**,
So that **I understand how the app is performing** (FR31).

**Acceptance Criteria:**

**Given** the admin opens the Filament dashboard
**When** the page loads
**Then** key metrics are displayed: total users, active users (30d), total subscriptions tracked, premium conversions
**And** metrics update in real-time
**And** trend indicators show week-over-week change

---

### Story 7.5: Funnel Visualization

As an **admin**,
I want **to see a funnel from download to retention**,
So that **I can identify drop-off points** (FR32).

**Acceptance Criteria:**

**Given** the admin views the dashboard
**When** the funnel widget loads
**Then** it shows: Downloads → First Subscription → 5+ Subscriptions → Day 7 Retention → Day 30 Retention
**And** each stage shows count and conversion percentage
**And** the visualization is a horizontal funnel chart

---

### Story 7.6: Community Suggestions Queue

As an **admin**,
I want **to review community-suggested subscriptions**,
So that **I can add popular ones to the catalogue** (FR36, FR37).

**Acceptance Criteria:**

**Given** users have submitted suggestions
**When** the admin opens the Suggestions resource
**Then** pending suggestions are listed with count of submissions
**And** admin can "Approve" (creates catalogue item) or "Reject"
**And** approved suggestions are removed from queue
**And** rejected suggestions are marked (can be bulk-deleted)

---

### Story 7.7: Merge Duplicate Subscriptions

As an **admin**,
I want **to merge duplicate catalogue entries**,
So that **typos don't create fragmented data** (FR38).

**Acceptance Criteria:**

**Given** the admin identifies duplicate entries (e.g., "Netflix" and "Netfix")
**When** they select "Merge" action
**Then** they choose the primary entry to keep
**And** all references are updated to the primary
**And** the duplicate is deleted
**And** merge is logged for audit

---

### Story 7.8: Most-Added Subscriptions Ranking

As an **admin**,
I want **to see which subscriptions are most popular**,
So that **I can prioritize catalogue quality** (FR39).

**Acceptance Criteria:**

**Given** the admin views the dashboard
**When** the ranking widget loads
**Then** top 10 most-added subscriptions are displayed
**And** each shows name, logo, and count
**And** ranking updates daily

---

### Story 7.9: User Threshold Alerts

As an **admin**,
I want **to receive alerts when user milestones are reached**,
So that **I can celebrate and prepare for scale** (FR40).

**Acceptance Criteria:**

**Given** the system tracks total users
**When** thresholds are crossed (1K, 5K, 10K, 50K, 100K)
**Then** an alert notification is sent (email or Slack)
**And** the milestone is logged
**And** each threshold only triggers once

---

## Epic 8: Analytics & Insights

### Story 8.1: Analytics Service Setup

As a **developer**,
I want **an analytics service for event tracking**,
So that **we can understand user behavior**.

**Acceptance Criteria:**

**Given** the app needs analytics
**When** AnalyticsService is implemented
**Then** it provides a track(event, properties) method
**And** events are queued locally and sent in batches
**And** no sensitive data is included (NFR-S4, NFR-S5)
**And** user ID is anonymized (SHA256 hash)
**And** the service handles offline gracefully

---

### Story 8.2: Sentry Crash Reporting

As a **developer**,
I want **crash reports sent to Sentry**,
So that **we can fix issues quickly** (FR46).

**Acceptance Criteria:**

**Given** the app crashes
**When** the crash occurs
**Then** the crash report is sent to Sentry
**And** no sensitive user data is included (NFR-S4)
**And** crash-free rate is trackable (target: 99.9% - NFR-R1)
**And** Sentry DSN is configured via environment

---

### Story 8.3: Funnel Event Tracking

As a **product owner**,
I want **key funnel events tracked**,
So that **I can measure user progression** (FR41, FR42, FR43).

**Acceptance Criteria:**

**Given** the user performs key actions
**When** each milestone is reached
**Then** the following events are tracked:
- `app_downloaded` - first launch (FR41)
- `first_subscription_added` - first subscription created (FR42)
- `five_subscriptions_reached` - 5th subscription added (FR43)
**And** each event includes anonymous user ID and timestamp
**And** events are sent to backend analytics endpoint

---

### Story 8.4: Retention Event Tracking

As a **product owner**,
I want **retention events tracked**,
So that **I can measure user stickiness** (FR44).

**Acceptance Criteria:**

**Given** the user returns to the app
**When** they open it on D1, D7, D30 after first launch
**Then** retention events are tracked: `retention_d1`, `retention_d7`, `retention_d30`
**And** first launch date is stored locally
**And** each retention event fires only once per user

---

### Story 8.5: Premium Conversion Tracking

As a **product owner**,
I want **premium conversions tracked**,
So that **I can measure monetization** (FR45).

**Acceptance Criteria:**

**Given** the user subscribes to premium
**When** the purchase completes
**Then** `premium_conversion` event is tracked
**And** event includes: price tier, days since first launch
**And** no payment details are sent (privacy)

---

### Story 8.6: Analytics API Endpoint

As a **developer**,
I want **an API endpoint to receive analytics events**,
So that **iOS can send tracking data**.

**Acceptance Criteria:**

**Given** the iOS app sends events
**When** POST /api/v1/analytics is called
**Then** events are validated and stored
**And** batch uploads are supported (array of events)
**And** endpoint handles 100K events/day (NFR-SC3)
**And** response is < 500ms (NFR-SC2)

---

### Story 8.7: Retention Cohorts View

As an **admin**,
I want **to view retention cohorts by signup week**,
So that **I can see if retention is improving** (FR47).

**Acceptance Criteria:**

**Given** the admin opens the Filament dashboard
**When** they view the Retention Cohorts widget
**Then** a table shows signup weeks as rows
**And** columns show D1, D7, D30 retention percentages
**And** cells are color-coded (green > 50%, red < 20%)
**And** data is filterable by date range

---

## Epic 9: Premium Monetization

### Story 9.1: StoreKit 2 Service Setup

As a **developer**,
I want **a StoreKitService for in-app purchases**,
So that **we can monetize the app** (NFR-I1).

**Acceptance Criteria:**

**Given** the app needs IAP functionality
**When** StoreKitService is implemented
**Then** it uses StoreKit 2 with async/await
**And** it handles all purchase states (purchasing, purchased, failed, restored)
**And** premium status is cached locally
**And** the service works offline (cached status)

---

### Story 9.2: Premium Offer Screen

As a **user**,
I want **to see what premium offers**,
So that **I can decide if it's worth upgrading** (FR48).

**Acceptance Criteria:**

**Given** the user taps "Premium" in Settings
**When** the PremiumOfferView opens
**Then** it shows premium benefits clearly
**And** pricing is fetched from StoreKit (localized)
**And** a prominent "Subscribe" button is displayed
**And** current free vs premium comparison is shown

---

### Story 9.3: Premium Purchase Flow

As a **user**,
I want **to subscribe to premium via In-App Purchase**,
So that **I can unlock additional features** (FR48).

**Acceptance Criteria:**

**Given** the user taps "Subscribe"
**When** they complete the Apple payment flow
**Then** premium status is activated immediately
**And** the UI updates to show premium badge
**And** `premium_conversion` analytics event is sent
**And** haptic feedback (success) is triggered

**Given** the purchase fails or is cancelled
**When** the flow ends
**Then** an appropriate message is shown
**And** the user can retry

---

### Story 9.4: Premium Status Verification

As a **developer**,
I want **to verify premium status via StoreKit**,
So that **we can gate premium features reliably** (FR51).

**Acceptance Criteria:**

**Given** the app launches
**When** StoreKitService initializes
**Then** current entitlements are checked via StoreKit 2
**And** premium status is updated from App Store
**And** status is cached for offline use
**And** expired subscriptions are handled gracefully

---

### Story 9.5: Restore Purchases

As a **user**,
I want **to restore my previous purchases**,
So that **I can recover premium on a new device** (FR49).

**Acceptance Criteria:**

**Given** the user taps "Restore Purchases"
**When** StoreKit syncs with App Store
**Then** previous purchases are restored
**And** premium status is activated if valid
**And** a confirmation message is shown
**And** if no purchases found, a helpful message is displayed

---

### Story 9.6: Manage Subscription Link

As a **user**,
I want **to manage my subscription via iOS Settings**,
So that **I can cancel or change my plan easily** (FR50).

**Acceptance Criteria:**

**Given** the user is premium
**When** they tap "Manage Subscription" in Settings
**Then** they are deep-linked to iOS subscription management
**And** the correct subscription is pre-selected
**And** they can cancel, upgrade, or downgrade

---

### Story 9.7: Premium Feature Gating

As a **developer**,
I want **to gate certain features for premium users**,
So that **free users are incentivized to upgrade**.

**Acceptance Criteria:**

**Given** premium-only features are defined
**When** a free user tries to access them
**Then** a soft paywall is shown (preview + upgrade CTA)
**And** premium users access features directly
**And** feature gating is centralized in PremiumService

**Premium Features (MVP):**
- Unlimited subscriptions (free = 5 max)
- Export data
- Custom categories

---

## Summary

| Epic | Stories | FRs Covered |
|------|---------|-------------|
| 1. Foundation & Auth | 4 | FR1-4 |
| 2. Subscription Core | 7 | FR5-13, FR25 |
| 3. Catalogue Integration | 5 | FR14, FR26-27, FR30 |
| 4. Acquittement Conscient | 6 | FR15-19 |
| 5. Widget Experience | 5 | FR20-24 |
| 6. Data Backup & Recovery | 3 | FR28-29 |
| 7. Backend Admin Panel | 9 | FR31-40 |
| 8. Analytics & Insights | 7 | FR41-47 |
| 9. Premium Monetization | 7 | FR48-51 |
| **TOTAL** | **53** | **51 FRs (100%)** |

