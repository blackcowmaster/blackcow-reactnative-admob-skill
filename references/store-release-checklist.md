# Store And Release Checklist

Use this reference for release readiness. Do not use it to mutate AdMob,
App Store, Play Console, or store metadata without explicit user approval.

## IDs And Ownership

Check:

- Android package/application ID matches the AdMob Android app.
- iOS bundle ID matches the AdMob iOS app.
- AdMob App IDs are in root app config.
- Ad unit IDs are centralized in runtime config.
- Debug builds use `TestIds`; release builds use production units.
- Production IDs are not exercised on unregistered test devices.

## AdMob Console Boundary

Use `store-admob` only when the user wants account/app/ad-unit provisioning and
approves browser/account mutation. For bare RN CLI, this skill should usually
collect IDs and wire local config rather than automate the console.

## iOS Privacy

Before release, check current Google/Apple guidance for:

- `NSUserTrackingUsageDescription` when ATT is used.
- `SKAdNetworkItems` / `SKAdNetworkIdentifier` entries for attribution.
- App Store privacy labels/data disclosure.
- Mediation partner SKAdNetwork identifiers when mediation is used.

Do not paste a stale SKAdNetwork identifier list from this skill; always fetch
the current Google list.

## Android / Play

Check:

- Play data safety answers match ad SDK behavior.
- app-ads.txt is configured when the product requires it.
- test-device/debug-only consent settings are not shipped to production.
- Android `applicationId` matches the AdMob app configuration.

## Build And Smoke Proof

Minimum release-adjacent evidence:

- dependency/config diff reviewed
- TypeScript/lint/build passes for touched files
- iOS pods updated if iOS changed
- Android/iOS debug build attempted when available
- test ad load observed or static limitation reported
- no ad placement violates the RN policy checklist
