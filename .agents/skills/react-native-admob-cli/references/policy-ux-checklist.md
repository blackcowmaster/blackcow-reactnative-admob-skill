# RN Policy And UX Checklist

Use this reference when placing or reviewing ads in React Native UI. Borrow
policy principles from `admob-ux-best-practices`, but translate implementation
to RN components and navigation patterns.

## Good Placements

- Top banner below a stable header.
- Bottom anchored banner only when separated from tab bars, gesture areas, and
  high-frequency buttons.
- Inline feed/list banner as a `FlatList` or section item.
- Interstitial after a completed flow, level, or explicit navigation transition.
- Rewarded ad behind a clear user action and value exchange.

## Avoid

- Near `TextInput`, send buttons, destructive buttons, tab buttons, or primary
  CTAs.
- In forms, checkout, login, settings, permission prompts, or error recovery.
- During typing, dragging, swiping, gameplay, loading spinners, or splash screens.
- As sticky overlays that cover app content.
- Stacking banner and full-screen ads in the same moment.

## RN Layout Checks

- Respect safe areas on notched and gesture-navigation devices.
- Reserve stable banner height or collapse intentionally on failure.
- Verify touch targets around the ad cannot cause accidental clicks.
- In feeds, render ads as data items with stable keys.
- In chat, place ads away from composer controls and keyboard transitions.
- In games or high-tap screens, omit banners.

## Review Questions

- Could a normal thumb path accidentally tap the ad?
- Does the ad appear immediately after a high-frequency action?
- Does the ad move content while the user is tapping?
- Is the close button reachable on iOS full-screen ads?
- Does a failed ad leave the screen usable?
- Does the user have a non-ad path to continue?
