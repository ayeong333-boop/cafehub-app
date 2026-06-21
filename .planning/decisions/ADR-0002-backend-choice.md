# ADR-0002: Backend 선택 — Firebase

**상태**: ✅ **승인됨** (2024.11.15)  
**결정자**: [당신의 이름]  
**날짜**: 2024.11.15

---

## 🎯 문제 정의 (Context)

CafeHub 백엔드는 다음을 만족해야 합니다:

1. **개발 시간**: 서버 개발에 시간 쓰지 말기 (프론트 우선)
2. **인증**: 회원가입/로그인 빠르게 구현
3. **실시간 업데이트**: 예약 상황, 리뷰 실시간 동기화
4. **파일 저장**: 사진 업로드 (Firebase Storage)
5. **확장성**: 사용자 100명 동시 접속 지원
6. **비용**: 무료/저가 서비스 (학생 프로젝트)
7. **배포 복잡도**: DevOps 학습 최소화

---

## 💭 고려한 대안들

### 1️⃣ Firebase (선택) ⭐

**장점 ✅**:

**개발 속도**
- 인증, DB, 저장소 **통합 제공** → 각각 1-2일씩 절약 (총 5-7일)
- 백엔드 코드 거의 안 씀 → 프론트에 집중 가능
- Firestore가 실시간 DB → Subscription으로 자동 동기화

**비용**
- 무료 Tier: 월 1GB 읽기, 1GB 쓰기, 1GB 저장소
- CafeHub 규모면 **충분** (50개 카페, 100명 사용자 기준)
- 가격 계산:
  ```
  예상 비용 (월): $0 (무료 Tier)
  실제 비용: $0-5 (초과분 미미)
  ```

**보안**
- Google에서 관리 → 보안 업데이트 자동
- Firestore 보안 규칙 → SQL 주입 불가능
- SSL/TLS 자동 → HTTPS 기본

**인증**
- `firebase_auth` 패키지로 5줄 코드:
  ```dart
  await FirebaseAuth.instance.createUserWithEmailAndPassword(
    email: email,
    password: password,
  );
  ```
- Google, Apple, Facebook 인증 **즉시 추가 가능**

**실시간 기능**
- Firestore Listeners:
  ```dart
  FirebaseFirestore.instance.collection('bookings')
    .snapshots()
    .listen((snapshot) {
      // 예약 변경 즉시 반영
    });
  ```

**파일 저장**
- Firebase Storage:
  ```dart
  await FirebaseStorage.instance
    .ref('reviews/img_001.jpg')
    .putFile(file);
  ```

**단점 ❌**:
- **벤더 락인**: Firebase 나가려면 마이그레이션 어려움
- **쿼리 제한**: SQL처럼 복잡한 쿼리 어려움 (NoSQL이라 당연)
- **비용**: 대규모 앱되면 비쌈 (월 수백 달러 가능)
- **커스터마이징 한계**: 특이한 로직은 Cloud Functions 필요

**6주 프로젝트 적합성**: ⭐⭐⭐⭐⭐ (5/5)

---

### 2️⃣ AWS (Amplify)

**장점 ✅**:
- **강력함**: AppSync (GraphQL) + DynamoDB (NoSQL) + S3 (파일)
- **확장성**: 대규모 애플리케이션도 지원
- **유연성**: 커스터마이징 많음
- **업계 표준**: AWS 경험 쌓기 좋음

**단점 ❌**:
- **설정 복잡**: IAM, VPC, API Gateway 등 많음
- **학습 곡선**: 높음 (2-3주 필요)
- **개발 속도**: Firebase보다 느림
- **비용**: 무료 Tier 1년 뒤부터 유료 (월 10-20달러)
- **DevOps**: 자동 스케일링 설정 복잡

**6주 프로젝트 적합성**: ⭐⭐☆☆☆ (2/5)

---

### 3️⃣ 자체 서버 구축 (Node.js + MongoDB)

**장점 ✅**:
- **완전 제어**: 원하는 대로 커스터마이징
- **학습 가치**: 백엔드 개발 경험
- **저비용**: 초기에는 무료 (Heroku, Railway 등)

**단점 ❌**:
- **개발 시간 3배**: 인증, DB 설계, API 개발 모두 직접
  ```
  Firebase: 2-3일
  자체 서버: 8-10일
  ```
- **DevOps**: 배포, 모니터링, 로그 관리 필요
- **확장성**: 트래픽 늘면 스케일링 직접 해야 함
- **보안**: 데이터 암호화, 인증 직접 구현 → 버그 위험

**예상 일정**:
```
환경 설정: 2-3일
인증: 2-3일
API 개발: 5-7일
배포: 2-3일
─────────────────
총: 11-16일 (플랜이 남지 않음!)
```

**6주 프로젝트 적합성**: ❌ (시간 부족)

---

### 4️⃣ Supabase (PostgreSQL + Auth)

**장점 ✅**:
- **Firebase 대안**: Firebase처럼 간단하면서도 SQL 사용
- **무료 Tier**: 충분함 (500MB, 월 5,000건 API 호출)
- **SQL**: 복잡한 쿼리 가능

**단점 ❌**:
- **실시간**: Firebase만큼 쉽지 않음
- **새로운 서비스**: 커뮤니티 작음, 한국 자료 부족
- **파일 저장**: 별도 설정 필요 (Firebase는 자동)

**6주 프로젝트 적합성**: ⭐⭐⭐☆☆ (3/5)

---

## 📊 의사결정 매트릭스

| 평가 기준 | 가중치 | Firebase | AWS | 자체서버 | Supabase |
|---------|--------|----------|-----|---------|---------|
| 개발 속도 | 35% | 10 | 4 | 2 | 7 |
| 비용 | 20% | 10 | 5 | 4 | 9 |
| 보안 | 20% | 10 | 9 | 5 | 8 |
| 실시간 기능 | 15% | 10 | 7 | 6 | 7 |
| 학습 난이도 | 10% | 9 | 3 | 4 | 6 |
| **총점** | **100%** | **9.8** | **5.5** | **3.7** | **7.4** |

✅ **Firebase 압도적 승리**

---

## ✅ 최종 결정: **Firebase**

### 선택 이유

#### 1️⃣ "개발 속도" 최우선 (35%)
```
Firebase: 
  - Auth 1일
  - Firestore 1일
  - Storage 1일
  - 총 3일 + 통합 1일 = 4일
  
자체 서버:
  - 환경 2일
  - 인증 3일
  - API 5일
  - 배포 2일
  - 총 12일 (3배!)
```

**결론**: Firebase는 4일, 자체 서버는 12일  
→ 8일 절약 = 중간발표 deadline 여유

#### 2️⃣ "프론트엔드 집중" 가능
```
Firebase 선택:
- 백엔드 코드 거의 0줄
- Firestore 규칙만 설정 (5분)
- 나머지 6주 전부 Flutter에 집중

자체 서버 선택:
- 3주는 서버 개발
- 3주만 프론트
- 앱 품질 낮음
```

#### 3️⃣ "실시간" 기능 무료
```
Firebase Firestore:
- 변경사항 자동 감지
- 리스너 추가 = 1줄 코드
- 예약 상황 즉시 동기화

자체 서버:
- WebSocket 또는 Polling 구현 필요
- 추가 2-3일 소요
- 버그 위험
```

#### 4️⃣ "무료" → 비용 제로
```
예상 사용량 (월):
- 읽기: 50 카페 × 100명 × 10회 = 50,000건 ✅ 무료 범위 (1GB)
- 쓰기: 50명 × 2회 = 100건 ✅ 무료 범위
- 저장소: 50개 카페 × 3사진 = 150개 사진 ≈ 300MB ✅ 무료 범위

결론: 완전 무료 (Firebase 무료 Tier에 100% 포함)
```

#### 5️⃣ "보안" 자동
```
Firebase:
- Google 관리
- SSL/TLS 기본
- DDoS 방어 자동
- 데이터 암호화 기본

자체 서버:
- 직접 관리 필요
- SSL 인증서 설치 필요
- 보안 업데이트 모니터링 필요
- 버그로 인한 데이터 유출 위험
```

---

## 🚀 구현 계획

### 아키텍처

```
┌──────────────────────────────────────┐
│         Flutter 앱                    │
├──────────────────────────────────────┤
│                                      │
│  UI Layer (Screens)                  │
│  ├─ login_screen.dart               │
│  ├─ map_screen.dart                 │
│  └─ booking_screen.dart             │
│                                      │
│  ViewModel (State Management)        │
│  ├─ auth_viewmodel.dart             │
│  └─ booking_viewmodel.dart          │
│                                      │
│  Repository (Data Abstraction)       │
│  ├─ firebase_repository.dart        │
│  └─ firestore_repository.dart       │
│                                      │
└──────────────────────────────────────┘
           ↓
┌──────────────────────────────────────┐
│         Firebase                     │
├──────────────────────────────────────┤
│                                      │
│  Authentication (회원가입/로그인)    │
│  ├─ Email/Password                  │
│  └─ Google OAuth                    │
│                                      │
│  Firestore (실시간 NoSQL DB)        │
│  ├─ /cafes (카페 정보)              │
│  ├─ /users (사용자)                 │
│  ├─ /bookings (예약)                │
│  └─ /reviews (리뷰)                │
│                                      │
│  Storage (파일 저장)                │
│  └─ /reviews/{reviewId}/images      │
│                                      │
│  Security Rules (접근 제어)         │
│  └─ 인증된 사용자만 쓰기 가능       │
│                                      │
└──────────────────────────────────────┘
```

### Firestore 데이터 구조

```
cafehub-prod (프로젝트)
│
├─ users/ (사용자)
│  ├─ {userId}
│  │  ├─ email: "user@example.com"
│  │  ├─ name: "John Doe"
│  │  ├─ profileImage: "gs://..."
│  │  └─ createdAt: 2024-11-15
│  └─ ...
│
├─ cafes/ (카페 정보)
│  ├─ cafe001
│  │  ├─ name: "YC 카페"
│  │  ├─ address: "서울 강남구..."
│  │  ├─ location: GeoPoint(37.497, 127.027)
│  │  ├─ rating: 4.7
│  │  ├─ amenities: ["WiFi", "충전"]
│  │  ├─ images: ["gs://...", "gs://..."]
│  │  └─ totalSeats: 20
│  └─ ...
│
├─ bookings/ (예약 내역)
│  ├─ booking001
│  │  ├─ userId: "user123"
│  │  ├─ cafeId: "cafe001"
│  │  ├─ date: "2024-11-20"
│  │  ├─ time: "14:00"
│  │  ├─ people: 2
│  │  ├─ status: "confirmed"
│  │  ├─ totalPrice: 8000
│  │  └─ createdAt: 2024-11-15
│  └─ ...
│
└─ reviews/ (리뷰)
   ├─ review001
   │  ├─ userId: "user123"
   │  ├─ cafeId: "cafe001"
   │  ├─ rating: 5
   │  ├─ comment: "정말 좋았어요!"
   │  ├─ images: ["gs://...", "gs://..."]
   │  └─ createdAt: 2024-11-20
   └─ ...
```

---

## ⚠️ 위험 및 대응

| 위험 | 확률 | 대응 |
|-----|------|------|
| Firebase API 한계 | 낮음 | 복잡한 쿼리는 Cloud Functions로 해결 |
| 실시간 동기화 지연 | 중간 | 로컬 캐싱 + 낙관적 UI 업데이트 |
| 데이터 비용 초과 | 낮음 | 초기 설계 단계에 비용 시뮬레이션 |
| 규칙 설정 오류 | 중간 | 단위 테스트로 규칙 검증 |

---

## ✅ 검증 기준

다음을 확인했으면 좋은 결정:

- [x] Firebase 가격 계산기 확인 → 무료 Tier 충분
- [x] Firestore 규모 제한 확인 → 문제 없음
- [x] firebase_flutter 패키지 안정성 확인 → 공식 유지보수 중
- [x] Google Cloud Console 접근 확인 → OK
- [x] Stripe + Firebase 호환성 확인 → 문제 없음

---

## 📚 참고 자료

- [Firebase 공식 문서](https://firebase.google.com/docs)
- [Firestore 가격](https://firebase.google.com/pricing)
- [Firebase for Flutter](https://firebase.flutter.dev/)
- [Firestore 설계 가이드](https://firebase.google.com/docs/firestore/best-practices)
- [Firestore 보안 규칙](https://firebase.google.com/docs/firestore/security/start)

---

**결정 일자**: 2024.11.15  
**검토자**: [교수 이름]  
**상태**: ✅ 승인됨
