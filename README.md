# blackcow-reactnative-admob-skill

Bare React Native CLI AdMob skill for safe `react-native-google-mobile-ads` setup, consent, UX, and release checks.

한국어 설명은 아래에 함께 정리되어 있습니다.

## Install

```bash
npx skills add https://github.com/blackcowmaster/blackcow-reactnative-admob-skill.git --yes --global
```

## English

### What This Is

This repository packages the `react-native-admob-cli` skill for Codex/OpenAI Skills.

The skill is a focused guide for bare React Native CLI apps that use `react-native-google-mobile-ads`. It helps an agent set up AdMob without accidentally applying Expo-managed snippets, Flutter examples, AdMob console automation, or store deployment assumptions to a bare React Native project.

### Use This Skill For

- installing and configuring `react-native-google-mobile-ads`
- placing AdMob App IDs in the root `app.json`
- separating native App IDs from runtime ad unit IDs
- adding or reviewing banner, interstitial, rewarded, and app-open ads
- handling UMP consent, ATT, and SDK initialization order
- debugging no-fill, startup crashes, test IDs, and failed ad loads
- checking AdMob-related release readiness

### Do Not Use This Skill For

- Expo-managed apps or EAS-first app shells
- Flutter apps
- AdMob console provisioning
- App Store or Play Console deployment automation
- mutating production AdMob, store, or browser sessions without explicit approval

For those cases, use a more specific Expo, Flutter, AdMob console, or store deployment skill.

### What Is Included

- `SKILL.md`: the main routing instructions
- `agents/openai.yaml`: display metadata for skill-capable clients
- `references/rn-cli-setup.md`: package manager, native App IDs, rebuild checks
- `references/privacy-consent.md`: UMP, ATT, initialization, privacy options
- `references/ad-components.md`: TypeScript ad unit config and ad component patterns
- `references/policy-ux-checklist.md`: React Native ad placement and accidental-click review
- `references/store-release-checklist.md`: release checks for IDs, privacy, Play, and App Store
- `references/troubleshooting.md`: no-fill, startup crashes, consent issues, Jest setup

### Suggested Prompt

```text
Use $react-native-admob-cli to integrate AdMob into my bare React Native CLI app safely.
```

### Validation

Before publishing changes to this skill, run:

```bash
python /path/to/skill-creator/scripts/quick_validate.py .
```

For real app work, the proof should usually include dependency/config review, TypeScript or lint checks for touched files, native rebuild attempts when feasible, and test ad evidence or a clear note that no simulator/device was available.

## 한국어

### 이 스킬의 목적

이 저장소는 Codex/OpenAI Skills에서 사용할 `react-native-admob-cli` 스킬을 담고 있습니다.

이 스킬은 `react-native-google-mobile-ads`를 사용하는 bare React Native CLI 앱을 위한 가이드입니다. Expo-managed 설정, Flutter 예시, AdMob 콘솔 자동화, 스토어 배포 자동화 지침이 bare React Native 프로젝트에 섞이지 않도록 경계를 잡아주는 역할을 합니다.

### 사용할 때

- `react-native-google-mobile-ads` 설치와 설정이 필요할 때
- 루트 `app.json`에 AdMob App ID를 설정해야 할 때
- 네이티브 App ID와 런타임 광고 단위 ID를 분리해야 할 때
- 배너, 전면, 리워드, 앱 오픈 광고를 추가하거나 검수할 때
- UMP 동의, ATT, SDK 초기화 순서를 점검할 때
- no-fill, 시작 시 크래시, 테스트 ID, 광고 로드 실패를 디버깅할 때
- AdMob 관련 릴리즈 준비 상태를 확인할 때

### 사용하지 말아야 할 때

- Expo-managed 앱이나 EAS 중심 앱 셸
- Flutter 앱
- AdMob 콘솔에서 앱이나 광고 단위를 생성하는 작업
- App Store 또는 Play Console 배포 자동화
- 명시적인 승인 없이 프로덕션 AdMob, 스토어, 브라우저 세션을 변경하는 작업

이런 경우에는 Expo, Flutter, AdMob 콘솔, 스토어 배포에 맞는 별도 스킬을 사용하는 편이 안전합니다.

### 포함된 파일

- `SKILL.md`: 스킬의 메인 라우팅 지침
- `agents/openai.yaml`: 스킬 지원 클라이언트용 표시 메타데이터
- `references/rn-cli-setup.md`: 패키지 매니저, 네이티브 App ID, 리빌드 점검
- `references/privacy-consent.md`: UMP, ATT, 초기화 순서, 개인정보 옵션
- `references/ad-components.md`: TypeScript 광고 단위 설정과 광고 컴포넌트 패턴
- `references/policy-ux-checklist.md`: React Native 광고 배치와 오탭 위험 검수
- `references/store-release-checklist.md`: ID, 개인정보, Play, App Store 릴리즈 점검
- `references/troubleshooting.md`: no-fill, 시작 크래시, 동의 문제, Jest 설정

### 추천 프롬프트

```text
Use $react-native-admob-cli to integrate AdMob into my bare React Native CLI app safely.
```

### 검증

스킬을 수정한 뒤에는 아래 명령으로 기본 구조를 확인합니다.

```bash
python /path/to/skill-creator/scripts/quick_validate.py .
```

실제 앱 작업에서는 의존성 및 설정 변경 확인, 변경 파일의 TypeScript 또는 lint 검증, 가능한 경우 네이티브 리빌드, 테스트 광고 로드 증거를 함께 확인하는 것이 좋습니다.

## Repository Layout

```text
.
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
    |-- ad-components.md
    |-- policy-ux-checklist.md
    |-- privacy-consent.md
    |-- rn-cli-setup.md
    |-- store-release-checklist.md
    `-- troubleshooting.md
```
