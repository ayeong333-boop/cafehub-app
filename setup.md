# 🔧 CafeHub 개발 환경 설정 (SETUP)

**목표**: CafeHub 앱 개발을 위한 로컬 개발 환경 구성

---

## 1. 요구사항

### 최소 사양
```
OS: Windows 10+, macOS 10.15+, Ubuntu 20.04+
RAM: 8GB 이상
Storage: 50GB 여유 공간
```

### 필수 설치 항목
- Flutter SDK 3.35.3
- Dart SDK 3.9.2
- Git
- Android Studio / Xcode (플랫폼별)

---

## 2. Flutter & Dart 설치

### 2.1 Windows

```bash
# 1. Flutter SDK 다운로드 및 설치
# https://flutter.dev/docs/get-started/install/windows 에서 다운로드

# 2. 환경 변수 설정
# PATH에 flutter/bin 추가

# 3. 설치 확인
flutter --version
dart --version
```

### 2.2 macOS

```bash
# Homebrew를 통한 설치 (권장)
brew install flutter

# 설치 확인
flutter doctor
```

### 2.3 Linux

```bash
# 1. 저장소 클론
git clone https://github.com/flutter/flutter.git -b stable

# 2. PATH 설정
export PATH="$PATH:`pwd`/flutter/bin"

# 3. 설치 확인
flutter doctor
```

---

## 3. 안드로이드 개발 환경 설정

### 3.1 Android Studio 설치

```bash
# https://developer.android.com/studio 에서 다운로드 및 설치
```

### 3.2 SDK 설정

```bash
# Android Studio 실행 → SDK Manager
# 설치 항목:
#   - Android SDK Platform 32, 33, 34
#   - Android Emulator
#   - Android SDK Command-line Tools

# ANDROID_HOME 환경 변수 설정
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
```

### 3.3 가상 장치 생성

```bash
# Android Studio 실행 → Virtual Device Manager
# 또는 명령어로:
flutter emulators --create --name cafehub_emu
flutter emulators --launch cafehub_emu
```

---

## 4. iOS 개발 환경 설정 (macOS만)

### 4.1 Xcode 설치

```bash
# App Store에서 Xcode 설치
# 또는 명령어로:
xcode-select --install
```

### 4.2 CocoaPods 설치

```bash
sudo gem install cocoapods
```

### 4.3 iPhone 시뮬레이터 실행

```bash
open -a Simulator
```

---

## 5. Firebase 프로젝트 설정

### 5.1 Firebase 프로젝트 생성

```
1. Firebase Console (https://console.firebase.google.com) 접속
2. "새 프로젝트 만들기" 클릭
3. 프로젝트명: "CafeHub"
4. Google Analytics 활성화 (선택)
5. 프로젝트 생성 완료 대기
```

### 5.2 Firebase App 등록

#### Android
```
1. Firebase Console → Android 앱 추가
2. Android Package Name: com.cafehub.app
3. google-services.json 다운로드
4. android/app/ 폴더에 저장
```

#### iOS
```
1. Firebase Console → iOS 앱 추가
2. Bundle ID: com.cafehub.app
3. GoogleService-Info.plist 다운로드
4. ios/Runner/ 폴더에 저장
```

### 5.3 Firebase 서비스 활성화

```
Firebase Console에서 활성화:
✅ Authentication (이메일/비밀번호)
✅ Firestore Database (프로덕션 모드)
✅ Cloud Storage
```

---

## 6. GitHub 저장소 설정

### 6.1 저장소 클론

```bash
git clone https://github.com/ayeong333-boop/cafehub-app.git
cd cafehub-app
```

### 6.2 브랜치 구조 설정

```bash
# main 브랜치 (배포)
git checkout main

# develop 브랜치 (개발)
git checkout -b develop origin/develop

# feature 브랜치 (기능 개발)
git checkout -b feature/auth
```

---

## 7. Flutter 프로젝트 설정

### 7.1 의존성 설치

```bash
cd cafehub-app
flutter pub get
```

### 7.2 주요 패키지

```yaml
# pubspec.yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.0.0
  firebase_core: ^2.0.0
  firebase_auth: ^4.0.0
  cloud_firestore: ^4.0.0
  dio: ^5.0.0

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^2.0.0
```

### 7.3 패키지 설치

```bash
flutter pub get
flutter pub upgrade
```

---

## 8. 개발 도구 설정

### 8.1 코드 포매팅

```bash
# 코드 자동 포매팅
dart format lib/

# Lint 체크
flutter analyze
```

### 8.2 IDE 플러그인 설치

**VS Code**:
- Flutter
- Dart
- Firebase Explorer (선택)

**Android Studio**:
- Flutter
- Dart
- Firebase Tools (선택)

---

## 9. 개발 시작

### 9.1 앱 실행 (디버그 모드)

```bash
# 디바이스 목록 확인
flutter devices

# 앱 실행
flutter run

# Hot Reload 활성화 (코드 변경 시 자동 재로드)
```

### 9.2 앱 실행 (릴리즈 모드)

```bash
flutter run --release
```

### 9.3 로그 확인

```bash
flutter logs
```

---

## 10. 트러블슈팅

### 10.1 Flutter Doctor 오류

```bash
# 진단 실행
flutter doctor

# 문제 해결
flutter doctor --android-licenses  # Android 라이센스 동의
```

### 10.2 Firebase 연동 오류

```
해결책:
1. google-services.json 파일 확인
2. Firebase 프로젝트명 일치 확인
3. Firebase Console에서 앱 등록 재확인
```

### 10.3 의존성 충돌

```bash
# 의존성 업데이트
flutter pub upgrade

# 캐시 초기화
flutter clean
flutter pub get
```

---

## 11. 개발 팁

```
1. Hot Reload: 코드 저장 후 자동 리로드 (상태 유지)
2. Hot Restart: 수동 재시작 (상태 초기화)
3. Device 전환: flutter devices로 확인 후 선택
4. Debug: print() 또는 debugger 사용
5. Performance: DevTools → Performance 탭 확인
```

---

**작성자**: 팀 리더  
**최종 업데이트**: 2026-06-22  
**상태**: ✅ 완료
