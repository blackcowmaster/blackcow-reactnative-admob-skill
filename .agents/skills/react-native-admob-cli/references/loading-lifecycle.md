# Loading Lifecycle

Use this reference when an RN CLI app needs interstitial, rewarded, or app-open
ads to load ahead of time, show only when ready, recover after failures, and
stay out of the core flow.

## Source Basis

This file is derived from `.omo/evidence/task-1-source-refresh.md`, retrieved
2026-07-07. Do not update lifecycle code from memory; re-check the current
Google and Invertase docs before changing native behavior or library APIs.

Official source mappings used here:

- Google app-open ads: preload/availability management, foreground lifecycle
  observer pattern, cold start loading screen limits, main-content avoidance,
  and app-open ad expiry after four hours.
- Invertase `react-native-google-mobile-ads`: initialize before loading ads,
  use full-screen ad event listeners for load/show/close/error patterns, and
  use test IDs in development.
- Google frequency caps: frequency caps limit impressions per user over a time
  window and can take time to apply, so local cooldown logic should complement
  console-side caps.
- Google UMP and Invertase consent docs: ad requests should wait until consent
  and SDK initialization allow ads, but ad failure must not block core flow.

## Lifecycle Rules

- Use preload-before-show for every full-screen format. Create and load the ad
  before the product moment where it might be shown.
- Treat interstitial, rewarded, and app-open full-screen ad objects as one-shot
  instances. After a show attempt reaches closed or failed terminal handling,
  discard the instance and create a fresh one before the next load.
- Track explicit loading, loaded, showing, closed, and failed states instead of
  asking UI code to infer readiness from callbacks.
- Never call show before load has completed. If the state is not loaded, skip
  the impression opportunity and continue the user action.
- On closed, reload for the next eligible opportunity after the app has returned
  to normal UI state.
- On failed load or failed show, record the error class and reload with backoff.
  Repeated failures should increase delay and eventually pause until the next
  session, network recovery, or remote-config refresh.
- For app-open ads, store the load timestamp and reject stale ads; Google app
  open guidance maps expiry to four hours.
- For app-open cold start behavior, show only while the app is still on a
  loading screen or equivalent waiting surface. Do not show after main content
  is visible.
- Use a local cooldown and frequency gate before showing. Console frequency
  caps are still required for production policy and revenue control, but local
  gates protect UX immediately.
- Ads are optional. Do not block startup, navigation, login, checkout,
  messaging, rewards reconciliation, or any other core flow because an ad is
  loading, failed, or unavailable.

## State Machine

| State | Trigger | Allowed action | Next state | Implementation note |
| --- | --- | --- | --- | --- |
| idle | consent and SDK initialization allow ads | create a new one-shot instance and call `load()` | loading | Exit early if ads are disabled, consent blocks requests, or cooldown/frequency gate says no. |
| loading | load succeeds | mark load timestamp and hold the instance | loaded | UI may now ask the manager to show at an eligible product moment. |
| loading | load fails | record error and schedule retry | failed | Apply backoff; do not block core flow while waiting. |
| loaded | product moment occurs and gates pass | call `show()` | showing | Never show before load; if app-open is older than four hours, discard and reload. |
| loaded | product moment occurs and gates fail | keep or discard by format policy | loaded | Cooldown, frequency cap, current route, or foreground state can skip the opportunity. |
| loaded | app-open load age exceeds four hours | destroy stale instance and create a fresh one | loading | A stale app-open ad should not be shown. |
| showing | full-screen ad closes | clear instance and record last-show timestamp | closed | Resume the original user flow, then reload for the next opportunity. |
| showing | show fails | clear instance and schedule retry | failed | Continue the app action as if no ad was available. |
| closed | post-close cleanup completes | create and load the next one-shot instance | loading | Avoid immediate re-show; cooldown must still apply. |
| failed | backoff delay expires | create and load a new one-shot instance | loading | Increase backoff for repeated failures; reset after a successful load/show cycle. |

## Manager Checklist

An RN CLI agent can implement a small manager per full-screen format:

- Keep module-level or provider state: `state`, `instance`, `loadStartedAt`,
  `loadedAt`, `lastShownAt`, `failureCount`, and listener unsubscribe handles.
- Expose `preload(reason)`, `canShow(context)`, `showIfReady(context)`,
  `markClosed()`, `markFailed(error)`, and `dispose()` operations.
- Start preload only after consent, request configuration, and
  `mobileAds().initialize()` have settled successfully.
- Register listeners immediately after creating the ad object: loaded, closed,
  failed-to-load, failed-to-show, and earned-reward for rewarded ads.
- In `canShow(context)`, check loaded state, one-shot instance presence, current
  screen safety, no other full-screen ad showing, cooldown, frequency gate, and
  app-open age.
- In `showIfReady(context)`, return a structured skipped result instead of
  throwing when the ad is loading, failed, blocked by cooldown, or stale.
- For rewarded ads, keep reward granting separate from closed handling. Grant
  only from the earned-reward event and make duplicate callbacks idempotent.
- For app-open ads during cold start, coordinate with the app's loading screen:
  if main content has rendered, skip the show and continue normally.
- After closed or failed, always unregister listeners, clear the one-shot
  instance, and schedule reload with the manager's current backoff policy.
- Log enough fields for debugging: format, unit id alias, state, trigger,
  skip reason, load age, failure count, and response info when available.

## Backoff And Frequency

Use simple, bounded retry behavior:

- first failure: retry after a short delay such as 30 seconds
- repeated failures: grow backoff up to a session-level ceiling
- network/offline signal: pause loading until connectivity returns
- repeated no-fill or policy-like errors: avoid tight loops and wait for the
  next app foreground or remote-config interval

Keep show gates separate from load gates:

- Load opportunistically after consent/init so an ad is ready later.
- Show only at safe product moments such as a completed level, saved draft,
  post-result screen, or explicit rewarded opt-in.
- Enforce cooldown after every show attempt that reaches showing.
- Respect AdMob console frequency caps and local frequency logic; do not treat
  either one as a substitute for the other.

## App-Open Specifics

- App-open preload should happen before the next foreground opportunity, not
  during arbitrary in-app interactions.
- During cold start, the only acceptable surface is a loading screen or similar
  waiting state.
- If the app has already displayed main content, skip the app-open ad and
  preload for the next eligible foreground event.
- If no ad is ready, continue startup normally. The user should never wait for
  ad loading when the app is otherwise ready.
- If an app-open ad was loaded more than four hours ago, discard it and reload.

## Do-Not Warnings

- Do not show before load; a not-ready ad is a skipped opportunity, not an app
  error.
- Do not block startup or a core flow on ad initialization, loading, show, close,
  or failure.
- Do not show after main content is visible during app-open cold start handling.
- Do not keep reusing a full-screen instance after it has been shown, closed, or
  failed.
- Do not spin retry loops without backoff; this produces noisy logs and poor
  user experience without improving fill.
