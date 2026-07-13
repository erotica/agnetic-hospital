# 프로젝트 11: 약품 상호작용 자동 경고

> **Agent:** Clinical Quality Agent
> **예상 기간:** 5주 (Phase 3)

## 1. 목표
환자 약물 처방 시 상호작용/금기 사항 실시간 분석, 약물 부작용 20% 감소

## 2. 모델: Knowledge Graph + Graph Neural Network
- 약품-약품, 약품-질병, 약품-유전자 관계 Knowledge Graph
- GNN으로 새로운 상호작용 예측
- FHIR MedicationRequest Resource 기반

## 3. 출력
- 실시간 상호작용 경고 (WARN/CAUTION/STOP)
- 대체 약품 제안
- 환자 모니터링 항목 추천

## 4. 참고
- 심평원 DUR — https://www.hira.or.kr/dur
- PharmaQuest AI — https://github.com/sureshtammali/PharmaQuest-AI
- ML 기반 QTc 변화 예측 — https://pubmed.ncbi.nlm.nih.gov/42083122/
