# 프로젝트 8: 의무 기록 자동 요약 및 코드 추천

> **Agent:** Clinical Quality Agent
> **예상 기간:** 5주 (Phase 3)

## 1. 목표
LLM/NLP로 의사 진료 기록을 자동 요약하고 진단/수술 코드(KCD/EDI) 추천, 기록 시간 50% 감소

## 2. 모델: LLM (Claude Code) + NER + Classification
- 의사 진료 기록 텍스트 → LLM 요약
- NER로 진단명/시술명 추출
- 코드 추천 (KCD-8, EDI)

## 3. 출력
- 자동 생성 진료 기록 요약
- 추천 진단 코드 (확률 순위)
- 누락 코드 경고

## 4. 참고 사례
- JLK 의무기록 요약 AI — https://www.jlk.kr
- MEDICAL IP AI Note — https://www.medicalip.com
- Seq2Seq 경과기록 요약 — https://pubmed.ncbi.nlm.nih.gov/36268128/
