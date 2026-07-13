# 프로젝트 18: 처방 패턴 분석 및 진료 행태 피드백

> **Agent:** Clinical Quality Agent
> **예상 기간:** 2주 (Phase 1 - Quick Win)

## 1. 목표
의사별 처방 패턴 분석 및 동료 비교 피드백 제공, 적정 처방률 15% 증가

## 2. 모델: K-means Clustering + SHAP + Peer Comparison
- 처방 강도, 항생제율, 주사제율, 검사 건수 등으로 클러스터링
- 동료 그룹 대비 비교 차트 자동 생성
- 이상 처방 패턴 (Outiler) 경고

## 3. 출력
- 의사별 처방 프로파일 리포트
- 동료 그룹 대비 비교 차트
- 가이드라인 기반 개선 추천

## 4. 참고 사례
- 심평원 처방패턴 분석 — https://www.hira.or.kr
- Drug Prescription Analysis (GitHub) — https://github.com/09Varshitha/From-Data-to-Diagnosis-Drug-Prescription-Analysis-Using-ML
