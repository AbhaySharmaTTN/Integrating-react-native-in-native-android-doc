# React Native Integration Guide

## Integrating React Native into Native Android Application

| Property                 | Value                 |
| ------------------------ | --------------------- |
| **Project Name**         | ManageAppHost         |
| **React Native Version** | 0.83.1                |
| **Target SDK**           | Android 14+ (API 36+) |
| **Minimum SDK**          | Android 7 (API 24+)   |
| **Java Version**         | 17                    |

---

## Table of Contents

1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Setup & Installation](#setup--installation)
4. [Configuration Files](#configuration-files)
5. [Native Android Integration](#native-android-integration)
6. [React Native Components](#react-native-components)
7. [Build & Run](#build--run)

---

## Project Structure

### Root Directory

```
integratedReactNativeProject/
├── android/                           # this will store the existing android project
│   ├── app/
│   │   ├── src/
│   │   │   ├── main/
│   │   │   │   ├── java/com/example/naitveandroidma/
│   │   │   │   │   ├── MainActivity.kt
│   │   │   │   │   ├── MyReactActivity.kt
│   │   │   │   │   ├── MainApplication.kt
│   │   │   │   │   ├── ReactActivityModule.kt
│   │   │   │   │   └── ReactActivityPackage.kt
│   │   │   │   ├── AndroidManifest.xml
│   │   │   │   └── res/
│   │   │   ├── androidTest/
│   │   │   └── test/
│   │   ├── build.gradle                    # App-level gradle config
│   │   └── proguard-rules.pro
│   ├── build.gradle                        # Project-level gradle config
│   ├── gradle.properties
│   ├── settings.gradle
│   └── gradle/wrapper/
├── index.js                           # React Native entry point
├── babel.config.js                    # Babel configuration
├── metro.config.js                    # Metro bundler config
├── react-native.config.js             # React Native CLI config
├── package.json                       # Dependencies
```

---

## Setup & Installation

### Prerequisites

This integration was done with the default version used created by Android Studio

- **Node.js:** >= 20
- **Yarn**
- **Android SDK:** API 24+ (minimum)
- **Target SDK:** API 36
- **Java Development Kit:** Version 17

### Step 1: Install Dependencies

Add the following in **package.json**

```json
{
  "name": "ManageAppHost",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "lint": "eslint .",
    "start": "react-native start",
    "test": "jest"
  },
  "dependencies": {
    "react": "19.2.0",
    "react-native": "0.83.1",
    "@react-native/new-app-screen": "0.83.1",
    "react-native-safe-area-context": "^5.5.2",
    "@react-navigation/native": ">=6.0.16",
    "@react-navigation/native-stack": ">=6.9.4",
    "@react-native-community/netinfo": ">=11.3.2",
    "lodash": "^4.17.21",
    "react-native-gesture-handler": "^2.28.0",
    "react-native-linear-gradient": "^2.8.3",
    "react-redux": ">=8.0.5",
    "reactotron-redux": "^3.2.1",
    "redux": ">=4.2.0",
    "redux-thunk": ">=2.4.2",
    "@react-native-masked-view/masked-view": "^0.3.2",
    "react-native-fast-image": "^8.6.3",
    "react-native-svg": ">=15.5.0",
    "react-native-screens": ">=3.18.2",
    "react-native-reanimated": "^4.1.3",
    "react-native-worklets": "^0.7.0",
    "tp-freemium-react-native": "file:/Users/abhaysharma/Documents/ManageAppSDK/ManageAppSDK/ManageApp/tp-freemium-react-native"
  },
  "devDependencies": {
    "@babel/core": "^7.25.2",
    "@babel/preset-env": "^7.25.3",
    "@babel/runtime": "^7.25.0",
    "@react-native-community/cli": "20.0.0",
    "@react-native-community/cli-platform-android": "20.0.0",
    "@react-native-community/cli-platform-ios": "20.0.0",
    "@react-native/babel-preset": "0.83.1",
    "@react-native/eslint-config": "0.83.1",
    "@react-native/metro-config": "0.83.1",
    "@react-native/typescript-config": "0.83.1",
    "@types/jest": "^29.5.13",
    "@types/react": "^19.2.0",
    "@types/react-test-renderer": "^19.1.0",
    "eslint": "^8.19.0",
    "jest": "^29.6.3",
    "prettier": "2.8.8",
    "react-test-renderer": "19.2.0",
    "typescript": "^5.8.3"
  },
  "engines": {
    "node": ">=20"
  }
}
```

Then run the following command

```bash
# Install JavaScript dependencies
yarn install
```

### Step 2: Configure Android Environment

To set up environment for android, react native refer this: [Set up your environment](https://reactnative.dev/docs/set-up-your-environment?os=macos)

Ensure the following environment variables are set:

```bash
# Add to ~/.bashrc, ~/.zshrc, or ~/.bash_profile
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

### Step 3: Verify Installation

```bash
# Check Android SDK installation
$ANDROID_HOME/tools/bin/sdkmanager --list | grep "Android SDK"

# Verify Java version
java -version

# Check React Native CLI
npx react-native doctor
```

---

## Configuration Files

### 1. **babel.config.js**

```javascript
module.exports = {
  presets: ["module:@react-native/babel-preset"],
  plugins: ["react-native-worklets/plugin"],
};
```

---

### 2. **metro.config.js**

```javascript
const { getDefaultConfig } = require("@react-native/metro-config");
module.exports = getDefaultConfig(__dirname);
```

---

### 3. **react-native.config.js**

Configures React Native CLI and asset linking.

```javascript
module.exports = {
  assets: ["./node_modules/<path-to-sdk>/src/assets/fonts"],
};
```

---

### 4. **android/build.gradle**

Project-level Gradle configuration.

```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath("com.android.tools.build:gradle:7.3.1")
        classpath("com.facebook.react:react-native-gradle-plugin")
    }

    ext {
        compileSdkVersion = 36
        targetSdkVersion = 36
        minSdkVersion = 24
    }
}

plugins {
    alias(libs.plugins.android.application) apply false
    alias(libs.plugins.kotlin.android) apply false
}
```

---

### 5. **android/app/build.gradle**

App-level Gradle configuration.

**Configuration:**

```gradle
plugins {
  ...
  id("com.facebook.react")
}

android {
    namespace 'com.example.naitveandroidma'
    compileSdk 36

    defaultConfig {
        applicationId "com.example.naitveandroidma"
        minSdk 24
        targetSdk 36
        versionCode 1
        versionName "1.0"
    }

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_17 // version 17
        targetCompatibility JavaVersion.VERSION_17 // version 17
    }

    kotlinOptions {
        jvmTarget = JavaVersion.VERSION_17.toString() // version 17
    }

    java {
        toolchain {
            languageVersion = JavaLanguageVersion.of(17)
        }
    }
}

dependencies {
    ...

    implementation("com.facebook.react:react-android")
    implementation("com.facebook.react:hermes-android")
}

react {
    autolinkLibrariesWithApp()
}
```

---

### 6. **settings.gradle**

**Configuration:**

```gradle
import org.gradle.api.initialization.resolve.RepositoriesMode

pluginManagement {
    repositories {
        google {
            content {
                includeGroupByRegex("com\\.android.*")
                includeGroupByRegex("com\\.google.*")
                includeGroupByRegex("androidx.*")
            }
        }
        mavenCentral()
        gradlePluginPortal()
    }

    includeBuild("../node_modules/@react-native/gradle-plugin")
}

plugins { id("com.facebook.react.settings") }

extensions.configure(com.facebook.react.ReactSettingsExtension){ ex -> ex.autolinkLibrariesFromCommand() }

includeBuild("../node_modules/@react-native/gradle-plugin")

dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.PREFER_SETTINGS) // change this to set RepositoriesMode to PREFER_SETTINGS
    repositories {
        google()
        mavenCentral()
    }
}

rootProject.name = "NaitveAndroidMA"
include ':app'
```

---

### 7. **gradle.properties**

```properties
# add these
reactNativeArchitectures=armeabi-v7a,arm64-v8a,x86,x86_64
newArchEnabled=true
hermesEnabled=true
```

---

## Native Android Integration

### 1. **MyReactActivity.kt** - React Native Activity

Extends `ReactActivity` to host React Native components.

```kotlin
import android.os.Bundle
import com.facebook.react.ReactActivity
import com.facebook.react.ReactActivityDelegate
import com.facebook.react.defaults.DefaultNewArchitectureEntryPoint.fabricEnabled
import com.facebook.react.defaults.DefaultReactActivityDelegate


class MyReactActivity : ReactActivity() {

    override fun getMainComponentName(): String = "ManageAppHost"

    override fun createReactActivityDelegate(): ReactActivityDelegate {
        return object : DefaultReactActivityDelegate(
            this,
            mainComponentName,
            fabricEnabled
        ) {
            override fun getLaunchOptions(): Bundle {
                val initialProps = Bundle()
                initialProps.putString("accessToken", "...")
                return initialProps
            }
        }
    }
}
```

**Key Methods:**

- `getMainComponentName()`: Returns the root React component name ("Manage"). This should return the same name mentioned in `AppRegistry.registerComponent("Manage", () => {})` in **index.js**
- `getLaunchOptions()`: Passes initial props to React component
- `createReactActivityDelegate()`: Customizes React Native initialization

**Props Passed:**

- `accesstoken`: Token needed for authentication in Manage App
- Can add additional props as needed

---

### 2. **Native Modules** - Bridge Communication

#### ReactActivityModule.kt

Provides native functionality accessible from React.

```kotlin
import com.facebook.react.bridge.ReactApplicationContext
import com.facebook.react.bridge.ReactContextBaseJavaModule
import com.facebook.react.bridge.ReactMethod

class ReactActivityModule(private val reactContext: ReactApplicationContext)
    : ReactContextBaseJavaModule(reactContext) {

    override fun getName() = "ReactActivityModule"

    @ReactMethod
    fun closeReactNative() {
        reactContext.currentActivity?.finish()
    }
}
```

**Features:**

- `closeReactNative()`: Closes the React Native Activity and returns to native app

---

#### ReactActivityPackage.kt

Registers the native module with React Native.

```kotlin
import android.view.View
import com.facebook.react.ReactPackage
import com.facebook.react.bridge.NativeModule
import com.facebook.react.bridge.ReactApplicationContext
import com.facebook.react.uimanager.ReactShadowNode
import com.facebook.react.uimanager.ViewManager

class ReactActivityPackage: ReactPackage {
    override fun createNativeModules(
        reactContext: ReactApplicationContext
    ): MutableList<NativeModule> =
        listOf(ReactActivityModule(reactContext)).toMutableList()

    override fun createViewManagers(
        reactContext: ReactApplicationContext
    ): MutableList<ViewManager<View, ReactShadowNode<*>>> = mutableListOf()
}
```

---

### 3. **MainApplication.kt** - React Application Host

Configures the React Native runtime and packages.

```kotlin
import android.app.Application
import com.facebook.react.ReactApplication
import com.facebook.react.ReactHost
import com.facebook.react.defaults.DefaultReactHost.getDefaultReactHost
import com.facebook.react.PackageList
import com.facebook.react.ReactNativeApplicationEntryPoint.loadReactNative

class MainApplication : Application(), ReactApplication {

    override val reactHost: ReactHost by lazy {
        getDefaultReactHost(
            context = applicationContext,
            packageList = PackageList(this).packages.apply {
                // Add custom packages here
                add(ReactActivityPackage())
            },
        )
    }

    override fun onCreate() {
        super.onCreate()
        loadReactNative(this)
    }
}
```

**Responsibilities:**

- Creates and manages the React Native host
- Registers native modules and packages
- Initializes React Native runtime

---

### 4. **AndroidManifest.xml** - Android Manifest file

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <!-- Add internet permission -->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Set the application name as the application file name created  -->
    <!-- also set usesCleartextTraffic to true -->
    <application
        android:name=".MainApplication"
        android:usesCleartextTraffic="true"
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.TestIntegration">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- Register the React activity created -->
        <activity android:name=".MyReactActivity"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>
    </application>

</manifest>
```

---

## React Native Components

### 1. **index.js** - Main Entry Point

Root React component that initializes the app.

```javascript
import React, { useEffect, useState } from "react";
import { AppRegistry, NativeModules, StyleSheet, View } from "react-native";
import ManageAppRoot, { ManageAppConstants } from "tp-freemium-react-native";

const { ReactActivityModule } = NativeModules;

const ManageAppHost = ({ accessToken }) => {
  const [isReady, setIsReady] = useState(false);

  // Initialize ManageApp SDK
  useEffect(() => {
    if (!accessToken) {
      console.warn("[ManageApp SDK] Missing accessToken prop");
      return;
    }
    ManageAppConstants.setAccesstoken(accessToken);
    setIsReady(true);
  }, [accessToken]);

  // Close React Native Activity
  const handleClose = () => {
    try {
      ManageAppConstants.resetManageApp();
      ReactActivityModule?.closeReactNative?.();
    } catch (error) {
      console.error("[ManageApp SDK] Failed to close", error);
    }
  };

  return (
    <View style={styles.container}>
      {isReady && (
        <View style={styles.sdkContainer}>
          <ManageAppRoot onClose={handleClose} />
        </View>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#000",
    justifyContent: "center",
  },
  sdkContainer: {
    flex: 1,
  },
});

AppRegistry.registerComponent("ManageAppHost", () => Manage);
```

**Key Features:**

- Accepts `accessToken` prop from native layer
- Initializes tp-freemium SDK (Manage App SDK)
- Provides close functionality to return to native app

---

## Build & Run

### Development

#### 1. Start Metro Bundler

Open a terminal and run:

```bash
yarn react-native run-android
```

The Metro bundler will start on port 8081 by default.

---

## Points to Remember

### If you want to launch Manage App SDK in app while development:

- The dev server has to be running (start the server by running `yarn react-native run-android`)
- The android project has to be build using the command mentioned in the point above

### Integration Details

This integration was done by the default versions set by Android Studio while creating a new android project:

- **Android Studio version:** Android Studio Narwhal Feature Drop | 2025.1.2 Patch 1

---

## Resources

- [React Native Official Documentation](https://reactnative.dev)
- [Integrating with Existing Apps](https://reactnative.dev/docs/integration-with-existing-apps)
- [React Native Bridge Documentation](https://reactnative.dev/docs/native-modules-android)
- [Android Developers Guide](https://developer.android.com)
- [Gradle Documentation](https://gradle.org/releases)

---
