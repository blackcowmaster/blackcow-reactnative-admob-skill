# Privacy, Consent, ATT, And Initialization

Use this reference when adding or auditing consent, request configuration, ATT,
or SDK startup.

## Freshness Rule

Before editing privacy-sensitive code, check current Invertase
`react-native-google-mobile-ads` docs and Google UMP docs. UMP, ATT, and
platform privacy requirements change more often than ordinary component code.

## Ordering

Do not load ads until the app has settled consent and initialized the SDK.

Preferred order:

1. Configure request flags and test devices when needed.
2. Request UMP consent info on every app launch.
3. Present the consent form if required.
4. Check `canRequestAds`.
5. On iOS, request ATT if the app's privacy flow includes it.
6. Call `mobileAds().initialize()`.
7. Load ads only after ads can be requested and initialization has settled.

Set `delay_app_measurement_init: true` in app config when app measurement must
wait for consent.

## Consent State

- Do not permanently persist consent state as if it were static. Consent
  eligibility and privacy options can change between launches.
- UMP `canRequestAds` is false until consent info has been requested.
- Guard against duplicate ad loading: both the post-update and post-form paths
  can produce `canRequestAds: true`.
- If consent gathering errors, current Google UMP guidance allows checking
  whether prior-session consent still permits ad requests.

## RN Initialization Pattern

Keep startup idempotent:

```tsx
import mobileAds, {
  AdsConsent,
  AdsConsentStatus,
} from "react-native-google-mobile-ads";

let initializing: Promise<boolean> | null = null;

export function initializeAdsOnce() {
  if (!initializing) initializing = initializeAds();
  return initializing;
}

async function initializeAds() {
  const consentInfo = await AdsConsent.requestInfoUpdate();

  if (
    consentInfo.isConsentFormAvailable &&
    consentInfo.status === AdsConsentStatus.REQUIRED
  ) {
    await AdsConsent.loadAndShowConsentFormIfRequired();
  }

  const { canRequestAds } = await AdsConsent.getConsentInfo();
  if (!canRequestAds) return false;

  await mobileAds().initialize();
  return true;
}
```

Adapt to the current library docs if `gatherConsent()` is the better fit for the
app. Keep ads optional; a consent or initialization failure should not block
core app startup.

## Test Consent

For development, use UMP debug geography and test device identifiers when the
current docs require them. Remove hardcoded test consent devices before release
unless the project already gates them to debug builds.

## iOS ATT

Use the project's existing permissions stack. In bare RN, prefer an existing
native/RN permission library over adding Expo tracking APIs.

When ATT is required:

- ensure `NSUserTrackingUsageDescription` or the library's supported app config
  equivalent is present
- request ATT before ad loading
- wait for the completion callback before loading ads
- do not assume ATT is always promptable; device/account restrictions can
  suppress the prompt

## Privacy Options

If UMP says privacy options are required, surface an entry point in settings or
another stable app location. Do not hide the only privacy-options entry behind
an ad component.
