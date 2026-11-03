# Döner Kebab Simulator for Android

A 3D kebab shop game. Shave meat off the spit, fill the pita to match the customer's ticket, and earn tips. The game runs fully offline once installed.

The game itself is `www/index.html`. The `android` folder is a ready Android Studio project that wraps it as an app (built with Capacitor).

You need to build the APK once, using one of the two options below. Then you can share the APK file with friends.

---

## Option A: Build on your PC with Android Studio

1. Install **Android Studio** (free): https://developer.android.com/studio
2. Unzip this folder somewhere, for example `C:\Projects\doner-app`.
3. In Android Studio choose **File → Open** and pick the **`android`** folder inside it (not the top folder).
4. Wait while Android Studio downloads what it needs ("Gradle sync"). The first time takes a few minutes.
5. Get the APK: **Build → Build App Bundle(s) / APK(s) → Build APK(s)**.
   When it finishes, click **locate** in the popup. The file is
   `android/app/build/outputs/apk/debug/app-debug.apk`.

To install straight onto your own phone instead: turn on *Developer options → USB debugging* on the phone, plug it in, and press the green **Run ▶** button.

## Option B: Let GitHub build it (no Android Studio)

1. Create a free account at https://github.com and make a new repository (for example `doner-kebab-simulator`).
2. Upload everything in this folder **except `node_modules`** (drag the files into the repository page and click *Commit changes*). Make sure the hidden `.github` folder is included; if your file browser hides it, use GitHub Desktop or `git` instead.
3. Open the **Actions** tab. The *Build Android APK* job starts by itself and takes about 5 minutes.
4. When it shows a green tick, open the run and download **doner-kebab-simulator-apk** at the bottom. Unzip it to get `doner-kebab-simulator.apk`.

Each time you change the game and upload again, GitHub builds a new APK.

---

## Installing the APK on a phone

1. Send the `.apk` file to the phone (email, Google Drive, Messenger, USB cable).
2. Tap it. Android asks you to allow installing from that app ("Install unknown apps"). Allow it once.
3. Tap **Install**. The game appears as **Döner Sim**.

Google Play Protect may warn that the app is from an unknown developer. That is normal for apps that are not on the Play Store; tap *Install anyway*.

This is a debug build, which is fine for sharing with friends. Publishing on Google Play needs a signed release build and a Play developer account (one-time fee).

## Changing the game

1. Edit `www/index.html`.
2. Install Node.js (https://nodejs.org), then in this folder run:
   ```
   npm install
   npx cap sync android
   ```
3. Build again with option A or B.

## What's in the app version

- Plays in landscape, full screen, and keeps the screen awake.
- Touch controls: hold to shave or pour, slide to aim the knife, tap ingredients in the bar.
- Short vibration while shaving and when serving.
- Remembers your total döners served, tips, and best tip.
- Customers tip less the longer they wait.
- Pauses when you leave the app.
