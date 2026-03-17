# Android Build Process in Expo

You are able to make an Android build for this project because it is an **Expo** managed project. Expo abstracts away the complex native Android (Java/Kotlin) and iOS (Objective-C/Swift) code, allowing you to write your app in JavaScript/TypeScript using React Native.

Here is an explanation of the process and how it works:

## Why you can build for Android
1. **React Native**: Your app uses React Native, a framework that translates your JavaScript components into native Android UI components.
2. **Expo CLI**: Expo provides powerful tools to either run your app in the "Expo Go" client app or compile it into a standalone `.apk` or `.aab` file.
3. **No `android` folder required**: In a managed Expo project (like this one), you don't see an `android/` or `ios/` folder. Expo generates these native folders dynamically when you build the app.

## The Android Build Process
There are three main ways to build and run this app on Android:

### 1. Development (Expo Go)
- Run `npm run android` or `npx expo start --android`.
- This starts a local development server (Metro bundler).
- You can open the Expo Go app on your Android phone, scan the QR code, and your JavaScript bundle is downloaded and rendered instantly.

### 2. Local Native Build (expo run:android)
- Run `npx expo run:android`.
- This process runs "Prebuild", which automatically generates the `android/` folder containing the native Android Studio project based on your `app.json` configuration.
- It then uses Gradle (the Android build system) to compile the Java/Kotlin code and packages your JavaScript bundle into a debug `.apk`.
- Finally, it installs it on your connected emulator or device.

### 3. Cloud Build (EAS Build)
- Run `npm install -g eas-cli` and then `eas build -p android`.
- Expo Application Services (EAS) zips your project and sends it to Expo's cloud servers.
- The cloud servers run the Prebuild step, compile the Android app using Gradle, and provide you with a download link for the `.aab` (for Google Play Store) or `.apk` (for direct installation).

## Performance and Smoothness
React Native uses a "Bridge" (or the New Architecture with JSI) to communicate between your JavaScript code and the native Android UI thread.
To keep the app smooth:
- **Use the Native Driver**: When using animations, always set `useNativeDriver: true`. This offloads the animation work from the JavaScript thread to the native UI thread, preventing frame drops.
- **Avoid heavy work on the main thread**: Keep your React renders fast and avoid running complex logic during animations.
- **New Architecture**: Your `app.json` has `"newArchEnabled": true`, which enables React Native's New Architecture. This eliminates the asynchronous bridge, allowing synchronous and highly performant communication between JS and Native, leading to a much smoother experience.
