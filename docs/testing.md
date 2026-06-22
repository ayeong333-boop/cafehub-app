# 🧪 CafeHub 테스트 전략 (TESTING)

**목표**: 높은 품질의 안정적인 앱 제공

---

## 1. 테스트 전략

### 1.1 테스트 피라미드

```
        △ E2E 테스트 (5%)
       / \
      /   \ 통합 테스트 (15%)
     /     \
    /       \ 단위 테스트 (80%)
   /_________\
```

### 1.2 테스트 종류

| 타입 | 범위 | 속도 | 개수 |
|------|------|------|------|
| 단위 테스트 | 함수/클래스 | 매우 빠름 | 많음 |
| 통합 테스트 | 모듈/기능 | 보통 | 보통 |
| E2E 테스트 | 전체 앱 | 느림 | 적음 |

---

## 2. 단위 테스트

### 2.1 Domain Layer 테스트

```dart
test/domain/usecases/
├── login_usecase_test.dart
├── search_cafe_usecase_test.dart
└── create_reservation_usecase_test.dart
```

### 2.2 Data Layer 테스트

```dart
test/data/repositories/
├── auth_repository_test.dart
├── cafe_repository_test.dart
└── review_repository_test.dart
```

---

## 3. 테스트 실행

```bash
# 모든 테스트 실행
flutter test

# 특정 파일 테스트
flutter test test/domain/usecases/login_usecase_test.dart

# 커버리지 생성
flutter test --coverage
```

---

## 4. 테스트 케이스

### 4.1 로그인 기능

```
✓ 유효한 credentials로 로그인 성공
✓ 잘못된 password로 로그인 실패
✓ 존재하지 않는 user로 로그인 실패
✓ JWT 토큰 발급 확인
```

### 4.2 검색 기능

```
✓ 키워드로 카페 검색
✓ 필터 적용 검색
✓ 빈 결과 처리
✓ 검색 속도 (1초 이내)
```

---

## 5. 성능 테스트

```bash
# 앱 시작 시간 측정
time flutter run --release

# 메모리 사용량 모니터링
flutter run --profile

# DevTools에서 성능 분석
flutter pub global activate devtools
devtools
```

---

## 6. 보안 테스트

```
✓ 비밀번호 암호화 저장 확인
✓ JWT 토큰 유효성 검사
✓ Firebase 보안 규칙 검증
✓ 민감 정보 로깅 금지
```

---

**상태**: ✅ 완료
