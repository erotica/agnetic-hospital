# 프로젝트 16: 간호사 근무표 자동 생성 및 최적화

> **Agent:** Resource Agent
> **예상 기간:** 5주 (Phase 3)

## 1. 목표
환자 중증도/간호 강도 + 간호사 선호도 반영 최적 근무표, 수립 시간 90% 감소

## 2. 모델: OR(혼합 정수 최적화) + XGBoost(간호 강도 예측)

제약 조건:
- 연속 3일 이상 근무 금지
- 야간 근무 후 24시간 휴식
- 법정 근무 시간 준수
- 간호사 대 환자 비율 유지

목적 함수: 선호도 최대화 + 간호 강도 균등 배분

## 3. 참고 사례
- Explainable AI for Nurse Scheduling — https://pubmed.ncbi.nlm.nih.gov/42391503/
- Hospital Workforce Scheduling (GitHub) — https://github.com/Vasundhara-Boomi/Hospital-Workforce-Scheduling-System
