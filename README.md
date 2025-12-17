<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://github.com/user-attachments/assets/0aa67016-6eaf-458a-adb2-6e31a0763ed6" />
</div>

# Run and deploy your AI Studio app

This contains everything you need to run your app locally.

View your app in AI Studio: https://ai.studio/apps/drive/1YhlzjUYmstQncoEutncBZgo94h99SEK5

## Run Locally

**Prerequisites:**  Node.js


1. Install dependencies:
   `npm install`
2. Set the `GEMINI_API_KEY` in [.env.local](.env.local) to your Gemini API key
3. Run the app:
   `npm run dev`
```

## Android APK / AAB (Capacitor + CI)

This project can be packaged as an Android app using Capacitor.
A GitHub Actions workflow was added at `.github/workflows/android-build.yml` to:

- Build the web app (Vite)
- Ensure Capacitor Android platform
- Produce a debug APK artifact
- Optionally produce a signed release AAB when signing secrets are provided

### CI signing secrets

If you want CI to produce a signed release AAB, add these repository secrets:
- `KEYSTORE_BASE64` — Base64-encoded keystore file contents (JKS or PKCS12)
- `KEYSTORE_PASSWORD` — Keystore password
- `KEY_ALIAS` — Key alias
- `KEY_PASSWORD` — Key password

The workflow decodes the keystore into `release_keystore/keystore.jks` and passes signing properties to Gradle.

### Run locally (summary)

- Build web assets: `npm run build`
- Copy into Android project: `npx cap copy`
- Open in Android Studio: `npx cap open android`
- CLI debug build: `npm run android:build:debug` (or `cd android && ./gradlew assembleDebug`)

### Build a signed release APK in CI

A separate workflow (`.github/workflows/android-release-apk.yml`) builds a signed release APK on a tag push (v*) or when run manually.

Required signing secrets (set in repository settings):
- `KEYSTORE_BASE64` — Base64-encoded keystore file (JKS/PKCS12)
- `KEYSTORE_PASSWORD` — Keystore password
- `KEY_ALIAS` — Key alias
- `KEY_PASSWORD` — Key password

If secrets are missing the workflow will attempt to build a release APK (unsigned) and upload whatever release APK artifact was produced.

When a tag (v*) is pushed the workflow now also creates a **GitHub Release** and attaches the built APK(s) to the Release for easy downloads by QA or other contributors.
