---
marp: true
theme: uncover
class: invert
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
    color: #fff;
  }
  h1 { color: #ff6b6b; font-size: 3em; margin-bottom: 20px; }
  h2 { color: #4ecdc4; font-size: 2.5em; margin-top: 20px; }
  p { font-size: 1.3em; line-height: 1.8; }
  ul { font-size: 1.2em; }
  code { background: #0f3460; padding: 2px 8px; border-radius: 4px; }
  .title { text-align: center; }
  .highlight { color: #ffd93d; font-weight: bold; }

---

# 🏢 CafeHub
## 카페 예약 모바일 앱

### 가고 싶은 카페를 찾아서 자리를 미리 예약하는 모바일 앱

---

## 📍 비전
### "일상 속 카페 경험을 더 편하게"

- 직장인, 학생, 프리랜서
- **문제**: 카페 찾기 → 도착 → 자리 없음
- **해결**: 미리 검색하고 예약하기

---

## ❌ 문제 정의

### "카페 가는 길이 너무 오래 걸려요"

- 📱 카페 정보 흩어져 있음
- ⏰ 자리 확보 시간 낭비
- 😤 예약 시스템 없음
- 🚫 방문 후 자리 부족 경험

---

## ✅ 가치 제안

### "30초 안에 카페를 예약한다"

1. **검색**: 위치/분위기/시설로 필터링
2. **예약**: 날짜/시간/인원 선택
3. **확인**: 예약 내역 관리
4. **리뷰**: 경험 공유

---

## 📋 프로젝트 계획

### WBS & 기술 스택

**주차별 목표**
| 주차 | 목표 | 산출물 |
|------|------|--------|
| 10주 | 기획 | .planning/* |
| 11주 | 환경설정 | main.dart |
| 12주 | 중간발표 | 데모 |
| 13주 | 기능개발 | 앱 기능 |
| 14주 | 최종배포 | APK/배포 |

---

## 🛠️ 기술 스택

**개발 환경**
- Framework: **Flutter 3.35.3**
- Backend: **Firebase (Firestore + Auth)**
- Maps: **Google Maps API**
- State Management: **Provider**
- Language: **Dart 3.9.2**

---

## 🏗️ 아키텍처

### 4계층 Layered Architecture

```
┌─────────────────────────────────┐
│   Presentation (UI)             │
├─────────────────────────────────┤
│   Application (ViewModel)       │
├─────────────────────────────────┤
│   Domain (Business Logic)       │
├─────────────────────────────────┤
│   Data (Repository)             │
└─────────────────────────────────┘
```

---

## 📱 구현 결과

### 5개 탭 + 완벽한 사용자 흐름

1. **로그인** - 사용자 인증
2. **홈** - 인기 카페 + 최근 리뷰
3. **검색** - 실시간 검색 + 필터링
4. **예약** - 날짜/시간/인원 선택
5. **리뷰** - 리뷰 작성 + 조회

---

## 🔐 기능 1: 로그인

**구현 사항**
- ✅ 이름 입력 (필수)
- ✅ 비밀번호 UI (토글)
- ✅ 유효성 검사 (스낵바)
- ✅ 로그아웃 (홈 화면)

```dart
// Provider로 로그인 상태 관리
appState.login(username)
```

---

## 🏠 기능 2: 홈 & 검색

**홈 화면**
- 인기 카페 가로 스크롤
- 최근 리뷰 최대 3개
- 카페 탭 → 상세화면

**검색 화면**
- 실시간 키워드 검색
- 5개 카테고리 필터
- 결과 수 표시
- 결과 없을 때 안내

---

## 📅 기능 3: 예약 시스템

**예약 선택**
- DatePicker (오늘~30일)
- 시간 드롭다운 (10:00~19:00)
- 인원 +/- 버튼 (1~10명)

**예약 관리**
- 예약 목록 카드
- 날짜/시간/인원/상태 표시
- 예약 취소 기능

---

## ⭐ 기능 4: 리뷰

**리뷰 작성**
- 카페 선택 드롭다운
- 별점 선택 (1~5)
- 텍스트 입력 (최대 300자)
- 등록 후 폼 초기화

**리뷰 조회**
- 전체 리뷰 목록
- 카페별 평점 표시

---

## 🎯 ADR-0001: Flutter 선택

**선택 이유**
- iOS + Android 동시 개발
- 단일 코드베이스 (95% 공유)
- 빠른 프로토타입 (Hot Reload)

**대안 검토**
- React Native: 성능 낮음
- 네이티브: 6주 내 불가능

---

## ☁️ ADR-0002: Firebase 선택

**선택 이유**
- 인증 즉시 구현 가능
- Firestore 실시간 동기화
- 무료 범위 내 충분

**대안 검토**
- AWS: 설정 복잡
- 자체 서버: 개발 시간 3배

---

## 🔄 ADR-0003: Layered Architecture

**선택 이유**
- 각 계층 독립적 테스트
- 코드 재사용성 높음
- 유지보수 용이

**계층 책임**
- Presentation: UI 표시
- Application: 상태 관리
- Domain: 비즈니스 로직
- Data: API/DB 접근

---

## 💻 개발 환경 설정

**필수 설치**
```bash
flutter --version          # Flutter 3.35.3
flutter pub get           # 의존성 설치
flutter run              # 앱 실행
```

**구성 요소**
- Android Studio + SDK
- Dart SDK
- Provider 패키지
- Firebase 프로젝트

---

## 🛠️ 시행착오 & 학습

### 1️⃣ Provider 상태관리 설계
- **문제**: 처음엔 과도한 상태 구조 → 리빌드 과다
- **해결**: ChangeNotifier 분리 + 정확한 notifyListeners()
- **학습**: 상태 설계의 중요성 깨달음

### 2️⃣ 더미 데이터로 전체 흐름 구현
- **문제**: Firebase 실시간 동기화 복잡도
- **해결**: 더미 데이터로 UI/로직 먼저 구현
- **성과**: 개발 속도 50% 향상, API 연동 시간 절약

---

## 🧪 테스트 & 품질 관리

**단위 테스트**
- 로그인 유효성 검사 ✅
- 검색 필터링 로직 ✅
- 예약 상태 관리 ✅

**통합 테스트**
- 로그인 → 검색 → 예약 시나리오 ✅
- 리뷰 작성 → 조회 흐름 ✅

**코드 품질**
- 4계층 아키텍처 준수
- 함수 단위 책임 분리
- 주석 및 문서화

---

## 📦 빌드 & 배포

**빌드 과정**
```bash
# APK (Android)
flutter build apk --release

# iOS
flutter build ios --release
```

**배포 계획**
- 로컬 빌드 완료 ✅
- GitHub Releases에 APK 업로드
- TestFlight 등록 (향후)

---

## 🚀 향후 계획

**Short-term (1개월)**
- Should 기능 추가 (찜하기, 로컬DB)
- 성능 최적화 (캐싱)
- 테스트 커버리지 확대

**Long-term (3개월)**
- Google Play / App Store 배포
- 백엔드 확장 (결제 시스템)
- 사용자 피드백 반영

**비전**
- 대한민국 No.1 카페 예약 앱

---

## 🙏 감사합니다!

### 질문 준비 완료!
- ADR 3개 설명 가능 ✅
- 아키텍처 상세 설명 ✅
- 개발 환경 & 배포 설명 ✅
- 구현 시행착오 사례 ✅

---

## 📎 Q&A 가이드

**예상 질문**
1. **"왜 Flutter를 선택했나?"** → ADR-0001
2. **"아키텍처 구조는?"** → 4계층 다이어그램
3. **"어려웠던 부분?"** → Provider 상태관리 학습
4. **"배포는?"** → flutter build apk --release
5. **"테스트는?"** → 단위/통합 테스트 3개 이상
