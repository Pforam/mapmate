# MapMate 🗺️👥

MapMate is a collaborative mobile application built with Flutter that combines standard Google Maps navigation with a smart meet-up planner. It allows groups of friends or family members to share live coordinates in real-time, automatically computes an optimized meeting point, and suggests nearby venues like cafés, parks, and restaurants.

---

## 🚀 Tech Stack & Architecture
* **Framework:** Flutter (Dart SDK 3.x+)
* **State Management:** Provider
* **Backend & Real-Time Sync:** Firebase Authentication, Realtime Database, and Cloud Firestore
* **APIs:** Google Maps SDK, Places API, Distance Matrix API

---

## 🛠️ Prerequisites & Setup Guide for Teammates

Follow these steps to set up and run the project locally on your machine:

### 1. Clone the Repository
Open your terminal and clone the project:
```bash
git clone [https://github.com/YOUR_USERNAME/mapmate.git](https://github.com/YOUR_USERNAME/mapmate.git)
cd mapmate

### 2. Install Dependencies
Run the following command to download all required packages specified in pubspec.yaml:
flutter pub get

### 3. Firebase Configuration
This project uses Firebase. Since configuration files contain environment keys, they are excluded from Git. You must link your own Firebase account:

Make sure you have the Firebase CLI installed and logged in:
firebase login
Run the FlutterFire configuration tool to generate your local firebase_options.dart:
flutterfire configure

### 4. Google Maps API Keys Setup
You need a Google Maps API key with Maps SDK for Android/iOS, Places API, and Distance Matrix API enabled.

For Android: Open android/app/src/main/AndroidManifest.xml and make sure your API key is inserted:
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_GOOGLE_MAPS_API_KEY"/>

For iOS: Open ios/Runner/AppDelegate.swift and ensure your API key is configured:
GMSServices.provideAPIKey("YOUR_GOOGLE_MAPS_API_KEY")

### 5. Run the Application
Connect a physical device or start an Android/iOS emulator, then run:
flutter run

📂 Project Directory Structure

lib/
├── main.dart                             <-- App entry point & Firebase initialization
├── models/
│   ├── user_model.dart                   <-- User profile data schema
│   ├── session_model.dart                <-- Meetup session metadata model
│   └── place_model.dart                  <-- Venue place item model
├── services/
│   ├── auth_service.dart                 <-- Firebase authentication & Google Sign-In
│   ├── database_service.dart             <-- Realtime database sync & session management
│   └── location_service.dart             <-- Geolocator, midpoint routing & API hooks
├── providers/
│   ├── auth_provider.dart                <-- Reactive state management for authentication
│   └── meetup_provider.dart              <-- Active session state, participants & venue lists
└── views/
    ├── auth/login_screen.dart            <-- Google Sign-In user interface
    ├── map/map_screen.dart               <-- Main interactive map, custom pins & route polylines
    ├── meetup/create_session_screen.dart <-- Room code generation screen
    ├── meetup/join_session_screen.dart   <-- Room code entry screen
    └── widgets/venue_card_sheet.dart     <-- Bottom sheet displaying recommended cafes/parks