---
title: "Mobile Automation Framework"
layout: default
---

# 🚀 모바일 자동화 프레임워크 구축  
**Appium + Pytest + YAML Locator + GitLab CI/CD 기반 E2E 테스트 시스템**

> SOOP 모바일 앱(Android/iOS/TV)의 반복 테스트·성능 모니터링·회귀 검증을  
> 자동화로 전환하여 **품질·속도·일관성**을 동시에 확보한 프로젝트

---

# 📌 1. 성과 및 효과

| 성과 | 임팩트 |
|------|--------|
| 메뉴얼 TC의 **30% 자동화 전환** | 반복·UI 중심 업무 제거 → QA 리소스 확보 |
| **핵심 사용자 플로우 커버** | 로그인/송출/Live/VOD 등 주요 기능 안정성 확보 |
| **릴리즈 전 결함 조기 발견** | 사전 검증으로 배포 품질 안정 |
| **24h 무인 자동 실행 체계** | 야간 테스트 + 메신저 알림 대응 |

> 자동화 도입 이후 **이슈 대응 리드타임 40% 단축**, 품질 모니터링 체계 확립

---

# 📌 2. 아키텍처 개요

아키텍처 요소는 다음과 같은 흐름으로 동작한다:
pytest entry
↓
QaClass (공통 로직)
↓
Platform Class (Android/iOS/TV)
↓
YAML Locator + Appium Driver
↓
GitLab CI/CD (빌드 수급 → 설치 → 병렬 실행)

| 구성 요소 | 역할 |
|-----------|-------|
| **Test Code (pytest)** | 테스트 진입점 |
| **QaClass** | UI 제어·성능·로그·DB 연동 등 공통 기능 |
| **Platform Class** | Android/iOS/TV 시나리오 오케스트레이션 |
| **YAML Locators** | UI 요소 정의 및 유지보수 분리 |
| **Appium Driver** | 실제 디바이스 동작 |
| **CI/CD (GitLab)** | 앱 설치 → 테스트 자동 실행 → 리포트 수집 |

📷 **아키텍처 이미지 삽입 위치**  
`/assets/automation/framework_architecture.png`

---

# 📌 3. 테스트 코드의 흐름

## 3-1. 파일 구조 개요

| 파일명 | 역할 | 주요 기능 |
|--------|------|-----------|
| `QaClass.py` | 공통 액션 허브 | element_action / scroll / 성능추적 |
| `AndroidClass.py` | 시나리오 조립 | 로그인 → 검색 → 입장 → 광고처리 |
| `Android_live_test.py` | Live 기능 모듈 | 채팅/이모티콘/화질/멀티뷰 |
| `android_elements.yaml` | UI 리소스 | id, aid, xpath 기반 locator |
| `apk_gitlab_install.py` | GitLab 아티팩트 설치 | APK 다운로드 → 설치 → 실행 |
| `.gitlab-ci.yml` | 파이프라인 | OS/기기별 병렬 pytest 실행 |

---

## 3-2. 시나리오 구조 흐름
앱 실행
↓
인트로 배너 처리
↓
로그인 → 퀵뷰 체크
↓
방송 참여 및 광고 처리
↓
Live 기능(채팅/화질/종료 등) 검증
↓
테스트 종료 → 성능 데이터 자동 기록

---

## 3-3. 코드 샘플

### 📄 QaClass 핵심 메서드
```python
def element_action(self, id='', find='', action='', value='', timeout=40):
    """
    Element 공통 동작
    """
    result = common.appium.element_action(
        driver=self.driver, elements=self.elements, id=id, find=find,
        action=action, value=value, timeout=timeout
    )
    assert result
    return result
def login(self, account_id, account_pw):
    self.element_action('navigation_more_btn', 'id', 'click')
    self.element_action('login_btn', 'xpath', 'click')
    self.element_action('login_id_box', 'xpath', 'input', account_id)
    self.element_action('login_pw_box', 'xpath', 'input', account_pw)
    self.element_action('login_btn', 'xpath', 'click')
def login(self, account_id, account_pw):
    self.element_action('navigation_more_btn', 'id', 'click')
    self.element_action('login_btn', 'xpath', 'click')
    self.element_action('login_id_box', 'xpath', 'input', account_id)
    self.element_action('login_pw_box', 'xpath', 'input', account_pw)
    self.element_action('login_btn', 'xpath', 'click')
📌 4. 자동화 성과 자료

📄 PDF 첨부 (자동화 개선 자료)
👉 /assets/automation/2024_automation_result.pdf

2023 → 2024 주요 개선 요약

YAML 리팩토링

Scenario/Common 구조 정리

마크 기반 테스트 세분화

성능 로그 자동화

유지보수 시간 50% ↓

신규 QA 온보딩 2배 속도 ↑

📊 성과 그래프 / 이미지 삽입 위치
👉 /assets/automation/performance_graph.png

📌 5. 테스트 결과 대시보드 (실시간 모니터링)

성공/실패 비율 자동 수집

CI/CD 아티팩트 및 QA Dashboard로 동기화

Dev/QE 공동 모니터링 구조

📷 이미지 삽입
👉 /assets/automation/test_dashboard.png

📌 6. 호환성 테스트 자동화

Android 10~15 전 기기에서 기능 일관성을 자동 검증하는 구조.

Android OS 10~15  
   ↓  
pytest compatibility  
   ↓  
기능별 테스트(Home/Explore/Live/More)  
   ↓  
성능 로그 자동 기록  
   ↓  
QA Dashboard

주요 특징

OS/디바이스별 회귀 테스트 자동화

기능 독립 모듈 구조로 유지보수 용이

성능 데이터 자동 연동

📌 7. 데모 영상

🎬 데모 영상(mp4) 삽입
👉 /assets/automation/demo.mp4

GitHub는 mp4를 그대로 재생 가능하므로
아래처럼 바로 넣으면 됨:

<video src="/assets/automation/demo.mp4" controls width="600"></video>

📌 8. 프로젝트 핵심 가치

반복 테스트 자동화 → 품질 안정성 확보 + 업무 리소스 확보

플랫폼 공통 구조(QaClass) → 일관된 테스트 품질 유지

YAML Locator → UI 변경에도 낮은 수정 비용

CI/CD 병렬 실행 → 배포 전 품질 게이트 확보

