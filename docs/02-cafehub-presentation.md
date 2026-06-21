---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Noto Sans KR', sans-serif;
  }
  h1, h2 {
    color: #8B4513;
  }
  .title-slide {
    background: linear-gradient(135deg, #8B4513 0%, #D2691E 100%);
    color: white;
  }
  .problem-slide {
    background: #fff5f5;
  }
  .vision-slide {
    background: #fffaf0;
  }
  .solution-slide {
    background: #f5fffb;
  }
  .impact-box {
    background: #8B4513;
    color: white;
    padding: 30px;
    border-radius: 12px;
    text-align: center;
    font-size: 1.3em;
    font-weight: bold;
    margin: 20px 0;
  }
  .problem-box {
    background: #ffebee;
    border-left: 5px solid #e91e63;
    padding: 20px;
    margin: 15px 0;
    border-radius: 8px;
  }
  .scenario-item {
    background: white;
    padding: 20px;
    margin: 15px 0;
    border-radius: 8px;
    border-left: 4px solid #D2691E;
    box-shadow: 0 2px 10px rgba(0,0,0,0.08);
  }
  .stat-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 15px;
    margin: 20px 0;
  }
  .stat-item {
    background: white;
    padding: 20px;
    border-radius: 12px;
    text-align: center;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    border-top: 4px solid #8B4513;
  }
  .stat-number {
    font-size: 2.5em;
    font-weight: bold;
    color: #8B4513;
  }
  .stat-label {
    color: #666;
    font-size: 0.9em;
    margin-top: 5px;
  }
  .tech-stack {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 15px;
    margin: 20px 0;
  }
  .tech-item {
    background: white;
    padding: 15px;
    border-radius: 8px;
    text-align: center;
    border: 2px solid #D2691E;
  }
  .tech-item strong {
    color: #8B4513;
    display: block;
    margin-top: 5px;
  }
  .user-icon {
    display: inline-block;
    width: 40px;
    height: 40px;
    background: #D2691E;
    color: white;
    border-radius: 50%;
    text-align: center;
    line-height: 40px;
    margin-right: 10px;
    font-weight: bold;
  }
  em {
    color: #8B4513;
    font-style: normal;
    font-weight: bold;
  }
---

<!-- Slide 1: Title -->
<div class="title-slide">

# ☕ CafeHub

## 가고 싶은 카페를 찾아서 자리를 미리 예약하다

**발표자:** 팀 이름  
**날짜:** 2024.12.01

---

</div>

---

<!-- Slide 2: 문제 정의 -->

<div class="problem-slide">

## 🎯 우리가 푸는 문제

<div class="problem-box">
<strong>💔 현재의 고민</strong><br>
카페를 가고 싶은데 자리가 없어서 돌아온 경험, 다들 있으시죠?
</div>

### 세 가지 어려움

<div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 15px;">

<div>
**1️⃣ 시간 낭비**
- 카페 도착 후 자리 확인
- 만석이면 다른 곳 찾기
- 왕복 30분 손실
</div>

<div>
**2️⃣ 확인의 어려움**
- 오픈 시간? 분위기? 시설?
- 여러 카페 사이트 찾아봐야 함
- 리뷰가 산재되어 있음
</div>

<div>
**3️⃣ 계획 불가**
- 그룹 미팅 장소 확보 어려움
- 데이트/업무 미팅 확정 불가
- "자리 있나?" 몇 번이나 물어봐야 함
</div>

</div>

---

</div>

---

<!-- Slide 3: 타겟 사용자 -->

## 👥 이 앱이 필요한 사람들

<div style="margin-top: 20px;">

<div class="scenario-item">
<span class="user-icon">💼</span>
<strong>직장인</strong><br>
점심시간 카페에서 노트북 작업, 미팅 장소로 자주 이용<br>
<em style="font-size: 0.9em;">"1시간만 자리 확보하고 싶어"</em>
</div>

<div class="scenario-item">
<span class="user-icon">📚</span>
<strong>학생 / 프리랜서</strong><br>
공부, 작업을 위해 한적한 카페 찾음<br>
<em style="font-size: 0.9em;">"조용하고 시간 제한 없는 카페"</em>
</div>

<div class="scenario-item">
<span class="user-icon">💑</span>
<strong>데이트 중인 커플</strong><br>
좋은 분위기 카페에서 시간을 보내고 싶음<br>
<em style="font-size: 0.9em;">"예쁜 카페 자리를 미리 예약하고 싶어"</em>
</div>

</div>

---

---

<!-- Slide 4: 한 줄 비전 -->

<div class="vision-slide">

## ✨ 우리의 비전

<div class="impact-box">
"예약 가능한 카페를 찾고, 시간을 낭비 없이 자리를 확보한다"
</div>

### 핵심 가치

<table style="width: 100%; margin-top: 30px;">
<tr>
<td style="text-align: center; padding: 20px; background: #fff8e1; border-radius: 8px;">
<strong style="font-size: 1.2em; color: #F57F17;">🎯 시간 낭비 제로</strong><br>
클릭 3번 만에 예약 완료<br>
도착 전에 자리 100% 확보
</td>
</tr>
<tr height="15"></tr>
<tr>
<td style="text-align: center; padding: 20px; background: #e3f2fd; border-radius: 8px;">
<strong style="font-size: 1.2em; color: #1565c0;">📍 한곳에서 관리</strong><br>
카페 정보, 예약, 리뷰<br>
모든 것이 한 앱에
</td>
</tr>
<tr height="15"></tr>
<tr>
<td style="text-align: center; padding: 20px; background: #f3e5f5; border-radius: 8px;">
<strong style="font-size: 1.2em; color: #6a1b9a;">⭐ 신뢰할 수 있는 정보</strong><br>
사진과 리뷰로 검증된 카페만<br>
내가 원하는 분위기 찾기
</td>
</tr>
</table>

---

</div>

---

<!-- Slide 5: 사용자 시나리오 -->

## 📖 CafeHub 이렇게 쓴다

<div class="scenario-item">
<strong>상황 1️⃣: 점심시간 회의 예정</strong><br>
2시간 뒤 팀장님과 카페에서 만나기로 함<br>
→ <em>지금 바로 "강남역 조용한 카페" 검색해서 2시간 예약</em><br>
→ 시간에 딱 맞춰 가서 즉시 앉기
</div>

<div class="scenario-item">
<strong>상황 2️⃣: 주말 데이트 계획</strong><br>
예쁜 카페에서 2시간 시간 보내고 싶음<br>
→ <em>사진으로 본 카페들을 비교하고, 분위기별 필터링</em><br>
→ 평점 높은 곳 선택해서 자리 예약 (결제는 현장)
</div>

<div class="scenario-item">
<strong>상황 3️⃣: 카페 새로 알아보기</strong><br>
주변 카페는 다 가봤는데, 새로운 곳을 찾고 싶음<br>
→ <em>내가 간 카페들의 리뷰를 보고, 비슷한 분위기 추천받기</em><br>
→ "예약 가능" 한정으로만 검색해서 확실한 곳만 가기
</div>

---

---

<!-- Slide 6: 기술 스택 선택 -->

<div class="solution-slide">

## 🛠️ 기술 스택 (왜 이 선택인가?)

<div class="tech-stack">

<div class="tech-item">
<strong>Frontend</strong><br>
Flutter
<br><em style="font-size: 0.8em;">iOS/Android 동시<br>빠른 개발</em>
</div>

<div class="tech-item">
<strong>Backend</strong><br>
Firebase
<br><em style="font-size: 0.8em;">인증/DB 무료<br>배포 간단</em>
</div>

<div class="tech-item">
<strong>Maps</strong><br>
Google Maps API
<br><em style="font-size: 0.8em;">정확한 위치<br>필터링</em>
</div>

<div class="tech-item">
<strong>결제</strong><br>
Stripe
<br><em style="font-size: 0.8em;">선예약 결제<br>국내 카드 지원</em>
</div>

<div class="tech-item">
<strong>상태관리</strong><br>
Provider
<br><em style="font-size: 0.8em;">간단함<br>학습곡선 낮음</em>
</div>

<div class="tech-item">
<strong>Database</strong><br>
Firestore
<br><em style="font-size: 0.8em;">실시간 예약<br>확장성</em>
</div>

</div>

---

</div>

---

<!-- Slide 7: 핵심 기능 -->

## ✅ Must (반드시 만들 기능)

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 20px;">

<div style="background: #e8f5e9; padding: 15px; border-radius: 8px;">
<strong>🔍 검색/필터</strong><br>
✅ 지도에 카페 표시<br>
✅ 위치별 필터링<br>
✅ 분위기 선택 필터<br>
✅ 시설 확인
</div>

<div style="background: #e8f5e9; padding: 15px; border-radius: 8px;">
<strong>📅 예약 시스템</strong><br>
✅ 날짜/시간/인원 선택<br>
✅ 결제 처리<br>
✅ 예약 확인/취소<br>
✅ 예약 알림
</div>

<div style="background: #e8f5e9; padding: 15px; border-radius: 8px;">
<strong>⭐ 리뷰</strong><br>
✅ 별점 매기기<br>
✅ 리뷰 작성<br>
✅ 사진 업로드<br>
✅ 다른 리뷰 보기
</div>

<div style="background: #e8f5e9; padding: 15px; border-radius: 8px;">
<strong>👤 사용자</strong><br>
✅ 회원가입/로그인<br>
✅ 프로필 관리<br>
✅ 예약 내역<br>
✅ 찜한 카페
</div>

</div>

---

---

<!-- Slide 8: 진행 상황 -->

## 📊 지금까지의 진행 (12월 1주)

<div class="stat-grid">

<div class="stat-item">
<div class="stat-number">✅</div>
<div class="stat-label">
완료 (21%)<br>
회원가입/로그인<br>
Google Maps 기초
</div>
</div>

<div class="stat-item">
<div class="stat-number">🚧</div>
<div class="stat-label">
진행중 (21%)<br>
지도 필터링<br>
리뷰 작성
</div>
</div>

<div class="stat-item">
<div class="stat-number">⏳</div>
<div class="stat-label">
예정 (58%)<br>
예약 시스템<br>
결제 연동
</div>
</div>

</div>

**중간발표 데모:**
- 1️⃣ 지도에서 카페 검색
- 2️⃣ 필터링 (위치, 분위기)
- 3️⃣ 예약 흐름 (미완성)

---

---

<!-- Slide 9: 일정 계획 -->

## 📅 남은 일정 (12월-12월 말)

| 주 | 목표 | 상태 |
|------|------|------|
| 12주차 | **Must 50% + 데모 (지금)** | 🚧 진행 |
| 13주차 | 예약 시스템 완료 | ⏳ 예정 |
| 14주차 | 리뷰, 최적화, 배포 | ⏳ 예정 |

**크리티컬:**
- 13주차에 **결제 연동** 꼭 끝내야 함
- 14주차에 **성능 최적화** (로딩 속도)

---

---

<!-- Slide 10: 어려운 점 & 해결 방안 -->

## ⚠️ 예상되는 어려운 점

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px;">

<div style="background: #ffebee; border-left: 4px solid #f44336; padding: 15px; border-radius: 8px;">
<strong>🔴 Google Maps 성능</strong><br>
지도 로딩 느림<br>
→ <em>캐싱 + 페이지네이션</em>
</div>

<div style="background: #ffebee; border-left: 4px solid #f44336; padding: 15px; border-radius: 8px;">
<strong>🔴 Stripe 테스트</strong><br>
결제 API 복잡함<br>
→ <em>테스트 카드로 미리 검증</em>
</div>

<div style="background: #ffebee; border-left: 4px solid #f44336; padding: 15px; border-radius: 8px;">
<strong>🔴 실시간 예약</strong><br>
동시 예약시 충돌<br>
→ <em>Firestore 트랜잭션</em>
</div>

<div style="background: #ffebee; border-left: 4px solid #f44336; padding: 15px; border-radius: 8px;">
<strong>🔴 카페 정보 수집</strong><br>
초기 카페 데이터 필요<br>
→ <em>50개 카페부터 시작</em>
</div>

</div>

---

---

<!-- Slide 11: 성공 기준 -->

## 🎯 이 프로젝트의 성공 기준

<div class="stat-grid">

<div class="stat-item">
<div class="stat-number">5개</div>
<div class="stat-label">
핵심 기능<br>
모두 구현 완료<br>
버그 제로
</div>
</div>

<div class="stat-item">
<div class="stat-number">50개</div>
<div class="stat-label">
카페 데이터<br>
예약 가능 카페<br>
리뷰 포함
</div>
</div>

<div class="stat-item">
<div class="stat-number">80%</div>
<div class="stat-label">
사용성<br>
직관적 UI<br>
3탭으로 예약 완료
</div>
</div>

</div>

---

---

<!-- Slide 12: 마무리 -->

<div class="title-slide">

## 🎬 Summary

**문제:** "자리 있나?" 몇 번이나 물어봐야 함  
**해결:** 클릭 3번으로 자리 확보, 시간 낭비 제로  
**차별성:** 예약 + 평가, 한곳에서 모든 것을

### Q & A

---

</div>
