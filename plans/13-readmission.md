# 프로젝트 13: 재입원 위험 예측 및 퇴원 관리

> **Agent:** Patient Flow Agent
> **예상 기간:** 4주 (Phase 2)

## 1. 목표
퇴원 환자의 30일 내 재입원 위험 예측으로 재입원율 15% 감소

## 2. 모델: XGBoost + LACE Index + Survival Analysis
- LACE Index 확장: LOS, Acute, Charlson, ED visits
- XGBoost: 추가 임상 Feature 포함

핵심 Feature: 퇴원 전 Lab 마지막 값, 퇴원처(집/요양원), 약제 수, 정기 외래 방문 여부, 주 진단, 동반질환 수

## 3. 출력
- 재입원 위험 점수 (LOW/MED/HIGH)
- 위험 요인별 설명
- 퇴원 후 관리 계획 추천

## 4. 참고
- 정신과 재입원 예측 — https://pubmed.ncbi.nlm.nih.gov/42421253/
- 30일 재입원 예측 (대만) — https://pubmed.ncbi.nlm.nih.gov/42387552/
