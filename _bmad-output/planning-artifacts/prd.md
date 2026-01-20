---
stepsCompleted: ['step-01-init', 'step-02-discovery', 'step-03-success', 'step-04-journeys', 'step-05-domain', 'step-06-innovation', 'step-07-project-type', 'step-08-scoping', 'step-09-functional', 'step-10-nonfunctional', 'step-11-polish', 'step-12-complete']
completedAt: '2026-01-19'
status: complete
inputDocuments:
  - '_bmad-output/planning-artifacts/product-brief-monAppMobile-2026-01-19.md'
  - '_bmad-output/analysis/brainstorming-session-2026-01-19.md'
workflowType: 'prd'
documentCounts:
  briefs: 1
  research: 0
  brainstorming: 1
  projectDocs: 0
classification:
  projectType: mobile_app_ios
  domain: personal_finance_lifestyle
  complexity: medium
  projectContext: greenfield
date: 2026-01-19
author: Steeven
---

# Product Requirements Document - monAppMobile

**Author:** Steeven
**Date:** 2026-01-19

## Executive Summary

### Vision

**monAppMobile** est une application iOS de gestion d'abonnements qui redonne aux utilisateurs le contrôle sur leurs dépenses récurrentes — sans jamais accéder à leurs données bancaires.

### Différenciateur Unique

| Approche traditionnelle | monAppMobile |
|------------------------|--------------|
| Connexion bancaire requise | ❌ **Zéro accès banque** |
| Détection passive | ✅ **Acquittement conscient** (swipe pour valider) |
| Notifications intrusives | ✅ **Widget-first** (présence passive) |
| Affichage mensuel | ✅ **Annuel par défaut** (amplifier la prise de conscience) |

### Utilisateur Cible

**Marc, Dev SaaS freelance** — Profil tech-savvy, utilise 10-20 abonnements (pro + perso), méfiant envers les apps qui demandent ses accès bancaires, veut reprendre le contrôle sans y passer des heures.

### North Star Metric

**Nombre d'acquittements par semaine** — Mesure l'engagement actif et la création d'habitude.

### Scope MVP

8 features essentielles : Sign in Apple, Ajout manuel, Liste annuelle, Widget, Acquittement, Score de Contrôle, Affichage annuel, Admin Filament.

---

## Success Criteria

### User Success

**Moment "Aha" :** L'utilisateur voit son total annuel d'abonnements pour la première fois — prise de conscience immédiate de l'ampleur de ses dépenses récurrentes.

| Métrique | Cible | Mesure |
|----------|-------|--------|
| Score de Contrôle | > 90% | % d'abos acquittés avant prélèvement |
| Abos trackés | > 5 par user actif | Moyenne abos/utilisateur |
| Premier abo ajouté | < 2 min | Temps onboarding → 1er abo |
| Streak maintenu | > 7 jours | Jours consécutifs à 100% |
| Découverte abo oublié | ≥ 1 | User trouve un abo qu'il avait oublié |

**Moments de succès utilisateur :**
- Voir son total annuel → Prise de conscience
- Acquitter tous ses prélèvements du mois → Sentiment de maîtrise
- Décider d'annuler un abo sous-utilisé → Valeur concrète

---

### Business Success

| Horizon | Métrique | Cible |
|---------|----------|-------|
| **M3** | Téléchargements | 1 000+ |
| **M3** | Activation (≥1 abo) | 60% des downloads |
| **M3** | Engagement (≥5 abos) | 30% des activés |
| **M12** | Téléchargements | 10 000+ |
| **M12** | Rétention J30 | > 25% |
| **M12** | Conversion premium | 3-5% |

**North Star Metric :** Nombre d'acquittements par semaine

---

### Technical Success

| Critère | Cible |
|---------|-------|
| Performance Widget | Refresh < 1s, affichage < 100ms |
| Sync Catalogue | < 2s au lancement, cache 24h |
| Offline | 100% fonctionnel sans connexion |
| Crash Rate | < 0.1% (99.9% crash-free) |

---

### Measurable Outcomes

| Outcome | Indicateur | Signal de succès |
|---------|------------|------------------|
| **Valeur perçue** | NPS ou rating App Store | > 4.5 étoiles |
| **Habitude créée** | Rétention J30 | > 25% |
| **Monétisation viable** | MRR | Premiers revenus M12 |
| **Produit utilisable** | Usage créateur | Utilisation quotidienne J+7 |

---

## Product Scope

### MVP - Minimum Viable Product

| Feature | Priorité | Description |
|---------|----------|-------------|
| Sign in with Apple | P0 | Auth anonyme, privacy-first |
| Ajout manuel | P0 | Saisie abo (nom, montant, fréquence, date) |
| Liste des abos | P0 | Vue avec montants annuels |
| Widget iOS | P0 | Prochains prélèvements sur écran d'accueil |
| Acquittement conscient | P0 | Swipe pour valider |
| Score de Contrôle | P1 | % acquittés à temps |
| Affichage annuel | P0 | Montants en annuel par défaut |
| Admin Filament | P0 | Catalogue, logos, métriques |

**Critère MVP :** App fonctionnelle que le créateur utilise au quotidien.

---

### Growth Features (Post-MVP / V2)

| Feature | Raison du report |
|---------|------------------|
| Scan IA de factures | Saisie manuelle suffit pour valider le concept |
| Yearly Wrapped | Feature viralité, pas essentielle pour core |
| Animation Loot Box | Polish, pas critique |
| Gamification avancée | Badges, achievements, leaderboards |
| Streaks | Valider engagement de base d'abord |
| Notifications push | Widgets suffisent pour MVP |

---

### Vision (Future / V3+)

- Version Android
- Mode famille (comptes liés)
- Insights avancés (coût/utilisation, tendances)
- Export comptable pour freelances
- API partenaires

---

## User Journeys

### Journey 1 : Marc, Dev SaaS — Utilisateur Principal

#### Opening Scene (Le Problème)

*Vendredi soir, fin de trimestre. Marc fait sa compta freelance. Il épluche ses relevés bancaires pour identifier ses charges déductibles.*

**Sa frustration :**
- "C'est quoi ce prélèvement de 23,99€ ? Ah, Notion... je croyais que c'était 10€"
- Il découvre qu'il paie encore Figma alors qu'il utilise maintenant Sketch
- Son total SaaS ? Aucune idée. Il doit tout recalculer à la main
- Les apps bancaires veulent ses identifiants — hors de question

**État émotionnel :** Frustré, submergé, méfiant envers les solutions existantes.

#### Rising Action (Découverte & Onboarding)

*Marc tombe sur monAppMobile sur l'App Store. "Gestion abonnements sans connexion bancaire" — ça l'intrigue.*

**Étape 1 — Installation**
- Télécharge l'app (< 50 MB)
- Sign in with Apple en 1 tap
- Pas de formulaire, pas de permissions bizarres
- **Pensée :** "Ok, c'est rapide."

**Étape 2 — Premier Abo**
- L'app propose "Ajoute ton premier abo"
- Il tape "Claude" → le logo apparaît automatiquement (catalogue)
- Il entre 20$/mois
- **L'app affiche : 240$/an**
- **Pensée :** "Wow, 240$ par an juste pour Claude..."

**Étape 3 — Ajout en série**
- Il ajoute Cursor, Notion, Hostinger, GitHub Pro...
- Chaque abo : logo auto, montant annuel affiché
- En 10 minutes, il a ajouté ses 15 abos principaux

#### Climax (Le Moment "Aha")

*Marc a fini de saisir ses abos. Il regarde l'écran récapitulatif.*

**L'app affiche :**
> **Total annuel : 2 847 €**
> - Pro : 1 923 €
> - Perso : 924 €

**Réaction de Marc :** "Putain. 2 847€ par an. C'est un iPhone Pro Max."

Il réalise :
- Figma à 144€/an alors qu'il ne l'utilise plus → **à annuler**
- 3 services de stockage cloud différents → **à consolider**
- Son total pro est déductible → **il a sa liste pour le comptable**

**État émotionnel :** Choqué, puis soulagé d'enfin VOIR la réalité.

#### Resolution (Usage Quotidien)

*Les semaines suivantes...*

**Matin, écran d'accueil iPhone :**
- Widget monAppMobile : "Demain : Cursor 20$"
- Marc swipe → Acquitté ✓
- Score de Contrôle : 94%

**Chaque prélèvement :**
- Il le voit venir 3 jours avant
- Il l'acquitte consciemment
- Zéro surprise sur son compte

**Fin de trimestre :**
- Export de sa liste d'abos pro en 2 taps
- Compta faite en 5 minutes au lieu de 2 heures

**Nouvelle réalité :** Marc contrôle ses abos. Il ne les subit plus.

#### Journey Requirements (Marc)

| Capability | Requirement |
|------------|-------------|
| **Onboarding** | Sign in with Apple < 2 taps |
| **Catalogue** | Logos/noms auto-complétés |
| **Saisie** | Ajout abo < 30 secondes |
| **Affichage** | Total annuel visible immédiatement |
| **Widget** | Prochains prélèvements J-3 |
| **Acquittement** | Swipe depuis widget ou app |
| **Score** | % de contrôle temps réel |
| **Catégorisation** | Pro vs Perso (pour export) |

---

### Journey 2 : Admin (Steeven) — Gestion Catalogue

#### Opening Scene

*Tu lances monAppMobile. Les premiers utilisateurs arrivent. Ils ajoutent des abos que tu ne connais pas.*

**Ton besoin :**
- Voir qui utilise l'app et comment
- Gérer le catalogue de logos/noms
- Valider les suggestions communautaires
- Détecter les problèmes

#### Rising Action (Usage Admin)

**Matin — Check Dashboard**
- Tu ouvres Filament sur ton Mac
- Dashboard : 127 users, 342 abos ajoutés, 89% activation
- Funnel visuel : Downloads → 1er abo → 5+ abos → J30
- **Pensée :** "Le funnel tient, 60% ajoutent un premier abo"

**Notification — Nouvelle Suggestion**
- Alerte : "5 users ont ajouté 'Kagi Search' sans logo"
- Tu check : c'est un moteur de recherche payant
- Tu ajoutes le logo + catégorie "Productivité"
- Les 5 users voient le logo apparaître automatiquement

**Hebdo — Analyse Catalogue**
- Top abos ajoutés : Netflix, Spotify, Claude, ChatGPT
- Tu priorises les logos manquants
- Tu corriges une typo dans "Netflixx" → merge avec Netflix

#### Climax (Alerte Seuil)

*3 semaines après le lancement.*

**Notification Filament :** "🎉 1 000 téléchargements atteints"

Tu regardes les métriques :
- 1 024 downloads
- 614 users avec ≥1 abo (60% ✓)
- 187 users avec ≥5 abos (30% ✓)
- Rétention J7 : 28%

**Réaction :** "Ça marche. Les gens reviennent."

#### Resolution (Routine Admin)

**Routine quotidienne (5 min) :**
- Check funnel
- Valider 2-3 suggestions de logos
- Vérifier crash rate (< 0.1%)

**Routine hebdo (15 min) :**
- Analyse cohortes rétention
- Priorisation logos manquants
- Review métriques conversion premium

**Nouvelle réalité :** Tu pilotes l'app avec des données, pas à l'aveugle.

#### Journey Requirements (Admin)

| Capability | Requirement |
|------------|-------------|
| **Dashboard** | Métriques clés en temps réel |
| **Funnel** | Visualisation Downloads → Premium |
| **Catalogue CRUD** | Ajouter/modifier/supprimer abos |
| **Logos** | Upload, validation, fallback lettre |
| **Suggestions** | File d'attente avec fréquence |
| **Alertes** | Notification seuils franchis |
| **Cohortes** | Rétention par semaine d'inscription |
| **Merge** | Fusionner abos dupliqués/typos |

---

### Journey Requirements Summary

| User Type | Journeys | Capabilities Révélées |
|-----------|----------|----------------------|
| **Marc (Primary)** | Onboarding, Saisie, Acquittement quotidien | Auth, Catalogue, Widget, Score, Export |
| **Admin** | Dashboard, Validation, Monitoring | Filament CRUD, Métriques, Alertes, Cohortes |

---

## Domain-Specific Requirements

### Privacy & Data Architecture

| Donnée | Stockage | Justification |
|--------|----------|---------------|
| Abonnements (nom, montant, date, catégorie) | 🔒 **Local** (CoreData) | Privacy-first, données financières sensibles |
| User ID Apple | ☁️ **Serveur** (hash anonymisé) | Lier user → métriques sans identité |
| Événements analytics | ☁️ **Serveur** (anonymes) | Funnel, rétention, sans données perso |
| Statut premium | ☁️ **Serveur** | Vérification abonnement StoreKit |
| Suggestions d'abos | ☁️ **Serveur** | Enrichissement catalogue communautaire |

**Principe :** Données financières = local / Métriques anonymes = serveur

---

### Analytics (Minimal)

| Event | Description |
|-------|-------------|
| `app_download` | Installation |
| `first_abo_added` | Premier abonnement ajouté |
| `five_abos_reached` | 5+ abonnements |
| `retention_d1/d7/d30` | Retour J1, J7, J30 |
| `premium_converted` | Conversion premium |
| `crash_report` | Via Firebase Crashlytics ou Sentry |

**Pas de tracking :** Montants, noms d'abos, données personnelles.

---

### Backup & Persistence

| Aspect | Comportement |
|--------|--------------|
| **iCloud Backup** | ✅ Activé — abos sauvegardés automatiquement |
| **Changement iPhone** | Données restaurées via iCloud |
| **Suppression compte Apple** | Données locales persistent, perte sync serveur uniquement |
| **Désinstallation app** | Données supprimées (sauf backup iCloud) |

---

### In-App Purchases

| Règle Apple | Implementation |
|-------------|----------------|
| **StoreKit 2** | API moderne pour achats |
| **Restore Purchases** | Bouton obligatoire dans Settings |
| **Commission** | 30% Apple (15% si < 1M$/an) |
| **Cancel facile** | Deep link vers gestion abonnements iOS |
| **Tier unique** | 1 abonnement premium mensuel (MVP) |

---

### Security (MVP)

| Mesure | Priorité | Implementation |
|--------|----------|----------------|
| **Keychain** | P0 | Tokens Sign in Apple |
| **Encryption CoreData** | P0 | Encryption at rest (défaut iOS) |
| **HTTPS** | P0 | Toutes requêtes API |
| **Certificate Pinning** | P2 | V2 |
| **Biometric Lock** | P2 | Face ID/Touch ID optionnel |

---

### Compliance Summary

| Réglementation | Statut | Action |
|----------------|--------|--------|
| **RGPD** | ✅ Conforme | Données locales, analytics anonymes |
| **App Store Guidelines** | ✅ À respecter | StoreKit, Restore, pas de dark patterns |
| **PCI-DSS** | ⬜ N/A | Pas de transactions financières |
| **Open Banking** | ⬜ N/A | Pas de connexion bancaire |

---

## Innovation & Novel Patterns

### Detected Innovation Areas

| Innovation | Type | Différenciation |
|------------|------|-----------------|
| **Privacy-First** | Positionnement | Seule app de gestion abos sans connexion bancaire |
| **Acquittement Conscient** | Gesture UX | Swipe pour valider = user acteur, pas spectateur |
| **Widget-First Engagement** | Engagement | Présence passive vs notifications intrusives |
| **Annual Display Default** | Psychology | Amplification douleur financière pour prise de conscience |
| **Catalogue Communautaire** | Community | Users enrichissent la base de données |

---

### Market Context & Competitive Landscape

| Concurrent | Connexion Bancaire | Acquittement | Approche |
|------------|-------------------|--------------|----------|
| **Bankin** | ✅ Requise | ❌ Passif | Agrégation bancaire |
| **Linxo** | ✅ Requise | ❌ Passif | Agrégation bancaire |
| **Bobby** | ❌ Manuel | ❌ Passif | Liste simple |
| **Truebill/Rocket Money** | ✅ Requise | ❌ Passif | Agrégation + annulation |
| **monAppMobile** | ❌ Manuel | ✅ **Actif** | Contrôle conscient |

**Positionnement unique :** Intersection "Manuel + Actif" — aucun concurrent n'occupe ce créneau.

---

### Validation Approach

| Innovation | Méthode de Validation | Critère de Succès |
|------------|----------------------|-------------------|
| **Acquittement Conscient** | Dogfooding créateur | Utilisation quotidienne naturelle J+7 |
| **Acquittement Conscient** | Taux d'acquittement users | > 80% des prélèvements acquittés |
| **Widget-First** | Rétention J7 | > 25% (users reviennent via widget) |
| **Annual Display** | Feedback qualitatif | "Wow" moment lors premier total annuel |
| **Privacy-First** | Reviews App Store | Mentions positives privacy dans avis |

---

### Risk Mitigation

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Users n'adoptent pas le swipe | UX abandonnée | Tutoriel onboarding + fallback bouton "Vu" |
| Saisie manuelle trop fastidieuse | Friction onboarding | Catalogue riche + auto-complétion logos |
| Widget ignoré | Perte engagement | Alerte J-1 si prélèvement non acquitté (V2) |
| Privacy-first pas compris | Mauvais positionnement | Messaging clair "Zéro accès banque" dès App Store |

---

## Mobile App iOS — Specific Requirements

### Project-Type Overview

| Aspect | Choix |
|--------|-------|
| **Plateforme** | iOS uniquement (iPhone + iPad) |
| **Langage** | Swift 5.9+ |
| **Framework UI** | SwiftUI |
| **Architecture** | MVVM |
| **iOS Minimum** | iOS 17+ (pour SwiftData, WidgetKit moderne) |
| **Distribution** | App Store uniquement |

---

### Platform Requirements

| Requirement | Specification |
|-------------|---------------|
| **iPhone** | ✅ Obligatoire — écran principal |
| **iPad** | ✅ Supporté — layout adaptatif |
| **Mac (Catalyst)** | ❌ Non prévu MVP |
| **Apple Watch** | ❌ Non prévu (complication future possible) |
| **Orientation** | Portrait principal, Landscape iPad |

**iOS Version Strategy :**
- iOS 17+ minimum pour bénéficier de SwiftData et des dernières APIs WidgetKit
- Couvre ~85% des iPhones actifs

---

### Device Permissions

| Permission | Usage | Moment de demande |
|------------|-------|-------------------|
| **Sign in with Apple** | Authentification | Onboarding |
| **Notifications** | Alertes prélèvements (V2) | Après 1er abo ajouté |
| **Camera** | Scan factures (V2) | Quand user tape "Scanner" |
| **Network** | Sync catalogue, analytics | Implicite |

**Principe :** Demander les permissions au moment où l'utilisateur comprend pourquoi.

---

### Offline Mode

| Fonctionnalité | Offline | Online requis |
|----------------|---------|---------------|
| Voir ses abos | ✅ | |
| Ajouter un abo | ✅ | |
| Modifier/Supprimer | ✅ | |
| Acquitter | ✅ | |
| Widget | ✅ | |
| Score de contrôle | ✅ | |
| Sync catalogue logos | | ✅ Premier lancement |
| Suggestions communautaires | | ✅ |
| Premium check | | ✅ Périodique |

**Architecture Offline :**
- CoreData/SwiftData pour stockage local
- Cache catalogue logos (24h TTL)
- Queue de sync pour suggestions (upload quand online)

---

### Push Strategy (V2)

| Type | Contenu | Timing |
|------|---------|--------|
| **Alerte J-3** | "Cursor 20$ dans 3 jours" | 3 jours avant prélèvement |
| **Alerte J-1** | "Netflix demain, acquitte-le" | Si non acquitté J-1 |
| **Rappel hebdo** | "3 prélèvements cette semaine" | Lundi matin |

**MVP :** Pas de push — widgets suffisent.
**V2 :** Push optionnels, user peut désactiver par type.

---

### Store Compliance

| Règle Apple | Implementation |
|-------------|----------------|
| **App Review Guidelines** | Pas de dark patterns, privacy respectée |
| **StoreKit 2** | Achats in-app pour premium |
| **Restore Purchases** | Bouton obligatoire dans Settings |
| **Subscription Management** | Deep link vers gestion iOS |
| **Privacy Nutrition Label** | Déclarer : Analytics (anonymes), Sign in Apple |
| **App Tracking Transparency** | Non requis (pas de tracking cross-app) |

---

### Technical Architecture Considerations

```
┌─────────────────────────────────────────────────────────────┐
│                    iOS App (Swift/SwiftUI)                  │
├─────────────────────────────────────────────────────────────┤
│  UI Layer                                                   │
│  ├── SwiftUI Views (3 onglets max)                         │
│  ├── WidgetKit Extension                                    │
│  └── App Intents (Shortcuts, Siri)                         │
├─────────────────────────────────────────────────────────────┤
│  Business Layer                                             │
│  ├── ViewModels (MVVM)                                      │
│  ├── Services (Auth, Sync, Analytics)                       │
│  └── Domain Models (Subscription, User, Score)              │
├─────────────────────────────────────────────────────────────┤
│  Data Layer                                                 │
│  ├── SwiftData / CoreData (local storage)                  │
│  ├── Keychain (tokens)                                      │
│  └── UserDefaults (preferences)                             │
├─────────────────────────────────────────────────────────────┤
│  Network Layer                                              │
│  ├── URLSession + async/await                               │
│  ├── Catalogue API Client                                   │
│  └── Analytics Event Queue                                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ HTTPS REST
┌─────────────────────────────────────────────────────────────┐
│                   Laravel API Backend                        │
└─────────────────────────────────────────────────────────────┘
```

---

### Implementation Considerations

| Aspect | Recommandation |
|--------|----------------|
| **Dependency Management** | Swift Package Manager (SPM) |
| **CI/CD** | Xcode Cloud ou Fastlane |
| **Testing** | XCTest + XCUITest |
| **Analytics SDK** | Firebase Analytics (léger) ou PostHog |
| **Crash Reporting** | Firebase Crashlytics ou Sentry |
| **Code Signing** | Automatic signing (Xcode managed) |

---

## Project Scoping & Phased Development

### MVP Strategy & Philosophy

**MVP Approach :** Problem-Solving MVP
> Résoudre LE problème core avec le minimum de features. Pas de polish, pas de nice-to-have.

**Validation Criteria :** Le créateur utilise l'app quotidiennement pour ses propres abos.

**Resource Requirements :**
- 1 développeur iOS (Swift/SwiftUI)
- 1 développeur Backend (Laravel/Filament) — peut être la même personne
- Pas de designer dédié (SwiftUI natif + SF Symbols)

---

### MVP Feature Set (Phase 1)

**Core User Journeys Supported :**
- ✅ Marc : Onboarding → Saisie → Acquittement quotidien
- ✅ Admin : Dashboard → Validation catalogue → Monitoring

**Must-Have Capabilities :**

| # | Feature | Priorité |
|---|---------|----------|
| 1 | Sign in with Apple | P0 |
| 2 | Ajout manuel d'abonnement | P0 |
| 3 | Liste des abos (montants annuels) | P0 |
| 4 | Widget iOS (prochains prélèvements) | P0 |
| 5 | Acquittement conscient (swipe) | P0 |
| 6 | Score de Contrôle (%) | P0 |
| 7 | Affichage annuel par défaut | P0 |
| 8 | Admin Filament (catalogue, logos, métriques) | P0 |

---

### Post-MVP Features

**Phase 2 — Engagement (V2) :**
- Scan IA de factures (OCR + LLM)
- Notifications push (J-3, J-1)
- Streaks intelligents
- Animation Loot Box au scan

**Phase 3 — Viralité (V3) :**
- Yearly Wrapped partageable
- Gamification (badges, achievements)
- Insights coût/utilisation

**Phase 4 — Expansion (V4+) :**
- Version Android
- Mode famille (comptes liés)
- Export comptable freelances
- API partenaires

---

### Risk Mitigation Strategy

**Technical Risks :**
- Prototyper WidgetKit en premier (validation technique)
- Utiliser SwiftData avec fallback CoreData
- HTTPS + Keychain dès le départ

**Market Risks :**
- Messaging App Store clair : "Zéro connexion bancaire"
- Dogfooding intensif avant launch public
- Feedback beta testeurs (TestFlight)

**Resource Risks :**
- Scope MVP ultra-strict (8 features, pas plus)
- Pas de scope creep — tout le reste est V2+
- Ship & iterate plutôt que perfect & delay

---

## Functional Requirements

### 1. User Authentication

- **FR1:** User can sign in using Sign in with Apple
- **FR2:** User can sign out from the app
- **FR3:** User can use the app anonymously (no personal data required beyond Apple ID)
- **FR4:** System can restore user session automatically on app launch

---

### 2. Subscription Management

- **FR5:** User can add a new subscription manually
- **FR6:** User can specify subscription name, amount, frequency, and next billing date
- **FR7:** User can categorize subscription as Pro or Personal
- **FR8:** User can edit an existing subscription
- **FR9:** User can delete a subscription
- **FR10:** User can view all subscriptions in a list
- **FR11:** User can see subscription amounts displayed in annual format by default
- **FR12:** User can see the total annual cost of all subscriptions
- **FR13:** User can see total cost split by category (Pro vs Personal)
- **FR14:** System can auto-complete subscription name and logo from catalogue

---

### 3. Acquittement & Control

- **FR15:** User can acknowledge an upcoming payment (acquittement conscient)
- **FR16:** User can see which payments are pending acknowledgment
- **FR17:** User can see their Control Score (percentage of acknowledged payments)
- **FR18:** System can calculate Control Score based on timely acknowledgments
- **FR19:** User can see payments that passed without acknowledgment

---

### 4. Widget Experience

- **FR20:** User can add a widget to iOS home screen
- **FR21:** Widget can display upcoming payments (next 3-5 days)
- **FR22:** Widget can show payment amount and subscription name
- **FR23:** User can acknowledge payment directly from widget (swipe action)
- **FR24:** Widget can refresh automatically to show current data

---

### 5. Data & Sync

- **FR25:** User can use the app fully offline (local data storage)
- **FR26:** System can sync subscription catalogue on first launch
- **FR27:** System can cache catalogue logos with 24h TTL
- **FR28:** User can have data backed up via iCloud
- **FR29:** User can restore data when changing devices
- **FR30:** System can queue community suggestions for upload when online

---

### 6. Admin & Catalogue

- **FR31:** Admin can view dashboard with key metrics
- **FR32:** Admin can see funnel visualization (Downloads → Activation → Engagement → Retention)
- **FR33:** Admin can add a new subscription to the catalogue
- **FR34:** Admin can edit subscription details in catalogue
- **FR35:** Admin can upload and manage subscription logos
- **FR36:** Admin can view community-suggested subscriptions queue
- **FR37:** Admin can validate or reject community suggestions
- **FR38:** Admin can merge duplicate subscriptions (typos)
- **FR39:** Admin can see most-added subscriptions ranking
- **FR40:** Admin can receive alerts when thresholds are reached (1K, 5K, 10K users)

---

### 7. Analytics & Metrics

- **FR41:** System can track app download event
- **FR42:** System can track first subscription added event
- **FR43:** System can track 5+ subscriptions reached event
- **FR44:** System can track retention events (D1, D7, D30)
- **FR45:** System can track premium conversion event
- **FR46:** System can report crash events
- **FR47:** Admin can view retention cohorts by signup week

---

### 8. Premium & Monetization

- **FR48:** User can subscribe to premium tier via In-App Purchase
- **FR49:** User can restore previous purchases
- **FR50:** User can manage subscription via iOS subscription settings
- **FR51:** System can verify premium status via StoreKit

---

## Non-Functional Requirements

### Performance

| NFR | Requirement | Mesure |
|-----|-------------|--------|
| **NFR-P1** | Widget refresh completes in < 1 second | Time from trigger to display update |
| **NFR-P2** | Widget displays content in < 100ms | Initial render time |
| **NFR-P3** | App launches to usable state in < 2 seconds | Cold start time |
| **NFR-P4** | Catalogue sync completes in < 2 seconds | First launch sync duration |
| **NFR-P5** | Local data operations complete in < 100ms | CRUD on subscriptions |
| **NFR-P6** | App remains responsive during background sync | No UI blocking |

---

### Security

| NFR | Requirement | Mesure |
|-----|-------------|--------|
| **NFR-S1** | All authentication tokens stored in iOS Keychain | Security audit |
| **NFR-S2** | Local data encrypted at rest (iOS default) | Encryption verification |
| **NFR-S3** | All API communications over HTTPS | Network inspection |
| **NFR-S4** | No sensitive data logged in crash reports | Log audit |
| **NFR-S5** | User financial data never transmitted to server | Data flow audit |
| **NFR-S6** | Sign in with Apple token refresh handled gracefully | Session continuity |

---

### Reliability

| NFR | Requirement | Mesure |
|-----|-------------|--------|
| **NFR-R1** | Crash-free session rate > 99.9% | Firebase/Sentry metrics |
| **NFR-R2** | App functions 100% offline for core features | Airplane mode testing |
| **NFR-R3** | Data persists across app updates | Upgrade testing |
| **NFR-R4** | iCloud backup restores all user data | Device migration test |
| **NFR-R5** | Widget updates reliably via WidgetKit timeline | Widget refresh consistency |

---

### Scalability

| NFR | Requirement | Mesure |
|-----|-------------|--------|
| **NFR-SC1** | Backend handles 10K concurrent users | Load testing |
| **NFR-SC2** | Catalogue API response < 500ms at peak | API latency monitoring |
| **NFR-SC3** | Analytics ingestion handles 100K events/day | Event queue capacity |

---

### Accessibility

| NFR | Requirement | Mesure |
|-----|-------------|--------|
| **NFR-A1** | Full VoiceOver support for all screens | Accessibility audit |
| **NFR-A2** | Dynamic Type support for text scaling | Font size testing |
| **NFR-A3** | Minimum touch target size 44x44 points | UI inspection |
| **NFR-A4** | Color contrast meets WCAG AA standard | Contrast checker |

---

### Integration

| NFR | Requirement | Mesure |
|-----|-------------|--------|
| **NFR-I1** | StoreKit 2 integration handles all purchase states | IAP testing |
| **NFR-I2** | Sign in with Apple supports account recovery | Auth flow testing |
| **NFR-I3** | API gracefully handles offline/online transitions | Network transition tests |
| **NFR-I4** | iCloud sync handles merge conflicts | Multi-device testing |

