# DNS-TGM200 Tank Gauging PC Monitoring Software

[![Flutter](https://img.shields.io/badge/Flutter-3.x%20(Desktop)-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Platform](https://img.shields.io/badge/Platform-Windows%20x64-0078D6?logo=windows&logoColor=white)](https://microsoft.com/windows)
[![Version](https://img.shields.io/badge/Version-v0.12.0-2EA44F)](https://github.com/cqb0709/DNS-TGM200/releases)
[![Dart](https://img.shields.io/badge/Dart-3.x-0175C2?logo=dart&logoColor=white)](https://dart.dev)

**DNS-TGM200**은 산업용 탱크 게이징(Tank Gauging) 시스템을 위한 **Windows PC 전용 실시간 모니터링 및 운영 관리 소프트웨어**입니다.  
TGM200 현장 계측 장비와의 실시간 양방향 통신, 정밀 재고 마감 연산, 다채널 알람 관리, 자동화된 정밀 보고서 생성 및 이메일 발송 기능을 통합 제공합니다.

---

## 📌 주요 핵심 기능 (Key Features)

### 1. 실시간 대시보드 & 탱크 상세 모니터링

* **탱크 종합 관제**: 8기 탱크의 레벨(높이), 총용량, 실재고량($Dr$), 공간 용적, 온도, 수분, 밀도 실시간 표출
* **KPI 지표 시각화**: 전체 주유소/기지 합산 재고율 및 상태별 탱크 카드 모니터링
* **정밀 계측 상세**: 탱크별 계측 데이터 시계열 추적, VCF/PCR 보정 계수 및 입출고 이력 상세 조회

### 2. 고신뢰성 알람 및 안전 관리

* **실시간 위험 감지**: 고액위(HH/H), 저액위(LL/L), 이상 누유, 통신 두절 등 다채널 알람 실시간 감지
* **전역 긴급 팝업 (`AlarmPopup`)**: 위험 알람 발생 시 화면 전역에 즉각적인 알림 모달 노출 및 조치 지원
* **알람 조치 연동**: 확인 필요 ↔ 알람 해제 토글 및 보고서 연동 이력 관리

### 3. 운영 관리 & 재고 마감 엔진

* **정밀 입고 관리 (`Operation`)**: 유조차 입고 시작/종료 제어, 입고 예정량 및 실입고량 정밀 측정
* **마감 재고 관리**: Base/User PCR 7계수 보정을 통한 정밀 보정값 산출 및 User_pcr 적용
* **기간 집계 엔진**: 전일 재고 + 입고량 − 출고량 = 당일 재고 일일 마감 로직 내장

### 4. 정밀 보고서 엔진 & 백그라운드 자동화

* **6종 표준 보고서**: 일일 마감, 일자별 재고, 실시간 모니터링, 알림·이벤트, 누유 검사, 월간 요약
* **다양한 출력 포맷**: `pdf`, `excel`, `printing` 연동을 통한 인쇄 미리보기, 정밀 PDF 문서, Excel(`.xlsx`), CSV 내보내기
* **자동 스케줄러 (`ReportSchedulerService`)**: 매일/매주/매월 전일자(D-1) 기준 자동 집계 및 파일 저장
* **이메일 자동 발송 (`EmailService`)**: Mailjet SMTP 기반 보고서 자동 첨부 발송

### 5. 산업용 통신 보안 & 장비 연동

* **양방향 소켓 통신 (`WebSocketService`)**: TGM200 프로토콜 기반 비동기 이벤트 스트림 및 자동 재연결
* **보안 핸드셰이크**: Windows DPAPI 기반 OS 보안 저장소(`SecureStorageService`)에 8자리 통신 키 암호화 보관 및 AES 세션 토큰 인증
* **프로세스 자동 재부팅**: 시스템 설정 변경 및 캐시 초기화 시 Windows 앱 프로세스 자동 재부팅 제어

### 6. 데스크톱 UX 최적화 & 로깅

* **고해상도(4K/QHD) 대응**: `AppScaleWrapper`를 통한 실시간 UI 표시 배율(80% ~ 200%) 스케일링
* **표준 로컬 로깅 (`AppLogger`)**: `%LOCALAPPDATA%\DNS-TGM200\logs` 경로에 일자별 자동 롤링 로그 파일 보관 및 감사 로그 기록

### 7. 스마트 인앱 자동 업데이트 (In-App Auto-Update)

* **배포 형태별 맞춤 업데이트 (`UpdateService`)**:
  * **Inno Setup 설치형**: 임시 폴더로 신규 Setup.exe 백그라운드 다운로드 ➔ SHA-256 무결성 검증 ➔ `/SILENT` 무인 교체 설치 및 자동 재실행
  * **무설치 포터블(7-Zip SFX) / ZIP**: 릴리즈 다운로드 공식 웹페이지로 자동 연동
* **비침습적 2시간 스케줄러**: 앱 진입 3초 후 1회 확인(팝업) + 상시 가동 중에는 2시간마다 조용히 체크하여 **상단 헤더에 부드럽게 깜빡이는 다운로드 아이콘 버튼만 활성화** (작업자 방해 원천 방지)
* **네트워크 복원력 (Fail-Safe)**: 불안정한 네트워크 환경을 고려하여 다운로드 및 무결성 검사 실패 시 **최대 3회 자동 재시도 루프** 탑재

---

## 🛠️ 기술 스택 (Tech Stack)

| 구분 | 기술 / 라이브러리 |
| --- | --- |
| **Framework** | Flutter (Desktop Windows), Dart SDK 3.x |
| **Runner Engine** | Win32 / C++ Desktop Runner |
| **Window Control** | `window_manager` |
| **Security & Auth** | `flutter_secure_storage` (Windows DPAPI), `encrypt` (AES) |
| **Document & Export** | `pdf`, `printing`, `excel`, `file_picker` |
| **Networking & Mail** | `dart:io` (WebSocket, HTTP), `mailer` (Mailjet SMTP) |
| **Storage & Utils** | `shared_preferences`, `intl`, `package_info_plus` |

---

## 📂 프로젝트 아키텍처

```text
lib/
├── main.dart                  앱 엔트리포인트 · 전역 UI 스케일러 · 로그인/자동로그인 · 메인 셸
├── ui.dart                    디자인 시스템(다크 네이비 & 골드 앰버) · 공통 UI 위젯
├── data.dart                  전역 상태 관리(AppState) · 탱크 데이터 및 재고 마감 계산 엔진
│
├── screens/                   UI 화면 레이어
│   ├── dashboard.dart         대시보드 화면
│   ├── tanks.dart             탱크 상세 정보 및 진단
│   ├── alarm.dart             알람 모니터링 및 확인
│   ├── alarm_popup.dart       실시간 긴급 알람 팝업 모달
│   ├── operation.dart         정밀 입고 및 마감 재고 관리
│   ├── history.dart           입출고/누유검사/점검/운영 이력
│   ├── reports.dart           보고서 조회 및 스케줄러 설정
│   ├── report_export.dart     PDF / Excel / CSV 미리보기 및 내보내기 전용 뷰어
│   ├── settings.dart          시스템 설정 (단위계/통신/탱크/사용자/테마/업데이트)
│   └── system_config_dialog.dart 시스템 환경설정 팝업 (보안 통신 키/로그 레벨/재부팅)
│
├── services/                  비즈니스 및 백그라운드 서비스
│   ├── websocket_service.dart 실시간 웹소켓 통신 및 AES 세션 핸드셰이크
│   ├── secure_storage_service.dart Windows DPAPI 기반 보안 키 보관 및 프로세스 제어
│   ├── app_logger.dart        파일 로거 (일자별 롤링 및 사용자 액션 감사 로그)
│   ├── report_scheduler_service.dart 보고서 백그라운드 자동 집계 스케줄러
│   ├── update_service.dart    스마트 인앱 업데이트 (설치형/포터블 판별, 무인 설치, 2시간 스케줄러)
│   └── email_service.dart     Mailjet SMTP 기반 이메일 발송
│
├── state/                     통신 프로토콜 정의 (protocol_code.dart)
└── utils/                     안전 파싱 유틸리티 (parse_utils.dart)

windows/                       Windows C++ 네이티브 러너 소스 및 앱 리소스
assets/                        공식 로고(tlg-logo.png) 및 Paperlogy 폰트
```


---

## 🚀 시작하기 (Getting Started)

### 1. 개발 환경 요구사항

* **OS**: Windows 10 이상 (64-bit)
* **Flutter SDK**: 3.x Stable 채널 (`>=3.0.0 <4.0.0`)
* **Build Tool**: Visual Studio 2019/2022 ('C++를 사용한 데스크톱 개발' 워크로드 설치 필요)

### 2. 설치 및 실행

```bat
# 1. 의존성 패키지 설치
flutter pub get

# 2. 디버그 모드 실행
flutter run -d windows

# 3. 릴리즈 배포용 빌드
flutter build windows --release
```

### 3. 배포 (Deployment) 및 릴리즈 체계

#### 📦 지원 배포 패키지 (3종)
본 프로젝트는 GitHub Actions 자동 빌드 파이프라인(`release.yml`)을 통해 3가지 배포본을 동시 생성합니다:
1. **공식 설치형 (`dns_tgm200-windows-vX.X.X-setup.exe`)**: Inno Setup 기반 단일 설치 관리자 (Program Files 자동 설치, 바로가기 생성, 인앱 원클릭 무인 자동 업데이트 지원)
2. **무설치 포터블 (`dns_tgm200-windows-vX.X.X-portable.exe`)**: 7-Zip SFX 기반 단일 무설치 실행 파일 (설치 과정 없이 즉시 실행)
3. **표준 압축본 (`dns_tgm200-windows-vX.X.X.zip`)**: DLL 및 종속 애셋이 포함된 표준 ZIP 아카이브

#### 🌐 CI/CD 릴리즈 파이프라인 및 이중 배포 (Dual Release)
* **Private Repository (`DNS-TGM200`)**: 내부 보관용으로 3종 패키지 + `version.json` 아카이빙
* **Public Repository (`DNS-TGM200-Release`)**: 외부 배포용으로 3종 패키지 릴리즈 및 `version.json` 정적 CDN 서빙 (`raw.githubusercontent.com`)
* 클라이언트 앱은 GitHub API Rate Limit 영향을 받지 않는 정적 CDN을 통해 최신 버전을 감지하고 안전하게 업데이트를 수행합니다.

---

## 💻 명령줄 인수 및 실행 옵션 (Command-Line Arguments)

`dns_tgm200.exe` 실행 시 명령어 뒤에 인자값(옵션/플래그)을 붙여 디스플레이 모드, 통신 포트, 로그 레벨, 스케줄러 등을 즉시 제어할 수 있습니다.

### 📌 지원 옵션 목록

| 카테고리 | 옵션명 (Option Flag) | 기본값 | 설명 및 효과 |
| :--- | :--- | :---: | :--- |
| **디스플레이** | `--fullscreen`, `-f` | 해제 | 작업표시줄까지 덮는 전체화면 모드로 앱 실행 |
| | `--kiosk` | 해제 | 관제실 전용 키오스크 잠금 모드 (전체화면 + 타이틀바 제거 + 항상 위 고정) |
| | `--scale=<배율>` | `auto` | UI 표시 배율 강제 지정 (예: `--scale=1.25`, `--scale=1.5`) |
| | `--display=<번호>` | `1` | 다중 모니터 환경에서 실행할 모니터 번호 지정 (1부터 시작, 예: `--display=2`) |
| | `--minimized` | 해제 | 창을 작업표시줄로 최소화한 상태로 백그라운드 시작 |
| **통신/네트워크**| `--port=<포트>`, `-p <포트>` | `8888` | WebSocket 서버 바인딩 포트 변경 (포트 충돌 방지 및 다중 실행용) |
| | `--handshake-key=<키>` | - | 8자리 통신 핸드셰이크 키를 OS 보안 저장소에 즉시 주입/등록 (배포 스크립트용) |
| **로깅/진단** | `--debug` | 해제 | 디버그 모드 강제 활성화 (로그 레벨 `debug` 전환 및 콘솔 출력 켜기) |
| | `--log-level=<level>` | `info` | 파일 로그 기록 수준 강제 지정 (`debug`, `info`, `warning`, `error`, `fatal`) |
| | `--log-console` | 해제 | Windows 터미널(CMD/PowerShell) `stdout`으로 실시간 로그 동시 출력 |
| | `--reset-settings` | 해제 | 기동 시 로컬 `SharedPreferences` 및 저장된 설정을 초기화하고 기본값으로 시작 |
| **스케줄러** | `--no-scheduler` | 해제 | 백그라운드 자동 보고서 생성 및 이메일 발송 스케줄러 비활성화 (테스트 시 오발송 방지) |
| **도움말** | `--help`, `-h`, `/?` | - | 사용 가능한 모든 명령줄 옵션 안내를 콘솔에 출력하고 종료 |

### 🚀 사용 예시

#### 1. 릴리즈 배포 실행 파일 (`dns_tgm200.exe`)
```powershell
# 2번 모니터에서 키오스크 모드로 실행
.\dns_tgm200.exe --kiosk --display=2

# 포트를 8889로 변경하고 디버그 로그 및 콘솔 출력을 켜서 실행
.\dns_tgm200.exe --port=8889 --debug --log-console

# 테스트 중 이메일 발송 스케줄러를 끄고 UI 배율을 1.5배로 강제 지정
.\dns_tgm200.exe --no-scheduler --scale=1.5

# 전체 옵션 도움말 출력
.\dns_tgm200.exe --help
```

> **Tip (바로가기 등록):** Windows 바탕화면의 바로가기 아이콘 우클릭 → **속성** → **대상(Target)** 경로 끝에 한 칸 띄우고 `...\dns_tgm200.exe --kiosk` 형태로 등록하면 클릭 시 항상 키오스크 모드로 띄울 수 있습니다.

#### 2. 개발 환경 (`flutter run`)
`--dart-entrypoint-args` 플래그를 사용하여 개발/디버그 중에도 동일하게 테스트할 수 있습니다:
```bash
# 단일 옵션 테스트
flutter run -d windows --dart-entrypoint-args="--kiosk"

# 복수 옵션 동시 전달
flutter run -d windows --dart-entrypoint-args="--port=8889,--debug,--log-console"
```

## 📖 추가 개발 가이드

보다 상세한 프로젝트 개발/빌드 배경 및 설정 가이드는 **[FLUTTER_GUIDE.md](FLUTTER_GUIDE.md)** 문서를 참조하세요.

---

## 📜 오픈소스 라이브러리 및 라이선스 (Open Source Licenses)

본 프로젝트에서 사용 중인 주요 오픈소스 라이브러리와 라이선스 정보입니다. 모든 종속성은 상용 소프트웨어 개발 및 배포에 안전한 허용적(Permissive) 오픈소스 라이선스를 따릅니다.

| 라이브러리 (Library) | 라이선스 (License) | 주요 역할 |
| --- | --- | --- |
| [flutter_secure_storage](https://pub.dev/packages/flutter_secure_storage) | BSD 3-Clause | Windows DPAPI 기반 통신 보안 키 보관 |
| [encrypt](https://pub.dev/packages/encrypt) | BSD 3-Clause | AES 대칭키 소켓 통신 암호화 |
| [window_manager](https://pub.dev/packages/window_manager) | MIT | Windows 데스크톱 창 크기 및 제어 |
| [pdf](https://pub.dev/packages/pdf) & [printing](https://pub.dev/packages/printing) | Apache 2.0 | 정밀 PDF 보고서 문서 생성 및 인쇄 미리보기 |
| [excel](https://pub.dev/packages/excel) | MIT | Excel(`.xlsx`) 보고서 스프레드시트 생성 |
| [file_picker](https://pub.dev/packages/file_picker) | MIT | 파일 저장 및 첨부 Windows 대화상자 |
| [mailer](https://pub.dev/packages/mailer) | MIT | Mailjet SMTP 기반 보고서 자동 메일 발송 |
| [shared_preferences](https://pub.dev/packages/shared_preferences) | BSD 3-Clause | UI 배율, 스케줄러 등 로컬 설정 저장 |
| [intl](https://pub.dev/packages/intl) | BSD 3-Clause | 날짜/일시 및 숫자 표기 포맷팅 |
| [package_info_plus](https://pub.dev/packages/package_info_plus) | BSD 3-Clause | 앱 버전 정보 동적 조회 |
| **Paperlogy Font** | SIL OFL 1.1 | 전역 적용 서체 (`assets/fonts/`) |
