# 프로젝트 17: 환자 만족도 예측 및 서비스 개선

> **Agent:** Financial Agent
> **예상 기간:** 3주 (Phase 1)

## 1. 목표
EMR/설문 데이터 기반 만족도 예측으로 환자 만족도 10% 증가

## 2. 모델: NLP + XGBoost + SHAP
- 환자 피드백 텍스트 → NLP 감성 분석 (Korean BERT)
- 진료 대기/응대 시간 → XGBoost 회귀
- SHAP으로 만족도 결정 요인 분석

## 3. 출력
- 부서별/항목별 예상 만족도 점수
- 만족도 결정 요인 분석 리포트
- 개선 우선순위 추천

## 4. 참고: https://pubmed.ncbi.nlm.nih.gov/42393679/
