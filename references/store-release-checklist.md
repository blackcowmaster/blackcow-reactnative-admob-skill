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
- `SKAdNetworkItems` entries for attribution provider identifiers.
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

## Measurement And Debug Proof

Collect release evidence from a real debug build, simulator/device, CI build, or
static review artifact. Do not use keyword presence as proof.

- ILRD / impression-level revenue: if the app consumes paid event callbacks,
  record the event path, currency/value handling, precision, ad unit alias, and
  analytics destination. If ILRD is not implemented, state that explicitly.
- `ResponseInfo`: capture response info for at least one test request when the
  format exposes it, including adapter/winning-network details when available.
- Ad Inspector: capture proof that Ad Inspector opened on a configured test
  device and checked ad units, adapter status, privacy/consent, and delivery
  troubleshooting.
- Mediation: list each enabled mediation partner and verify disclosure in
  consent messaging, App Store privacy labels, Play data safety, and any partner
  dashboard requirements.
- app-ads.txt: capture the verified publisher-domain status or record why the
  product does not require it for the release.

## ILRD / Debug Evidence Template

Use this template for release-adjacent debug proof.

```md
## Ad release evidence

Build:
- Platform:
- Build variant:
- App ID / bundle ID:
- Ad unit alias:
- Test device:

ILRD / impression-level revenue:
- Implemented: yes/no
- Callback or listener path:
- Fields captured:
- Analytics destination:
- Limitation:

ResponseInfo:
- Format:
- Request timestamp:
- ResponseInfo captured:
- Adapter or winning network:
- Error/no-fill details:

Ad Inspector:
- Opened on configured test device: yes/no
- Ad unit test result:
- Adapter status:
- Privacy/consent status:
- Screenshot/log artifact:

Disclosure:
- mediation partner list:
- app-ads.txt proof:
- App Store privacy labels checked:
- Play data safety checked:
```

## Build And Smoke Proof

Minimum release-adjacent evidence:

- dependency/config diff reviewed
- TypeScript/lint/build passes for touched files
- iOS pods updated if iOS changed
- Android/iOS debug build attempted when available
- test ad load observed or static limitation reported
- no ad placement violates the RN policy checklist

## Release Proof Checklist

- `app.json` and native identifiers match the AdMob apps for every shipped
  platform.
- Production ad unit IDs are centralized and test IDs are not shipped in release
  config.
- Consent, ATT, privacy labels, and data safety answers match the enabled ad SDK
  and mediation partner set.
- app-ads.txt status is captured or a scoped not-applicable reason is recorded.
- ILRD, `ResponseInfo`, and Ad Inspector evidence is captured, or each missing
  item has a concrete blocker such as no device, no selected mediation partner,
  no account access, or no ILRD implementation.
- Evidence artifacts include the exact command, build, device/simulator, binary
  observable, and file path used for the release decision.
