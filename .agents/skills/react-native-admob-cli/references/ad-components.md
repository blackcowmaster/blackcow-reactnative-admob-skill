# Ad Components And Runtime Patterns

Use this reference for RN TypeScript ad units, banners, interstitials, rewarded
ads, and app-open ads.

For preload/reload state, cooldowns, one-shot full-screen instances, and
app-open age checks, use `references/loading-lifecycle.md`. For format choice,
frequency caps, floors, and revenue experiments, use
`references/monetization-strategy.md`.

## Runtime Config

Keep AdMob App IDs and ad unit IDs separate:

- App IDs: native/app config in root `app.json`.
- Ad unit IDs: runtime config module, environment config, or remote config.

Use test IDs in development:

```tsx
import { TestIds } from "react-native-google-mobile-ads";

export const adUnits = {
  banner: __DEV__ ? TestIds.BANNER : "ca-app-pub-xxxxx/yyyyy",
  interstitial: __DEV__ ? TestIds.INTERSTITIAL : "ca-app-pub-xxxxx/yyyyy",
  rewarded: __DEV__ ? TestIds.REWARDED : "ca-app-pub-xxxxx/yyyyy",
  appOpen: __DEV__ ? TestIds.APP_OPEN : "ca-app-pub-xxxxx/yyyyy",
};
```

App IDs and ad unit IDs are not passwords, but centralizing them makes debug/prod
switching and review safer.

## Safe Banner

```tsx
import { useState } from "react";
import { View } from "react-native";
import {
  BannerAd,
  BannerAdSize,
} from "react-native-google-mobile-ads";
import { adUnits } from "./adUnits";

export function SafeBannerAd() {
  const [failed, setFailed] = useState(false);
  if (failed) return null;

  return (
    <View
      collapsable={false}
      accessibilityElementsHidden
      importantForAccessibility="no-hide-descendants"
    >
      <BannerAd
        unitId={adUnits.banner}
        size={BannerAdSize.ANCHORED_ADAPTIVE_BANNER}
        onAdFailedToLoad={() => setFailed(true)}
      />
    </View>
  );
}
```

Notes:

- Use anchored adaptive banners for fixed top/bottom banners.
- Use inline adaptive banners as list items for feeds.
- On iOS, check current docs for foreground reload behavior if banners return
  empty after app suspension.
- If a banner fails to load, collapse it or reserve a deliberate placeholder;
  never let layout jump into a tap target.

## Interstitials

- Show only at natural transitions.
- Preload, then show only after loaded.
- Unsubscribe event listeners on unmount.
- Continue the app flow when loading/showing fails.
- On iOS, ensure status bar handling does not make the close button unreachable.

## Rewarded Ads

- User must intentionally choose to watch.
- Grant rewards only after the earned-reward event.
- Handle duplicate reward callbacks defensively in app state/server logic.
- For valuable rewards, inspect server-side verification support.

## App Open Ads

Use app-open ads sparingly. They are for app load/foreground moments, not random
interruptions.

Rules:

- Show from a loading/waiting state, not after the user reaches main content.
- Do not show the first app-open ad before the user has used the app enough to
  understand it.
- Track load time; Google guidance says app-open ads expire after four hours.
- Do not show if another full-screen ad is showing.
- If no ad is ready, continue startup normally and preload for the next chance.

## Manager Guardrails

Keep full-screen ad managers small and explicit:

- Gate all load calls behind consent, request configuration, and
  `mobileAds().initialize()` completion or intentional settlement.
- Track `idle`, `loading`, `loaded`, `showing`, `closed`, and `failed` states;
  UI code should receive a skipped result instead of guessing readiness.
- Treat each interstitial, rewarded, and app-open instance as one-shot. Clear
  listeners and create a fresh instance after close or show failure.
- Run `canShow(context)` before every show attempt: loaded state, safe route,
  no other full-screen ad, cooldown, frequency gate, and app-open age.
- Log format, ad unit alias, state, trigger, skip reason, load age, failure
  count, and `ResponseInfo` when the library exposes it.
- Keep monetization decisions in an experiment record. Do not hardcode a format
  as universally best; measure product moment, user intent, eCPM, fill/match,
  show rate, ARPDAU, retention, and complaints.

## Optional Ads

Treat ads as an optional monetization layer:

- log initialization failures
- gate rendering behind consent/init/module availability
- never throw during root app startup because an ad dependency is unavailable
