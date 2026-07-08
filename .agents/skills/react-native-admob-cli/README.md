# blackcow-reactnative-admob-skill

[한국어](README.ko.md)

Bare React Native CLI AdMob skill for safe `react-native-google-mobile-ads` setup, consent, ad UX, monetization, measurement, and release checks.

## Overview

This repository packages the `react-native-admob-cli` skill for Codex/OpenAI Skills.

Use it for bare React Native CLI apps with native `ios/` and `android/` folders. The skill keeps AdMob work focused on `react-native-google-mobile-ads` and helps avoid mixing in Expo-managed snippets, Flutter examples, AdMob console automation, or store deployment workflows.

## Install

Use the skill-directory URL so supporting files under `references/` are installed with the skill.

```bash
npx skills add https://github.com/blackcowmaster/blackcow-reactnative-admob-skill/tree/main/.agents/skills/react-native-admob-cli --yes --global
```

## What It Covers

- Bare React Native CLI AdMob setup with root `app.json` App ID checks.
- UMP consent, ATT, SDK initialization order, and test-device safety.
- Banner, interstitial, rewarded, and app-open ad patterns.
- Monetization strategy that compares product moments, formats, eCPM, fill, show rate, retention, and user complaints without promising a universal best earner.
- Ad lifecycle guidance for preload, one-shot full-screen ads, reload after close/fail, cooldowns, app-open freshness, and "never block core flow" rules.
- Mediation and native checks for AndroidManifest, Gradle, Podfile, Info.plist, adapter freshness, SDK status, Ad Inspector, ResponseInfo, SKAdNetwork freshness, and partner disclosure.
- Measurement and release checks for ILRD, app-ads.txt, privacy labels, data safety, mediation partner disclosure, and proof artifacts.

Do not use it as the primary guide for Expo-managed apps, Flutter apps, AdMob console provisioning, or store deployment automation.

## Files

The skill includes `SKILL.md`, `agents/openai.yaml`, and focused references for RN CLI setup, privacy/consent, ad components, UX policy checks, monetization strategy, ad lifecycle, mediation/native setup, release checks, and troubleshooting.

## Suggested Prompt

```text
Use $react-native-admob-cli to integrate AdMob into my bare React Native CLI app safely.
```

## Validation

```bash
python /path/to/skill-creator/scripts/quick_validate.py .agents/skills/react-native-admob-cli
```
