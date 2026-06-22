# 📦 CafeHub 배포 가이드 (DEPLOY)

**목표**: CafeHub 앱을 Play Store / App Store에 배포

---

## 1. Android 배포 (Google Play Store)

### 1.1 릴리스 빌드 생성

```bash
flutter build apk --release
flutter build appbundle --release
```

### 1.2 Google Play Console 설정

```
1. https://play.google.com/console 접속
2. "새 앱 만들기" 클릭
3. 앱명: CafeHub
4. 카테고리: 라이프스타일
5. 콘텐츠 등급 설정
```

### 1.3 APK 업로드

```
1. Play Console → 앱 → 출시 관리
2. 프로덕션 → 새 출시 만들기
3. APK 업로드
4. 앱 정보 입력 (스크린샷, 설명)
5. 출시 검토 대기 (2~3시간)
```

---

## 2. iOS 배포 (App Store)

### 2.1 릴리스 빌드 생성

```bash
flutter build ios --release
```

### 2.2 앱 서명 (Code Signing)

```
1. Xcode 실행
2. ios/Runner.xcworkspace 열기
3. Runner → Signing & Capabilities
4. Team 선택 및 서명 인증서 확인
```

### 2.3 App Store Connect 설정

```
1. https://appstoreconnect.apple.com 접속
2. 내 앱 → 앱 추가
3. 앱명: CafeHub
4. 번들 ID: com.cafehub.app
```

### 2.4 IPA 생성 및 업로드

```bash
# 1. Xcode에서 빌드
# 2. Product → Archive
# 3. Organizer에서 "Distribute App"
# 4. App Store Connect 선택
# 5. 업로드
```

---

## 3. 버전 관리

### 3.1 버전 번호 설정

```yaml
# pubspec.yaml
version: 1.0.0+1

# 형식: major.minor.patch+buildNumber
# 1.0.0: 첫 배포
# 1.0.1: 버그 수정
# 1.1.0: 새 기능
# 2.0.0: 주요 변경
```

### 3.2 변경 로그 (CHANGELOG)

```markdown
## [1.0.0] - 2026-06-22
### Added
- 카페 검색 기능
- 예약 시스템
- 리뷰 작성

### Fixed
- 로그인 오류 수정
```

---

## 4. 배포 전 체크리스트

```
[ ] 모든 테스트 통과
[ ] 버그 수정 완료
[ ] 버전 번호 업데이트
[ ] 스크린샷 준비 (5장)
[ ] 앱 설명 작성
[ ] 개인정보 정책 작성
[ ] 개발자 정보 등록
[ ] 앱 아이콘 검증 (512x512)
```

---

## 5. 배포 후 모니터링

```
Play Store / App Store Console에서:
- 앱 이용자 수 모니터링
- 별점 및 리뷰 확인
- 충돌 보고서 검토
- 성능 지표 분석
```

---

**상태**: ✅ 준비 완료
