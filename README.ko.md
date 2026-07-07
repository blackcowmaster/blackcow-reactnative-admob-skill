# blackcow-reactnative-admob-skill

[English](README.md)

bare React Native CLI 앱에서 `react-native-google-mobile-ads`를 안전하게 설정하고, 동의 흐름, 광고 UX, 릴리즈 점검을 돕는 AdMob 스킬입니다.

## 개요

이 저장소는 Codex/OpenAI Skills에서 사용할 `react-native-admob-cli` 스킬 패키지입니다.

`ios/`, `android/` 네이티브 폴더가 있는 bare React Native CLI 앱을 기준으로 합니다. `react-native-google-mobile-ads` 작업에 집중하고, Expo-managed 설정, Flutter 예시, AdMob 콘솔 자동화, 스토어 배포 흐름이 섞이지 않도록 경계를 잡아줍니다.

## 설치

```bash
npx skills add https://github.com/blackcowmaster/blackcow-reactnative-admob-skill.git --yes --global
```

## 범위

사용하기 좋은 경우:

- bare React Native CLI 앱에 AdMob을 붙일 때
- 루트 `app.json`에 AdMob App ID를 설정할 때
- 배너, 전면, 리워드, 앱 오픈 광고 패턴이 필요할 때
- UMP 동의, ATT, SDK 초기화 순서를 점검할 때
- 광고 배치, 릴리즈 준비, 문제 해결을 확인할 때

Expo-managed 앱, Flutter 앱, AdMob 콘솔 생성 작업, 스토어 배포 자동화에는 이 스킬을 주 가이드로 쓰지 않는 편이 좋습니다.

## 파일

스킬은 `SKILL.md`, `agents/openai.yaml`, 그리고 RN CLI 설정, 개인정보/동의, 광고 컴포넌트, UX 정책 점검, 릴리즈 점검, 문제 해결을 위한 reference 파일들로 구성되어 있습니다.

## 추천 프롬프트

```text
Use $react-native-admob-cli to integrate AdMob into my bare React Native CLI app safely.
```

## 검증

```bash
python /path/to/skill-creator/scripts/quick_validate.py .
```
