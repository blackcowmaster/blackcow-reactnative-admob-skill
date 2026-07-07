# Mediation And Native Setup

Use this reference for bare React Native CLI apps that already use
`react-native-google-mobile-ads` and need mediation, native project checks, or
release/debug proof. It is sourced from `.omo/evidence/task-1-source-refresh.md`
and the official Google/Invertase docs listed there; re-check those docs before
changing native setup, privacy, mediation, or SKAdNetwork guidance.

## Boundary

Mediation is an ad-source routing and diagnostics concern, not a replacement for
normal ad-format integration. Integrate the target format first, keep ad unit IDs
in runtime config, and use AdMob console or partner dashboards only when the user
explicitly asks for account mutation.

Do not paste a static adapter dependency matrix here. Each mediation partner has
current adapter setup steps, minimum SDK requirements, privacy settings, and
platform-specific notes. Fetch the partner's current Google mediation page during
implementation, then wire only the partner(s) the app actually uses.

Do not paste a static SKAdNetwork list here. For iOS attribution, fetch the
current Google and partner docs during release work and update `Info.plist` from
those sources.

## React Native Surface

- Confirm root `app.json` has the bare RN
  `react-native-google-mobile-ads` Android and iOS App IDs for shipped
  platforms.
- Call `mobileAds().initialize()` once at app launch before loading ads; with
  mediation, wait for the initialization promise to settle and log adapter
  statuses for release/debug evidence.
- Expose a debug-only path to `MobileAds().openAdInspector()` or equivalent app
  diagnostics. Ad Inspector is for configured test devices and should not become
  a public user feature.
- Keep mediation partner choices out of JavaScript constants unless they are
  part of app telemetry. The authoritative waterfall/bidding setup lives in
  AdMob and partner configuration.

## Android mediation checklist

- `AndroidManifest.xml` / AndroidManifest metadata:
  - Verify the Google Mobile Ads App ID is present through the project's current
    RN config path.
  - For legacy SDK compatibility work, check whether `OPTIMIZE_INITIALIZATION`
    and `OPTIMIZE_AD_LOADING` metadata is still needed; current Google docs say
    those flags are default-enabled for newer legacy GMA SDK versions.
- Gradle:
  - Inspect `android/build.gradle`, `android/app/build.gradle`, and version
    catalogs for existing Google Mobile Ads, Kotlin, Android Gradle plugin, and
    mediation partner dependency patterns.
  - Add only the current adapter dependency required by the selected partner
    docs; do not copy an adapter version from this skill.
  - Rebuild Android after native dependency or manifest changes.
- Activity context caveat:
  - Some mediated networks require an `Activity` context for ad objects. If an
    RN bridge, custom native module, or view manager creates ad objects, ensure
    it uses the current Activity when required and handles the null-Activity
    case without crashing.
- Banner refresh:
  - When AdMob controls banner refresh for a mediated banner unit, disable
    third-party banner refresh in partner dashboards to avoid double refresh.
- Native ads:
  - For mediated native ads, follow the selected partner policy and current
    Google guidance for mediated native loading; do not assume every Google-only
    native helper applies to mediation.
- Privacy:
  - Add mediation partners to the relevant AdMob Privacy & messaging partner
    lists for GDPR and US states privacy flows when those laws apply.

## iOS mediation checklist

- `Info.plist`:
  - Confirm the iOS AdMob App ID reaches native config through the project's
    supported path.
  - For ATT, use a clear `NSUserTrackingUsageDescription` only when the app
    requests tracking authorization.
  - For SKAdNetwork, add provider entries from current Google and partner docs
    during release work. Keep the provider values in `SKAdNetworkIdentifier`.
- Podfile / CocoaPods:
  - Inspect `ios/Podfile`, `Podfile.lock`, and generated pods before adding a
    partner SDK.
  - Add only the current pod or adapter instructions from the selected partner
    docs; do not copy a pod version from this skill.
  - Run the project's pod install command and rebuild iOS after changes.
- Initialization:
  - Initialize Google Mobile Ads explicitly before loading ads. With mediation,
    wait for initialization completion so partner adapters can participate in
    the first request.
- Response info:
  - Capture the winning ad network from `GADResponseInfo` or the RN-exposed
    response info path when available, and include it in debug/release evidence.
- Privacy:
  - Match App Store privacy labels and consent copy to all ad and mediation
    partners actually enabled for the app.

## Adapter Freshness

Before implementing or reviewing a partner:

- Fetch the current Google mediation page for that partner and platform.
- Check the partner SDK, adapter, repository, minimum OS, and privacy notes.
- Check whether the partner needs extra manifest entries, Gradle repositories,
  CocoaPods sources, `Info.plist` keys, SKAdNetwork entries, or dashboard setup.
- Record the source date and URL in the task evidence.
- Avoid broad dependency updates unless the partner docs or build failure
  requires them.

## Initialization And Debug Evidence

Collect evidence from the real app or a test build, not from keyword presence:

- SDK initialization completed or timed out in a controlled way.
- Adapter status output was captured after initialization.
- `ResponseInfo` was captured for at least one test request when the format
  exposes it.
- Ad Inspector opened on a configured test device and was used to verify
  adapter integrations, ad unit tests, and privacy troubleshooting.
- Any unavailable evidence states the exact blocker, such as no device,
  no native build, no selected mediation partner, or no account access.

## Release Privacy Rule

Mediation expands the disclosure surface. Before release, list every enabled ad
source and SDK partner, then verify:

- AdMob Privacy & messaging partner disclosure includes those partners where
  required.
- UMP or other consent flow can request/update consent before ad requests.
- Play data safety and App Store privacy labels match SDK behavior.
- iOS SKAdNetwork entries are refreshed from current Google and partner docs.
- No production request uses debug-only test-device or consent settings.
