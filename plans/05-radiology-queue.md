# 프로젝트 5: 판독 대기 시간 예측

> **Agent:** Resource Agent
> **예상 기간:** 3주 (Phase 1)

## 1. 목표
검사 접수 데이터를 기반으로 판독 대기 시간을 예측하고 판독 대기 시간 25% 감소

## 2. 모델: XGBoost + Queue Simulation
- 검사 종류별 판독 시간 분포 학습
- 판독사별 처리 속도 프로파일
- Queueing Theory 기반 대기열 시뮬레이션

## 3. Feature: 검사 종류(X-ray/CT/MRI/US), 긴급도, 판독사 수, 시간대, 접수 누적량

## 4. 참고 사례
- JLK Inspection AI — https://www.jlk.kr
- Lunit INSIGHT CXR — https://www.lunit.io
- Vuno Med — https://www.vuno.co/en
- 두개내출혈 DL 탐지 — https://pubmed.ncbi.nlm.nih.gov/39017032/
