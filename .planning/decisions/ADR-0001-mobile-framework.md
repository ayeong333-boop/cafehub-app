# ADR-0001: Mobile Framework — Flutter 선택

**상태**: ✅ **승인됨** (2024.11.15)  
**결정자**: [당신의 이름]  
**날짜**: 2024.11.15

---

## 🎯 문제 정의 (Context)

CafeHub는 다음 조건을 만족해야 합니다:

1. **시간 제한**: 6주 내 완성 (10~14주차)
2. **플랫폼**: iOS + Android **동시 배포**
3. **팀 규모**: 1명 (개인 프로젝트)
4. **개발 속도**: 빠른 프로토타입 개발 필수
5. **품질**: 사용 가능한 수준 이상

### 선택할 수 있는 옵션

| 기준 | Flutter | React Native | 네이티브 (Swift+Kotlin) |
|------|---------|-------------|------------------------|
| 개발 언어 | Dart | JavaScript | Swift + Kotlin |
| iOS 개발 경험 | ✅ 있음 | ✅ 있음 | ✅ 있음 |
| Android 개발 경험 | ❌ 없음 | ❌ 없음 | ✅ 있음 (Kotlin) |
| 학습 시간 | 1-2주 | 2-3주 | 네이티브는 불가능 (시간 X) |
| 코드 공유 | 95% | 70-80% | 0% (별도 개발) |
| 성능 | 좋음 | 중간 | 최고 |
| 커뮤니티 | 매우 활발 | 매우 활발 | 큼 |
| 배포 난이도 | 쉬움 | 중간 | 어려움 |
| 6주 완성 가능 | ✅ 가능 | ✅ 가능 | ❌ 불가능 |

---

## 💭 고려한 대안들

### 1️⃣ Flutter (선택)

**장점 ✅**:
- **빠른 개발 속도**: 핫 리로드 (3초 반영) → 개발 시간 30% 단축
- **단일 코드베이스**: 95% 코드 공유 (iOS/Android 동시 빌드)
- **강력한 UI 라이브러리**: Material + Cupertino 기본 제공
- **성능**: 네이티브에 가까운 성능 (60fps 쉽게 달성)
- **배포 간단**: 플러터 CLI로 원클릭 배포
- **커뮤니티**: Google 공식 지원, 한국 커뮤니티 활발
- **패키지 생태계**: pub.dev에 30만+ 패키지 (Firebase, Maps 지원)
- **회사 채택**: Google, BMW, Alibaba, eBay 등 대규모 앱 사용

**단점 ❌**:
- **Dart 학습**: 새로운 언어 학습 필요 (하지만 학습 곡선 낮음)
- **일부 네이티브 기능**: 복잡한 기능은 Platform Channel 필요할 수 있음
- **앱 크기**: 네이티브보다 큼 (~50MB)

**6주 프로젝트 적합성**: ⭐⭐⭐⭐⭐ (5/5)

---

### 2️⃣ React Native

**장점 ✅**:
- **JavaScript 기반**: 이미 알고 있는 언어 사용 가능
- **코드 공유**: 70-80% 코드 공유
- **큰 커뮤니티**: Meta 공식 지원
- **빠른 개발**: 핫 리로드 지원

**단점 ❌**:
- **성능 문제**: JavaScript 인터프리터 오버헤드 → 30fps 이하 가능성
- **라이브러리 불안정**: 자주 breaking change
- **네이티브 기능 필요시**: iOS/Android 코드 작성 필수
- **학습 곡선**: 그 쪽 생태계 학습 필요 (Expo vs Bare 선택 등)
- **배포 복잡도**: 네이티브 설정 많음

**6주 프로젝트 적합성**: ⭐⭐⭐☆☆ (3/5)

---

### 3️⃣ 네이티브 개발 (Swift + Kotlin)

**장점 ✅**:
- **최고 성능**: 60fps 안정적
- **최신 기능**: iOS/Android 최신 API 바로 사용 가능
- **앱 크기**: 최소화 가능

**단점 ❌**:
- **개발 시간 2배**: iOS와 Android 별도 개발 필요
- **코드 공유 불가**: 모든 로직 중복 작성
- **6주는 불가능**: 최소 12주 이상 필요
- **러닝커브**: 플랫폼별 다른 패러다임 학습

**6주 프로젝트 적합성**: ❌❌❌ (불가능)

---

## ✅ 최종 결정: **Flutter**

### 선택 이유

1. **시간**: "6주 내 배포" 달성 가능한 **유일한 선택**
   - 핫 리로드 덕분에 개발 속도 3배
   - iOS/Android 동시 코드 작성으로 50% 시간 절약

2. **기술 부채 최소**: Dart는 배우기 쉬움
   - TypeScript나 JavaScript와 유사한 문법
   - 1주일이면 기본 사용 가능

3. **Firebase 연동**: 공식 지원 + 최고 품질
   - `firebase_core`, `cloud_firestore`, `firebase_storage` 모두 well-maintained
   - 문서 한국어 지원 있음

4. **Google Maps**: 최고 품질 통합
   - `google_maps_flutter` 패키지 공식 지원
   - 성능 최적화 자체 처리

5. **배포 경험**: 나중에 기술 뽐낼 수 있음
   - Flutter 경험 → 이력서 매력적
   - 크로스플랫폼 개발 트렌드

---

## 📊 의사결정 매트릭스

| 평가 기준 | 가중치 | Flutter | React Native | 네이티브 |
|---------|--------|---------|-------------|---------|
| 6주 완성 가능 | 40% | 10 | 6 | 0 |
| 코드 공유 | 20% | 9.5 | 8 | 0 |
| 배포 난이도 | 20% | 9 | 5 | 3 |
| 커뮤니티 | 10% | 9 | 10 | 9 |
| 성능 | 10% | 8 | 6 | 10 |
| **총점** | **100%** | **9.3** | **6.8** | **1.5** |

✅ **Flutter 압도적 승리**

---

## 🚀 구현 계획

### 언제 사용?

```
11주차: Flutter 환경 설정 (1-2일)
- Flutter SDK 설치
- Android Studio / Xcode 설정
- 첫 프로젝트 생성

11-14주차: 앱 개발 (전 기간)
- Dart 학습과 병행 (처음 3-4일)
- 이후 풀 속도 개발
```

### 기본 패키지

```yaml
# pubspec.yaml

dependencies:
  flutter:
    sdk: flutter
  
  # 상태관리
  provider: ^6.0.0
  
  # 네트워킹 & Firebase
  firebase_core: ^2.0.0
  cloud_firestore: ^4.0.0
  firebase_auth: ^4.0.0
  firebase_storage: ^11.0.0
  google_maps_flutter: ^2.2.0
  
  # UI
  intl: ^0.18.0
  shimmer: ^3.0.0
  
  # 로컬 저장소
  hive: ^2.2.0
  hive_flutter: ^1.1.0
  
  # 결제
  stripe_flutter: ^8.0.0
  
  # 기타
  http: ^1.0.0
  geolocator: ^9.0.0
```

---

## ⚠️ 위험 및 대응

| 위험 | 확률 | 대응 |
|-----|------|------|
| Dart 학습 곡선 | 중간 | 처음 3일 집중 학습, 유튜브 강좌 활용 |
| 라이브러리 버전 이슈 | 낮음 | 안정적인 버전 선택 (latest 아님) |
| iOS 빌드 오류 | 중간 | 정기적 빌드 테스트 (매주) |
| 성능 최적화 | 중간 | 성능 프로파일러 활용, 14주차에 최적화 |

---

## ✅ 검증 기준

다음을 확인했으면 좋은 결정:

- [x] Flutter 공식 문서 검토 완료
- [x] Firebase + Flutter 호환성 확인
- [x] Google Maps Flutter 패키지 검토
- [x] 커뮤니티 활동도 확인 (활발함)
- [x] 팀원 동의 (개인 프로젝트이지만 확인)

---

## 📚 참고 자료

- [Flutter 공식 문서](https://flutter.dev/docs)
- [Flutter for Beginners (YouTube)](https://www.youtube.com/playlist?list=PLjxrf2q8roU3ahJVrSgAnPjzkpGQQcLu)
- [Dart 언어 가이드](https://dart.dev/guides)
- [Firebase + Flutter](https://firebase.flutter.dev/)
- [Google Maps for Flutter](https://pub.dev/packages/google_maps_flutter)

---

**결정 일자**: 2024.11.15  
**검토자**: [교수 이름]  
**상태**: ✅ 승인됨
