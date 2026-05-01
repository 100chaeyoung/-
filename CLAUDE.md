# Wage Clock

실시간 시급 계산기 — 순수 HTML/CSS/JS 단일 파일 토이 프로젝트.

## 구조

```
wage-clock/
└── index.html   # 전체 앱 (HTML + CSS + JS 인라인)
```

외부 의존성 없음. 빌드 과정 없음. `index.html` 하나가 전부다.

## 배포

GitHub Pages: Settings → Pages → Branch `main` / `/ (root)`

원격 저장소:
- `origin`  → https://github.com/100chaeyoung/-.git
- `origin2` → https://github.com/100chaeyoung/-1.git

## 디자인 토큰 (CSS 변수)

| 변수 | 값 | 용도 |
|------|----|------|
| `--gold` | `#F5C518` | 주요 포인트, 시작 버튼, 진행 바 |
| `--gold-dark` | `#C9A000` | 통화 기호, 강조 텍스트 |
| `--gold-bg` | `#FFFBEA` | 골드 배경 틴트 |
| `--black` | `#111111` | 주요 텍스트, 정지 버튼 |
| `--gray-1` | `#F5F5F2` | 입력 배경, 초기화 버튼 |

배경색은 시급에 따라 cool gray(`#F5F5F2`) → warm gold(`#FFE49A`)로 JS가 동적으로 보간한다 (`updateWarmth(rate)`).

## 핵심 JS 구조

```
State        elapsed, startAt, running, lastInt, shownMilestones, pileH[]
Timer        startTimer / stopTimer / resetTimer
Tick loop    requestAnimationFrame(tick) — 시간·금액·진행 바·마일스톤·동전 스폰
Coins        autoCoins(rate) + spawnCoin(fromClick, cx, cy)
             settled 동전은 10초 후 fade-out 제거, pileH 감소
Milestones   ₩1,000 / 3,000 / 5,000 / 10,000 / 30,000 / 50,000 / 100,000 / 500,000
Confetti     triggerConfetti() — 목표 금액 100% 달성 시 1회
Quotes       QUOTES 배열, 4초마다 fade 전환 (setInterval)
Warmth       updateWarmth(rate) — body background RGB 보간
```

## 제약 사항

- **외부 라이브러리 금지** — CDN 포함 일절 사용하지 않는다.
- **서버 불필요** — 로컬 파일로 열거나 GitHub Pages로 정적 서빙.
- **단일 파일 유지** — CSS·JS를 별도 파일로 분리하지 않는다.
- **카드 높이 고정** — `.progress-section`은 `visibility` 토글(display 변경 금지), `.quote-block`은 `height: 56px` 고정.
- **명언 18자 이내** — `QUOTES` 배열에 새 항목 추가 시 본문 18자 초과 금지.
- **동전 착지선** — `card.getBoundingClientRect().bottom` 기준, footer 침범 방지.

## 스타일 원칙

- 미니멀 카드 레이아웃, 큰 숫자 중심
- 골드/옐로우 포인트, 밝은 배경, 블랙 텍스트
- 이모지 과다 사용 금지, 그라데이션 금지
- 반응형 (모바일 우선, 380px 이하 별도 미디어쿼리)
