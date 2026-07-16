# React Native Android Build & Publish

A GitHub composite action that automates building a React Native Android application and publishing it to the Google Play Store.

## Usage

Create a workflow file (e.g., `.github/workflows/deploy.yml`) in your React Native project repository:

```yaml
name: Deploy Android App

on:
  push:
    branches:
      - main # or your default branch

jobs:
  build-and-publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Build and Publish App
        uses: georgechitechi/android-packager@v1
        with:
          # Required Inputs
          keystore-base64: ${{ secrets.ANDROID_KEYSTORE_BASE64 }}
          keystore-password: ${{ secrets.ANDROID_KEYSTORE_PASSWORD }}
          key-alias: ${{ secrets.ANDROID_KEY_ALIAS }}
          key-password: ${{ secrets.ANDROID_KEY_PASSWORD }}
          service-account-json: ${{ secrets.PLAY_CONSOLE_JSON }}
          package-name: 'com.yourcompany.app'
          
          # Optional Inputs
          # working-directory: '.'
          # node-version: '18'
          # java-version: '17'
          # track: 'internal' # Options: internal, alpha, beta, production
          # build-type: 'aab' # Options: aab, apk
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `working-directory` | The directory where the React Native app is located | No | `.` |
| `node-version` | Node.js version to use | No | `18` |
| `java-version` | Java version to use (e.g. 17) | No | `17` |
| `keystore-base64` | Base64 encoded Android keystore | **Yes** | |
| `keystore-password` | Password for the keystore | **Yes** | |
| `key-alias` | Alias for the key | **Yes** | |
| `key-password` | Password for the key | **Yes** | |
| `service-account-json`| Google Play Console service account JSON | **Yes** | |
| `package-name` | The Android application package name | **Yes** | |
| `track` | The Google Play track (`internal`, `alpha`, `beta`, `production`) | No | `internal` |
| `build-type` | Build type: `aab` or `apk` | No | `aab` |

## Setup Instructions

1. **Keystore Base64**: Generate a release keystore for Android and encode it to base64. 
   ```bash
   base64 release.keystore > release.keystore.base64
   ```
   Copy the contents and add it as a GitHub Secret (`ANDROID_KEYSTORE_BASE64`).

2. **Google Play Service Account**: Create a service account in your Google Cloud Console, give it permissions in Google Play Console, download the JSON key, and add the entire JSON file contents as a GitHub Secret (`PLAY_CONSOLE_JSON`).

3. **Other Secrets**: Make sure to add `ANDROID_KEYSTORE_PASSWORD`, `ANDROID_KEY_ALIAS`, and `ANDROID_KEY_PASSWORD` as GitHub Secrets.
