---
title: 'Scanner de factures IA'
slug: 'invoice-scanner'
created: '2026-01-20'
status: 'implemented'
stepsCompleted: [1, 2, 3, 4]
tech_stack:
  - SwiftUI
  - PhotosUI (PHPickerViewController)
  - PDFKit
  - UIImagePickerController (caméra)
  - Laravel 12
  - OpenAI Vision API (gpt-4o)
files_to_modify:
  - MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift
  - MySubGuard/Core/Services/InvoiceScannerService.swift (nouveau)
  - MySubGuard/Core/Components/ImagePicker.swift (nouveau)
  - MySubGuard/Core/Components/PhotoPicker.swift (nouveau)
  - MySubGuard/Core/Components/DocumentPicker.swift (nouveau)
  - backend/app/Http/Controllers/Api/V1/InvoiceScanController.php (nouveau)
  - backend/routes/api.php
  - backend/.env
code_patterns:
  - Services singleton (APIClient.shared pattern)
  - async/await pour appels réseau
  - Sheet modal pour UI picker
  - @State pour form fields binding
  - UIViewControllerRepresentable pour wrappers UIKit
  - Laravel Controller avec JsonResponse
  - Routes groupées sous /v1/
test_patterns:
  - Manual testing avec factures réelles
  - Mock service pour UI tests
---

# Tech-Spec: Scanner de factures IA

**Created:** 2026-01-20
**Status:** Review
**Version cible:** 0.1.1-beta.1

## Overview

### Problem Statement

Ajouter manuellement un abonnement est fastidieux quand l'utilisateur a déjà la facture (email, photo, PDF) sous les yeux. Il doit recopier manuellement le nom du service, le montant, et la date d'échéance.

### Solution

Ajouter un bouton "Scanner" dans AddSubscriptionView qui permet de capturer une photo, sélectionner une image de la galerie, ou importer un PDF. L'image est envoyée au backend Laravel qui appelle OpenAI Vision API pour extraire les données de la facture. Les champs du formulaire sont ensuite pré-remplis avec les données extraites.

### Scope

**In Scope:**
- Bouton "Scanner" dans AddSubscriptionView (toolbar)
- Capture : caméra (UIImagePickerController), galerie photo (PHPicker), import PDF (DocumentPicker)
- Nouveau service iOS `InvoiceScannerService` pour orchestrer le scan
- Nouveau endpoint Laravel `POST /api/v1/invoice/scan`
- Intégration OpenAI Vision API (gpt-4o) côté backend
- Extraction : nom du service, montant, date d'échéance, fréquence
- Pré-remplissage du formulaire avec les données extraites
- Fréquence mensuelle par défaut si non détectée
- Fallback manuel si l'IA ne reconnaît pas certains champs
- Loading state pendant le scan

**Out of Scope:**
- Reconnaissance de relevés bancaires complets
- OCR offline (on-device) sans réseau
- Historique des scans précédents
- Support multi-pages PDF (première page uniquement)
- Détection automatique de doublons

## Context for Development

### Codebase Patterns

**iOS (AddSubscriptionView.swift):**
- Form fields sont des `@State` properties : `name`, `amountText`, `frequency`, `nextBillingDate`, `category`
- Pattern existant de pré-remplissage via `selectCatalogueItem()` (lignes 309-326) - à réutiliser
- Haptic feedback sur actions (`UINotificationFeedbackGenerator`)
- Sheet modals pour UI secondaires
- Async Task blocks pour appels réseau

**Laravel Backend:**
- Controllers dans `app/Http/Controllers/Api/V1/`
- Pattern : validation → process → JsonResponse
- Routes groupées sous `/v1/` dans `routes/api.php`
- Pas de SDK OpenAI installé - à ajouter via composer

**APIClient (iOS):**
- Singleton `APIClient.shared`
- Méthode `post<T: Decodable, B: Encodable>(_ endpoint: String, body: B)`
- Encoding snake_case automatique
- Base URL: `http://127.0.0.1:8001/api/v1` (debug) / `https://api.mysubguard.com/api/v1` (prod)

### Files to Reference

| File | Purpose | Lignes clés |
| ---- | ------- | ----------- |
| `AddSubscriptionView.swift` | Vue cible - ajouter bouton Scanner | L45-236 (body), L309-326 (selectCatalogueItem pattern) |
| `APIClient.swift` | Client réseau - réutiliser post() | L113-128 (post method) |
| `CatalogueController.php` | Pattern Laravel controller | L11-37 (index pattern) |
| `api.php` | Routes - ajouter /invoice/scan | L13-22 (groupe v1) |

### Technical Decisions

| Décision | Choix | Justification |
|----------|-------|---------------|
| API Vision | OpenAI gpt-4o | Meilleur rapport qualité/prix pour OCR+extraction |
| Architecture | iOS → Laravel → OpenAI | Clé API sécurisée côté serveur, changeable sans update app |
| Image encoding | Base64 dans JSON | Simple, compatible APIClient existant |
| PDF handling | PDFKit → UIImage | Extraire première page comme image |
| Compression | JPEG 0.8 quality | Réduire payload tout en gardant lisibilité |
| Timeout | 60s | OpenAI Vision peut être lent sur images complexes |

## Implementation Plan

### Phase 1: Backend Laravel (faire en premier)

- [ ] **Task 1: Installer le SDK OpenAI**
  - File: `backend/composer.json`
  - Action: Exécuter `composer require openai-php/laravel`
  - Notes: Le package configure automatiquement le service provider

- [ ] **Task 2: Configurer la clé API OpenAI**
  - File: `backend/.env`
  - Action: Ajouter `OPENAI_API_KEY=sk-...`
  - Notes: Ne pas commiter le .env, utiliser .env.example pour documenter

- [ ] **Task 3: Créer InvoiceScanController**
  - File: `backend/app/Http/Controllers/Api/V1/InvoiceScanController.php`
  - Action: Créer le controller avec méthode `scan(Request $request)`
  - Notes: Valider `image_base64` required|string, appeler OpenAI, retourner JSON structuré
  - Code structure:
    ```php
    public function scan(Request $request): JsonResponse
    {
        $validated = $request->validate(['image_base64' => 'required|string']);

        $response = OpenAI::chat()->create([
            'model' => 'gpt-4o',
            'messages' => [
                ['role' => 'user', 'content' => [
                    ['type' => 'text', 'text' => $this->getPrompt()],
                    ['type' => 'image_url', 'image_url' => [
                        'url' => 'data:image/jpeg;base64,' . $validated['image_base64']
                    ]]
                ]]
            ],
            'max_tokens' => 500
        ]);

        $content = $response->choices[0]->message->content;
        $data = json_decode($content, true);

        return response()->json(['data' => $data]);
    }
    ```

- [ ] **Task 4: Ajouter la route /invoice/scan**
  - File: `backend/routes/api.php`
  - Action: Ajouter `Route::post('/invoice/scan', [InvoiceScanController::class, 'scan']);` dans le groupe v1
  - Notes: Pas d'auth requise pour MVP

### Phase 2: iOS - Service et DTOs

- [ ] **Task 5: Créer InvoiceScannerService**
  - File: `MySubGuard/Core/Services/InvoiceScannerService.swift`
  - Action: Créer le service singleton avec structs Request/Response
  - Code structure:
    ```swift
    import Foundation
    import UIKit
    import PDFKit

    // MARK: - DTOs
    struct InvoiceScanRequest: Encodable {
        let imageBase64: String
    }

    struct InvoiceScanResponse: Decodable {
        let data: InvoiceData
    }

    struct InvoiceData: Decodable {
        let serviceName: String?
        let amount: Double?
        let currency: String?
        let billingDate: String?
        let frequency: String?
    }

    // MARK: - Service
    final class InvoiceScannerService {
        static let shared = InvoiceScannerService()
        private let apiClient = APIClient.shared
        private init() {}

        func scan(image: UIImage) async throws -> InvoiceData {
            guard let imageData = image.jpegData(compressionQuality: 0.8) else {
                throw InvoiceScanError.imageConversionFailed
            }
            let base64 = imageData.base64EncodedString()
            let request = InvoiceScanRequest(imageBase64: base64)
            let response: InvoiceScanResponse = try await apiClient.post("/invoice/scan", body: request)
            return response.data
        }

        func extractFirstPage(from url: URL) -> UIImage? {
            guard let document = PDFDocument(url: url),
                  let page = document.page(at: 0) else { return nil }
            let bounds = page.bounds(for: .mediaBox)
            let renderer = UIGraphicsImageRenderer(size: bounds.size)
            return renderer.image { ctx in
                UIColor.white.set()
                ctx.fill(bounds)
                ctx.cgContext.translateBy(x: 0, y: bounds.height)
                ctx.cgContext.scaleBy(x: 1, y: -1)
                page.draw(with: .mediaBox, to: ctx.cgContext)
            }
        }
    }

    enum InvoiceScanError: LocalizedError {
        case imageConversionFailed
        case pdfExtractionFailed
        var errorDescription: String? {
            switch self {
            case .imageConversionFailed: return "Impossible de convertir l'image"
            case .pdfExtractionFailed: return "Impossible de lire le PDF"
            }
        }
    }
    ```

### Phase 3: iOS - Pickers UIKit Wrappers

- [ ] **Task 6: Créer ImagePicker (caméra)**
  - File: `MySubGuard/Core/Components/ImagePicker.swift`
  - Action: Créer UIViewControllerRepresentable pour UIImagePickerController
  - Code structure:
    ```swift
    import SwiftUI
    import UIKit

    struct ImagePicker: UIViewControllerRepresentable {
        @Binding var image: UIImage?
        @Environment(\.dismiss) private var dismiss
        var sourceType: UIImagePickerController.SourceType = .camera

        func makeUIViewController(context: Context) -> UIImagePickerController {
            let picker = UIImagePickerController()
            picker.sourceType = sourceType
            picker.delegate = context.coordinator
            return picker
        }

        func updateUIViewController(_ uiViewController: UIImagePickerController, context: Context) {}

        func makeCoordinator() -> Coordinator { Coordinator(self) }

        class Coordinator: NSObject, UIImagePickerControllerDelegate, UINavigationControllerDelegate {
            let parent: ImagePicker
            init(_ parent: ImagePicker) { self.parent = parent }

            func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
                if let image = info[.originalImage] as? UIImage {
                    parent.image = image
                }
                parent.dismiss()
            }

            func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
                parent.dismiss()
            }
        }
    }
    ```

- [ ] **Task 7: Créer PhotoPicker (galerie)**
  - File: `MySubGuard/Core/Components/PhotoPicker.swift`
  - Action: Créer wrapper pour PHPickerViewController
  - Code structure:
    ```swift
    import SwiftUI
    import PhotosUI

    struct PhotoPicker: UIViewControllerRepresentable {
        @Binding var image: UIImage?
        @Environment(\.dismiss) private var dismiss

        func makeUIViewController(context: Context) -> PHPickerViewController {
            var config = PHPickerConfiguration()
            config.filter = .images
            config.selectionLimit = 1
            let picker = PHPickerViewController(configuration: config)
            picker.delegate = context.coordinator
            return picker
        }

        func updateUIViewController(_ uiViewController: PHPickerViewController, context: Context) {}

        func makeCoordinator() -> Coordinator { Coordinator(self) }

        class Coordinator: NSObject, PHPickerViewControllerDelegate {
            let parent: PhotoPicker
            init(_ parent: PhotoPicker) { self.parent = parent }

            func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
                parent.dismiss()
                guard let provider = results.first?.itemProvider,
                      provider.canLoadObject(ofClass: UIImage.self) else { return }
                provider.loadObject(ofClass: UIImage.self) { image, _ in
                    DispatchQueue.main.async {
                        self.parent.image = image as? UIImage
                    }
                }
            }
        }
    }
    ```

- [ ] **Task 8: Créer DocumentPicker (PDF)**
  - File: `MySubGuard/Core/Components/DocumentPicker.swift`
  - Action: Créer wrapper pour UIDocumentPickerViewController
  - Code structure:
    ```swift
    import SwiftUI
    import UniformTypeIdentifiers

    struct DocumentPicker: UIViewControllerRepresentable {
        @Binding var pdfURL: URL?
        @Environment(\.dismiss) private var dismiss

        func makeUIViewController(context: Context) -> UIDocumentPickerViewController {
            let picker = UIDocumentPickerViewController(forOpeningContentTypes: [UTType.pdf])
            picker.delegate = context.coordinator
            return picker
        }

        func updateUIViewController(_ uiViewController: UIDocumentPickerViewController, context: Context) {}

        func makeCoordinator() -> Coordinator { Coordinator(self) }

        class Coordinator: NSObject, UIDocumentPickerDelegate {
            let parent: DocumentPicker
            init(_ parent: DocumentPicker) { self.parent = parent }

            func documentPicker(_ controller: UIDocumentPickerViewController, didPickDocumentsAt urls: [URL]) {
                parent.pdfURL = urls.first
                parent.dismiss()
            }

            func documentPickerWasCancelled(_ controller: UIDocumentPickerViewController) {
                parent.dismiss()
            }
        }
    }
    ```

### Phase 4: iOS - Intégration AddSubscriptionView

- [ ] **Task 9: Ajouter les @State pour le scanner**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter après les autres @State (ligne ~36):
    ```swift
    // Scanner state
    @State private var showScannerOptions = false
    @State private var showCamera = false
    @State private var showPhotoPicker = false
    @State private var showDocumentPicker = false
    @State private var isScanning = false
    @State private var scanError: String?
    @State private var scannedImage: UIImage?
    @State private var scannedPDFURL: URL?
    ```

- [ ] **Task 10: Ajouter le bouton Scanner dans la toolbar**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter dans `.toolbar` (après ToolbarItem cancellationAction):
    ```swift
    ToolbarItem(placement: .topBarLeading) {
        Button {
            showScannerOptions = true
        } label: {
            Image(systemName: "doc.text.viewfinder")
        }
        .disabled(isScanning)
    }
    ```

- [ ] **Task 11: Ajouter le confirmationDialog pour choisir la source**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter après `.toolbar`:
    ```swift
    .confirmationDialog("Scanner une facture", isPresented: $showScannerOptions) {
        Button("Prendre une photo") { showCamera = true }
        Button("Choisir une image") { showPhotoPicker = true }
        Button("Importer un PDF") { showDocumentPicker = true }
        Button("Annuler", role: .cancel) {}
    }
    ```

- [ ] **Task 12: Ajouter les sheets pour les pickers**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter après le confirmationDialog:
    ```swift
    .sheet(isPresented: $showCamera) {
        ImagePicker(image: $scannedImage, sourceType: .camera)
    }
    .sheet(isPresented: $showPhotoPicker) {
        PhotoPicker(image: $scannedImage)
    }
    .sheet(isPresented: $showDocumentPicker) {
        DocumentPicker(pdfURL: $scannedPDFURL)
    }
    ```

- [ ] **Task 13: Ajouter les onChange handlers pour déclencher le scan**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter après les sheets:
    ```swift
    .onChange(of: scannedImage) { _, newImage in
        if let image = newImage {
            Task { await handleScannedImage(image) }
            scannedImage = nil
        }
    }
    .onChange(of: scannedPDFURL) { _, newURL in
        if let url = newURL {
            Task { await handleScannedPDF(url) }
            scannedPDFURL = nil
        }
    }
    ```

- [ ] **Task 14: Implémenter handleScannedImage et handleScannedPDF**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter dans la section // MARK: - Actions:
    ```swift
    // MARK: - Scanner Actions

    private func handleScannedImage(_ image: UIImage) async {
        await MainActor.run { isScanning = true; scanError = nil }

        do {
            let result = try await InvoiceScannerService.shared.scan(image: image)
            await MainActor.run {
                applyScannedData(result)
                isScanning = false

                let generator = UINotificationFeedbackGenerator()
                generator.notificationOccurred(.success)
            }
        } catch {
            await MainActor.run {
                scanError = error.localizedDescription
                isScanning = false

                let generator = UINotificationFeedbackGenerator()
                generator.notificationOccurred(.error)
            }
        }
    }

    private func handleScannedPDF(_ url: URL) async {
        guard url.startAccessingSecurityScopedResource() else { return }
        defer { url.stopAccessingSecurityScopedResource() }

        guard let image = InvoiceScannerService.shared.extractFirstPage(from: url) else {
            await MainActor.run { scanError = "Impossible de lire le PDF" }
            return
        }

        await handleScannedImage(image)
    }

    private func applyScannedData(_ data: InvoiceData) {
        if let serviceName = data.serviceName, !serviceName.isEmpty {
            name = serviceName
        }
        if let amount = data.amount {
            amountText = String(format: "%.2f", amount)
        }
        if let dateString = data.billingDate,
           let date = ISO8601DateFormatter().date(from: dateString) {
            nextBillingDate = date
        }
        if let freq = data.frequency?.lowercased() {
            switch freq {
            case "yearly", "annual": frequency = .yearly
            case "weekly": frequency = .weekly
            case "quarterly": frequency = .quarterly
            default: frequency = .monthly
            }
        } else {
            frequency = .monthly // Default
        }
    }
    ```

- [ ] **Task 15: Ajouter l'overlay de chargement**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter après `.sheet(isPresented: $showDocumentPicker)`:
    ```swift
    .overlay {
        if isScanning {
            ZStack {
                Color.black.opacity(0.4).ignoresSafeArea()
                VStack(spacing: 16) {
                    ProgressView()
                        .scaleEffect(1.5)
                        .tint(.white)
                    Text("Analyse en cours...")
                        .foregroundStyle(.white)
                        .font(.headline)
                }
                .padding(32)
                .background(.ultraThinMaterial, in: RoundedRectangle(cornerRadius: 16))
            }
        }
    }
    ```

- [ ] **Task 16: Ajouter l'alerte d'erreur**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Ajouter après l'overlay:
    ```swift
    .alert("Erreur de scan", isPresented: .init(
        get: { scanError != nil },
        set: { if !$0 { scanError = nil } }
    )) {
        Button("OK", role: .cancel) {}
    } message: {
        Text(scanError ?? "Une erreur est survenue")
    }
    ```

- [ ] **Task 17: Ajouter les imports nécessaires**
  - File: `MySubGuard/Features/Subscriptions/Views/AddSubscriptionView.swift`
  - Action: Vérifier que `import PhotosUI` est présent (sinon l'ajouter)

### Acceptance Criteria

```gherkin
Feature: Scanner de factures IA

  Scenario: AC1 - Bouton Scanner visible
    Given l'utilisateur ouvre AddSubscriptionView
    Then un bouton avec l'icône "doc.text.viewfinder" est visible dans la toolbar gauche

  Scenario: AC2 - Menu de choix de source
    Given l'utilisateur est sur AddSubscriptionView
    When il appuie sur le bouton Scanner
    Then un menu apparaît avec les options:
      | Prendre une photo |
      | Choisir une image |
      | Importer un PDF |
      | Annuler |

  Scenario: AC3 - Scan photo caméra réussi
    Given l'utilisateur a sélectionné "Prendre une photo"
    And il capture une facture Netflix montrant "15,99 €/mois"
    When l'analyse est terminée
    Then le champ "name" contient "Netflix"
    And le champ "amount" contient "15.99"
    And la fréquence est "monthly"
    And un haptic feedback success est déclenché

  Scenario: AC4 - Scan image galerie réussi
    Given l'utilisateur a sélectionné "Choisir une image"
    And il sélectionne un screenshot d'email Spotify
    When l'analyse est terminée
    Then les champs sont pré-remplis avec les données extraites

  Scenario: AC5 - Scan PDF réussi
    Given l'utilisateur a sélectionné "Importer un PDF"
    And il sélectionne un PDF de facture Adobe
    When l'analyse est terminée
    Then la première page du PDF a été analysée
    And les champs sont pré-remplis

  Scenario: AC6 - Loading state affiché
    Given l'utilisateur a capturé une image
    When l'analyse est en cours
    Then un overlay avec ProgressView et "Analyse en cours..." est affiché
    And le bouton Scanner est désactivé

  Scenario: AC7 - Extraction partielle (fallback)
    Given une image où seul le nom du service est lisible
    When l'analyse est terminée
    Then le champ "name" est pré-rempli
    And les autres champs restent vides
    And l'utilisateur peut les remplir manuellement

  Scenario: AC8 - Erreur réseau gérée
    Given pas de connexion internet
    When l'utilisateur tente un scan
    Then une alerte "Erreur de scan" s'affiche
    And le formulaire reste utilisable
    And un haptic feedback error est déclenché

  Scenario: AC9 - Fréquence par défaut
    Given une facture sans indication de fréquence
    When l'analyse est terminée
    Then la fréquence est définie sur "monthly" par défaut
```

## Additional Context

### Dependencies

**iOS (frameworks natifs):**
- `PhotosUI` - PHPickerViewController pour galerie
- `PDFKit` - extraction page PDF
- `UIKit` - UIImagePickerController pour caméra
- `UniformTypeIdentifiers` - UTType.pdf

**Laravel (à installer):**
```bash
composer require openai-php/laravel
```

**Configuration requise:**
- `OPENAI_API_KEY` dans `.env` Laravel
- Permissions iOS: `NSCameraUsageDescription` dans Info.plist (si pas déjà présent)

### Prompt OpenAI Vision

```
Analyze this invoice/receipt image and extract subscription information.
Return ONLY a valid JSON object with these exact fields:
{
  "service_name": "string or null",
  "amount": number or null,
  "currency": "string or null (e.g. EUR, USD)",
  "billing_date": "string ISO8601 or null",
  "frequency": "monthly|yearly|weekly|quarterly or null"
}

Rules:
- Extract the subscription/service name (e.g., Netflix, Spotify, Adobe)
- Extract the price amount as a number (e.g., 15.99)
- Detect the currency from symbols (€=EUR, $=USD, £=GBP)
- If a date is visible, format it as ISO8601 (YYYY-MM-DD)
- Detect frequency from words like "monthly", "per month", "annual", "yearly"
- If a field cannot be determined with confidence, set it to null
- Respond with valid JSON only, no markdown, no explanation
```

### Testing Strategy

**Tests manuels (priorité haute):**
1. Facture Netflix (email screenshot) - EUR, mensuel
2. Facture Spotify (PDF) - EUR, mensuel
3. Facture Adobe Creative Cloud (email) - EUR, annuel
4. Facture avec qualité moyenne
5. Document non-facture (doit retourner des nulls)

**Tests automatisés (optionnel pour MVP):**
- Mock InvoiceScannerService pour tester applyScannedData()
- Test Laravel avec mock OpenAI client

### Risk Assessment

| Risque | Impact | Mitigation |
|--------|--------|------------|
| OpenAI rate limit | Medium | Afficher erreur claire, retry manuel |
| Coût API élevé | Low | Compression image, 1 seul appel par scan |
| Extraction incorrecte | Medium | Permettre édition manuelle, fallback gracieux |
| Timeout sur images lourdes | Medium | Timeout 60s, compression 0.8 |

### Notes

- L'utilisateur possède déjà une clé API OpenAI
- Version cible : 0.1.1-beta.1
- Compression JPEG 0.8 pour équilibrer qualité/taille
- Timeout 60s côté APIClient (vérifier si configurable)
- Info.plist: `NSCameraUsageDescription` probablement déjà présent pour Sign in with Apple
