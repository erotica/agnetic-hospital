# 프로젝트 10: 진료비 심사 자동화 및 이상 탐지

> **Agent:** Financial Agent
> **예상 기간:** 3주 (Phase 1)

## 1. 목표
진료비 청구 내역 자동 검토로 심사 시간 60% 감소, 부당 청구 탐지율 40% 증가

## 2. 모델: Rule Engine + Anomaly Detection + XGBoost
- Rule Engine: 1차 규칙 기반 검증 (수가코드/진단 일치도)
- Anomaly Detection: 평균 청구액 대비 이상 편차 탐지
- XGBoost: 과거 심사 결과 기반 이상 청구 분류

## 3. 참고 사례
- 심평원 AI 심사 — https://www.hira.or.kr
- AI 의료 청구 자동화 — https://pubmed.ncbi.nlm.nih.gov/41384167/
- ClaimPilot AI (GitHub) — https://github.com/Chitnish/claimpilot-ai
