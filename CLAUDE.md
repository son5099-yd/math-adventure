# 수학 친구들의 모험 (Math Adventure)

초등학교 3학년 자녀를 위한 수학·과학 학습 게임. 사용자(부모)는 비개발자.

## 게임 개요
- **URL**: https://son5099-yd.github.io/math-adventure/
- **GitHub**: https://github.com/son5099-yd/math-adventure
- **메인 파일**: `index.html` (단일 파일 SPA, 빌드 도구 없음)
- **로컬 미러**: `/Users/yongdae/Desktop/수학모험.html` (배포 시 자동 동기화)

## 디자인 원칙 (절대 깨면 안 됨)
1. **폭력 없음**: 동물·곤충을 "물리치는" 표현 금지. 적이 아니라 도와줄 친구.
   - ⚔️ → 💭, "물리치자" → "도와줘요", "보스" → "마지막 친구"
2. **부드러운 피드백**: 정답 시 친구가 폴짝 기뻐함, 오답 시 같이 🤔 생각.
   - 빨간색 hue-rotate 같은 다친 듯한 효과 금지.
3. **3학년 수준 한국어**: 어려운 단어 피하고 친근한 말투. 한자어는 (괄호) 설명.
4. **격려 위주**: 오답 시 "쓰러졌어요" 같은 부정적 표현 금지. "잠깐 쉬어가요" 같은 표현 사용.

## 게임 구조
- **6개 세계**: 도형의 숲, 구구단 성, 나눗셈 동굴, 덧셈 마을, 분수 섬, 과학 실험실
- **각 세계**: 6명의 친구 (마지막은 리더), 8문제 풀어야 클리어
- **2가지 모드**:
  - 🌟 오늘의 도전: 날짜 시드 기반 (mulberry32 RNG), 같은 날은 같은 문제
  - 🎲 자유 연습: Math.random 사용, 매번 다른 문제
- **HP 시스템**: 시작 3개. 오답 시 1 감소. 0이 되면 게임오버 후 재도전.
- **풀이 기능**: 오답 시 노란 카드로 풀이 표시, 아이가 "알겠어요!" 눌러야 진행.

## 코드 구조
모든 코드는 `index.html` 한 파일에 있음 (~1000줄):
- `<style>`: 화면별 CSS (.title-screen, .map-screen, .battle-screen.{forest|castle|cave|village|island|lab})
- `<script>`:
  - `WORLDS` 배열: 각 세계의 친구·스토리 데이터
  - `state` 객체: 게임 상태 (mode, hp, currentWorld 등)
  - `mulberry32`, `dateSeed`, `R()`: 시드 기반 난수
  - `gen*()` 함수들: 문제 생성기 (genShape, genMultiply, genDivide, genAddSub, genFraction, genScience)
  - `render*()` 함수들: 화면 렌더링 (renderTitle, renderMap, renderBattle, worldClear, gameOver)
  - `answer()`, `showExplanation()`, `continueAfterExplain()`: 답변 처리

## 문제 추가 방법
- **수학 문제**: 해당 `gen*()` 함수의 배열에 객체 추가. 반드시 `explain` 필드 포함.
- **과학 문제**: `genScience()` 의 questions 배열에 추가. 3학년 과학 교과 주제 위주.
- **새 친구**: 해당 세계의 `enemies`와 `enemyNames` 배열에 동시 추가.
- **새 세계**: `WORLDS`에 객체 추가 + `genQuestion()` switch에 case 추가 + `.battle-screen.{id}` CSS 배경 추가.

## 배포 방법
사용자가 변경 사항 확정한 후:
- `/deploy "변경 내용 설명"` 슬래시 커맨드 사용 (권장)
- 또는 수동:
  ```
  cp /Users/yongdae/Desktop/math-adventure/index.html "/Users/yongdae/Desktop/수학모험.html"
  cd /Users/yongdae/Desktop/math-adventure
  git -c user.email="son5099-yd@users.noreply.github.com" -c user.name="son5099-yd" commit -am "메시지" -q
  git push -q
  ```
- 변경은 GitHub Pages에서 1~2분 후 반영. iPad에서는 새로고침 필요.

## iPad 최적화
- `apple-mobile-web-app-capable`: 홈화면 추가 시 앱처럼 전체화면 실행
- 더블탭 확대·핀치 줌·스크롤 바운스 모두 방지됨
- 세로 모드일 때 "iPad를 가로로 돌려주세요" 오버레이 자동 표시
- 홈화면 아이콘: 주황 배경 🦊 (data URL SVG)

## 주의사항
- **이모지 일관성**: 주인공 🦊 꼬마 여우. 숲에 다른 여우 친구 추가 금지 (혼동).
- **시드 기반 문제 생성**: 새 문제 추가 시 배열 순서가 바뀌면 "오늘의 도전" 결과도 바뀜.
- **CSS class 명명**: `.enemy`, `.attack-anim`, `.hurt`는 내부 이름이라 안 바꿔도 됨 (사용자에게 안 보임).
