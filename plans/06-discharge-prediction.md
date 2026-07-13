# 프로젝트 6: 퇴원 예측 및 환자 흐름 관리

> **Agent:** Patient Flow Agent
> **예상 기간:** 4주 (Phase 2)

## 1. 목표
입원 환자의 퇴원 시점을 예측하여 병상 회전을 최적화하고 평균 재원일수 10% 감소

## 2. 모델: XGBoost + Survival Analysis (Cox Proportional Hazards)

핵심 Feature: 주 진단(ICD-10), 동반 질환 수, 수술 유무/종류, LOS 경과일, 퇴원 목적지, 간호 등급, 검사 결과 정상화율

## 3. 출력
- 개인별 예상 퇴원일 (신뢰구간)
- 조기 퇴원 가능 환자 리스트
- 퇴원 지연 요인 분석
- 퇴원 후 사후 관리 추천

## 4. 참고: https://pubmed.ncbi.nlm.nih.gov/38291406/
