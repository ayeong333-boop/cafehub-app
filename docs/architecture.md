# CafeHub — 아키텍처 설계

> **목표**: 새로운 기능 추가 시 어디에 파일을 만들지 알 수 있어야 합니다.

---

## 🏗️ **전체 아키텍처 (4-Layer Pattern)**

```
┌─────────────────────────────────────────────┐
│         PRESENTATION LAYER                  │
│  (UI • Screens • Widgets • Theme)          │
├─────────────────────────────────────────────┤
│       APPLICATION LAYER                     │
│  (ViewModel • State Management)             │
├─────────────────────────────────────────────┤
│         DOMAIN LAYER                        │
│  (Entities • Business Rules • Services)     │
├─────────────────────────────────────────────┤
│          DATA LAYER                         │
│  (Repositories • API • Database)            │
└─────────────────────────────────────────────┘
```

---

## 📊 **각 계층별 역할**

### **1️⃣ Presentation Layer** (사용자가 보는 것)

**위치**: `lib/presentation/`

**책임**:
- 화면 UI 그리기
- 사용자 입력 받기
- 상태 표시

**파일 예시**:

```
presentation/
├── screens/
│   ├── home_screen.dart (홈 화면)
│   ├── cafe_detail_screen.dart (카페 상세)
│   ├── booking_screen.dart (예약 화면)
│   └── review_screen.dart (리뷰 작성)
│
├── widgets/
│   ├── cafe_card.dart (카페 카드)
│   ├── rating_bar.dart (평점 표시)
│   └── booking_form.dart (예약 폼)
│
└── theme/
    └── app_theme.dart (색상, 폰트)
```

**규칙**:
- ✅ DO: 버튼, 텍스트, 이미지 표시
- ✅ DO: ViewModel에서 상태 받아서 표시
- ❌ DON'T: API 직접 호출
- ❌ DON'T: 데이터 계산/변환

---

### **2️⃣ Application Layer** (상태 관리)

**위치**: `lib/application/viewmodels/`

**책임**:
- 화면 상태 관리
- UI 로직 처리
- Domain Layer 호출

**파일 예시**:

```
application/
└── viewmodels/
    ├── home_viewmodel.dart
    │   ├── searchCafes(keyword)
    │   ├── filterByDistance()
    │   └── navigateToDetail()
    │
    ├── booking_viewmodel.dart
    │   ├── selectDate()
    │   ├── selectTime()
    │   └── submitBooking()
    │
    └── review_viewmodel.dart
        ├── submitReview()
        └── uploadImage()
```

**규칙**:
- ✅ DO: UI 상태 관리 (로딩, 에러, 데이터)
- ✅ DO: UseCase 호출
- ❌ DON'T: 직접 API 호출
- ❌ DON'T: UI 렌더링

**코드 예시**:

```dart
class HomeViewModel extends ChangeNotifier {
  final SearchCafesUseCase _searchCafesUseCase;
  
  List<Cafe> cafes = [];
  bool isLoading = false;
  
  void searchCafes(String keyword) async {
    isLoading = true;
    notifyListeners();
    
    // Domain UseCase 호출 (비즈니스 로직)
    final result = await _searchCafesUseCase.execute(keyword);
    
    cafes = result;
    isLoading = false;
    notifyListeners();
  }
}
```

---

### **3️⃣ Domain Layer** (핵심 규칙)

**위치**: `lib/domain/`

**책임**:
- 비즈니스 로직
- 데이터 검증
- UseCase 정의

**파일 예시**:

```
domain/
├── entities/
│   ├── cafe_entity.dart (카페 데이터)
│   ├── booking_entity.dart (예약 데이터)
│   ├── review_entity.dart (리뷰 데이터)
│   └── user_entity.dart (사용자 데이터)
│
└── services/
    ├── search_cafes_usecase.dart
    │   ├── 조건 검증
    │   └── 검색 로직
    │
    ├── create_booking_usecase.dart
    │   ├── 예약 가능 여부 확인
    │   └── 예약 생성
    │
    └── submit_review_usecase.dart
        ├── 리뷰 검증
        └── 리뷰 저장
```

**규칙**:
- ✅ DO: 순수 Dart 코드 (Flutter 의존성 X)
- ✅ DO: 비즈니스 규칙 정의
- ✅ DO: Entity 정의 (모델의 틀)
- ❌ DON'T: Firebase, HTTP 직접 사용
- ❌ DON'T: UI 관련 코드

**Entity 예시**:

```dart
class CafeEntity {
  final String id;
  final String name;
  final double latitude;
  final double longitude;
  final double rating;
  final int totalReviews;
  
  CafeEntity({
    required this.id,
    required this.name,
    required this.latitude,
    required this.longitude,
    required this.rating,
    required this.totalReviews,
  });
}
```

---

### **4️⃣ Data Layer** (데이터 소스)

**위치**: `lib/data/`

**책임**:
- API 호출
- 데이터베이스 접근
- 데이터 변환

**파일 예시**:

```
data/
├── repositories/
│   ├── cafe_repository_impl.dart
│   │   ├── searchCafes()
│   │   └── getCafeDetail()
│   │
│   ├── booking_repository_impl.dart
│   │   ├── createBooking()
│   │   └── getBookingHistory()
│   │
│   └── review_repository_impl.dart
│       ├── submitReview()
│       └── getReviews()
│
├── datasources/
│   ├── firebase_datasource.dart (인증)
│   ├── firestore_datasource.dart (DB)
│   ├── maps_datasource.dart (지도)
│   └── stripe_datasource.dart (결제)
│
└── models/
    ├── cafe_model.dart (API응답 파싱)
    ├── booking_model.dart
    ├── review_model.dart
    └── user_model.dart
```

**규칙**:
- ✅ DO: Firebase, API 호출
- ✅ DO: JSON ↔ Dart 변환
- ✅ DO: Repository 구현
- ❌ DON'T: ViewModel이 직접 사용
- ❌ DON'T: 비즈니스 로직

**Model 예시**:

```dart
class CafeModel {
  final String id;
  final String name;
  final double rating;
  
  CafeModel({
    required this.id,
    required this.name,
    required this.rating,
  });
  
  // Firestore JSON → CafeModel
  factory CafeModel.fromFirestore(Map<String, dynamic> json) {
    return CafeModel(
      id: json['id'],
      name: json['name'],
      rating: json['rating'].toDouble(),
    );
  }
  
  // CafeModel → Entity (Domain으로 전달)
  CafeEntity toEntity() {
    return CafeEntity(
      id: id,
      name: name,
      rating: rating,
    );
  }
}
```

---

## 📂 **전체 폴더 구조**

```
cafehub-app/
├── lib/
│   ├── main.dart
│   ├── app.dart (앱 진입점)
│   │
│   ├── presentation/ (사용자가 보는 것)
│   │   ├── screens/
│   │   │   ├── home_screen.dart
│   │   │   ├── cafe_detail_screen.dart
│   │   │   ├── booking_screen.dart
│   │   │   └── review_screen.dart
│   │   ├── widgets/
│   │   │   ├── cafe_card.dart
│   │   │   └── rating_bar.dart
│   │   └── theme/
│   │       └── app_theme.dart
│   │
│   ├── application/ (상태 관리)
│   │   └── viewmodels/
│   │       ├── home_viewmodel.dart
│   │       ├── booking_viewmodel.dart
│   │       └── review_viewmodel.dart
│   │
│   ├── domain/ (핵심 규칙)
│   │   ├── entities/
│   │   │   ├── cafe_entity.dart
│   │   │   ├── booking_entity.dart
│   │   │   └── review_entity.dart
│   │   └── services/ (UseCase)
│   │       ├── search_cafes_usecase.dart
│   │       ├── create_booking_usecase.dart
│   │       └── submit_review_usecase.dart
│   │
│   └── data/ (데이터 소스)
│       ├── repositories/
│       │   ├── cafe_repository_impl.dart
│       │   ├── booking_repository_impl.dart
│       │   └── review_repository_impl.dart
│       ├── datasources/
│       │   ├── firebase_datasource.dart
│       │   ├── firestore_datasource.dart
│       │   └── maps_datasource.dart
│       └── models/
│           ├── cafe_model.dart
│           ├── booking_model.dart
│           └── review_model.dart
│
├── test/
│   └── widget_test.dart
│
├── pubspec.yaml
├── pubspec.lock
│
└── docs/
    ├── setup.md (설정 가이드)
    └── architecture.md (이 파일)
```

---

## 🔄 **데이터 흐름 (예: 카페 검색)**

```
1️⃣ 사용자 입력 (Presentation)
   SearchScreen: 사용자가 "강남역" 입력 & "검색" 버튼 클릭
   
2️⃣ ViewModel 호출
   HomeViewModel.searchCafes("강남역")
   
3️⃣ UseCase 실행 (Domain)
   SearchCafesUseCase.execute("강남역")
   ├─ 입력값 검증
   └─ 비즈니스 규칙 확인
   
4️⃣ Repository 호출 (Data)
   CafeRepository.searchByLocation("강남역")
   
5️⃣ Datasource 호출 (API/DB)
   FirestoreDatasource.queryCafes(...)
   
6️⃣ API 응답 (Firebase)
   ← List<CafeModel> 받음
   
7️⃣ 역순으로 변환 & 반환
   CafeModel → CafeEntity (Domain으로)
   
8️⃣ ViewModel 상태 업데이트
   HomeViewModel:
   - isLoading = false
   - cafes = List<Cafe>
   - notifyListeners()
   
9️⃣ UI 업데이트 (Presentation)
   SearchScreen:
   - cafes 리스트 표시
```

**코드로 보면:**

```dart
// 1. Presentation (UI)
SearchScreen(){
  onPressed: () {
    viewModel.searchCafes("강남역");  // ← ViewModel 호출
  }
}

// 2. Application (ViewModel)
class HomeViewModel {
  void searchCafes(String keyword) async {
    isLoading = true;
    cafes = await _searchUseCase.execute(keyword);  // ← UseCase 호출
    isLoading = false;
  }
}

// 3. Domain (UseCase)
class SearchCafesUseCase {
  Future<List<Cafe>> execute(String keyword) async {
    if (keyword.isEmpty) throw Exception("검색어 입력");
    return await _repository.search(keyword);  // ← Repository 호출
  }
}

// 4. Data (Repository)
class CafeRepositoryImpl {
  Future<List<Cafe>> search(String keyword) async {
    final models = await _datasource.queryCafes(keyword);  // ← API 호출
    return models.map((m) => m.toEntity()).toList();  // ← Model → Entity 변환
  }
}

// 5. Data (Datasource - 실제 API)
class FirestoreDatasource {
  Future<List<CafeModel>> queryCafes(String keyword) async {
    final snapshot = await FirebaseFirestore.instance
      .collection('cafes')
      .where('name', isGreaterThanOrEqualTo: keyword)
      .get();
    return snapshot.docs.map((d) => CafeModel.fromFirestore(d.data())).toList();
  }
}
```

---

## ✅ **새로운 기능 추가 체크리스트**

### **예: "찜하기" 기능 추가**

**1️⃣ Domain 계층**
```
□ FavoriteEntity 생성
□ FavoriteCafeUseCase 생성 (비즈니스 로직)
```

**2️⃣ Application 계층**
```
□ FavoriteViewModel 생성
□ addFavorite() 메서드
□ favorites 상태 관리
```

**3️⃣ Presentation 계층**
```
□ FavoriteScreen 생성
□ 하트 버튼 UI 추가
□ ViewModel 바인딩
```

**4️⃣ Data 계층**
```
□ FavoriteRepositoryImpl 생성
□ FirestoreDatasource에 saveFavorite() 추가
□ FavoriteModel 생성
```

**5️⃣ 테스트 & 검증**
```
□ 로컬 테스트
□ Firebase 데이터 확인
□ UI에서 동작 확인
```

---

## 🎯 **의존성 관계**

```
❌ 역방향 의존성 금지!

Presentation → Application ✅ (OK)
Application → Domain ✅ (OK)
Domain → Data ✅ (OK)

Application ← Data ❌ (NO!)
Presentation ← Domain ❌ (NO!)
```

---

## 📚 **참고 링크**

- [Clean Architecture 가이드](https://resocoder.com/flutter-clean-architecture)
- [MVVM 패턴](https://www.youtube.com/watch?v=kDEflqVtkqM)
- [Repository 패턴](https://refactoring.guru/design-patterns/repository)

---

**작성자**: [당신의 이름]  
**작성일**: 2024.11.22  
**버전**: 1.0
