---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
completedAt: '2026-01-19'
status: complete
inputDocuments:
  - '_bmad-output/planning-artifacts/prd.md'
  - '_bmad-output/planning-artifacts/product-brief-monAppMobile-2026-01-19.md'
workflowType: 'ux-design'
date: 2026-01-19
author: Steeven
---

# UX Design Specification — monAppMobile

**Author:** Steeven
**Date:** 2026-01-19

---

## Executive Summary

### Project Vision

monAppMobile est une application iOS de gestion d'abonnements qui permet aux utilisateurs de reprendre le contrôle sur leurs dépenses récurrentes — sans jamais connecter leur compte bancaire. L'approche UX est centrée sur l'engagement actif plutôt que le suivi passif.

### Target Users

**Persona Principal : Marc, Dev SaaS (32 ans)**
- Freelance tech-savvy avec 15-20 abonnements (SaaS pro + streaming perso)
- Méfiant envers les apps demandant l'accès bancaire
- Besoin : visibilité totale et contrôle conscient sur ses dépenses récurrentes
- Usage : quotidien via widget iOS, saisie ponctuelle dans l'app

**Caractéristiques UX Utilisateur :**
- Familier avec les patterns iOS natifs
- Préfère la simplicité à la richesse fonctionnelle
- Valorise la privacy et le contrôle de ses données
- Disponible quelques secondes par jour (widget), quelques minutes par mois (ajout/gestion)

### Key Design Challenges

| Défi | Solution UX Envisagée |
|------|----------------------|
| **Friction saisie manuelle** | Catalogue riche avec auto-complétion + logos |
| **Adoption du swipe acquittement** | Onboarding visuel + fallback bouton "Vu" |
| **Lisibilité widget** | Information dense mais scannable en 2 secondes |
| **Choc du total annuel** | Framing positif : "prise de conscience" pas culpabilité |

### Design Opportunities

| Opportunité | Impact |
|-------------|--------|
| **Moment "Aha" Annuel** | Premier affichage du total annuel = mémorable et différenciant |
| **Widget-First Engagement** | Présence passive sur home screen > notifications intrusives |
| **Simplicité Radicale** | 3 onglets max, SwiftUI natif, SF Symbols = confiance immédiate |
| **Score de Contrôle** | Gamification légère qui renforce le sentiment de maîtrise |

---

## Core User Experience

### Defining Experience

**Core Loop :** Widget → Voir prélèvement → Swipe acquitter → Score augmente → Sentiment de contrôle

L'expérience est centrée sur l'**acquittement conscient** — transformer un événement passif (prélèvement automatique) en action consciente (validation explicite). Chaque interaction renforce le sentiment de maîtrise financière.

### Platform Strategy

| Aspect | Choix | Justification |
|--------|-------|---------------|
| **Plateforme** | iOS 17+ | SwiftData, WidgetKit moderne, 85% iPhones actifs |
| **Devices** | iPhone (principal) + iPad (adaptatif) | Portrait first, landscape iPad |
| **Framework** | SwiftUI + SF Symbols | Natif, performant, accessible |
| **Offline** | Core features 100% offline | Données locales, sync catalogue async |
| **Intégrations** | Sign in Apple, WidgetKit, StoreKit 2 | Écosystème Apple natif |

### Effortless Interactions

| Interaction | Effort | Implementation |
|-------------|--------|----------------|
| Voir prochains prélèvements | **0 tap** | Widget home screen |
| Ajouter abo connu | **< 30s** | Auto-complétion catalogue + logos |
| Acquitter un prélèvement | **1 geste** | Swipe horizontal + haptic feedback |
| Voir total annuel | **Toujours visible** | Header persistant dans liste |
| Catégoriser Pro/Perso | **1 tap** | Toggle dans formulaire ajout |

### Critical Success Moments

| Moment | Description | Indicateur de Succès |
|--------|-------------|---------------------|
| **Onboarding** | Sign in Apple → 1er abo < 2 min | Conversion 60% |
| **Moment Aha** | 1er affichage total annuel | Réaction "wow" |
| **1er Acquittement** | Swipe compris et exécuté | Pas de confusion |
| **Widget Installé** | Proposé après 1er abo | Taux d'installation |
| **Habitude Créée** | Retour J7 via widget | Rétention > 25% |

### Experience Principles

1. **Action > Information** — Chaque écran pousse à AGIR, pas juste à VOIR
2. **Zéro Friction** — Chaque interaction optimisée pour le minimum de gestes
3. **Prise de Conscience** — Affichage annuel par défaut pour amplifier l'impact
4. **Privacy Absolue** — Jamais d'accès bancaire, données locales, confiance
5. **iOS Natif** — SwiftUI, SF Symbols, patterns Apple pour familiarité immédiate

---

## Desired Emotional Response

### Primary Emotional Goals

| Priorité | Émotion | Description |
|----------|---------|-------------|
| **#1** | **Contrôle** | "Je maîtrise mes abos, je ne les subis plus" |
| **#2** | **Prise de Conscience** | "Wow, je dépense TANT par an ?" |
| **#3** | **Confiance** | "Mes données restent sur mon iPhone" |
| **#4** | **Fierté** | "Mon Score de Contrôle est à 100%" |

### Emotional Journey Mapping

| Phase | État Initial | État Cible | Transition |
|-------|--------------|------------|------------|
| **Découverte** | Méfiance (apps bancaires) | Curiosité confiante | "Zéro accès banque" |
| **Onboarding** | "Encore une app..." | "C'est rapide !" | Sign in 1 tap |
| **Moment Aha** | Ignorance du total | Choc → Prise de conscience | Total annuel affiché |
| **Usage quotidien** | Anxiété prélèvements | Sérénité contrôlée | Widget visible |
| **Fidélisation** | Habitude passive | Fierté active | Score de Contrôle |

### Micro-Emotions

**À Cultiver :**
- Confiance dès le premier écran (privacy messaging)
- Satisfaction après chaque acquittement (micro-feedback)
- Surprise positive au total annuel (pas culpabilisation)
- Efficacité dans chaque interaction (zéro friction)

**À Éviter :**
- Culpabilité ou jugement sur les dépenses
- Anxiété liée aux notifications intrusives
- Frustration liée à la saisie manuelle
- Méfiance sur l'utilisation des données

### Design Implications

| Décision Design | Émotion Supportée |
|-----------------|-------------------|
| Swipe explicite (pas auto-acquittement) | Contrôle actif |
| Haptic feedback sur validation | Satisfaction immédiate |
| Montants annuels par défaut | Prise de conscience amplifiée |
| Widget home screen (pas push) | Sérénité vs anxiété |
| Score visible en permanence | Fierté et progression |
| "Données locales" badge | Confiance privacy |

### Emotional Design Principles

1. **Empowerment > Culpabilité** — L'app responsabilise, elle ne juge pas
2. **Action > Passivité** — Chaque interaction renforce le sentiment de contrôle
3. **Clarté > Surprise** — Prélèvements visibles 3 jours avant, zéro surprise
4. **Fierté > Obligation** — Le Score de Contrôle célèbre, il ne punit pas
5. **Confiance > Méfiance** — Privacy-first visible à chaque étape

---

## UX Pattern Analysis & Inspiration

### Inspiring Products Analysis

| App | Ce qu'on retient | Application monAppMobile |
|-----|------------------|-------------------------|
| **Things 3** | Gestes naturels, feedback satisfaisant, iOS natif | Swipe acquittement, animations, SF Symbols |
| **Apple Fitness** | Anneaux de progression, widgets efficaces, célébration | Score de Contrôle, widget prélèvements |
| **1Password** | Messaging privacy, confiance immédiate | "Zéro accès banque" visible |
| **Spotify** | Yearly Wrapped viral, données personnalisées | Yearly Wrapped abos (V2) |
| **Bobby** | Simplicité gestion manuelle | Base à surpasser avec engagement actif |

### Transferable UX Patterns

**Navigation :**
- Tab Bar 3 items : Liste / Widget / Settings
- Swipe actions sur les cellules (acquitter, supprimer)
- Pull to refresh pour sync catalogue

**Interaction :**
- Swipe horizontal = action principale (acquittement)
- Haptic feedback sur toute action de validation
- Auto-complete avec logos pour ajout rapide

**Visual :**
- SF Symbols pour cohérence iOS
- Progress ring pour Score de Contrôle
- Couleur accent pour actions positives (vert acquitté)

### Anti-Patterns to Avoid

| À Éviter | Raison |
|----------|--------|
| Demande d'accès bancaire | Friction + méfiance utilisateur |
| Notifications push agressives | Anxiété → désinstallation |
| Onboarding > 3 écrans | Abandon avant 1er abo |
| Tracking 100% passif | Pas d'engagement, app oubliée |
| Gamification culpabilisante | "Tu as raté ton objectif" = négatif |
| Dark patterns premium | Perte de confiance long terme |

### Design Inspiration Strategy

**Adopter directement :**
- Swipe actions iOS (Mail/Things 3) → acquittement
- Progress rings (Apple Fitness) → Score de Contrôle
- SF Symbols + Dynamic Type → accessibilité native

**Adapter :**
- Yearly Wrapped (Spotify) → version abonnements (V2)
- Badges (Fitness) → version simplifiée sans pression

**Éviter catégoriquement :**
- Toute demande de données bancaires
- Notifications anxiogènes
- Jugement sur les montants dépensés

---

## Design System Foundation

### Design System Choice

**Choix : Apple Native Stack (SwiftUI + HIG + SF Symbols)**

Pour une application iOS native ciblant des utilisateurs tech-savvy familiers avec l'écosystème Apple, le design system natif est le choix optimal. Il garantit :
- Familiarité immédiate pour les utilisateurs iOS
- Performance optimale avec SwiftUI
- Accessibilité intégrée (VoiceOver, Dynamic Type)
- Dark Mode automatique
- Mises à jour iOS gratuites

### Rationale for Selection

| Critère | Évaluation |
|---------|------------|
| **Familiarité utilisateur** | iOS natif = zéro courbe d'apprentissage |
| **Vitesse de développement** | Composants SwiftUI prêts à l'emploi |
| **Maintenance** | Apple maintient, pas nous |
| **Accessibilité** | Conforme par défaut |
| **Cohérence** | SF Symbols + HIG = cohérence garantie |

### Implementation Approach

| Couche | Technologie |
|--------|-------------|
| **UI Components** | SwiftUI Views natifs |
| **Icons** | SF Symbols (symboles système) |
| **Typography** | SF Pro via Dynamic Type |
| **Colors** | Semantic Colors (adaptatives) |
| **Layout** | SwiftUI Stacks + Grids |
| **Animation** | SwiftUI Animations natives |

### Customization Strategy

**Palette de Couleurs :**

| Rôle | Couleur | Usage |
|------|---------|-------|
| `accentColor` | Vert #34C759 | Acquittement, succès, contrôle |
| `secondaryColor` | Bleu système | Liens, informations |
| `warningColor` | Orange #FF9500 | Prélèvements J-3 |
| `destructiveColor` | Rouge système | Suppression |

**Composants Personnalisés :**

| Composant | Personnalisation |
|-----------|------------------|
| `AcknowledgeSwipe` | Swipe action avec haptic + animation checkmark |
| `ControlScoreRing` | Progress ring style Apple Fitness |
| `SubscriptionCard` | Card avec logo, montant annuel, status |
| `WidgetView` | Vue compacte pour WidgetKit |

**Design Tokens :**

| Token | Valeur |
|-------|--------|
| `spacing.small` | 8pt |
| `spacing.medium` | 16pt |
| `spacing.large` | 24pt |
| `cornerRadius.card` | 12pt |
| `cornerRadius.button` | 8pt |

---

## Defining User Experience

### The Defining Interaction

**"Swipe pour acquitter — reprendre le contrôle"**

L'expérience définissante de monAppMobile est l'**Acquittement Conscient** : transformer un prélèvement automatique passif en une validation explicite et consciente. Cette interaction est ce que les utilisateurs décriront à leurs amis.

### User Mental Model

| Modèle actuel | Modèle monAppMobile |
|---------------|---------------------|
| Prélèvements = subis | Prélèvements = validés |
| Découverte après coup | Anticipation J-3 |
| Passif (l'argent part) | Actif (je confirme) |
| Anxiété | Contrôle |

**Métaphore familière :** Comme marquer un email comme "lu" — mais pour confirmer qu'on est prêt pour un prélèvement.

### Success Criteria

| Critère | Mesure | Cible |
|---------|--------|-------|
| Compréhension immédiate | Swipe sans tutoriel | 80% users |
| Temps d'action | Durée swipe → confirmation | < 1 seconde |
| Satisfaction | Feedback ressenti | Haptic + visuel |
| Adoption | % prélèvements acquittés | > 80% |
| Habitude | Retour quotidien | Rétention J7 > 25% |

### Novel UX Patterns

**Pattern hybride : Établi + Innovant**

| Élément | Type | Référence |
|---------|------|-----------|
| Swipe horizontal | Établi | Mail, Things 3 |
| Validation financière | Innovant | Nouveau usage |
| Widget actionnable | Établi | iOS standard |
| Score de progression | Établi | Apple Fitness |

**Innovation :** Personne n'utilise le swipe pour la conscience financière. C'est notre différenciation.

### Experience Mechanics

**Flow complet de l'Acquittement :**

```
┌─────────────────────────────────────────────────────────────┐
│  1. INITIATION                                               │
│     Widget affiche: "Cursor 20$ — Demain"                   │
│     Badge dans app: "3 à acquitter"                         │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  2. INTERACTION                                              │
│     User swipe gauche → droite (~100pt)                     │
│     Couleur verte progressive pendant le geste              │
│     Alternative: bouton "Acquitter" si swipe non compris    │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  3. FEEDBACK                                                 │
│     ✓ Checkmark animé (scale up + fade)                     │
│     📳 Haptic feedback (medium impact)                       │
│     🟢 Background flash vert                                 │
│     📊 Score +1 (animation compteur)                        │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  4. COMPLETION                                               │
│     Abo → état "Acquitté"                                   │
│     Widget refresh automatique                              │
│     Prochain prélèvement affiché                            │
│     Score de Contrôle recalculé                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Visual Design Foundation

### Color System

**Palette iOS Native + Accent Personnalisé**

| Rôle | Couleur | Code Hex | SwiftUI |
|------|---------|----------|---------|
| Accent (Contrôle) | Vert | #34C759 | `.green` / custom |
| Secondary | Bleu | #007AFF | `.blue` |
| Warning | Orange | #FF9500 | `.orange` |
| Destructive | Rouge | #FF3B30 | `.red` |

**États d'Acquittement :**

| État | Couleur | Signification |
|------|---------|---------------|
| ✅ Acquitté | Vert | Validé, sous contrôle |
| ⏳ À acquitter | Orange | Action requise J-3 |
| ❌ Passé | Gris | Non acquitté, passé |

### Typography System

**SF Pro via Dynamic Type**

| Usage | Style SwiftUI | Personnalisation |
|-------|---------------|------------------|
| Total annuel | `.title` + bold | Impact visuel fort |
| Nom d'abo | `.headline` | Lisibilité première |
| Montant | `.title3` + SF Rounded | Chiffres lisibles |
| Date | `.caption` | Information secondaire |

**Principes :**
- Dynamic Type supporté pour accessibilité
- SF Pro Rounded pour les montants (plus friendly)
- Hiérarchie claire : Nom > Montant > Date

### Spacing & Layout Foundation

**8pt Grid System**

| Token | Valeur | Cas d'usage |
|-------|--------|-------------|
| xs | 4pt | Entre icône et label |
| sm | 8pt | Padding interne compact |
| md | 16pt | Padding standard |
| lg | 24pt | Séparation sections |
| xl | 32pt | Marge écrans |

**Layout Principles :**
1. Safe Area toujours respectée
2. Touch targets minimum 44x44pt
3. Cards avec corner radius 12pt
4. Contenu aligné sur grille 8pt

### Accessibility Considerations

| Critère | Implementation |
|---------|----------------|
| **Contraste** | WCAG AA minimum (4.5:1 texte) |
| **Dynamic Type** | Support xSmall → AX5 |
| **Touch Targets** | 44pt minimum |
| **VoiceOver** | Labels sur tous les éléments interactifs |
| **Reduce Motion** | Animations optionnelles |
| **Color Blind** | Ne pas dépendre uniquement de la couleur (icônes + texte) |

---

## Design Direction Decision

### Design Directions Explored

| Direction | Style | Points Forts |
|-----------|-------|--------------|
| **A: Clean Control** | Minimaliste, épuré | Calme, professionnel |
| **B: Bold Dashboard** | Data-forward, dense | Dynamique, puissant |
| **C: Card Focus** | Cards larges | Clair, organisé |
| **D: Widget Native** | iOS natif | Familier, cohérent, rapide |

### Chosen Direction

**Direction D : "Widget Native"** — iOS natif avec cohérence widget

Cette direction maximise la familiarité utilisateur et la vitesse de développement tout en maintenant une expérience cohérente entre l'app et le widget.

### Design Rationale

| Facteur | Justification |
|---------|---------------|
| **Cible utilisateur** | Marc est familier iOS, pas besoin de réapprendre |
| **Cohérence** | App ↔ Widget = même langage visuel |
| **MVP speed** | Composants SwiftUI natifs = développement rapide |
| **Accessibilité** | Conforme par défaut (VoiceOver, Dynamic Type) |
| **Maintenance** | Apple maintient, pas de dette design |

### Implementation Approach

**Composants SwiftUI à utiliser :**

| Écran | Composants |
|-------|------------|
| **Liste** | `List` avec sections groupées |
| **Cards** | `.listRowBackground` + custom view |
| **Score** | `ProgressView` circular + custom |
| **Actions** | `.swipeActions` natif |
| **Tab Bar** | `TabView` standard |

**Personnalisations :**
- Couleur accent vert pour acquittement
- SF Pro Rounded pour les montants
- Haptic feedback sur swipe
- Animations SwiftUI standards

---

## User Journey Flows

### Onboarding Journey

**Objectif :** Sign in → 1er abo ajouté < 2 minutes

**Flow :**
1. Launch → Welcome screen
2. Sign in with Apple (1 tap)
3. Écran "Ajoute ton 1er abo"
4. Search catalogue → Sélection → Confirmation
5. 💡 Total annuel affiché (Moment Aha)
6. Proposition widget
7. Home avec données

**Points de friction minimisés :**
- Pas de formulaire d'inscription
- Catalogue prioritaire sur saisie manuelle
- Widget proposé au moment optimal

### Acquittement Journey (Core Loop)

**Objectif :** Widget → Swipe → Satisfaction → Habitude

**Flow :**
1. Widget affiche prochain prélèvement
2. Tap → Deep link vers abo
3. Swipe gauche→droite
4. Feedback triple (visuel + haptique + score)
5. Widget refresh → prochain abo
6. Loop jusqu'à 100%

**Éléments de satisfaction :**
- Geste satisfaisant (swipe complet)
- Feedback immédiat multi-sensoriel
- Progression visible (score)

### Ajout Abonnement Journey

**Objectif :** Nouvel abo ajouté < 30 secondes

**Flow :**
1. Tap + → Search bar active
2. Tape nom → Résultats catalogue
3. Sélection → Auto-complétion logo/nom
4. Montant → Fréquence → Date → Catégorie
5. Enregistrer → Animation → Liste MAJ

**Optimisations :**
- Catalogue first (85% des abos courants)
- Fréquences prédéfinies
- Date intelligente (prochain mois)

### Widget Journey

**Objectif :** Glance → Information → Sérénité

**Flow :**
1. Widget visible sur home screen
2. Info : "Cursor 20€ Demain"
3. Option A : Glance seulement → tranquillité
4. Option B : Tap → App → Acquittement → Refresh

**Design Widget :**
- Information dense mais lisible
- Couleur indique urgence (🟠 J-3, 🟢 acquitté)
- Deep link vers abo précis

### Journey Patterns

| Pattern | Application |
|---------|-------------|
| **Feedback Loop** | Action → Confirmation → Progression |
| **Progressive Disclosure** | Résumé → Détails au tap |
| **Smart Defaults** | Pré-remplissage contextuel |
| **Optimistic UI** | Résultat immédiat, sync async |

### Flow Optimization Principles

1. **Minimize Time to Value** — Première valeur < 2 minutes
2. **Single Gesture Actions** — 1 geste = 1 action complète
3. **Immediate Feedback** — Réponse < 100ms perçue
4. **Error Prevention** — Validation inline, pas de modal
5. **Graceful Degradation** — Offline = core features disponibles

---

## Component Strategy

### Design System Components (SwiftUI Native)

| Composant | Usage |
|-----------|-------|
| `List` + `.listStyle(.insetGrouped)` | Liste principale |
| `TabView` | Navigation 3 onglets |
| `.swipeActions` | Base acquittement |
| `.searchable` | Recherche catalogue |
| `Form` | Formulaire ajout |
| `ProgressView(.circular)` | Base score |

### Custom Components

#### SubscriptionCard
- **Purpose:** Affichage riche d'un abonnement
- **States:** À acquitter (orange), Acquitté (vert), Passé (gris)
- **Content:** Logo 44x44, nom, montant annuel, date
- **A11y:** Label complet pour VoiceOver

#### AcknowledgeSwipeView
- **Purpose:** Geste d'acquittement avec feedback
- **Behavior:** Swipe 100pt → trigger → checkmark
- **Feedback:** Visuel progressif + haptic + animation
- **A11y:** Button fallback

#### ControlScoreRing
- **Purpose:** Visualisation du score de contrôle
- **Style:** Ring circulaire style Apple Fitness
- **Animation:** Progression animée au changement
- **Variants:** Large (home), Compact (widget)

#### AnnualTotalHeader
- **Purpose:** Total annuel sticky (Moment Aha)
- **Behavior:** Sticky au scroll, tap pour breakdown
- **Animation:** Compteur au premier affichage

#### WidgetSubscriptionView
- **Purpose:** Format compact pour WidgetKit
- **Sizes:** Small/Medium/Large
- **Interaction:** Deep link vers app

### Component Implementation Strategy

**Principes :**
1. Composants SwiftUI natifs en priorité
2. Custom components = wrappers autour de natifs
3. Design tokens partagés (colors, spacing)
4. Accessibility-first (VoiceOver labels, Dynamic Type)
5. Preview providers pour chaque composant

### Implementation Roadmap

| Phase | Composants | Dépendance |
|-------|------------|------------|
| **MVP P0** | SubscriptionCard, AcknowledgeSwipeView | Core experience |
| **MVP P0** | AnnualTotalHeader | Moment Aha |
| **MVP P1** | ControlScoreRing | Gamification |
| **MVP P1** | WidgetSubscriptionView | Engagement quotidien |
| **V2** | LootBoxAnimation, YearlyWrappedCard | Polish & Viralité |

---

## UX Consistency Patterns

### Feedback Patterns

| Action | Visuel | Haptique | Timing |
|--------|--------|----------|--------|
| Acquittement réussi | ✓ vert + flash | Medium | 300ms |
| Ajout abo | Scale + success | Success | 200ms |
| Suppression | Slide rouge | Warning | 250ms |
| Erreur | Shake + rouge | Error | 400ms |
| Tap standard | Opacity 0.7 | Light | 100ms |

**Règle :** Feedback < 100ms pour toute action.

### Loading & Empty States

**Loading :**
- Skeleton shimmer pour listes (pas spinner)
- Sync background = barre discrète
- Actions = spinner inline

**Empty States :**

| Contexte | Message | CTA |
|----------|---------|-----|
| Pas d'abos | "Ajoute ton premier abo" | + Ajouter |
| Pas de résultats | "Aucun résultat" | Créer manuellement |
| Tout acquitté | "Bravo ! 🎉" | — |

**Errors :**
- Réseau : Banner non-bloquant + retry auto
- Validation : Inline sous le champ
- Fatal : Modal avec recovery action

### Button Hierarchy

| Niveau | Style SwiftUI | Usage |
|--------|---------------|-------|
| Primary | `.borderedProminent` + vert | Ajouter, Acquitter |
| Secondary | `.bordered` | Annuler, Plus tard |
| Tertiary | `.plain` + bleu | Liens, navigation |
| Destructive | `.bordered` + rouge | Supprimer |

**Règle :** 1 seul Primary visible par écran.

### Form Patterns

| Élément | Pattern |
|---------|---------|
| Labels | Au-dessus du champ |
| Validation | Inline, immédiate |
| Erreur | Bordure + texte rouge |
| Keyboard | Type adapté (numeric, etc.) |
| Required | Tout requis par défaut |

### Navigation Patterns

| Pattern | Implementation |
|---------|----------------|
| Tab Bar | 3 items fixes |
| Drill-down | NavigationStack |
| Modal ajout | Sheet (pas fullscreen) |
| Dismiss | Swipe down + X |
| Deep link | Widget → Abo spécifique |

---

## Responsive Design & Accessibility

### Responsive Strategy (iOS)

**Approche Size Classes :**

| Size Class | Layout |
|------------|--------|
| Compact Width | Single column (iPhone) |
| Regular Width | NavigationSplitView optionnel (iPad) |

**Principes :**
- iPhone portrait = layout principal
- iPad = sidebar + detail si pertinent
- Split View supporté

### Accessibility Strategy (WCAG 2.1 AA)

| Critère | Implementation |
|---------|----------------|
| **Contraste** | Couleurs sémantiques iOS (4.5:1+) |
| **Touch Targets** | 44x44pt minimum |
| **Dynamic Type** | .font() system, pas de tailles fixes |
| **VoiceOver** | Labels descriptifs sur tous les éléments |
| **Reduce Motion** | Animations conditionnelles |
| **Dark Mode** | Couleurs sémantiques = auto |

### VoiceOver Labels

| Composant | Label |
|-----------|-------|
| SubscriptionCard | "{nom}, {montant} par an, {état}" |
| AcknowledgeAction | "Acquitter" + button fallback |
| ScoreRing | "Score de contrôle : {%}, {n} sur {total}" |

### Testing Strategy

| Test | Outil | Quand |
|------|-------|-------|
| VoiceOver | Device réel | Chaque feature |
| Dynamic Type | Simulateur AX5 | Chaque écran |
| Dark Mode | Settings toggle | Chaque écran |
| iPad | Simulateur + Split | Par sprint |
| Contraste | Accessibility Inspector | CI/CD |

### Implementation Guidelines

**SwiftUI Best Practices :**
1. `.font(.body)` jamais `.system(size:)`
2. `.accessibilityLabel()` sur tous les éléments interactifs
3. `@Environment(\.accessibilityReduceMotion)` pour animations
4. Semantic colors uniquement (pas de hex hardcodés)
5. `.frame(minWidth: 44, minHeight: 44)` pour touch targets

---
