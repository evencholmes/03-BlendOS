# BlendOS v5.0 — Blender Skill Tracker
## Capacitor Android Build Guide

---

### 📦 Step 1 — Install plugins

```bash
npm install @capacitor/core@6 @capacitor/android@6 @capacitor/cli@6
npm install @capacitor/filesystem@6 @capacitor/share@6 @capacitor/app@6 --legacy-peer-deps
```

> Must use `--legacy-peer-deps` for filesystem/share/app or you'll get peer conflict errors.

---

### 🤖 Step 2 — Add Android & sync

```bash
npx cap add android     # first time only
npx cap sync android
```

---

### 🔧 Step 3 — Gradle fixes (after first `cap add android`)

These files are pre-configured in this package but double-check after `cap add android`
regenerates them:

**`android/build.gradle`** — Gradle plugin version:
```groovy
classpath 'com.android.tools.build:gradle:8.3.0'
```

**`android/gradle/wrapper/gradle-wrapper.properties`** — Gradle wrapper:
```
distributionUrl=https\://services.gradle.org/distributions/gradle-8.7-bin.zip
```

**`android/gradle.properties`** — Add these two lines:
```
android.suppressUnsupportedCompileSdk=34
android.defaults.buildfeatures.buildconfig=true
```

**`android/app/src/main/AndroidManifest.xml`** — activity tag needs:
```xml
android:windowSoftInputMode="adjustResize"
```

**`android/app/src/main/res/values/styles.xml`** — Inside `AppTheme.NoActionBar`:
```xml
<item name="android:statusBarColor">#1d1d1d</item>
<item name="android:navigationBarColor">#1d1d1d</item>
<item name="android:windowDrawsSystemBarBackgrounds">true</item>
<item name="android:windowLightStatusBar">false</item>
<item name="android:windowLightNavigationBar">false</item>
```

---

### 🏗️ Step 4 — Build APK

Open Android Studio:
```bash
npx cap open android
```

Then: **Build → Generate Signed Bundle / APK**

For every new build after code changes:
```bash
npx cap sync android
```
Then in Android Studio: **Build → Clean Project → Build → Assemble**

---

### 🔁 Every new build checklist

```bash
npm install @capacitor/core@6 @capacitor/android@6 @capacitor/cli@6
npm install @capacitor/filesystem@6 @capacitor/share@6 @capacitor/app@6 --legacy-peer-deps
npx cap sync android
```
Then Clean → Assemble in Android Studio. 💪

---

### 📱 Features implemented for Capacitor

- ✅ **Export** — uses Filesystem + Share plugin (a.click() is blocked in WebView)
- ✅ **Back button** — modal → search → go home → minimize
- ✅ **Swipe navigation** — uses `function()` not arrow functions (older WebView safe)
- ✅ **Status/nav bar** — matches BlendOS dark theme `#1d1d1d`
- ✅ **Dual storage** — localStorage + IndexedDB (survives all but uninstall)
- ✅ **Keyboard** — adjustResize so keyboard doesn't cover inputs
- ✅ **Offline** — service worker caches all assets

---

### 📂 File structure

```
blendos-capacitor/
├── www/                    ← Capacitor reads this (webDir)
│   ├── index.html          ← Full BlendOS v5 app
│   ├── manifest.json       ← PWA manifest
│   ├── sw.js               ← Service worker
│   ├── icon-192.png
│   └── icon-512.png
├── android/
│   ├── build.gradle        ← Gradle 8.3.0
│   ├── gradle.properties   ← suppressUnsupportedCompileSdk etc
│   ├── gradle/wrapper/     ← Gradle 8.7
│   └── app/src/main/
│       ├── AndroidManifest.xml
│       └── res/
│           ├── mipmap-*/   ← All icon densities (48→192px)
│           └── values/styles.xml
├── capacitor.config.json   ← appId: com.jibunshidai.blendos
├── package.json
└── README.md               ← You are here
```

---

### 👥 Credits
- **Jibunshidai81** — Concept, Vision, Direction
- **Miss Claude** — Engineering (Anthropic)
- **John A. Sherlock** — Chief Complaint Officer 🍼
