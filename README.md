# blackcow-reactnative-admob-skill

[한국어](README.ko.md)

Bare React Native CLI AdMob skill for safe `react-native-google-mobile-ads` setup, consent, UX, and release checks.

## Overview

This repository packages the `react-native-admob-cli` skill for Codex/OpenAI Skills.

It is for bare React Native CLI apps with native `ios/` and `android/` folders. The skill keeps AdMob work focused on `react-native-google-mobile-ads` and helps avoid mixing in Expo-managed snippets, Flutter examples, AdMob console automation, or store deployment workflows.

## Install

```bash
npx skills add https://github.com/blackcowmaster/blackcow-reactnative-admob-skill.git --yes --global
```

## Scope

Use it for:

- bare React Native CLI AdMob setup
- root `app.json` AdMob App ID configuration
- banner, interstitial, rewarded, and app-open ad patterns
- UMP consent, ATT, and SDK initialization order
- ad placement, release-readiness, and troubleshooting checks

Do not use it as the primary guide for Expo-managed apps, Flutter apps, AdMob console provisioning, or store deployment automation.

## Files

The skill includes `SKILL.md`, `agents/openai.yaml`, and focused references for RN CLI setup, privacy/consent, ad components, UX policy checks, release checks, and troubleshooting.

## Suggested Prompt

```text
Use $react-native-admob-cli to integrate AdMob into my bare React Native CLI app safely.
```

## Validation

```bash
python /path/to/skill-creator/scripts/quick_validate.py .
```
