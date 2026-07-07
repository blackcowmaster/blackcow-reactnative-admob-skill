# Monetization Strategy

Use this reference when choosing AdMob ad formats, planning revenue experiments,
or reviewing monetization tradeoffs in a bare React Native CLI app.

Source basis: `.omo/evidence/task-1-source-refresh.md`, retrieved 2026-07-07.
The mapped official sources support product-moment placement guidance, eCPM
floor tradeoffs, frequency caps, and measurement-first decisions. They do not
support a universal format revenue ranking.

## format-selection Decision Table

| Product moment | Format | Fit | Main risk to monitor |
| --- | --- | --- | --- |
| Persistent utility or browsing screen with stable chrome | Banner, preferably anchored adaptive | Keeps monetization visible without interrupting the task. | Layout shifts, accidental taps, low show rate after failed loads. |
| Feed, list, or content stream where an ad can be a normal item | Native or inline adaptive banner | Matches an in-content placement without forcing a full-screen break. | User complaints, scroll friction, content/ad distinction issues. |
| Level complete, article boundary, checkout cancellation, or other natural transition | Interstitial | Uses an existing pause point instead of interrupting active input. | Retention/churn, frequency cap pressure, show rate drop from poor preload timing. |
| Optional boost, unlock, continue, extra life, or premium-like value exchange | Rewarded | Requires an intentional user choice and ties the ad to a clear reward. | Reward integrity, duplicate reward callbacks, user complaints about value fairness. |
| App launch or foreground return while a loading screen is already visible | App-open | Fits the opening/loading moment when preloaded and not shown after main content. | Startup churn, expired loads, overlapping full-screen ads. |

## Metrics Checklist

Inspect these together before changing placements, caps, or floors:

- `eCPM`: revenue per thousand impressions for the ad unit or cohort.
- fill rate and match rate: whether demand is available after constraints.
- show rate: loaded ads that actually reach users.
- impressions/session: pressure placed on a user session.
- ARPDAU: revenue per daily active user, not just per impression.
- retention/churn: next-day and longer-term behavior after monetization changes.
- user complaints: support tickets, reviews, refund requests, and qualitative
  feedback tied to ad placement.

Do not treat any single metric as sufficient. For example, a floor change can
raise `eCPM` while reducing fill or match rate, and an aggressive placement can
raise impressions/session while hurting retention/churn.

## Frequency Caps

Use a frequency cap when a full-screen or repeated placement can become user
pressure. Set caps at the app or ad-unit level based on the product moment:

- Interstitial: cap around natural transitions, not raw screen views.
- Rewarded: usually cap by reward economy abuse risk, not interruption risk.
- App-open: cap foreground-return opportunities so launch does not feel blocked.
- Banner/native: cap only when repeated impressions create measurable complaint
  or engagement risk.

Record the cap window, scope, and expected user impact before launch. AdMob
frequency caps may take time to apply and app-level caps can interact with
ad-unit caps, so measure after the cap is active rather than during rollout.

## eCPM Floors

Use floors as an experiment lever, not as a default optimization. A higher
floor can filter lower bids, but it can also reduce fill rate and affect match
rate. New or recently changed ad units may need learning time before floor
reporting is stable.

Floor change checklist:

- Define the ad unit, country or cohort, platform, and date window.
- Capture baseline `eCPM`, fill rate, match rate, show rate, ARPDAU, and
  retention/churn.
- Change one floor variable at a time.
- Keep a holdout or comparable cohort when traffic allows.
- Roll back if fill/match loss, churn, or user complaints outweigh ARPDAU gain.

## Experiment Template

```md
## Monetization experiment

Decision:
- Product moment:
- Candidate format:
- Placement or ad unit:
- Frequency cap:
- eCPM floor:

Evidence:
- Source mapping checked: .omo/evidence/task-1-source-refresh.md
- Baseline window:
- Baseline eCPM:
- Baseline fill rate / match rate:
- Baseline show rate:
- Baseline impressions/session:
- Baseline ARPDAU:
- Baseline retention/churn:
- Baseline user complaints:

Guardrail:
- Stop or roll back when:

Result:
- Revenue movement:
- Fill/match movement:
- Retention/churn movement:
- User complaint movement:
- Keep, iterate, or roll back:
```

## Anti-Overclaim Rule

Do not rank formats as a universal winner. Choose the format by product moment,
user intent, policy fit, and measured cohort results. When asked for the
"earning" format, answer with the experiment plan and metric checklist above.
