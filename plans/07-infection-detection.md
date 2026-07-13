# 프로젝트 7: 감염병 발생 조기 탐지

> **Agent:** Clinical Quality Agent
> **예상 기간:** 4주 (Phase 3)

## 1. 목표
EMR/LIS 데이터 실시간 모니터링 감염병 조기 탐지로 탐지 시간 70% 감소, 원내 감염률 30% 감소

## 2. 모델: 시계열 이상 탐지 + Graph Network
- 병동별 발열 환자 비율 시계열 이상 탐지
- Graph Network로 감염 확산 경로 예측
- Snowstorm(SNOMED CT) 기반 증상 매핑

## 3. 출력
- 감염 의심 조기 경보 (병동/시간)
- 잠재적 감염 경로 예측
- 격리실/보호구 필요량 예측

## 4. 참고: https://github.com/bbjonah/Digital-Epidemiology-System
