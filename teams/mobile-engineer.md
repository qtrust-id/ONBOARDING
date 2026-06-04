# Onboarding Guide — Mobile Apps Engineer

**Team:** Mobile Apps Engineering  
**Role Overview:** Build native and cross-platform mobile applications that give users and managers on-the-go access to the application's core features and approvals.

---

## 1. Welcome

You are building the mobile face of the application — the app that users interact with daily to access [project features]. Mobile is often the primary touch point for non-desk workers, making reliability and responsiveness non-negotiable.

The team supports four development approaches: **Android Native (Kotlin)**, **iOS Native (Swift)**, **Flutter**, and **React Native**. Which approach is used for a given feature or release is determined by the team lead based on project priorities, timelines, and team composition. This guide covers all four, with environment setup instructions for each.

All mobile apps consume the same **REST API** as the web application. The API base URL and auth method are in the Project Configuration Sheet. The mobile team does not maintain a separate backend.

---

## 2. Your Responsibilities

- Implement mobile screens based on approved Figma designs from the UIX Designer
- Integrate with the project's REST API (base URL from Project Config Sheet) using bearer token authentication
- Implement offline-first data caching for frequently accessed features ([feature] data defined in the Project Config Sheet)
- Handle push notifications (FCM for Android, APNs for iOS) for approvals and reminders
- Implement device features: camera (selfie check-in), GPS (location capture), biometric authentication
- Write unit and UI tests for implemented features
- Collaborate with the Backend team on API contracts — define required endpoints before development begins
- Maintain and update `by-team/mobile/api-contracts.md` whenever new endpoints are needed
- Submit apps to Google Play (internal track) and Apple TestFlight for QA testing
- Integrate the Sentry SDK into the app and triage crash reports (Android ANRs, iOS crashes, Flutter errors) it surfaces per platform project `[project-code]-mobile-android` / `-ios` (see [`tools/sentry.md`](../tools/sentry.md)) — connect the Sentry MCP in Claude Desktop

---

## 3. Development Approach Decision Guide

Before starting any feature, confirm with your team lead which approach to use.

| Approach | Best For | Trade-offs |
|---|---|---|
| **Android Native (Kotlin)** | Deep Android device integration, complex UI animations, Jetpack Compose | Android only; requires separate iOS app |
| **iOS Native (Swift)** | Tight iOS/iPadOS integration, App Store compliance requirements | iOS only; requires separate Android app |
| **Flutter (Dart)** | Single codebase, rich UI, fast feature parity across platforms | Dart learning curve; some platform APIs need plugins |
| **React Native (TypeScript)** | Team familiar with JS/TS; shared logic with web codebase | JS bridge overhead; some native feel limitations |

**Current recommendation for MVP:** Flutter — single codebase reduces delivery time for the initial set of modules (Attendance, Leave, Payslip).

---

## 4. Environment Setup

### 4.1 Common Setup (All Approaches)

**Clone the mobile repository:**
```bash
# Mobile app lives in a separate repository
git clone [MOBILE_REPO_URL]  # URL from Project Config Sheet
cd [project-code]-mobile
```

**API base URLs:**
```
Development : https://[project-code]-dev-[hash]-uc.a.run.app/api/v1
Staging     : https://[project-code]-staging-[hash]-uc.a.run.app/api/v1
Production  : https://[project-code].qtrust.id/api/v1
```

Store these in your environment configuration — never hardcode URLs.

---

### 4.2 Android Native (Kotlin)

**Prerequisites:**
- Android Studio (Hedgehog or later)
- JDK 17
- Android SDK with API level 26 (Android 8.0) as minimum, 34 (Android 14) as target

**Project setup:**
```bash
# Android Studio → Open → select [project-code]-mobile/android/
```

**Key dependencies (app/build.gradle.kts):**
```kotlin
dependencies {
    // Networking
    implementation("com.squareup.retrofit2:retrofit:2.11.0")
    implementation("com.squareup.retrofit2:converter-gson:2.11.0")
    implementation("com.squareup.okhttp3:logging-interceptor:4.12.0")

    // Dependency Injection
    implementation("com.google.dagger:hilt-android:2.51.1")
    kapt("com.google.dagger:hilt-compiler:2.51.1")

    // UI
    implementation("androidx.compose.ui:ui:1.7.0")
    implementation("androidx.compose.material3:material3:1.3.0")
    implementation("androidx.navigation:navigation-compose:2.8.0")

    // Local storage
    implementation("androidx.room:room-runtime:2.6.1")
    implementation("androidx.room:room-ktx:2.6.1")

    // Camera
    implementation("androidx.camera:camera-camera2:1.3.4")
    implementation("androidx.camera:camera-view:1.3.4")

    // Location
    implementation("com.google.android.gms:play-services-location:21.3.0")

    // Push notifications
    implementation("com.google.firebase:firebase-messaging-ktx:24.0.0")
}
```

**Architecture:** MVVM + Repository pattern with Hilt for DI.

---

### 4.3 iOS Native (Swift)

**Prerequisites:**
- macOS with Xcode 16 or later (iOS development requires macOS)
- iOS 16.0 minimum deployment target
- Apple Developer account (for device testing and TestFlight)

**Project setup:**
```bash
# Xcode → Open → select [project-code]-mobile/ios/[PROJECT_NAME].xcworkspace
```

**Package dependencies (Swift Package Manager):**
```
Alamofire        — 5.9.x     — HTTP networking
SwiftyJSON       — 5.x       — JSON parsing (alternative: Codable)
Kingfisher       — 7.x       — Image caching
KeychainSwift    — 20.x      — Secure token storage
Firebase/Messaging            — Push notifications
```

**Architecture:** MVVM with Combine for reactive binding. Use SwiftUI for new screens.

---

### 4.4 Flutter

**Prerequisites:**
```bash
# macOS
brew install flutter

flutter doctor   # verify all requirements are met
                 # Android Studio + Xcode both needed for dual-platform builds
```

**Setup:**
```bash
cd [project-code]-mobile/flutter
flutter pub get
flutter run      # runs on connected device or emulator
```

**Key packages (pubspec.yaml):**
```yaml
dependencies:
  flutter:
    sdk: flutter

  # Networking
  dio: ^5.7.0
  pretty_dio_logger: ^1.4.0

  # State management
  flutter_riverpod: ^2.5.0
  riverpod_annotation: ^2.3.5

  # Local storage
  hive_flutter: ^1.1.0
  flutter_secure_storage: ^9.2.0      # for auth token

  # Navigation
  go_router: ^14.0.0

  # Camera & Media
  camera: ^0.11.0
  image_picker: ^1.1.0

  # Location
  geolocator: ^13.0.0
  permission_handler: ^11.3.0

  # Push notifications
  firebase_messaging: ^15.0.0
  flutter_local_notifications: ^17.2.0

  # UI utilities
  cached_network_image: ^3.4.0
  intl: ^0.19.0                        # date/number formatting
  shimmer: ^3.0.0                      # loading skeleton screens
```

**Project structure (Flutter):**
```
lib/
├── main.dart
├── app/
│   ├── router.dart               ← go_router configuration
│   └── theme.dart                ← ThemeData from design tokens
├── core/
│   ├── api/
│   │   ├── api_client.dart       ← Dio setup with interceptors
│   │   └── api_endpoints.dart    ← All endpoint constants
│   ├── auth/
│   │   └── auth_provider.dart    ← Token management with Riverpod
│   └── storage/
│       └── local_storage.dart    ← Hive wrappers
├── features/
│   ├── [feature-a]/
│   │   ├── data/                 ← Repository, API calls, models
│   │   ├── domain/               ← Use cases
│   │   └── presentation/         ← Screens, widgets, providers
│   ├── [feature-b]/
│   └── ...
└── shared/
    ├── widgets/                  ← Reusable UI components
    └── utils/
```

---

### 4.5 React Native (TypeScript)

**Prerequisites:**
```bash
brew install node@20 watchman
npm install -g @react-native-community/cli
# Also install Android Studio and Xcode (same as Flutter)
```

**Setup:**
```bash
cd [project-code]-mobile/react-native
npm install
cd ios && pod install && cd ..
npx react-native run-android
npx react-native run-ios
```

**Key packages (package.json):**
```json
{
  "dependencies": {
    "@react-navigation/native": "^7.0.0",
    "@react-navigation/bottom-tabs": "^7.0.0",
    "@react-navigation/stack": "^7.0.0",
    "axios": "^1.7.0",
    "zustand": "^5.0.0",
    "react-native-mmkv": "^3.1.0",
    "react-native-keychain": "^9.1.0",
    "@react-native-camera-roll/camera-roll": "^7.8.0",
    "react-native-vision-camera": "^4.0.0",
    "react-native-geolocation-service": "^5.3.0",
    "@react-native-firebase/app": "^21.0.0",
    "@react-native-firebase/messaging": "^21.0.0",
    "react-native-fast-image": "^8.6.3",
    "dayjs": "^1.11.0"
  }
}
```

---

## 5. API Integration

All mobile apps authenticate using **bearer tokens**. Auth method is defined in the Project Config Sheet (e.g., Laravel Sanctum, JWT, OAuth2).

### Authentication Flow
```
1. POST /api/v1/auth/login
   Body: { email, password }
   Response: { data: { token, user } }

2. Store token securely:
   Android: EncryptedSharedPreferences or Keystore
   iOS: Keychain
   Flutter: flutter_secure_storage
   React Native: react-native-keychain

3. Include token in all subsequent requests:
   Authorization: Bearer {token}

4. POST /api/v1/auth/device-token
   Body: { device_token: "[FCM or APNs token]", platform: "android|ios" }
   (Call this after login to enable push notifications)

5. On logout:
   POST /api/v1/auth/logout
   Then clear stored token
```

### Handling Token Expiry
Implement a Dio/Retrofit/Axios interceptor that:
1. Catches 401 responses
2. Attempts token refresh via `POST /api/v1/auth/refresh`
3. Retries the original request with the new token
4. If refresh fails — logs the user out and redirects to login

### API Contracts
Before implementing any feature that requires a new or modified API endpoint, document it in `by-team/mobile/api-contracts.md` and notify the Backend Engineer team. Do not assume an endpoint exists — verify against `docs/api/openapi.yaml`.

---

## 6. Push Notifications

### Setup (Firebase Cloud Messaging)
1. Create a Firebase project for [PROJECT_NAME] in [Firebase Console](https://console.firebase.google.com)
2. Add Android and iOS apps to the Firebase project
3. Download `google-services.json` (Android) and `GoogleService-Info.plist` (iOS)
4. **Do not commit these files to GitHub** — store them in GCP Secret Manager and inject during CI/CD build

### Notification Categories
| Category | Trigger | Recipients |
|---|---|---|
| `approval_request` | User submits a request | Approver |
| `approval_result` | Approver approves/rejects | Requester |
| `[feature]_ready` | A batch process completes | Relevant users |
| `[feature]_reminder` | Scheduled reminder trigger | Relevant users |
| `announcement` | Admin broadcasts message | All users |

---

## 7. Offline Support

Features requiring offline support are defined in the Project Config Sheet. Common examples: [frequently accessed data], user profile. Implement a cache-first strategy:

1. On first load: fetch from API → store in local database (Room/CoreData/Hive/MMKV)
2. On subsequent loads: show cached data immediately → refresh from API in background
3. If API fails and cache exists: show cached data with a "Last updated: X" label
4. If no cache and API fails: show a meaningful empty state — not a raw error

Mark cached data with a `cached_at` timestamp. Invalidate when it is older than 1 hour.

---

## 8. Device Permissions

| Permission | Feature | When to Request |
|---|---|---|
| `CAMERA` | Selfie check-in | At check-in screen, first use |
| `ACCESS_FINE_LOCATION` | GPS check-in | At check-in screen, first use |
| `POST_NOTIFICATIONS` (Android 13+) | Push notifications | After login, with explanation |
| `USE_BIOMETRIC` | Biometric login | In settings, opt-in |

Always explain why you need a permission before requesting it. If the user denies, gracefully degrade — check-in without photo, or show a manual coordinate input.

---

## 9. Git Workflow

Mobile code lives in a separate repository (`[project-code]-mobile`), not in the main application repository.

```bash
git checkout develop
git pull origin develop
git checkout -b feature/[platform]-[module]-[description]

# Examples:
# feature/flutter-[module]-[description]
# feature/android-leave-request-form
# fix/ios-push-notification-token
```

PRs target `develop`. Require 1 reviewer. CI must build successfully for the relevant platform.

---

## 10. Working with Claude Desktop

**Generate a Flutter screen from Figma:**
> "Read the Figma design at [URL] and generate a Flutter screen for the [Feature] page. It should use a CameraPreview widget, display the current time and date, show a GPS status indicator, and have a circular action button. Use Riverpod for state management."

**Generate an API service class (Flutter):**
> "Write a Flutter Dart class using Dio to handle all [feature] API calls. Methods: [list relevant methods]. Include error handling that distinguishes between network errors, 401 (auth), and 422 (validation)."

**Generate a Kotlin ViewModel:**
> "Write a Kotlin ViewModel using Hilt and Jetpack Compose for the Leave Request screen. It should expose: form state (leave type, start/end dates, reason), a submit function that calls the API, and a UI state sealed class with Loading, Success, and Error states."

**Generate Swift code:**
> "Write a Swift async/await function using Alamofire to submit a leave request. Handle: network errors, 422 validation errors (parse field-level messages), and 401 unauthorized. Return a Result type."

---

## 11. CI/CD for Mobile

| Platform | Pipeline | Destination |
|---|---|---|
| Flutter (Android) | GitHub Actions → `flutter build apk` | Google Play Internal Track |
| Flutter (iOS) | GitHub Actions + Fastlane → `flutter build ipa` | TestFlight |
| Android Native | GitHub Actions → Gradle build | Google Play Internal Track |
| iOS Native | GitHub Actions + Fastlane → Xcode build | TestFlight |

Store signing certificates and provisioning profiles in GitHub Actions Secrets, never in the repository.

---

## 12. First Week Checklist

- [ ] Development environment set up for your primary platform (Flutter recommended)
- [ ] Development API URL configured and login tested on device/emulator
- [ ] Firebase project access granted — FCM working on test device
- [ ] Figma view access confirmed — mobile design files reviewed (`by-team/uix/figma-links.md`)
- [ ] `by-team/mobile/ux-notes.md` and `by-team/mobile/api-contracts.md` read thoroughly
- [ ] `docs/api/openapi.yaml` reviewed — understand all available endpoints
- [ ] Claude Desktop installed and workspace folder connected
- [ ] Sentry SDK integrated for your platform (DSN from Secret Manager) and Sentry MCP connected in Claude Desktop
- [ ] First GitHub Issue assigned in `[project-code]-mobile` repository
- [ ] Attended sprint planning
- [ ] Introduced yourself to Backend team — align on API contract priorities

---

## 13. Key Resources

| Resource | Location |
|---|---|
| Mobile API Contracts | `by-team/mobile/api-contracts.md` |
| Mobile UX Notes | `by-team/mobile/ux-notes.md` |
| Figma Mobile Screens | `by-team/uix/figma-links.md` |
| API Specification | `docs/api/openapi.yaml` |
| Design System Tokens | `designs/design-system/README.md` |
| Flutter Docs | [docs.flutter.dev](https://docs.flutter.dev) |
| Riverpod Docs | [riverpod.dev](https://riverpod.dev) |
| Android Developers | [developer.android.com](https://developer.android.com) |
| Apple HIG | [developer.apple.com/design/human-interface-guidelines](https://developer.apple.com/design/human-interface-guidelines) |
| Firebase FCM | [firebase.google.com/docs/cloud-messaging](https://firebase.google.com/docs/cloud-messaging) |
| React Navigation | [reactnavigation.org](https://reactnavigation.org) |

---

## Required Tools

See **[`tools/README.md`](../tools/README.md)** for the full tool matrix, setup order, and activation instructions.

**Your required tools:** Claude Desktop · GitHub (write) · Google Drive · Slack · Figma (viewer) · Sentry

| Tool | Setup Guide | Priority |
|---|---|---|
| Claude Desktop | [`tools/claude-desktop.md`](../tools/claude-desktop.md) | Day 1 |
| Google Drive | [`tools/google-drive.md`](../tools/google-drive.md) | Day 1 |
| GitHub | [`tools/github.md`](../tools/github.md) | Day 1 |
| Slack | [`tools/slack.md`](../tools/slack.md) | Day 1 |
| Figma | [`tools/figma.md`](../tools/figma.md) | Day 1 |
| Sentry | [`tools/sentry.md`](../tools/sentry.md) | Before first staging deploy |

> Connect tools in the order listed. The first four (Claude Desktop, Google Drive, GitHub, Slack) are required on Day 1 for every team member.
