# 🏗️ CafeHub 아키텍처 설계 (ARCHITECTURE)

## 프로젝트: 카페 예약 모바일 앱
**버전**: 1.0  
**작성일**: 2026-04-01  

---

## 1. 아키텍처 개요

### 1.1 시스템 구성도

```
┌─────────────────────────────────────────────────────────┐
│                    Flutter Frontend                      │
│  ┌──────────────────────────────────────────────────┐   │
│  │        Presentation Layer (UI/Screens)           │   │
│  │  - LoginScreen, HomeScreen, SearchScreen, etc    │   │
│  └──────────────────┬───────────────────────────────┘   │
│                     │                                     │
│  ┌──────────────────▼───────────────────────────────┐   │
│  │   Application Layer (State Management)           │   │
│  │  - Provider, ChangeNotifier, ViewModel           │   │
│  └──────────────────┬───────────────────────────────┘   │
│                     │                                     │
│  ┌──────────────────▼───────────────────────────────┐   │
│  │    Domain Layer (Business Logic)                 │   │
│  │  - UseCases, Entities, Repositories (Abstract)   │   │
│  └──────────────────┬───────────────────────────────┘   │
│                     │                                     │
│  ┌──────────────────▼───────────────────────────────┐   │
│  │     Data Layer (Repository Implementation)       │   │
│  │  - DataSources, Models, Database                 │   │
│  └──────────────────┬───────────────────────────────┘   │
└─────────────────────┼──────────────────────────────────┘
                      │ HTTP/REST
┌─────────────────────▼──────────────────────────────────┐
│              Firebase Backend (Cloud)                   │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Authentication (Firebase Auth)                  │  │
│  │  - JWT Token Management                         │  │
│  └──────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Database (Cloud Firestore)                      │  │
│  │  - Real-time Sync, NoSQL                         │  │
│  └──────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Storage (Cloud Storage)                         │  │
│  │  - Review Images, Cafe Photos                    │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘
```

### 1.2 계층 간 의존성

```
Presentation Layer
    ↓ 의존 (depends on)
Application Layer
    ↓ 의존
Domain Layer
    ↓ 의존
Data Layer
    ↓ 의존
Firebase Backend & Local Storage
```

**중요**: 역방향 의존성 금지 (Data → Domain 불가)

---

## 2. 4계층 Layered Architecture 상세

### 2.1 Presentation Layer (UI 계층)

**책임**: 사용자 인터페이스 렌더링 및 입력 처리

```dart
lib/presentation/
├── screens/
│   ├── login_screen.dart          # 로그인 화면
│   ├── home_screen.dart           # 홈 화면
│   ├── search_screen.dart         # 검색 화면
│   ├── reservation_screen.dart    # 예약 화면
│   └── review_screen.dart         # 리뷰 화면
├── widgets/
│   ├── cafe_card.dart             # 카페 카드 위젯
│   ├── reservation_item.dart      # 예약 항목 위젯
│   └── review_item.dart           # 리뷰 항목 위젯
└── providers/
    ├── auth_provider.dart         # 인증 상태 관리
    ├── cafe_provider.dart         # 카페 상태 관리
    ├── reservation_provider.dart  # 예약 상태 관리
    └── review_provider.dart       # 리뷰 상태 관리
```

**특징**:
- ✅ UI 렌더링 로직만 포함
- ✅ Provider를 통한 상태 관리
- ✅ 비즈니스 로직 포함 금지

### 2.2 Application Layer (State Management 계층)

**책임**: 상태 관리 및 화면 로직 조정

```dart
lib/application/
└── providers/
    ├── auth_provider.dart
    │   └── notifyListeners() → Presentation 업데이트
    ├── cafe_search_provider.dart
    │   └── SearchUseCase 호출
    ├── reservation_provider.dart
    │   └── ReservationUseCase 호출
    └── review_provider.dart
        └── ReviewUseCase 호출
```

**특징**:
- ✅ ChangeNotifier 기반 상태 관리
- ✅ UseCases 호출
- ✅ 에러 핸들링 및 로딩 상태 관리

### 2.3 Domain Layer (비즈니스 로직 계층)

**책임**: 핵심 비즈니스 로직 및 도메인 규칙 정의

```dart
lib/domain/
├── entities/
│   ├── user_entity.dart           # 사용자 엔티티
│   ├── cafe_entity.dart           # 카페 엔티티
│   ├── reservation_entity.dart    # 예약 엔티티
│   └── review_entity.dart         # 리뷰 엔티티
├── repositories/
│   ├── auth_repository.dart       # 인증 레포지토리 (추상)
│   ├── cafe_repository.dart       # 카페 레포지토리 (추상)
│   ├── reservation_repository.dart# 예약 레포지토리 (추상)
│   └── review_repository.dart     # 리뷰 레포지토리 (추상)
└── usecases/
    ├── login_usecase.dart         # 로그인 유스케이스
    ├── search_cafe_usecase.dart   # 카페 검색 유스케이스
    ├── create_reservation_usecase.dart # 예약 생성
    └── create_review_usecase.dart # 리뷰 작성
```

**특징**:
- ✅ 추상 클래스/인터페이스 사용
- ✅ 프레임워크 무관
- ✅ 순수 비즈니스 로직

### 2.4 Data Layer (데이터 접근 계층)

**책임**: 데이터 소스와의 통신 및 모델 변환

```dart
lib/data/
├── datasources/
│   ├── local_datasource.dart      # 로컬 저장소 (SharedPreferences)
│   └── remote_datasource.dart     # 원격 데이터소스 (Firebase)
├── models/
│   ├── user_model.dart            # 사용자 모델
│   ├── cafe_model.dart            # 카페 모델
│   ├── reservation_model.dart     # 예약 모델
│   └── review_model.dart          # 리뷰 모델
└── repositories/
    ├── auth_repository_impl.dart  # 인증 레포지토리 구현
    ├── cafe_repository_impl.dart  # 카페 레포지토리 구현
    ├── reservation_repository_impl.dart
    └── review_repository_impl.dart
```

**특징**:
- ✅ Firebase API 호출
- ✅ 로컬 캐싱
- ✅ Model ↔ Entity 변환 (Mapper)

---

## 3. 주요 클래스 다이어그램

### 3.1 인증 흐름

```
LoginScreen (UI)
    ↓ user input
AuthProvider (State)
    ↓ call
LoginUseCase (Domain)
    ↓ call
AuthRepository (Domain - Abstract)
    ↓ call
AuthRepositoryImpl (Data)
    ↓ HTTP POST
Firebase Auth
    ↓ JWT Token
AuthRepositoryImpl
    ↓ return
LoginUseCase
    ↓ return
AuthProvider → notifyListeners()
    ↓ rebuild
HomeScreen (UI)
```

### 3.2 카페 검색 흐름

```
SearchScreen (UI)
    ↓ input: keyword, filters
CafeProvider (State)
    ↓ call
SearchCafeUseCase (Domain)
    ↓ call
CafeRepository (Domain - Abstract)
    ↓ call (with cache check)
CafeRepositoryImpl (Data)
    ↓ check local
    ↓ if miss → Firebase Query
    ↓ save to cache
CafeRepositoryImpl
    ↓ return List<CafeEntity>
SearchCafeUseCase
    ↓ return
CafeProvider → notifyListeners()
    ↓ rebuild
SearchResultScreen (UI)
```

---

## 4. 데이터 흐름

### 4.1 CRUD 패턴

#### Create (예약 생성)
```
Input: reservation_date, time, num_people
  ↓
Validation (Domain)
  ↓
Availability Check (Domain)
  ↓
Create Firestore Document
  ↓
Return: Reservation with ID
```

#### Read (카페 조회)
```
Input: cafe_id
  ↓
Check LocalCache
  ↓
If miss: Query Firebase
  ↓
Save to LocalCache
  ↓
Return: Cafe Entity
```

#### Update (리뷰 수정)
```
Input: review_id, new_content
  ↓
Authorization Check (User == Author)
  ↓
Update Firestore Document
  ↓
Invalidate LocalCache
  ↓
Return: Updated Review
```

#### Delete (예약 취소)
```
Input: reservation_id
  ↓
Cancellation Policy Check (< 24h before)
  ↓
Delete Firestore Document
  ↓
Return: Cancellation Confirmation
```

---

## 5. 에러 처리 전략

### 5.1 에러 계층화

```
Domain Layer
└── Failure (추상)
    ├── NetworkFailure      # 네트워크 오류
    ├── ServerFailure       # 서버 오류
    ├── CacheFailure        # 캐시 오류
    └── ValidationFailure   # 유효성 검사 오류

Data Layer
└── Exception (Firebase)
    ├── FirebaseAuthException
    ├── FirebaseFirestoreException
    └── FirebaseStorageException
```

### 5.2 에러 처리 흐름

```
try {
  // Firebase Call
} catch (e) {
  // Convert Exception → Failure
  return Left(NetworkFailure());
}
// UI에 표시
```

---

## 6. 보안 설계

### 6.1 인증 & 인가

```
JWT Token 구조:
Header:
  {
    "alg": "HS256",
    "typ": "JWT"
  }

Payload:
  {
    "user_id": "user123",
    "email": "user@example.com",
    "iat": 1234567890,
    "exp": 1234654290  // 24시간
  }

Signature:
  HMACSHA256(base64(header) + "." + base64(payload), secret)
```

### 6.2 데이터 보안

```
Firebase Security Rules:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users only read/write own data
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }

    // Reservations
    match /reservations/{reservation} {
      allow read: if request.auth.uid == resource.data.user_id;
      allow create: if request.auth.uid != null;
    }
  }
}
```

---

## 7. 성능 최적화

### 7.1 캐싱 전략

```
Local Cache Hierarchy:
┌─────────────────────────┐
│  In-Memory Cache (RAM)  │  ← 가장 빠름, 휘발성
│  (Provider 상태)        │
└─────────────────────────┘
            ↓
┌─────────────────────────┐
│  Persistent Cache       │  ← 중간, 영구 저장
│  (SQLite/Hive)         │
└─────────────────────────┘
            ↓
┌─────────────────────────┐
│  Firebase Realtime DB   │  ← 가장 느림, 신뢰성 높음
│  (Cloud)               │
└─────────────────────────┘
```

### 7.2 이미지 최적화

```
Image Loading Strategy:
1. 썸네일 표시 (blur placeholder)
2. 낮은 해상도 로드 (progressive)
3. 원본 이미지 캐싱 (background)
4. 고해상도 표시 (final)
```

---

## 8. 테스트 전략

### 8.1 단위 테스트

```
test/domain/usecases/
├── login_usecase_test.dart
├── search_cafe_usecase_test.dart
└── create_reservation_usecase_test.dart

테스트 대상: Pure Functions (Domain Layer)
```

### 8.2 통합 테스트

```
test/data/repositories/
├── auth_repository_test.dart
├── cafe_repository_test.dart
└── reservation_repository_test.dart

테스트 대상: Repository + DataSource
```

### 8.3 Widget 테스트

```
test/presentation/screens/
├── login_screen_test.dart
├── home_screen_test.dart
└── search_screen_test.dart

테스트 대상: UI Rendering + Interaction
```

---

## 9. 배포 아키텍처

### 9.1 CI/CD 파이프라인

```
Developer Push
    ↓
GitHub Actions Trigger
    ↓
├── Run Tests
├── Build Lint Check
└── Build APK/IPA
    ↓
Test Pass?
    ├─ Yes → Deploy to Play Store/App Store
    └─ No → Notify Developer
```

---

## 10. 확장성 고려사항

### 10.1 마이크로서비스 전환 (향후)

```
현재 (단일 Firebase):
Frontend ←→ Firebase

향후 (마이크로서비스):
Frontend ←→ API Gateway
                ├→ Auth Service
                ├→ Cafe Service
                ├→ Reservation Service
                └→ Review Service
```

---

**작성자**: 성아영  
**검토자**: 팀 리더  
**승인 상태**: ✅ 완료
