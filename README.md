# Flutter Firebase Authentication App - Complete Setup Guide

A complete Flutter application with Firebase authentication featuring user registration, login, and a personalized home screen with counter functionality.

## 📱 Features

- Landing page with Hello message and auth buttons
- User registration with name, email, and password
- User login with email and password validation  
- Personalized welcome screen showing user's name
- Counter functionality (classic Flutter demo)
- Automatic authentication state management
- Secure user data storage in Firestore
- Clean, modern UI design

## 🛠 Prerequisites Installation

### 1. Install Java Development Kit (JDK)
bash
# Download and install JDK 8 or higher
# Windows: Download from Oracle or use Chocolatey
choco install openjdk11

# Verify installation
java -version


### 2. Install Android Studio
1. Download from [Android Studio Official Site](https://developer.android.com/studio)
2. Install with default settings
3. Open Android Studio
4. Go to *Tools > SDK Manager*
5. Install the latest Android SDK (API level 33 or higher)
6. Go to *Tools > AVD Manager* and create a virtual device

### 3. Install Flutter SDK
#### Windows:
1. Download Flutter SDK from [Flutter Official Site](https://docs.flutter.dev/get-started/install/windows)
2. Extract to C:\flutter (or your preferred location)
3. Add Flutter to PATH:
   - Open System Properties → Environment Variables
   - Add C:\flutter\bin to your PATH variable

#### macOS:
bash
# Using Homebrew
brew install flutter

# Or download and extract manually
# Add to ~/.zshrc or ~/.bash_profile:
export PATH="$PATH:/path/to/flutter/bin"


#### Linux:
bash
# Download and extract Flutter
sudo snap install flutter --classic

# Or manual installation
wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.x.x-stable.tar.xz
tar xf flutter_linux_3.x.x-stable.tar.xz
export PATH="$PATH:`pwd`/flutter/bin"


### 4. Verify Flutter Installation
bash
flutter doctor

This command checks your environment and displays a report. Fix any issues shown.

### 5. Install Git
bash
# Windows
choco install git

# macOS  
brew install git

# Linux
sudo apt install git


### 6. Install a Code Editor
- *VS Code* (Recommended): Download from [VS Code Official Site](https://code.visualstudio.com/)
  - Install Flutter and Dart extensions
- *Android Studio*: Already installed above
- *IntelliJ IDEA*: Alternative option

## 🚀 Project Setup

### Step 1: Create Flutter Project
bash
# Create new Flutter project
flutter create flutter_firebase_auth
cd flutter_firebase_auth

# Test the basic app
flutter run


### Step 2: Update Dependencies
Replace your pubspec.yaml with:

yaml
name: flutter_firebase_auth
description: A Flutter project with Firebase authentication.
publish_to: 'none'
version: 1.0.0+1

environment:
  sdk: '>=2.19.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  
  # Firebase dependencies
  firebase_core: ^2.24.2
  firebase_auth: ^4.15.3
  cloud_firestore: ^4.13.6
  
  cupertino_icons: ^1.0.2

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^2.0.0

flutter:
  uses-material-design: true


Install dependencies:
bash
flutter pub get


### Step 3: Replace main.dart
Replace the content of lib/main.dart with the complete authentication app code (provided in the artifacts above).

## 🔥 Firebase Setup

### Step 1: Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click *"Create a project"* or *"Add project"*
3. Enter project name: flutter-auth-app
4. *Google Analytics*: Choose your preference (optional for this project)
5. Click *"Create project"*
6. Wait for project creation and click *"Continue"*

### Step 2: Enable Authentication
1. In Firebase Console, click *"Authentication"* from left sidebar
2. Click *"Get started"*
3. Go to *"Sign-in method"* tab
4. Click on *"Email/Password"*
5. *Enable* the first option (Email/Password)
6. Click *"Save"*

### Step 3: Setup Firestore Database
1. Click *"Firestore Database"* from left sidebar
2. Click *"Create database"*
3. *Security rules: Choose **"Start in test mode"* (for development)
4. *Location*: Choose closest to your location
5. Click *"Done"*

### Step 4: Add Flutter App to Firebase
1. In Firebase Console, click the *Flutter icon* (or *"Add app"*)
2. *Platform: Select **Android* first
3. *Android package name*: com.example.flutter_firebase_auth
4. *App nickname*: Flutter Auth App (optional)
5. Click *"Register app"*

### Step 5: Download Configuration Files
1. *Download* google-services.json
2. *Move* this file to your Flutter project: android/app/google-services.json

### Step 6: Configure Android
#### Update android/build.gradle (project level):
gradle
buildscript {
    ext.kotlin_version = '1.7.10'
    repositories {
        google()
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:7.2.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        // Add this line for Firebase
        classpath 'com.google.gms:google-services:4.3.15'
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.buildDir = '../build'
subprojects {
    project.buildDir = "${rootProject.buildDir}/${project.name}"
}
subprojects {
    project.evaluationDependsOn(':app')
}

task clean(type: Delete) {
    delete rootProject.buildDir
}


#### Update android/app/build.gradle:
gradle
def localProperties = new Properties()
def localPropertiesFile = rootProject.file('local.properties')
if (localPropertiesFile.exists()) {
    localPropertiesFile.withReader('UTF-8') { reader ->
        localProperties.load(reader)
    }
}

def flutterRoot = localProperties.getProperty('flutter.sdk')
if (flutterRoot == null) {
    throw new GradleException("Flutter SDK not found. Define location with flutter.sdk in the local.properties file.")
}

def flutterVersionCode = localProperties.getProperty('flutter.versionCode')
if (flutterVersionCode == null) {
    flutterVersionCode = '1'
}

def flutterVersionName = localProperties.getProperty('flutter.versionName')
if (flutterVersionName == null) {
    flutterVersionName = '1.0'
}

// Add this line at the top
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply from: "$flutterRoot/packages/flutter_tools/gradle/flutter.gradle"
// Add this line for Firebase
apply plugin: 'com.google.gms.google-services'

android {
    compileSdkVersion 33
    ndkVersion flutter.ndkVersion

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    kotlinOptions {
        jvmTarget = '1.8'
    }

    sourceSets {
        main.java.srcDirs += 'src/main/kotlin'
    }

    defaultConfig {
        applicationId "com.example.flutter_firebase_auth"
        // Firebase requires minimum SDK 21
        minSdkVersion 21
        targetSdkVersion 33
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
        // Enable multidex for Firebase
        multiDexEnabled true
    }

    buildTypes {
        release {
            signingConfig signingConfigs.debug
        }
    }
}

flutter {
    source '../..'
}

dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    // Add multidex support
    implementation 'com.android.support:multidex:1.0.3'
}


### Step 7: Create firebase_options.dart
1. In Firebase Console, go to *Project Settings* (gear icon)
2. Scroll down to *"Your apps"* section
3. Click on your Android app
4. Copy the configuration details

Create lib/firebase_options.dart:
dart
import 'package:firebase_core/firebase_core.dart' show FirebaseOptions;
import 'package:flutter/foundation.dart'
    show defaultTargetPlatform, kIsWeb, TargetPlatform;

class DefaultFirebaseOptions {
  static FirebaseOptions get currentPlatform {
    if (kIsWeb) {
      return web;
    }
    switch (defaultTargetPlatform) {
      case TargetPlatform.android:
        return android;
      case TargetPlatform.iOS:
        return ios;
      default:
        throw UnsupportedError(
          'DefaultFirebaseOptions are not supported for this platform.',
        );
    }
  }

  // Replace these values with your actual Firebase project values
  static const FirebaseOptions android = FirebaseOptions(
    apiKey: 'YOUR_API_KEY_HERE',
    appId: 'YOUR_APP_ID_HERE',
    messagingSenderId: 'YOUR_SENDER_ID_HERE',
    projectId: 'YOUR_PROJECT_ID_HERE',
    storageBucket: 'YOUR_STORAGE_BUCKET_HERE',
  );

  static const FirebaseOptions ios = FirebaseOptions(
    apiKey: 'YOUR_IOS_API_KEY_HERE',
    appId: 'YOUR_IOS_APP_ID_HERE',
    messagingSenderId: 'YOUR_SENDER_ID_HERE',
    projectId: 'YOUR_PROJECT_ID_HERE',
    storageBucket: 'YOUR_STORAGE_BUCKET_HERE',
    iosBundleId: 'com.example.flutterFirebaseAuth',
  );

  static const FirebaseOptions web = FirebaseOptions(
    apiKey: 'YOUR_WEB_API_KEY_HERE',
    appId: 'YOUR_WEB_APP_ID_HERE',
    messagingSenderId: 'YOUR_SENDER_ID_HERE',
    projectId: 'YOUR_PROJECT_ID_HERE',
    authDomain: 'YOUR_PROJECT_ID_HERE.firebaseapp.com',
    storageBucket: 'YOUR_STORAGE_BUCKET_HERE',
  );
}


*⚠ Important*: Replace all YOUR_*_HERE values with actual values from Firebase Console.

## 🏗 App Structure


lib/
├── main.dart              # Main app entry point with all screens
├── firebase_options.dart  # Firebase configuration
android/
├── app/
    ├── google-services.json  # Firebase Android config
    ├── build.gradle         # Android app configuration
    └── src/main/AndroidManifest.xml
pubspec.yaml              # Dependencies


## 🧪 Testing & Running

### Step 1: Clean and Get Dependencies
bash
flutter clean
flutter pub get


### Step 2: Check for Issues
bash
flutter doctor

Fix any issues reported.

### Step 3: Run the App
bash
# Run on connected device/emulator
flutter run

# Run on specific device
flutter devices
flutter run -d device_id

# Run in debug mode with hot reload
flutter run --debug


### Step 4: Test Authentication Flow
1. *Landing Page*: Should show "Hello!" message with Login/Register buttons
2. *Register*: Test with name, email, password
3. *Login*: Test with registered credentials
4. *Home Screen*: Should show personalized welcome with user's name
5. *Counter*: Test increment functionality
6. *Logout*: Test logout and return to landing page

## 🔧 Troubleshooting

### Common Issues:

#### 1. Build Errors
bash
# Clean project
flutter clean
rm -rf build/
flutter pub get
flutter run


#### 2. Firebase Connection Issues
- Verify google-services.json is in android/app/
- Check if Firebase project has Authentication enabled
- Ensure package name matches in Firebase Console

#### 3. Authentication Errors
- Check Firebase Console → Authentication → Sign-in method
- Verify Email/Password is enabled
- Check Firestore security rules

#### 4. Gradle Build Issues
bash
# In android folder
cd android
./gradlew clean
cd ..
flutter run


#### 5. Multidex Issues
Ensure multiDexEnabled true is in android/app/build.gradle

### Firebase Security Rules (Production)
For production, update Firestore rules in Firebase Console:
javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}


## 📚 Additional Resources

- [Flutter Documentation](https://docs.flutter.dev/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Flutter Firebase Codelab](https://firebase.google.com/codelabs/firebase-get-to-know-flutter)

## 🚀 Next Steps

1. *Add profile management*: Edit user details
2. *Email verification*: Verify email after registration  
3. *Password reset*: Forgot password functionality
4. *Social login*: Google, Facebook authentication
5. *Push notifications*: Firebase Cloud Messaging
6. *Offline support*: Implement offline data caching

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

*🎉 Congratulations!* You now have a fully functional Flutter app with Firebase authentication. The app includes user registration, login, and a personalized experience with secure data storage.