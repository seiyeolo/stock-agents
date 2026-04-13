# 📈 Stock Agents — Claude Code 주식 분석 팀 v2

Claude Code 서브에이전트 기능을 활용한 **주식 분석 전문가 팀**입니다.
7명의 AI 전문가가 역할 분업하여 분석하고, 공격·방어 전략가가 **WRAP 의사결정 검증**을 거쳐 최종 투자 전략을 수립합니다.

> 🆕 **v2 업데이트**: 방어적 전략가 · 위기 대응 · 포지션 트래커 · 정량 리스크 분석가 추가, WRAP 4단계 검증 통합, DART MCP 출처 우선순위 적용

---

## 👥 에이전트 팀 구성 (7명)

### 🔍 분석가 3인 (병렬 실행)

| 에이전트 | 역할 | 모델 |
|---------|------|-----|
| `financial-analyst` | 재무제표 · 재무비율 · 밸류에이션 (DART MCP 우선) | Sonnet |
| `news-sentiment-analyst` | 뉴스 톤 · 시장 심리 · 이벤트 점수화 | Sonnet |
| `sector-researcher` | 산업 구조 · 경쟁사 · 메가트렌드 | Sonnet |

### 🎯 전략가 2인 (선택적 종합)

| 에이전트 | 프레임워크 | 용도 | 모델 |
|---------|-----------|-----|------|
| `aggressive-investment-strategist` | **ALPHA 매트릭스** + WRAP | 고수익 · 성장주 · 모멘텀 | Opus |
| `defensive-investment-strategist` | **SAFE 매트릭스** + WRAP | 자본 보전 · 배당 · 장기 보유 | Opus |

### 🛡️ 리스크 & 규율 3인 (보조)

| 에이전트 | 역할 | 모델 |
|---------|------|-----|
| `quantitative-risk-analyst` | 확률분포 · 팻테일 · Kelly Criterion · Z-score | Sonnet |
| `crisis-response-protocol` | 급락/전쟁/쇼크 시 자동 발동, 기본값 🛑 행동 보류 | Opus |
| `position-tracker` | 매수 Thesis 저장 · 매도 시 강제 반문 | Sonnet |

---

## ⚙️ 작동 방식

```
사용자 요청 ("SK하이닉스 공격적 관점으로 분석해줘")
    ↓
┌───────────────────────────────────────────────┐
│  Phase 1 — 분석가 3인 병렬 실행               │
│   financial-analyst (재무 + DART MCP)         │
│   news-sentiment-analyst (뉴스 톤 점수화)     │
│   sector-researcher (섹터 · 경쟁 · 메가트렌드)│
└───────────────────────────────────────────────┘
    ↓ (3개 리포트 집계)
┌───────────────────────────────────────────────┐
│  Phase 2 — 전략가 종합                        │
│   공격: ALPHA 매트릭스 (A/L/P/H/A)             │
│   방어: SAFE 매트릭스 (S/A/F/E)                │
│   ↓                                            │
│   WRAP 의사결정 검증 (필수)                    │
│    W — Widen (대안 비교)                       │
│    R — Reality-test (가정 검증)                │
│    A — Attain distance (거리두기)              │
│    P — Prepare to be wrong (Pre-mortem)        │
└───────────────────────────────────────────────┘
    ↓
최종 리포트: 진입가 · 목표가 · 손절가(또는 재평가 트리거) · 포지션 사이징
    ↓
position-tracker가 Thesis를 positions/ 폴더에 영속 저장
```

### 보조 워크플로우

- **급락 감지 시** → `crisis-response-protocol` 자동 발동
  - 지수 −3%↑ 급락, 전쟁·테러 키워드, 사용자 공황 요청
  - 과거 유사 이벤트 최소 5~15건 패턴 분석 → **기본값: 48시간 행동 보류**
- **매도 요청 시** → `position-tracker`가 4가지 반문 강제
  - "지금 파시려는 이유는?" / "Thesis 훼손됐나요?" / "1년 후 확신?" ...
- **포지션 사이징** → `quantitative-risk-analyst`가 Half-Kelly 기준 권고

---

## 🚀 설치 방법

### 1. 레포 클론
```bash
git clone https://github.com/seiyeolo/stock-agents.git
cd stock-agents
```

### 2. (권장) Anthropic 공식 financial-services-plugins 함께 설치
```bash
claude plugin marketplace add anthropics/financial-services-plugins
claude plugin install equity-research@financial-services-plugins
claude plugin install financial-analysis@financial-services-plugins
```
공식 플러그인의 `/dcf` `/comps` `/morning-note` `/catalysts` 등을 보조 도구로 사용.

### 3. Claude Code 실행
```bash
claude
```

### 4. 분석 요청
```
SK하이닉스 공격적 관점으로 분석해줘
삼성전자 방어적 관점으로 분석해줘
```

---

## 💬 사용 예시

### 종합 분석
```
SK하이닉스 공격적 관점으로 분석해줘         # → ALPHA 매트릭스 + WRAP
삼성전자 방어적 관점으로 분석해줘           # → SAFE 매트릭스 + WRAP + DCA 플랜
LG에너지솔루션 리스크 먼저 봐줘             # → quantitative-risk-analyst 주도
```

### 매매 관리
```
SK하이닉스 샀어, 진입가 130만원            # → position-tracker가 Thesis 기록
SK하이닉스 팔고 싶어                         # → 4가지 강제 반문 + 원칙 체크리스트
포트폴리오 현황 체크해줘                    # → positions/ 전체 조회
```

### 위기 상황
```
코스피 지금 -5%야 어떻게 해야 돼?          # → crisis-response-protocol 발동
전쟁 났대 팔아야 돼?                         # → 과거 13건 유사 사례 + 🛑 권고
```

### 단일 에이전트 호출
```
@financial-analyst 카카오 PER 얼마야?
@sector-researcher 2차전지 섹터 어때?
@quantitative-risk-analyst 코스피 변동성 분석해줘
```

---

## 📊 최종 리포트 산출물

### 공격적 전략 (ALPHA 매트릭스)
- **A**cceleration · **L**eadership · **P**ricing Power · **H**ype · **A**symmetry
- 가중 총점 (8.0+ 공격 매수)
- 진입가 · 1/2/스트레치 목표가 · 손절가
- 포지션 사이징 (5~15%)
- 주요 촉매제 및 손절 트리거

### 방어적 전략 (SAFE 매트릭스)
- **S**tability · **A**ssets & Moat · **F**CF & Dividend · **E**arnings Quality
- 가중 총점 (8.5+ 핵심 방어)
- DCA 3회 분할 진입 구간
- Fair Value (정상화 EPS × PER)
- **재평가 트리거** (손절이 아닌 Thesis Break 조건)
- 5년+ 보유 · 배당 재투자

### WRAP 의사결정 검증 (두 전략 공통 필수)
- W: 대안 최소 2~3개 비교 (다른 종목 · 현금 · 무위험 자산)
- R: 핵심 가정 3개 + 반대 증거 + 과거 유사 사례
- A: "10년 후 중요?", 24시간 관망, 감정 점검
- P: Pre-mortem — 실패 시나리오 3가지 + 확률

---

## 📁 파일 구조

```
stock-agents/
├── .claude/
│   ├── agents/
│   │   ├── financial-analyst.md
│   │   ├── news-sentiment-analyst.md
│   │   ├── sector-researcher.md
│   │   ├── aggressive-investment-strategist.md
│   │   ├── defensive-investment-strategist.md
│   │   ├── quantitative-risk-analyst.md
│   │   ├── crisis-response-protocol.md
│   │   └── position-tracker.md
│   └── settings.local.json
├── positions/              # (gitignored) 개인 투자 기록
└── README.md
```

---

## 🧠 설계 철학

### 1. **의사결정 과정 > 종목 추천**
AI가 종목을 찍어주는 것이 아니라, 투자자의 **의사결정 품질을 향상**시키는 도구입니다.
Heath 형제의 **WRAP 프레임워크**(*Decisive*, 2013)를 AI 에이전트에 접목.

### 2. **잃지 않는 길 > 최고 수익률**
- 공격적 전략도 **손절선과 Pre-mortem 필수**
- 정량 리스크 분석가는 Kelly Criterion의 **Half 이하** 권고
- 위기 대응 프로토콜의 기본값은 **"행동 보류"**

### 3. **충동 매매 차단**
- MTS 앱 대신 `position-tracker`에 매도 요청 → 4가지 반문
- 급락장에는 `crisis-response-protocol`이 과거 사례 데이터로 공포 상쇄
- "오디세이의 귀마개" 역할

### 4. **한국 시장 특화**
- `financial-analyst`는 DART MCP 최우선 (할루시네이션 차단)
- 원화·KRX·한국 섹터 컨텍스트
- Anthropic 공식 플러그인은 미국 시장 보조 도구로 병행

---

## ⚠️ 주의사항

> 이 도구는 **투자 참고용**입니다.
> 실제 투자 결정은 본인 판단 하에 이루어져야 하며,
> 투자 손실에 대한 책임은 본인에게 있습니다.

- 전략가는 **Opus 모델** 사용 (비용 높음, 단순 조회는 분석가만 호출)
- `position-tracker`의 `positions/` 폴더는 **gitignored** (개인 투자 기록 보호)
- 과거 통계는 미래를 보장하지 않습니다 (특히 `quantitative-risk-analyst` 결과)

---

## 🛠️ 요구사항

- [Claude Code](https://claude.ai/code) 설치
- Anthropic API 키 (Opus 사용 시 유료 플랜 권장)
- (선택) DART OpenAPI 키 — 한국 상장사 공시 데이터용
- (선택) Anthropic `financial-services-plugins` 마켓플레이스 추가

---

## 📚 참고 문헌 & 영감

- Chip & Dan Heath, *Decisive* (2013) — WRAP 프레임워크
- 머신러너, *할 수 있다! AI 주식 투자* (FM미디어) — 1인 투자 하우스 개념
- Warren Buffett & Charlie Munger — "잃지 않는 길" 철학
- Anthropic `financial-services-plugins` — 공식 재무 워크플로우

---

## 📜 라이선스

MIT License

---

*Built with ❤️ using [Claude Code](https://claude.ai/code) Sub-agents*
