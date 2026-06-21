# ADR-0003: 아키텍처 패턴 — Layered + MVVM

**상태**: ✅ **승인됨** (2024.11.15)  
**결정자**: [당신의 이름]  
**날짜**: 2024.11.15

---

## 🎯 문제 정의 (Context)

CafeHub 아키텍처는 다음을 만족해야 합니다:

1. **1명 개발**: 복잡한 구조 X, 이해하기 쉬워야 함
2. **6주 기한**: 학습 시간 최소화
3. **테스트 가능**: 나중에 버그 찾기 쉬워야 함
4. **확장성**: 기능 추가 시 영향 최소화
5. **유지보수**: 코드 읽기 쉬워야 함

---

## 💭 고려한 대안들

### 1️⃣ Layered + MVVM (선택) ⭐

**개념**:
```
┌─────────────────────────────┐
│  Presentation Layer         │
│  ├─ UI (Screens)           │
│  └─ ViewModel              │
├─────────────────────────────┤
│  Domain Layer               │
│  ├─ Entities               │
│  └─ Use Cases              │
├─────────────────────────────┤
│  Data Layer                 │
│  ├─ Repositories           │
│  ├─ Data Sources           │
│  └─ Models                 │
└─────────────────────────────┘
```

**장점 ✅**:

**학습 곡선 낮음**
```
Clean Architecture: 5-7시간
MVVM: 2-3시간 ✅ 선택
MVC: 1시간 (하지만 테스트 어려움)
```

**테스트 용이**
```
// LoginViewModel 테스트
test('로그인 성공 시 홈화면으로 이동', () {
  final viewModel = LoginViewModel(mockRepository);
  viewModel.login('user@example.com', 'password');
  
  expect(viewModel.state, isA<LoginSuccess>());
  expect(viewModel.nextRoute, equals('/home'));
});
```

**책임 분리 명확**
```
UI 변경 → Presentation Layer만 수정
비즈니스 로직 변경 → Domain Layer 수정
데이터 소스 변경 → Data Layer만 수정
```

**확장성 우수**
```
새로운 화면 추가?
1. Screen 생성
2. ViewModel 생성
3. Repository 호출
4. 완료!

다른 계층은 영향 없음
```

**단점 ❌**:
- **보일러플레이트**: 파일 많음 (model, entity, repository 등)
- **초기 설정**: 폴더 구조 복잡함
- **과설계**: 작은 기능에도 3-4개 파일 필요할 수 있음

**6주 프로젝트 적합성**: ⭐⭐⭐⭐☆ (4/5)

---

### 2️⃣ Clean Architecture

**개념**:
```
┌─────────────────────────────┐
│  Presentation              │
├─────────────────────────────┤
│  Domain (비즈니스 로직)      │
├─────────────────────────────┤
│  Data                       │
├─────────────────────────────┤
│  Infrastructure             │
└─────────────────────────────┘
```

**장점 ✅**:
- **최고의 테스트성**: 모든 계층 분리
- **재사용성**: 도메인 로직이 프레임워크 독립적
- **장기 유지보수**: 매우 우수
- **업계 표준**: 큰 프로젝트의 표준

**단점 ❌**:
- **학습 곡선**: 5-7시간 필요
- **보일러플레이트**: 매우 많음 (file 40개+)
- **과설계**: 6주 프로젝트에는 오버엔지니어링
- **생산성**: 초기 1-2주 설정에 시간 소요

**예상 구조**:
```
lib/
├─ presentation/
│  ├─ pages/
│  ├─ widgets/
│  ├─ bloc/ (또는 viewmodel/)
│  └─ routes/
├─ domain/
│  ├─ entities/
│  ├─ repositories/
│  └─ usecases/
├─ data/
│  ├─ datasources/
│  ├─ models/
│  └─ repositories/
├─ core/
│  ├─ constants/
│  ├─ error/
│  ├─ usecase/
│  └─ network/
└─ main.dart

총 40-50개 파일
```

**6주 프로젝트 적합성**: ⭐⭐☆☆☆ (2/5)
→ 너무 무거움

---

### 3️⃣ MVVM Only (아키텍처 X)

**개념**:
```
┌──────────────┐
│  UI (View)   │  ← 화면
├──────────────┤
│  ViewModel   │  ← 상태 관리
├──────────────┤
│  Model       │  ← 데이터
└──────────────┘
```

**장점 ✅**:
- **간단함**: 매우 빠른 학습 (1시간)
- **파일 적음**: 20-30개 파일
- **빠른 개발**: 1-2주 내 모든 화면 완성 가능

**단점 ❌**:
- **테스트 어려움**: View와 로직 분리 어려움
- **복잡도 증가**: 크고 복잡한 ViewModel 발생
- **재사용성 낮음**: 로직이 ViewModel에 고착
- **나중에 리팩토링 필요**: 기술 부채 증가

**예상 구조**:
```
lib/
├─ screens/
│  ├─ login_screen.dart
│  ├─ map_screen.dart
│  └─ ...
├─ viewmodels/
│  ├─ login_viewmodel.dart
│  ├─ map_viewmodel.dart
│  └─ ...
├─ models/
│  ├─ cafe_model.dart
│  ├─ booking_model.dart
│  └─ ...
└─ main.dart

총 20-30개 파일
```

**6주 프로젝트 적합성**: ⭐⭐⭐☆☆ (3/5)
→ 빠르지만 나중에 문제

---

### 4️⃣ MVC (Model-View-Controller)

**개념**:
```
┌─────────┐
│  View   │ ← UI
└────┬────┘
     ↓
┌──────────┐
│ Controller│ ← 비즈니스 로직
└────┬─────┘
     ↓
┌─────────┐
│  Model  │ ← 데이터
└─────────┘
```

**장점 ✅**:
- **매우 간단**: 배우기 쉬움 (30분)
- **빠른 프로토타입**: MVP 빠르게 만들 수 있음

**단점 ❌**:
- **테스트 거의 불가능**: View와 Logic 강결합
- **코드 스파게티**: Controller가 점점 커짐
- **유지보수 악**: 6주 뒤 코드 읽기 어려움

**6주 프로젝트 적합성**: ⭐☆☆☆☆ (1/5)
→ 학습 후 고생

---

## 📊 의사결정 매트릭스

| 평가 기준 | 가중치 | Layered+MVVM | Clean Arch | MVVM Only | MVC |
|---------|--------|---|---|---|---|
| 학습 시간 | 20% | 9 | 3 | 10 | 10 |
| 테스트 용이성 | 25% | 8 | 10 | 5 | 1 |
| 6주 완성 가능 | 25% | 9 | 4 | 9 | 8 |
| 코드 유지보수 | 20% | 8 | 9 | 4 | 2 |
| 확장성 | 10% | 8 | 10 | 5 | 2 |
| **총점** | **100%** | **8.4** | **6.1** | **6.8** | **3.2** |

✅ **Layered + MVVM 최고**

---

## ✅ 최종 결정: **Layered + MVVM**

### 선택 이유

#### 1️⃣ "학습 곡선 최소" (20%)
```
Clean Architecture: 7시간 → 너무 김
Layered+MVVM: 3시간 → 적당 ✅
MVVM Only: 2시간 → 짧지만 나중 문제
```

**결론**: "3시간 투자로 평생 이득" (나중에 Clean Arch로 마이그레이션 가능)

#### 2️⃣ "테스트 가능" (25% - 가장 중요)
```
MVC:
test('로그인', () {
  // View와 Controller 섞여있어서
  // 테스트 거의 불가능
  // ❌ 포기
});

MVVM Only:
test('로그인', () {
  final viewModel = LoginViewModel(mockRepository);
  viewModel.login('user', 'pass');
  expect(viewModel.state, isLoginSuccess);
  // ✅ 간단하지만
  // ViewModel이 계속 커짐
});

Layered+MVVM:
test('로그인', () {
  final useCase = LoginUseCase(mockRepository);
  final result = useCase.execute('user', 'pass');
  expect(result, isSuccess);
  // ✅ 각 계층 독립적으로 테스트 가능!
});
```

#### 3️⃣ "6주 내 완성" (25%)
```
Clean Architecture:
- 1주차: 아키텍처 이해 & 설정 (버려지는 주!)
- 2-6주차: 개발
- 결과: 개발 시간 5주 = 기능 축소 필수

Layered+MVVM:
- 1-2일: 이해 & 설정
- 6주: 개발에 집중
- 결과: 개발 시간 6주 = 기능 완전 구현 ✅
```

#### 4️⃣ "나중에 업그레이드 가능"
```
Layered+MVVM → Clean Architecture 마이그레이션:
- 새로운 Infrastructure Layer 추가
- 나머지는 그대로 (Breaking change X)
- 비용: 3-5일

반대:
- MVC → Layered 마이그레이션: 2-3주 (전체 리팩토링)
```

---

## 🏗️ CafeHub 폴더 구조

```
cafehub-app/
│
├─ lib/
│
│  ├─ main.dart
│  │
│  ├─ presentation/              📱 Presentation Layer
│  │  ├─ screens/
│  │  │  ├─ auth/
│  │  │  │  ├─ login_screen.dart
│  │  │  │  └─ signup_screen.dart
│  │  │  ├─ map/
│  │  │  │  └─ map_screen.dart
│  │  │  ├─ cafe/
│  │  │  │  └─ cafe_detail_screen.dart
│  │  │  ├─ booking/
│  │  │  │  └─ booking_screen.dart
│  │  │  └─ review/
│  │  │     └─ review_screen.dart
│  │  │
│  │  ├─ widgets/
│  │  │  ├─ cafe_card.dart
│  │  │  ├─ rating_bar.dart
│  │  │  └─ ...
│  │  │
│  │  └─ viewmodels/
│  │     ├─ auth_viewmodel.dart
│  │     ├─ map_viewmodel.dart
│  │     ├─ cafe_viewmodel.dart
│  │     ├─ booking_viewmodel.dart
│  │     └─ review_viewmodel.dart
│  │
│  ├─ domain/                    ⚙️ Domain Layer
│  │  ├─ entities/
│  │  │  ├─ cafe.dart
│  │  │  ├─ booking.dart
│  │  │  ├─ user.dart
│  │  │  └─ review.dart
│  │  │
│  │  └─ repositories/
│  │     ├─ auth_repository.dart (추상)
│  │     ├─ cafe_repository.dart (추상)
│  │     └─ booking_repository.dart (추상)
│  │
│  └─ data/                      💾 Data Layer
│     ├─ datasources/
│     │  ├─ firebase_datasource.dart
│     │  ├─ maps_datasource.dart
│     │  └─ stripe_datasource.dart
│     │
│     ├─ models/
│     │  ├─ cafe_model.dart (Entity → Model)
│     │  ├─ booking_model.dart
│     │  └─ review_model.dart
│     │
│     └─ repositories/
│        ├─ auth_repository_impl.dart
│        ├─ cafe_repository_impl.dart
│        └─ booking_repository_impl.dart
│
├─ pubspec.yaml
├─ README.md
└─ .gitignore
```

**총 파일 수**: 약 35-40개 (Clean Arch 50개보다 적음, MVC 25개보다 많음)

---

## 🔄 데이터 흐름

### 예시: 카페 검색

```
1️⃣ 사용자 입력 (UI)
   SearchScreen: "강남역" 입력
   
2️⃣ ViewModel 호출 (State Management)
   SearchViewModel.searchCafes("강남역")
   
3️⃣ Use Case 실행 (비즈니스 로직)
   SearchCafesUseCase.execute("강남역")
   
4️⃣ Repository 호출 (추상화)
   CafeRepository.searchByLocation()
   
5️⃣ Data Source (구현)
   FirebaseDatasource.queryCafes()
   
6️⃣ Firebase 호출
   GET /cafes?location=강남역
   
7️⃣ 역순으로 데이터 반환
   List<Cafe> → ViewModel → UI 업데이트
```

**장점**:
- 각 계층이 명확
- 테스트시 Repository만 Mock으로 바꿈
- Firebase를 Firestore로 바꾸려면? Data Layer만 수정!

---

## ⚠️ 위험 및 대응

| 위험 | 확률 | 대응 |
|-----|------|------|
| 초기 구조 설계 시간 | 중간 | 1-2일 투자 후 계속 진행 |
| 파일 많아지기 | 낮음 | 계층별로 organizing (폴더 구조) |
| 과설계 위험 | 중간 | "3단계 깊이"만 유지 |
| 팀원 이해 | N/A | 개인 프로젝트 (자신만 이해하면 OK) |

---

## ✅ 검증 기준

다음을 확인했으면 좋은 결정:

- [x] Provider 패키지 검토 → 공식 유지보수 중, 한국 자료 많음
- [x] Flutter MVVM 예제 검토 → 많고 좋음
- [x] 테스트 코드 작성 시뮬레이션 → 가능함
- [x] 6주 일정 시뮬레이션 → 충분함

---

## 📚 참고 자료

- [Flutter Architecture Samples](https://github.com/flutter/samples)
- [Provider 공식 문서](https://pub.dev/packages/provider)
- [Flutter MVVM 가이드](https://www.youtube.com/watch?v=kDEflqVtkqM)
- [Clean Architecture in Flutter (심화)](https://resocoder.com/flutter-clean-architecture)

---

## 🎬 다음: Clean Architecture로 마이그레이션

6개월 후 필요하면:
```
Domain Layer
├─ entities/
├─ repositories/
└─ usecases/  ← New!

Data Layer
├─ datasources/
├─ models/
└─ repositories_impl/

Presentation Layer
└─ ... (유지)

Infrastructure Layer  ← New!
├─ network/
├─ local/
└─ ...
```

가능하고 부담 없음!

---

**결정 일자**: 2024.11.15  
**검토자**: [교수 이름]  
**상태**: ✅ 승인됨  
**마이그레이션 계획**: 2025년 Q2 (선택)
