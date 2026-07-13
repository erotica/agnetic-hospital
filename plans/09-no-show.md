# 프로젝트 9: 외래 No-Show 예측 및 예약 최적화

> **Agent:** Patient Flow Agent
> **예상 기간:** 3주 (Phase 1 - Quick Win)

## 1. 목표
환자 특성과 과거 패턴으로 No-Show 위험을 예측하여 No-Show율 30% 감소, 외래 가동률 15% 증가

## 2. 모델: XGBoost + Overbooking 최적화

핵심 Feature:
- 과거 No-Show 횟수/비율 (가장 중요한 변수)
- 예약-진료 간격 (7일 초과 시 위험 증가)
- 거주지-병원 거리
- 강수량/미세먼지
- 예약 시간대 (오후 > 오전)

## 3. 출력
- 개인별 No-Show 확률
- 오버부킹 추천 인원
- No-Show 고위험 환자 리스트
- 알림 문자 발송 추천 시간

## 4. 참고 사례
- Radiology No-Show 예측 — https://pubmed.ncbi.nlm.nih.gov/41927364/
- Medical Appointment No-Show (GitHub) — https://github.com/TimKong21/Medical-Appointment-No-Show-Prediction
