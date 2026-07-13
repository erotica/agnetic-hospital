# 프로젝트 14: 검체 이송 로봇 최적 경로 계획

> **Agent:** Resource Agent
> **예상 기간:** 4주 (Phase 3)

## 1. 목표
검체 이송 로봇의 경로를 최적화하여 검체 이송 시간 40% 감소, 로봇 가동률 30% 증가

## 2. 필요 데이터
- 검체 채취/접수 시간 및 위치
- 로봇 위치/상태/배터리
- 병동별 검체 발생 패턴
- 병원 건물 맵 (엘리베이터/통로)

## 3. 모델: VRP(차량 경로 문제) + 실시간 동적 라우팅
- OR-Tools CP-SAT 기반 경로 최적화
- 실시간 검체 발생 이벤트 → 동적 재라우팅
- 배터리 충전 스케줄 통합 최적화

## 4. 참고 사례
- Aethon TUG Robots — https://aethon.com
- MediBot (GitHub) — https://github.com/AshishYadav165/medibot-specimen-runner-state-machine
- 병원 물류 로봇 효과 분석 — https://pubmed.ncbi.nlm.nih.gov/42031898/
