# Troubleshooting

Use this when ads do not load, builds fail after setup, or ad behavior is
unclear.

## Startup Crash Or Native Build Failure

Check:

- root `app.json` has valid platform App IDs under
  `react-native-google-mobile-ads`
- the app was rebuilt after changing App IDs or `delay_app_measurement_init`
- iOS pods were installed
- Android package/application ID and iOS bundle ID match the AdMob app
- Expo plugin config was not accidentally used in a bare RN CLI app

## Ads Not Showing

Common causes:

- AdMob account or app still pending approval.
- New app/ad unit has not warmed up.
- No inventory for the selected format/size.
- Wrong Android `applicationId` or iOS bundle ID.
- Test device not registered when using production units in development.
- Private DNS, VPN, emulator network, or content blocker prevents ad requests.
- app-ads.txt is missing or incorrect.
- Consent flow returns `canRequestAds: false`.

Use test IDs first. If test ads load but production ads do not, treat it as an
AdMob/account/config problem, not a component rendering problem.

## Consent Problems

- Call consent info update before checking `canRequestAds`.
- Do not rely on a permanently saved consent decision.
- Avoid duplicate loads when two consent callbacks both allow ads.
- Use debug geography only on test devices and only in debug paths.
- Surface privacy options when UMP says they are required.

## iOS UI Problems

- Empty banner after background/foreground can require the current documented
  foreground reload pattern.
- Interstitial close button can become unreachable if status bar/safe-area
  handling is wrong.
- ATT prompt can be unavailable because of system/account restrictions.
- Missing `NSUserTrackingUsageDescription` can break ATT review/runtime behavior.

## Runtime Resilience

- Log initialization failures with enough context to debug.
- Collapse or hide failed banners intentionally.
- Never block navigation, login, checkout, messaging, or core app startup because
  ads failed.
- Feature-flag ads when the product needs ad-free builds or regional rollouts.

## Jest And Unit Tests

If tests import modules that use `react-native-google-mobile-ads`, check the
current library testing docs. Register the package-provided Jest setup file
through the project's Jest `setupFiles` config:

```js
// jest.config.js
module.exports = {
  setupFiles: ["./node_modules/react-native-google-mobile-ads/jest.setup.ts"],
};
```

Prefer wiring this through the project's existing Jest config rather than
mocking each ad API ad hoc. If the project uses ESM or TypeScript Jest config,
keep the same `setupFiles` path and adapt only the config syntax.
