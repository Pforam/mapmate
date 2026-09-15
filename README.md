# MapMate 🗺️👥

MapMate is a collaborative mobile application built with Flutter that combines standard Google Maps navigation with a smart meet-up planner. It allows groups of friends or family members to share live coordinates in real-time, automatically computes an optimized meeting point, and suggests nearby venues like cafés, parks, and restaurants.

---

## 🚀 Tech Stack & Architecture

| Layer | Technology |
|---|---|
| **Framework** | Flutter (Dart SDK 3.x+) |
| **State Management** | Provider |
| **Backend & Real-Time Sync** | Firebase Authentication, Realtime Database, Cloud Firestore |
| **APIs** | Google Maps SDK, Places API, Distance Matrix API |

---

## 📋 Prerequisites

Before you begin, make sure the following are installed on your machine:

| Tool | Purpose | Notes |
|---|---|---|
| **Flutter SDK** | Core framework (bundles Dart) | Stable channel, 3.x or newer |
| **Git** | Cloning the repo & used internally by Flutter | Any recent version |
| **Android Studio** | Android SDK, platform tools & emulator | Required even if you code in VS Code |
| **VS Code** *(optional)* | Editor with Flutter/Dart extensions | Or use Android Studio directly |
| **Xcode + CocoaPods** | iOS builds | **macOS only** |
| **Node.js + npm** | Needed to install the Firebase CLI | LTS release recommended |

---

## 💻 Part 1 — Installing Flutter

> The official guide is the source of truth and is kept up to date:
> **https://docs.flutter.dev/install**

### Windows

1. **Install Git for Windows** — <https://git-scm.com/download/win>
2. **Download the Flutter SDK** (Windows, stable channel) from <https://docs.flutter.dev/install/archive>
3. **Extract** the zip to a path *without spaces or special characters*, e.g. `C:\src\flutter`
   *(Avoid `C:\Program Files\` — the space causes issues.)*
4. **Add Flutter to your PATH:**
   - Search Windows for **"Edit environment variables for your account"**
   - Select **Path** → **Edit** → **New**
   - Add `C:\src\flutter\bin`
   - Click **OK**, then close and reopen all terminals
5. **Verify:**
   ```bash
   flutter --version
   ```

### macOS

Using Homebrew (simplest):

```bash
brew install --cask flutter
xcode-select --install
```

Or install manually:

```bash
# Download the macOS SDK zip from https://docs.flutter.dev/install/archive
cd ~/development
unzip ~/Downloads/flutter_macos_<version>-stable.zip

# Add to PATH (use ~/.bashrc if you're on bash)
echo 'export PATH="$HOME/development/flutter/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

flutter --version
```

### Linux

```bash
# Install prerequisite packages (Debian/Ubuntu)
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install -y curl git unzip xz-utils zip libglu1-mesa

# Download the Linux SDK from https://docs.flutter.dev/install/archive, then:
cd ~/development
tar -xf ~/Downloads/flutter_linux_<version>-stable.tar.xz

echo 'export PATH="$HOME/development/flutter/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

flutter --version
```

### Quick alternative: install via VS Code

If you'd rather not touch PATH manually, install the **Flutter extension** in VS Code, then run
`Flutter: New Project` from the Command Palette (`Ctrl/Cmd + Shift + P`) — VS Code will offer to
download and configure the SDK for you. See <https://docs.flutter.dev/install/with-vs-code>.

---

## 🧰 Part 2 — Setting Up the Toolchain

### 1. Install Android Studio

Download from <https://developer.android.com/studio>. During the setup wizard, make sure these
components are installed:

- Android SDK
- Android SDK Command-line Tools
- Android SDK Build-Tools
- Android SDK Platform-Tools
- Android Emulator

Then install the **Flutter** and **Dart** plugins:
`Settings → Plugins → Marketplace → search "Flutter" → Install` (Dart installs alongside it).

### 2. Accept the Android licenses

```bash
flutter doctor --android-licenses
```

Type `y` at each prompt.

### 3. iOS setup (macOS only)

```bash
# Install Xcode from the App Store, then:
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch

# Install CocoaPods
sudo gem install cocoapods
```

### 4. Create an emulator / connect a device

**Android Emulator:**
Android Studio → **Device Manager** → **Create Device** → pick a phone (e.g. Pixel 7) →
select a system image (API 30+ recommended) → **Finish**.

**Physical Android device:**
Enable **Developer Options** (tap *Build Number* 7 times in Settings → About Phone), then turn on
**USB Debugging** and plug the device in.

**iOS Simulator (macOS):**
```bash
open -a Simulator
```

### 5. Verify everything

```bash
flutter doctor -v
```

Every relevant entry should show a green check. Resolve any ❌ or ⚠️ items before continuing.

You can confirm your device is visible with:

```bash
flutter devices
```

---

## 🛠️ Part 3 — Project Setup

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/mapmate.git
cd mapmate
```

### 2. Install Dependencies

Download all required packages specified in `pubspec.yaml`:

```bash
flutter pub get
```

### 3. Firebase Configuration

This project uses Firebase. Since configuration files contain environment keys, they are excluded
from Git. You'll need to link your own Firebase account.

First, create a project at <https://console.firebase.google.com> and enable **Authentication
(Google Sign-In)**, **Realtime Database**, and **Cloud Firestore**.

Then install the CLIs and log in:

```bash
# Firebase CLI (requires Node.js)
npm install -g firebase-tools
firebase login

# FlutterFire CLI
dart pub global activate flutterfire_cli
```

> If `flutterfire` isn't found afterwards, add Dart's pub cache to your PATH:
> `$HOME/.pub-cache/bin` on macOS/Linux, or `%LOCALAPPDATA%\Pub\Cache\bin` on Windows.

Finally, generate your local `firebase_options.dart`:

```bash
flutterfire configure
```

Select your Firebase project and the platforms (Android / iOS) you plan to build for.

### 4. Google Maps API Keys Setup

You'll need a Google Maps API key with **Maps SDK for Android**, **Maps SDK for iOS**,
**Places API**, and **Distance Matrix API** enabled. Create one in the
[Google Cloud Console](https://console.cloud.google.com/google/maps-apis) — note that billing must
be enabled on the project, though Google provides a free monthly usage tier.

**Android** — open `android/app/src/main/AndroidManifest.xml` and insert your API key inside the
`<application>` tag:

```xml
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_GOOGLE_MAPS_API_KEY"/>
```

**iOS** — open `ios/Runner/AppDelegate.swift` and configure your API key:

```swift
GMSServices.provideAPIKey("YOUR_GOOGLE_MAPS_API_KEY")
```

### 5. Run the Application

Connect a physical device or start an emulator, then run:

```bash
flutter run
```

To build a release artifact:

```bash
flutter build apk --release        # Android
flutter build ios --release        # iOS (macOS only)
```

---

## 🧯 Troubleshooting

| Problem | Fix |
|---|---|
| `flutter: command not found` | Flutter's `bin` folder isn't on your PATH. Re-check Part 1 and restart your terminal. |
| `flutter doctor` flags Android licenses | Run `flutter doctor --android-licenses` and accept all. |
| `flutterfire: command not found` | Add `$HOME/.pub-cache/bin` (or `%LOCALAPPDATA%\Pub\Cache\bin`) to your PATH. |
| Map renders as a blank grey grid | The API key is missing, restricted incorrectly, or the Maps SDK isn't enabled for the project. |
| CocoaPods errors on iOS | `cd ios && pod repo update && pod install` |
| Stale build / odd compile errors | `flutter clean && flutter pub get` |

---

## 📂 Project Directory Structure

```
lib/
├── main.dart                             # App entry point & Firebase initialization
├── models/
│   ├── user_model.dart                   # User profile data schema
│   ├── session_model.dart                # Meetup session metadata model
│   └── place_model.dart                  # Venue place item model
├── services/
│   ├── auth_service.dart                 # Firebase authentication & Google Sign-In
│   ├── database_service.dart             # Realtime database sync & session management
│   └── location_service.dart             # Geolocator, midpoint routing & API hooks
├── providers/
│   ├── auth_provider.dart                # Reactive state management for authentication
│   └── meetup_provider.dart              # Active session state, participants & venue lists
└── views/
    ├── auth/
    │   └── login_screen.dart             # Google Sign-In user interface
    ├── map/
    │   └── map_screen.dart               # Main interactive map, custom pins & route polylines
    ├── meetup/
    │   ├── create_session_screen.dart    # Room code generation screen
    │   └── join_session_screen.dart      # Room code entry screen
    └── widgets/
        └── venue_card_sheet.dart         # Bottom sheet displaying recommended cafes/parks
```
