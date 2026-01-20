---
stepsCompleted: [1, 2, 3, 4, 5]
inputDocuments:
  - '_bmad-output/analysis/brainstorming-session-2026-01-19.md'
date: 2026-01-19
author: Steeven
---

# Product Brief: monAppMobile

## Executive Summary

**monAppMobile** est une application iOS de gestion d'abonnements qui permet aux utilisateurs de reprendre le contrôle total sur leurs dépenses récurrentes, sans jamais connecter leur compte bancaire.

Dans un monde où les abonnements se multiplient (streaming, SaaS, cloud, services...), les utilisateurs perdent la trace de leurs engagements financiers. Les solutions existantes — apps bancaires ou tableurs — ne sont pas conçues pour ce problème et demandent souvent un accès intrusif aux données bancaires.

**monAppMobile** propose une approche radicalement différente : l'utilisateur saisit ses abonnements manuellement ou via scan de factures (OCR + IA), garde ses données localement sur son appareil, et reçoit des alertes avant chaque prélèvement. L'app transforme la gestion passive des abos en engagement actif grâce à une expérience gamifiée unique.

**Promesse core :** CONTRÔLE TOTAL — L'utilisateur ne subit plus ses abonnements, il les maîtrise consciemment.

---

## Core Vision

### Problem Statement

Les gens oublient leurs abonnements et perdent de l'argent sans s'en rendre compte. Entre les services de streaming, les outils SaaS, les abonnements cloud et les services divers, il est devenu impossible de suivre mentalement où va son argent chaque mois.

### Problem Impact

- **Financier :** Argent perdu sur des abonnements oubliés ou sous-utilisés
- **Émotionnel :** Surprises désagréables sur le compte bancaire
- **Comportemental :** Abonnements qui courent pendant des mois sans utilisation
- **Mental :** Charge cognitive de devoir se souvenir de tous ses engagements

**Populations les plus touchées :** Jeunes actifs, familles, freelances — tous ceux qui cumulent des abonnements personnels et/ou professionnels.

### Why Existing Solutions Fall Short

| Solution actuelle | Problème |
|-------------------|----------|
| **Apps bancaires** | Pas conçues pour ça, demandent accès bancaire intrusif |
| **Mémoire** | Non fiable, charge mentale |
| **Tableurs/Notes** | Fastidieux, pas de rappels, vite abandonné |
| **Rien** | Perte d'argent garantie |

**Constat :** Aucune solution dédiée, simple et respectueuse de la vie privée n'existe pour ce problème spécifique.

### Proposed Solution

**monAppMobile** — L'app qui te montre exactement où va ton argent chaque mois, sans jamais toucher à ta banque.

**Fonctionnement :**
1. L'utilisateur ajoute ses abos (saisie manuelle ou scan de factures)
2. L'IA extrait les données des factures scannées
3. L'app affiche les prélèvements à venir via widgets iOS
4. L'utilisateur "acquitte" chaque prélèvement (swipe conscient)
5. Un score de contrôle reflète sa maîtrise

**Principes fondamentaux :**
- Zéro connexion bancaire — données 100% locales
- Simplicité radicale — 3 onglets max
- Engagement actif — l'utilisateur est acteur, pas spectateur
- Gamification intelligente — dopamine au service du contrôle

### Key Differentiators

| Différenciateur | Description |
|-----------------|-------------|
| **Privacy-First** | Aucune connexion bancaire, données sur l'appareil uniquement |
| **Contrôle Actif** | Système d'acquittement conscient (swipe to validate) |
| **Gamification Unique** | Animation "loot box" au scan, score de contrôle, streaks |
| **Affichage Annuel** | Montants en annuel par défaut pour amplifier la prise de conscience |
| **Widgets iOS** | Présence passive sur l'écran d'accueil vs notifications intrusives |
| **Yearly Wrapped** | Récap annuel partageable style Spotify Wrapped |

**Avantage fondateur :** Le créateur est son propre utilisateur idéal — développeur cumulant des abonnements SaaS (Claude, Cursor, Notion, Hostinger...), permettant un feedback direct et des itérations rapides basées sur une compréhension intime du problème.

**Timing favorable :** Prolifération des modèles d'abonnement + sensibilité croissante à la vie privée = moment idéal pour une solution privacy-first dédiée.

---

## Target Users

### Primary Users

L'application s'adresse à toute personne possédant des abonnements récurrents, quel que soit son profil démographique. Le marché cible est volontairement large car le problème est universel. Trois personas représentatifs illustrent les différents contextes d'usage :

#### Persona 1 : Marc, 32 ans — Le Dev SaaS

**Contexte :**
- Développeur web/mobile freelance
- Cumule 15-20 abonnements professionnels et personnels
- Abos pro : Claude, Cursor, Notion, Hostinger, GitHub Pro, Figma, Adobe CC, AWS...
- Abos perso : Netflix, Spotify, Apple One, salle de sport...

**Problème vécu :**
- Perd la trace entre abos pro (déductibles) et perso
- Découvre des abos oubliés lors de sa compta trimestrielle
- Frustré par les apps qui demandent l'accès bancaire — il est sensible à la sécurité

**Ce qui le ferait dire "wow" :**
- Voir son total annuel d'abos SaaS pour sa déclaration fiscale
- Scanner une facture et que l'IA remplisse tout automatiquement
- Avoir un "Yearly Wrapped" de ses dépenses abos

**Motivation :** Contrôle total sur ses dépenses récurrentes pro et perso

---

#### Persona 2 : Léa, 27 ans — La Jeune Active Streaming

**Contexte :**
- Jeune cadre, célibataire, vie urbaine
- 8-12 abonnements divertissement et lifestyle
- Netflix, Spotify, Disney+, Prime Video, salle de sport, Headspace, presse en ligne...

**Problème vécu :**
- S'inscrit à des essais gratuits et oublie d'annuler
- Découvre des prélèvements "surprise" sur son relevé bancaire
- N'a aucune idée du total mensuel de ses abos

**Ce qui la ferait dire "wow" :**
- Widget sur l'écran d'accueil avec le prochain prélèvement
- Alerte 3 jours avant la fin d'un essai gratuit
- Voir que ses abos = 1 week-end à Barcelone par mois

**Motivation :** Ne plus jamais être surprise par un prélèvement

---

#### Persona 3 : Sophie & Thomas, 38 ans — La Famille

**Contexte :**
- Couple avec 2 enfants (8 et 12 ans)
- 12-18 abonnements familiaux
- Disney+, Xbox Game Pass, forfaits téléphone x4, box internet, assurances, sport enfants...

**Problème vécu :**
- Abos éparpillés entre les deux comptes bancaires
- Les enfants s'inscrivent à des services sans prévenir
- Impossible de savoir le budget "abonnements" total du foyer

**Ce qui les ferait dire "wow" :**
- Vue consolidée de TOUS les abos du foyer
- Savoir exactement combien part chaque mois en abos
- Pouvoir catégoriser : enfants / adultes / famille

**Motivation :** Visibilité totale sur le budget abonnements familial

---

### Secondary Users

Pour la V1, pas d'utilisateurs secondaires identifiés. L'app est mono-utilisateur avec données locales.

**Évolutions futures possibles :**
- Partage de vue avec conjoint (sans partager l'app)
- Export pour comptable (freelances)
- Mode "famille" avec comptes liés

---

### User Journey

#### Phase 1 : Découverte
- **Comment :** App Store (recherche "gestion abonnements"), bouche-à-oreille, réseaux sociaux
- **Déclencheur :** Découverte d'un prélèvement oublié, fin de mois difficile, volonté de faire le tri

#### Phase 2 : Onboarding
- Sign in with Apple (1 tap)
- Écran "Scanne ta première facture" ou "Ajoute ton premier abo"
- Zéro configuration, l'app fonctionne immédiatement

#### Phase 3 : Premier "Wow"
- **Scan :** Animation loot box révèle l'abo détecté → dopamine immédiate
- **Ajout manuel :** Logo auto-complété, montant annuel affiché → prise de conscience
- **Widget :** Voir son prochain prélèvement sur l'écran d'accueil

#### Phase 4 : Usage Quotidien
- Widget affiche les prélèvements à venir
- Swipe pour "acquitter" chaque prélèvement
- Score de contrôle augmente → sentiment de maîtrise

#### Phase 5 : Valeur Long Terme
- Streak de contrôle maintenu
- Insights hebdo : "Cette semaine : 47€ d'abos"
- Yearly Wrapped en décembre → moment de partage
- Décision d'annuler des abos sous-utilisés grâce aux données

---

## Success Metrics

### User Success Metrics

**Indicateurs de valeur pour l'utilisateur :**

| Métrique | Description | Cible |
|----------|-------------|-------|
| **Score de Contrôle** | % d'abos acquittés avant prélèvement | > 90% |
| **Abos trackés** | Nombre d'abonnements ajoutés par utilisateur | > 5 abos |
| **Premier abo ajouté** | Temps entre inscription et premier abo | < 2 min |
| **Découverte d'abo oublié** | Utilisateur découvre un abo qu'il avait oublié | Au moins 1 |
| **Streak maintenu** | Jours consécutifs avec 100% de contrôle | > 7 jours |

**Moments de succès utilisateur :**
- Premier scan réussi avec détection IA → "Wow, ça marche !"
- Voir son total annuel pour la première fois → Prise de conscience
- Acquitter tous ses prélèvements du mois → Sentiment de maîtrise
- Décider d'annuler un abo sous-utilisé grâce aux données → Valeur concrète

---

### Business Objectives

**Objectifs à 3 mois (validation marché) :**

| Objectif | Métrique | Cible |
|----------|----------|-------|
| Acquisition | Téléchargements App Store | 1 000+ |
| Activation | Users avec ≥1 abo ajouté | 60% des downloads |
| Engagement | Users avec ≥5 abos ajoutés | 30% des activés |

**Objectifs à 12 mois (croissance) :**

| Objectif | Métrique | Cible |
|----------|----------|-------|
| Acquisition | Téléchargements cumulés | 10 000+ |
| Rétention | Users actifs J30 | > 25% |
| Monétisation | Conversion premium | 3-5% |
| Revenus | MRR (Monthly Recurring Revenue) | Premiers revenus |

---

### Key Performance Indicators

**KPIs Prioritaires (dans l'ordre défini) :**

#### 1. Téléchargements
- **Mesure :** App Store Connect
- **Fréquence :** Quotidien
- **Cible M3 :** 1 000 | **Cible M12 :** 10 000

#### 2. Nombre d'abos ajoutés par utilisateur
- **Mesure :** Analytics in-app
- **Fréquence :** Hebdomadaire
- **Cible :** Moyenne > 5 abos/user actif
- **Segmentation :** Manuel vs Scan

#### 3. Rétention
- **Mesure :** Cohortes par semaine d'inscription
- **Fréquence :** Hebdomadaire
- **Cibles :**
  - J1 : > 40% (retour le lendemain)
  - J7 : > 25% (usage semaine)
  - J30 : > 15% (habitude créée)

#### 4. Conversion Free → Premium
- **Mesure :** Ratio users premium / users actifs
- **Fréquence :** Mensuel
- **Cible :** 3-5% des users actifs J30

---

### Métriques Admin (Dashboard Filament)

| Métrique | Utilité |
|----------|---------|
| **Funnel visuel** | Downloads → 1er abo → 5+ abos → J30 → Premium |
| **Abos les plus ajoutés** | Prioriser le catalogue de logos |
| **Suggestions communautaires** | File d'attente d'abos à valider |
| **Alertes seuils** | Notification quand cap franchi (1K, 5K, 10K users) |

---

### North Star Metric

**Métrique étoile du nord :**

> **Nombre d'acquittements par semaine**

Cette métrique unique capture :
- L'engagement (l'utilisateur revient)
- La valeur (il utilise la feature core)
- La rétention (il maintient l'habitude)
- Le contrôle (il valide consciemment ses abos)

Si les acquittements augmentent, tout le reste suit.

---

## MVP Scope

### Core Features (MVP)

L'MVP se concentre sur les fonctionnalités essentielles qui délivrent la promesse de **CONTRÔLE TOTAL** sans complexité :

| Feature | Description | Priorité |
|---------|-------------|----------|
| **Sign in with Apple** | Connexion 1-tap, anonyme, privacy-first | P0 |
| **Ajout manuel** | Saisie d'un abonnement avec nom, montant, fréquence, date | P0 |
| **Liste des abos** | Vue de tous les abonnements avec montants annuels | P0 |
| **Widget iOS** | Affichage des prochains prélèvements sur l'écran d'accueil | P0 |
| **Acquittement conscient** | Swipe pour valider chaque prélèvement avant qu'il arrive | P0 |
| **Score de Contrôle** | Pourcentage d'abos acquittés à temps | P1 |
| **Affichage annuel** | Montants en annuel par défaut pour amplifier la prise de conscience | P0 |

**Principe MVP :** Livrer une app fonctionnelle que le créateur peut utiliser lui-même au quotidien.

---

### Backend Admin (MVP)

Dashboard Filament v4 sur Laravel 12 pour gérer l'écosystème :

| Feature Admin | Description |
|---------------|-------------|
| **Catalogue CRUD** | Gestion des abonnements connus (nom, logo, catégorie) |
| **Gestion des logos** | Upload et validation des logos d'abonnements |
| **File de suggestions** | Validation des abonnements suggérés par la communauté |
| **Dashboard métriques** | Funnel, rétention, abos populaires, alertes seuils |

**Stack Backend :**
- Laravel 12 + Filament v4
- Laravel Cloud (hébergement)
- Supabase PostgreSQL

---

### Out of Scope (V2+)

Features différées pour garder le MVP focalisé :

| Feature | Version | Raison du report |
|---------|---------|------------------|
| **Scan IA de factures** | V2 | La saisie manuelle suffit pour valider le concept |
| **Yearly Wrapped** | V2 | Feature viralité, pas essentielle pour le core |
| **Animation Loot Box** | V2 | Polish, pas critique pour la valeur |
| **Gamification avancée** | V2 | Badges, achievements, leaderboards |
| **Streaks** | V2 | Nécessite d'abord valider l'engagement de base |
| **Notifications push** | V2 | Les widgets suffisent pour l'MVP |
| **Export PDF/CSV** | V2 | Cas d'usage secondaire |

---

### MVP Success Criteria

**Critères de validation du MVP :**

| Critère | Seuil | Délai |
|---------|-------|-------|
| **Usage personnel** | Le créateur utilise l'app au quotidien | J+7 |
| **Téléchargements** | 100+ downloads organiques | M+1 |
| **Activation** | 50%+ des downloads ajoutent 1 abo | M+1 |
| **Rétention J7** | 20%+ des activés reviennent | M+1 |
| **Abos moyens** | 3+ abos par utilisateur actif | M+2 |

**Signal de succès MVP :** L'app crée de la valeur réelle pour ses premiers utilisateurs, mesurée par le retour et l'ajout d'abonnements.

---

## Future Vision

### Roadmap par Version

#### V1 (MVP) — Fondations
- Gestion manuelle des abonnements
- Widget iOS avec prochains prélèvements
- Acquittement conscient
- Score de contrôle basique
- Admin Filament opérationnel

#### V2 — Engagement
- Scan IA de factures (OCR + LLM)
- Animation Loot Box au scan
- Yearly Wrapped (fin d'année)
- Streaks intelligents
- Notifications push optionnelles
- Gamification (badges, achievements)

#### V3 — Expansion
- Version Android
- Mode famille (comptes liés)
- Insights avancés (coût/utilisation, tendances)
- Recommandations IA (optionnelles)
- Export comptable pour freelances
- API partenaires (agrégateurs autorisés)

---

### Vision 2-3 Ans

**L'ambition :** Devenir LA référence pour la gestion d'abonnements privacy-first.

**Position visée :**
- Leader sur le segment "sans connexion bancaire"
- Communauté active qui enrichit le catalogue
- Revenus récurrents via premium (3-5% conversion)
- Expansion internationale (catalogue multilingue)

**Ce que monAppMobile ne deviendra PAS :**
- ❌ Une app bancaire (pas d'agrégation de comptes)
- ❌ Un gestionnaire de budget complet
- ❌ Une app qui vend les données utilisateurs

**Valeurs immuables :**
- Privacy-first : données locales, pas de tracking
- Contrôle utilisateur : l'utilisateur décide, pas l'app
- Simplicité : 3 onglets max, toujours
