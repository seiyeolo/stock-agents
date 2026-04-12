# 📈 Stock Agents — Claude Code 주식 분석 팀

Claude Code의 서브에이전트 기능을 활용한 **주식 분석 전문가 팀**입니다.  
5명의 AI 전문가가 병렬로 분석하고, 최종 투자 전략을 자동으로 수립합니다.

---

## 👥 에이전트 팀 구성

| 에이전트 | 역할 | 모델 |
|----------|------|------|
| `financial-analyst` | 재무제표 · 재무비율 · 밸류에이션 분석 | Sonnet |
| `news-sentiment-analyst` | 뉴스 톤 · 시장 심리 · 이벤트 분석 | Sonnet |
| `sector-researcher` | 산업 구조 · 경쟁사 · 메가트렌드 분석 | Sonnet |
| `valuation-expert` | DCF · 멀티플 · 비교기업 밸류에이션 | Sonnet |
| `aggressive-investment-strategist` | 4인 분석 종합 + 공격적 최종 전략 수립 | Opus |

---

## ⚙️ 작동 방식

```
사용자 요청
    ↓
┌─────────────────────────────────────┐
│  병렬 실행 (동시에 분석)              │
│  financial-analyst                  │
│  news-sentiment-analyst             │
│  sector-researcher                  │
│  valuation-expert                   │
└─────────────────────────────────────┘
    ↓
aggressive-investment-strategist
(4인 결과 종합 → 최종 투자 리포트)
    ↓
진입가 · 목표가 · 손절가 · 포지션 사이징
```

---

## 🚀 설치 방법

### 1. 레포 클론
```bash
git clone https://github.com/seiyeolo/stock-agents.git
cd stock-agents
```

### 2. Claude Code 실행
```bash
claude
```

### 3. 분석 요청
```
SK하이닉스 공격적 관점으로 분석해줘
```

끝입니다! 🎉

---

## 💬 사용 예시

### 종합 분석 (5명 풀가동)
```
삼성전자 공격적 관점으로 분석해줘
NVIDIA 지금 살만해?
LG에너지솔루션 투자 전략 짜줘
```

### 단일 에이전트 호출
```
@financial-analyst 삼성전자 PER 얼마야?
@valuation-expert 카카오 DCF 분석해줘
@news-sentiment-analyst 최근 한달 SK하이닉스 뉴스 분석해줘
```

---

## 📊 최종 리포트 산출물

- ✅ 투자 의견 (매수/보유/매도)
- ✅ 확신도 점수 (1~10점)
- ✅ 진입가 · 목표가 · 손절가
- ✅ 포지션 사이징 (포트폴리오 비중 %)
- ✅ 핵심 촉매 이벤트
- ✅ 리스크 시나리오

---

## 📁 파일 구조

```
stock-agents/
└── .claude/
    └── agents/
        ├── financial-analyst.md
        ├── news-sentiment-analyst.md
        ├── sector-researcher.md
        ├── valuation-expert.md
        └── aggressive-investment-strategist.md
```

---

## ⚠️ 주의사항

> 이 도구는 **투자 참고용**입니다.  
> 실제 투자 결정은 본인 판단 하에 이루어져야 하며,  
> 투자 손실에 대한 책임은 본인에게 있습니다.

- `aggressive-investment-strategist`는 **Opus 모델** 사용 (비용 높음)
- 단순 조회는 `@financial-analyst`만 호출해서 비용 절감 가능

---

## 🛠️ 요구사항

- [Claude Code](https://claude.ai/code) 설치
- Anthropic API 키 (유료 플랜 권장)

---

## 📜 라이선스

MIT License

---

*Built with ❤️ using [Claude Code](https://claude.ai/code) Sub-agents*
