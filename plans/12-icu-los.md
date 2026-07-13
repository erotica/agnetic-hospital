# 프로젝트 12: ICU 재실 기간 예측

> **Agent:** Clinical Quality Agent
> **예상 기간:** 4주 (Phase 2)

## 1. 목표
ICU 입실 환자 임상 데이터 기반 재실 기간 예측, ICU 병상 계획 정확도 25% 증가

## 2. 모델: LSTM(시계열) + XGBoost(초기 평가)
- LSTM: 첫 24시간 활력징후 시계열 학습
- XGBoost: APACHE/SOFA 점수, 진단, 동반질환 기반 예측
- 앙상블: 가중치 결합

## 3. Feature
- APACHE II/III 점수, SOFA 점수 추이
- 진단 코드, 연령, 동반질환
- 첫 24시간 활력징후 변동성
- 인공호흡기 사용 여부

## 4. 참고
- 공평한 ICU LOS 조기 예측 — https://pubmed.ncbi.nlm.nih.gov/42394012/
- CABG 환자 ICU LOS (MIMIC+eICU) — https://pubmed.ncbi.nlm.nih.gov/42365267/
- GitHub: XAI 기반 ICU LOS — https://github.com/venkateswarlunaidu/ICU-Length-of-Stay-Prediction
