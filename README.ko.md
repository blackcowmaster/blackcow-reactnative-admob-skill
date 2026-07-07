# blackcow-reactnative-admob-skill

[English](README.md)

bare React Native CLI 앱에서 `react-native-google-mobile-ads`를 안전하게 설정하고 동의, 광고 UX, 수익화, 측정, 릴리스 점검까지 다루는 AdMob 스킬입니다.

## 개요

이 저장소는 Codex/OpenAI Skills에서 사용할 `react-native-admob-cli` 스킬을 패키징합니다.

`ios/`와 `android/` 네이티브 폴더가 있는 bare React Native CLI 앱을 기준으로 합니다. 이 스킬은 AdMob 작업을 `react-native-google-mobile-ads`에 집중시키고 Expo-managed 설정, Flutter 예제, AdMob 콘솔 자동화, 스토어 배포 자동화가 섞이지 않도록 경계를 잡습니다.

## 설치

```bash
npx skills add https://github.com/blackcowmaster/blackcow-reactnative-admob-skill.git --yes --global
```

## 다루는 범위

- bare React Native CLI AdMob 설정과 루트 `app.json`의 App ID 점검
- UMP 동의, ATT, SDK 초기화 순서, 테스트 기기 안전장치
- 배너, 전면, 리워드, 앱 오프닝 광고 패턴
- 제품 순간, 광고 형식, eCPM, 채움률, 노출률, 유지율, 사용자 불만을 함께 보는 수익화 전략
- 미리 로드, 한 번만 쓰는 전체 화면 광고 인스턴스, 닫힘/실패 후 다시 로드, 쿨다운, 앱 오프닝 광고 신선도, 핵심 흐름 차단 금지를 포함한 광고 라이프사이클
- AndroidManifest, Gradle, Podfile, Info.plist, 어댑터 최신성, SDK 상태, Ad Inspector, ResponseInfo, SKAdNetwork 최신성, 파트너 고지를 확인하는 미디에이션 및 네이티브 점검
- ILRD, app-ads.txt, 개인정보 라벨, 데이터 보안, 미디에이션 파트너 고지, 증거 산출물을 포함한 측정 및 릴리스 점검

Expo-managed 앱, Flutter 앱, AdMob 콘솔 생성 작업, 스토어 배포 자동화의 기본 가이드로는 사용하지 마세요.

## 파일

스킬은 `SKILL.md`, `agents/openai.yaml`, 그리고 RN CLI 설정, 개인정보/동의, 광고 컴포넌트, UX 정책 점검, 수익화 전략, 광고 라이프사이클, 미디에이션/네이티브 설정, 릴리스 점검, 문제 해결을 위한 참조 문서로 구성됩니다.

## 추천 프롬프트

```text
Use $react-native-admob-cli to integrate AdMob into my bare React Native CLI app safely.
```

## 검증

```bash
python /path/to/skill-creator/scripts/quick_validate.py .
```
