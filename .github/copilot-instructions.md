## InventoryApp — Copilot instructions (concise)

This document gives an AI coding agent the minimum, actionable knowledge to be productive in this Flutter + Firebase app.

High level
- Flutter app (null-safe Dart, SDK >= 3.9.2). Entrypoint: `lib/main.dart`.
- Firebase back end: project configured by FlutterFire CLI. Generated config: `lib/firebase_options.dart` and `firebase.json`.
- Native platform folders used: `android/` (Kotlin Gradle Kotlin DSL), `ios/` (Xcode project), `windows/`, `linux/`, `macos/` — respect platform-specific files when changing native integration.

Where to look first (quick map)
- UI & app entry: `lib/main.dart` (app bootstrap).
- Firebase configuration: `lib/firebase_options.dart` (use `DefaultFirebaseOptions.currentPlatform` when initializing).
- Dependencies: `pubspec.yaml` (notable packages: `firebase_core`, `firebase_auth`, `cloud_firestore`, `firebase_messaging`, `firebase_storage`, `flutter_local_notifications`).
- Android native config: `android/app/google-services.json` (must remain in place for builds).

Important integration notes
- Firebase initialization: follow the example comment at top of `lib/firebase_options.dart` and call `Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform)` early in `main()` before using Firebase services.
- Generated file `lib/firebase_options.dart` contains API keys and platform ids produced by FlutterFire CLI. Treat additional secrets cautiously — avoid committing new service-account JSONs.
- `firebase.json` is present and shows FlutterFire CLI outputs. Use FlutterFire CLI to re-generate platform configs if needed.

Common developer workflows (commands for PowerShell)
- Get dependencies:
```powershell
flutter pub get
```
- Run on a connected device or Windows desktop:
```powershell
flutter run -d <device-id-or-windows>
```
- Build for Android/iOS/web:
```powershell
flutter build apk
flutter build ios
flutter build web
```
- Run tests and static analysis:
```powershell
flutter test
flutter analyze
```
- Format & fix imports:
```powershell
dart format .
dart fix --apply
```

Project-specific conventions and patterns
- Flutter + Firebase pattern: prefer initializing Firebase once at app start (see `firebase_options.dart` example). Search for `Firebase.initializeApp` when adding features that use Firebase.
- Keep Dart code inside `lib/`. Add feature modules as new files under `lib/` (e.g., `lib/features/<name>/` or `lib/services/`).
- The repo uses `flutter_lints` (see `pubspec.yaml`) — keep changes lint-friendly; run `flutter analyze` before PRs.
- Native build files use Kotlin DSL (`build.gradle.kts`, `settings.gradle.kts`) — be conservative when editing Gradle scripts; prefer small changes and test builds.

Edge cases and gotchas (from repo)
- `DefaultFirebaseOptions` throws for Linux if not configured — running on linux may require re-running FlutterFire CLI.
- Several platform targets exist; when adding a Firebase feature, update all supported platforms or document platform limitations.
- `google-services.json` already exists in `android/app` — do not overwrite without updating Firebase console and re-running FlutterFire CLI.

How to modify code safely
- Small focused edits: run `flutter pub get`, `dart format .`, `flutter analyze`, then `flutter test` and a quick `flutter run` on a device.
- When adding native Firebase capabilities (FCM, storage), update `lib/firebase_options.dart` via FlutterFire CLI and leave generated file checked in.

Files worth reading during code changes
- `lib/main.dart` — app bootstrap and where initialization belongs.
- `lib/firebase_options.dart` — generated Firebase options; example initialization comment is authoritative.
- `pubspec.yaml` — dependencies and Flutter configuration.
- `android/app/google-services.json`, `ios/Runner/Info.plist` and `ios/Runner/` assets — native glue points.

If unsure, ask the maintainer for:
- Which platforms are actively supported (mobile only vs desktop/web).
- Any secrets or API keys that should not be touched.

End — request feedback
If anything is unclear or you'd like more examples (e.g., where to add services, firestore rules, CI commands), tell me which area to expand.
