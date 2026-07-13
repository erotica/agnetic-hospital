# 프로젝트 19: 의료장비 예측 유지보수

> **Agent:** Resource Agent
> **예상 기간:** 4주 (Phase 3)

## 1. 목표
장비 작동 데이터 분석으로 고장 사전 예측, 장비 다운타임 50% 감소, 유지보수 비용 25% 감소

## 2. Feature
- 장비 온도/진동 추이 (IoT 센서)
- 에러 코드 발생 빈도
- 사용 시간 누적
- 마지막 정비 후 경과일
- 부품 교체 주기 대비 사용량

## 3. 모델: LSTM(시계열 이상 탐지) + Random Forest(고장 분류)
- LSTM: 정상 작동 패턴 학습 → 이상 징후 조기 탐지
- RF: 고장 유형 분류 및 잔여 수명 예측

## 4. 참고 사례
- Medical Equipment Failure (GitHub) — https://github.com/DARAads/PREDECTING-MEDICAL-EQUIPMENT-FAILURE
- ResourceHive (GitHub) — https://github.com/sahilkrishna123/resourcehive-final-year-project
