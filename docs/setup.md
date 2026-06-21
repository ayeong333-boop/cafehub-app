# CafeHub — 개발 환경 설정 가이드

> **목표**: 이 문서를 따라 5분 안에 앱을 실행할 수 있어야 합니다.

---

## 📋 필수 사항

### **필수 버전**

```
Flutter: 3.x.x 이상
Dart: 3.x.x 이상
Android SDK: API Level 21 이상 (API 35 권장)
iOS: iOS 12.0 이상 (iOS 14.0 권장)
Java: JDK 11 이상 (17 권장)
```

### **설치 시간**

```
처음부터: 30-45분
이미 설치된 경우: 5분
```

---

## 🚀 **Windows 사용자**

### **Step 1: Flutter SDK 설치**

1. https://flutter.dev/docs/get-started/install/windows 방문
2. **Flutter SDK** 최신 버전 다운로드 (.zip)
3. `C:\flutter` 폴더에 압축 해제

### **Step 2: 환경 변수 설정**

```
1. Windows 시작 > "환경 변수" 검색
2. "시스템 환경 변수 편집" 클릭
3. [환경 변수] 버튼 클릭
4. [새로 만들기] > 사용자 변수
   - 변수 이름: FLUTTER_HOME
   - 변수 값: C:\flutter
5. PATH 편집 > [새로 만들기] > %FLUTTER_HOME%\bin
6. [확인] 저장
```

### **Step 3: 설치 확인**

```powershell
# PowerShell 재시작 후:
flutter --version
flutter doctor
```

**예상 결과:**

```
Flutter 3.16.x
Dart 3.2.x
```

### **Step 4: 저장소 클론 및 설정**

```powershell
# 1. 저장소 클론
git clone https://github.com/ayeong333-boop/cafehub-app.git
cd cafehub-app

# 2. 의존성 설치
flutter pub get

# 3. 빌드 캐시 정리 (필요시)
flutter clean
flutter pub get
```

### **Step 5: 앱 실행**

```powershell
# Android 실행 (에뮬레이터 또는 기기 필요)
flutter run

# iOS 실행 (Mac 필요)
flutter run -d macos
```

---

## 🍎 **Mac 사용자**

### **Step 1: Homebrew로 설치 (권장)**

```bash
# Homebrew 설치 확인
brew --version

# Flutter 설치
brew install flutter

# 또는 수동 설치:
# https://flutter.dev/docs/get-started/install/macos에서 다운로드
# ~/flutter 에 압축 해제
```

### **Step 2: 환경 변수 설정**

```bash
# ~/.zshrc 또는 ~/.bash_profile 편집
nano ~/.zshrc

# 다음 줄 추가:
export PATH="$PATH:$HOME/flutter/bin"
export FLUTTER_HOME="$HOME/flutter"

# 저장: Ctrl+X > Y > Enter

# 적용
source ~/.zshrc
```

### **Step 3: 설치 확인**

```bash
flutter --version
flutter doctor
```

### **Step 4: Xcode 설정 (iOS 앱 실행시)**

```bash
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcode-select --reset

# CocoaPods 설치
sudo gem install cocoapods
```

### **Step 5: 저장소 클론 및 설정**

```bash
# 1. 저장소 클론
git clone https://github.com/ayeong333-boop/cafehub-app.git
cd cafehub-app

# 2. 의존성 설치
flutter pub get

# 3. iOS 의존성 설치 (처음 한 번)
cd ios
pod install
cd ..
```

### **Step 6: 앱 실행**

```bash
# iOS 시뮬레이터
flutter run

# iOS 기기
flutter run -d <device-id>

# Android
flutter run -d android
```

---

## 🐧 **Linux 사용자**

### **Step 1: 의존성 설치**

```bash
sudo apt-get update
sudo apt-get install -y \
  git \
  curl \
  unzip \
  xz-utils \
  zip \
  libglu1-mesa \
  openjdk-11-jdk-headless
```

### **Step 2: Flutter 설치**

```bash
# Flutter SDK 다운로드
cd ~/
git clone https://github.com/flutter/flutter.git
cd flutter
git checkout stable

# 환경 변수 설정
echo 'export PATH="$PATH:$HOME/flutter/bin"' >> ~/.bashrc
source ~/.bashrc
```

### **Step 3: 설치 확인**

```bash
flutter --version
flutter doctor
```

### **Step 4: Android SDK 설정**

```bash
# Android Studio 또는 SDK만 설치:
# https://developer.android.com/studio

# 또는 Flutter에 포함된 SDK 사용:
flutter doctor --android-licenses
# yes 를 여러번 입력
```

### **Step 5: 저장소 클론 및 설정**

```bash
git clone https://github.com/ayeong333-boop/cafehub-app.git
cd cafehub-app
flutter pub get
```

### **Step 6: 앱 실행**

```bash
# Android 에뮬레이터
flutter run

# 연결된 기기
flutter run -d <device-id>
```

---

## ⚠️ **문제 해결 (FAQ)**

### **Q1: "Flutter command not found"**

```
해결:
1. flutter --version 실행
2. 환경 변수 설정 다시 확인
3. Terminal/PowerShell 재시작
```

### **Q2: "Android SDK not found"**

```
해결:
flutter doctor -v
위 명령어로 Android SDK 경로 확인

필요시:
flutter config --android-sdk <경로>
```

### **Q3: "gradle build failed"**

```
해결:
flutter clean
flutter pub get
flutter run
```

### **Q4: "iOS pod install error"**

```
해결 (Mac):
cd ios
rm -rf Pods
rm -rf Pods.lock
pod install
cd ..
flutter run
```

### **Q5: "Emulator won't start"**

```
해결:
1. Android Studio > Device Manager 에서 에뮬레이터 생성
2. flutter emulators 로 확인
3. flutter run -d emulator-5554
```

---

## ✅ **첫 실행 체크리스트**

```
□ Flutter 설치 & flutter --version 확인
□ flutter doctor 모든 항목 초록색 (✓)
□ git clone 완료
□ flutter pub get 완료
□ flutter run 성공
□ "Hello World" 앱 화면에 나타남
```

---

## 🔗 **다음 단계**

```
1. docs/architecture.md 읽기
2. lib/ 구조 이해하기
3. 첫 화면 만들기 시작
```

---

**작성자**: [당신의 이름]  
**작성일**: 2024.11.22  
**마지막 수정**: 2024.11.22
