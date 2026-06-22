# ☕ CafeHub - 카페 예약 모바일 앱

> **가고 싶은 카페를 찾아서 자리를 미리 예약하는 모바일 앱**

[![Dart](https://img.shields.io/badge/Dart-3.9.2-blue)](https://dart.dev)
[![Flutter](https://img.shields.io/badge/Flutter-3.35.3-blue)](https://flutter.dev)
[![Firebase](https://img.shields.io/badge/Firebase-Latest-yellow)](https://firebase.google.com)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

---

## 📋 목차

- [프로젝트 개요](#-프로젝트-개요)
- [주요 기능](#-주요-기능)
- [기술 스택](#-기술-스택)
- [설치 및 실행](#-설치-및-실행)
- [프로젝트 구조](#-프로젝트-구조)
- [개발 문서](#-개발-문서)
- [팀 정보](#-팀-정보)

---

## 🎯 프로젝트 개요

### 개발 배경

일상적으로 많은 사람들이 **카페 방문 시 다음과 같은 문제**를 경험합니다:

1. **카페 정보 부족** - 위치, 분위기, 시설 정보가 산재
2. **자리 확보 어려움** - 도착했는데 자리가 없는 경우 빈번
3. **메뉴 고민** - 매번 반복되는 메뉴 선택의 번거로움

### 솔루션

**CafeHub**는 이 모든 문제를 해결하는 통합 솔루션입니다:

- 🔍 **검색**: 위치/분위기/시설로 카페 필터링
- 📅 **예약**: 날짜/시간/인원 선택으로 즉시 예약
- ⏰ **관리**: 예약 내역 한눈에 관리
- ⭐ **리뷰**: 방문 경험 공유 및 평가

### 기대 효과

✅ 시간 낭비 감소 (자리 확보 시간 단축)  
✅ 사용자 만족도 향상 (확실한 자리 보장)  
✅ 카페 정보 통합 관리 (한 앱에서 모두 해결)  

---

## 🌟 주요 기능

### 1️⃣ 회원 관리
- ✅ JWT 기반 안전한 인증
- ✅ 회원가입/로그인
- ✅ 자동 로그인 (토큰 기반)

### 2️⃣ 카페 검색
- ✅ 실시간 검색 기능
- ✅ 5가지 카테고리 필터
  - 📍 지역별
  - 🎨 분위기 (조용함/활기찬)
  - ⚙️ 시설 (와이파이/콘센트)
  - 👥 예약 가능 여부
  - 📅 영업 시간

### 3️⃣ 예약 시스템
- ✅ DatePicker로 날짜 선택 (오늘~30일)
- ✅ 시간 선택 (10:00~19:00, 30분 단위)
- ✅ 인원 선택 (1~10명)
- ✅ 예약 확정 알림

### 4️⃣ 예약 관리
- ✅ 예약 내역 조회
- ✅ 예약 상태 표시
  - 예약 대기 중
  - 예약 확정
  - 방문 완료
- ✅ 예약 취소 기능

### 5️⃣ 리뷰 시스템
- ✅ 별점 선택 (1~5점)
- ✅ 텍스트 리뷰 작성 (최대 300자)
- ✅ 리뷰 조회
- ✅ 카페별 평점 계산

---

## 🛠️ 기술 스택

### Frontend
```
Language:   Dart 3.9.2
Framework:  Flutter 3.35.3
State Mgmt: Provider
HTTP:       Dio
Platform:   Android / iOS
```

### Backend
```
Framework:  Firebase (Firestore + Auth)
Hosting:    Cloud Firestore
Auth:       Firebase Authentication
```

### Database
```
Primary:    Cloud Firestore
Real-time:  Yes
Sync:       Auto Sync
```

### Architecture
```
Pattern:    4-Layer Layered Architecture
├── Presentation (Screens, Widgets)
├── Application (State Management, ViewModel)
├── Domain (Business Logic, Use Cases)
└── Data (Repository, APIs)
```

자세한 내용은 [ARCHITECTURE.md](docs/ARCHITECTURE.md) 참조

---

## 📦 설치 및 실행

### 요구사항
- **Flutter SDK**: 3.35.3 이상
- **Dart SDK**: 3.9.2 이상
- **Android Studio** 또는 **Xcode** (플랫폼별)
- **Git**

### 설치 단계

#### 1. 저장소 클론
```bash
git clone https://github.com/ayeong333-boop/cafehub-app.git
cd cafehub-app
```

#### 2. 의존성 설치
```bash
flutter pub get
```

#### 3. Firebase 설정
```bash
# google-services.json (Android)
# GoogleService-Info.plist (iOS)
# 를 각각 android/app, ios/Runner 폴더에 배치
```

자세한 설정은 [SETUP.md](docs/SETUP.md) 참조

#### 4. 앱 실행
```bash
# 디바이스 확인
flutter devices

# 앱 실행
flutter run

# Release 빌드
flutter run --release
```

---

## 📁 프로젝트 구조

```
cafehub-app/
├── docs/                          # 문서 폴더
│   ├── README.md                  # 프로젝트 소개
│   ├── REQUIREMENTS.md            # 요구사항 정의서
│   ├── ARCHITECTURE.md            # 아키텍처 설명
│   ├── SETUP.md                   # 개발 환경 설정
│   ├── DEPLOY.md                  # 배포 가이드
│   ├── TESTING.md                 # 테스트 전략
│   ├── AGENTS.md                  # AI 에이전트 정책
│   ├── adr/                       # Architecture Decision Records
│   │   ├── ADR-0001-flutter-choice.md
│   │   ├── ADR-0002-firebase-choice.md
│   │   └── ADR-0003-layered-architecture.md
│   ├── images/                    # 앱 스크린샷
│   │   ├── login_screen.png
│   │   ├── home_screen.png
│   │   ├── search_screen.png
│   │   ├── reservation_screen.png
│   │   └── review_screen.png
│   ├── presentation.html          # 최종 발표자료
│   └── WBS.md                     # 프로젝트 계획
├── lib/
│   ├── main.dart                  # 앱 진입점
│   ├── presentation/              # UI 계층
│   │   ├── screens/
│   │   ├── widgets/
│   │   └── providers/
│   ├── application/               # 애플리케이션 계층
│   │   └── providers/
│   ├── domain/                    # 도메인 계층
│   │   ├── entities/
│   │   ├── repositories/
│   │   └── usecases/
│   └── data/                      # 데이터 계층
│       ├── datasources/
│       ├── models/
│       └── repositories/
├── test/                          # 단위/통합 테스트
├── pubspec.yaml                   # 프로젝트 설정 및 의존성
├── .gitignore
├── LICENSE
└── README.md                      # 이 파일
```

---

## 📚 개발 문서

| 문서 | 설명 | 링크 |
|------|------|------|
| **요구사항** | 프로젝트 요구사항 명세 | [REQUIREMENTS.md](docs/REQUIREMENTS.md) |
| **아키텍처** | 시스템 설계 및 구조 | [ARCHITECTURE.md](docs/ARCHITECTURE.md) |
| **ADR** | 아키텍처 결정 기록 | [adr/](docs/adr/) |
| **설정** | 개발 환경 구성 | [SETUP.md](docs/SETUP.md) |
| **배포** | 배포 방법 및 가이드 | [DEPLOY.md](docs/DEPLOY.md) |
| **테스트** | 테스트 전략 및 방법 | [TESTING.md](docs/TESTING.md) |
| **일정** | WBS 및 개발 일정 | [WBS.md](docs/WBS.md) |
| **정책** | AI 에이전트 정책 | [AGENTS.md](AGENTS.md) |

---

## 🎨 주요 화면

### 로그인
- 안전한 JWT 기반 인증
- 자동 로그인 지원

### 홈
- 인기 카페 가로 스크롤
- 최근 리뷰 최대 3개 표시
- 빠른 접근 메뉴

### 검색
- 실시간 검색
- 5개 카테고리 필터
- 결과 수 표시

### 예약
- DatePicker 기반 날짜 선택
- 시간/인원 선택
- 예약 확정

### 리뷰
- 별점 선택 (1~5)
- 텍스트 리뷰 작성
- 리뷰 조회 및 삭제

---

## 🔄 개발 워크플로우

### 브랜치 전략
```
main (배포용)
  ├── develop (개발용)
  │   ├── feature/auth
  │   ├── feature/search
  │   ├── feature/reservation
  │   └── feature/review
  └── hotfix/bug-fix
```

### Commit Convention
```
feat:     새로운 기능
fix:      버그 수정
docs:     문서 수정
style:    코드 스타일 변경
refactor: 코드 리팩토링
test:     테스트 추가/수정
chore:    설정 파일 변경
```

---

## 🧪 테스트

```bash
# 모든 테스트 실행
flutter test

# 커버리지 생성
flutter test --coverage
```

자세한 테스트 방법은 [TESTING.md](docs/TESTING.md) 참조

---

## 🚀 배포

### Android
```bash
flutter build apk --release
```

### iOS
```bash
flutter build ios --release
```

배포 가이드는 [DEPLOY.md](docs/DEPLOY.md) 참조

---

## 🤝 기여 방법

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📝 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다. 자세한 내용은 [LICENSE](LICENSE) 파일 참조

---

## 👥 팀 정보

**Fresh Code Team** - CafeHub 프로젝트 개발팀

| 이름 | 역할 | GitHub |
|------|------|--------|
| 성아영 | Frontend Developer | [@ayeong333-boop](https://github.com/ayeong333-boop) |
| - | - | - |
| - | - | - |

---

## 📞 문의

- 🐛 버그 리포트: [Issues](https://github.com/ayeong333-boop/cafehub-app/issues)
- 💬 기능 요청: [Discussions](https://github.com/ayeong333-boop/cafehub-app/discussions)
- 📧 이메일: ayeong333@example.com

---

## 🙏 감사의 말

- Google Gemini API
- Firebase Platform
- Flutter Community
- 모든 기여자들

---

**마지막 업데이트**: 2026-06-22  
**프로젝트 상태**: 진행 중 🔄
