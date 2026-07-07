# RN CLI Setup

Use this reference for bare React Native CLI projects only.

## Detect The App Shell

- Bare RN CLI: `package.json` has `react-native`, and both `ios/` and `android/`
  exist.
- Expo-managed: `app.json`/`app.config.*` has an `expo` object, EAS controls the
  app shell, or the project depends on Expo config plugins for native changes.
- Hybrid/bare with Expo modules: inspect native folders and existing config style
  before choosing. Prefer the project's existing native-config pattern.

Stop and reroute if the task is actually Expo, Flutter, AdMob console
automation, or store deployment.

## Package Manager

Respect the existing project:

- `pnpm-lock.yaml` -> `pnpm add react-native-google-mobile-ads`
- `yarn.lock` -> `yarn add react-native-google-mobile-ads`
- `bun.lockb` or `bun.lock` -> `bun add react-native-google-mobile-ads`
- otherwise -> `npm install react-native-google-mobile-ads`

Do not switch package managers while adding ads.

## Native App IDs

Use current Invertase docs as the source of truth before changing native config.
For bare RN, App IDs live in root `app.json` under
`react-native-google-mobile-ads`:

```json
{
  "name": "AppName",
  "displayName": "App Name",
  "react-native-google-mobile-ads": {
    "android_app_id": "ca-app-pub-xxxxxxxxxxxxxxxx~yyyyyyyyyyyy",
    "ios_app_id": "ca-app-pub-xxxxxxxxxxxxxxxx~zzzzzzzzzz",
    "delay_app_measurement_init": true
  }
}
```

Rules:

- Preserve existing `name`, `displayName`, and unrelated app config.
- Do not put bare RN IDs under `expo.plugins`.
- Configure only the platforms the app ships.
- Missing or invalid App IDs can crash at startup or fail native builds.
- Rebuild native projects after app ID or measurement-delay changes.

## iOS ATT Description

If ATT is implemented, ensure the app has a tracking usage description through
the project's current config path. With `react-native-google-mobile-ads`, check
the current docs for the supported `app.json` key; otherwise inspect
`Info.plist`.

Use a user-facing message that describes why tracking is requested. Do not add
ATT only because ads are present; match the app's privacy posture and legal
requirements.

## Rebuild And Native Checks

After dependency or app ID changes:

```bash
npx pod-install
npx react-native run-ios
npx react-native run-android
```

Use the project's actual build commands when they differ.

Check:

- `package.json` includes `react-native-google-mobile-ads`.
- root `app.json` has the needed platform App IDs.
- Android package/application ID matches the AdMob Android app.
- iOS bundle ID matches the AdMob iOS app.
- pods are installed after iOS dependency/config changes.
- native builds are rerun after app ID or measurement-delay changes.
