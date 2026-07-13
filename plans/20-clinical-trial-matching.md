# 프로젝트 20: 임상시험 환자 모집 자동 매칭

> **Agent:** Research Agent
> **예상 기간:** 5주 (Phase 3)

## 1. 목표
EMR 데이터 기반 임상시험 조건-환자 자동 매칭, 환자 모집 기간 40% 감소

## 2. 모델: NLP(프로토콜 파싱) + Rule-based Matching + Ranking
- NLP: 임상시험 프로토콜에서 선정/제외 조건 자동 추출
- Rule Engine: 환자 EMR 데이터와 조건 매칭
- Ranking: 적합도 점수 기반 우선순위

## 3. 출력
- 시험별 적격 환자 리스트 (우선순위)
- 환자별 매칭 가능 시험 목록
- 스크리닝 성공 확률 예측

## 4. 참고 사례
- TrialMatchAI — https://pubmed.ncbi.nlm.nih.gov/41876500/
- Clinical Trial Matching Pipeline (GitHub) — https://github.com/snaidu20/Clinical-Trial-Matching-AI-Pipeline
- LLM for Trial Recruitment — https://pubmed.ncbi.nlm.nih.gov/42202663/
