# 프로젝트 4: 처방전 자동 검토 및 이상 알림

> **Agent:** Clinical Quality Agent
> **예상 기간:** 4주 (Phase 3)

## 1. 목표
NLP + Rule Engine으로 처방전을 자동 검토하여 약사 업무 40% 감소, 조제 오류 50% 감소

## 2. 필요 데이터
| 데이터 | 출처 |
|:-------|:------|
| 처방전 데이터 (약품코드/용량/횟수) | OCS |
| 환자 진단/알레르기 정보 | EMR |
| 약품 DB (성분/상호작용/금기) | KIMS/DrugBank |
| 과거 이상반응 보고 | 약제부 |
| 신장/간 기능 검사 결과 | LIS |

## 3. 모델 구조

Step 1: Rule Engine 기반 1차 검증 (용량/금기/상호작용)
Step 2: XGBoost 기반 이상 처방 탐지
Step 3: Knowledge Graph 기반 대체 약품 추천

## 4. 출력물
- 처방 검토 결과 (PASS/WARN/STOP)
- 용량 이상 경보, 약품 상호작용 경고
- 대체 약품 추천 및 근거

## 5. 평가 지표: 검토 정확도 95% 이상, 오탐률 5% 미만

## 6. 참고 사례
- ML 부적절 처방 탐지 — https://pubmed.ncbi.nlm.nih.gov/38050067/
- 약학적 중재 예측 ML — https://pubmed.ncbi.nlm.nih.gov/39955846/
