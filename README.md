# 🏥 Agnetic Hospital

**Multi-Agent System for University Hospital AI — CDC + FHIR R4 기반 통합 예측·자동화 플랫폼**

> 20개 병원 AI 프로젝트를 6개 전담 Agent로 통합 운영하는 Multi-Agent 시스템

## 🎯 개요

Agnetic Hospital은 대학병원의 EMR/OCS/LIS 데이터를 **CDC(Change Data Capture)** 로 실시간 COPY하고 **FHIR R4**로 표준화하여, 6개 전담 Agent(Patient Flow, Resource, Clinical Quality, Financial, Supply Chain, Research)가 협업하는 **Multi-Agent 플랫폼**입니다.

## 🏗 시스템 아키텍처

```
운영 DB (Oracle/MSSQL)
     ↓ CDC (Debezium + Kafka)
FHIR R4 표준화 (Patient/Observation/Encounter)
     ↓ 실시간 COPY
Analytics Data Platform (PostgreSQL + ES + Redis)
     ↓ Feature Store
┌──────────────────────────────────────────────┐
│           6개 전담 Agent + Orchestrator       │
└──────────────────────────────────────────────┘
     ↓
통합 대시보드 + Action Engine
```

## 📋 Agent 구성 (20개 프로젝트)

| Agent | 담당 프로젝트 | 핵심 모델 |
|:------|:-------------|:----------|
| 🧑‍⚕️ **Patient Flow** | 병상배치·응급실·퇴원·재입원·No-Show | Prophet + XGBoost |
| 🏗️ **Resource** | 수술실·검체로봇·간호스케줄·장비정비 | OR + LSTM |
| 🩺 **Clinical Quality** | 처방검토·감염·기록요약·약품상호작용·처방패턴 | NLP + KG + LLM |
| 💰 **Financial** | 진료비심사·폐기물·만족도 | Anomaly Detection |
| 📦 **Supply Chain** | 재고예측·구매추천·검사량예측 | ARIMA + XGBoost |
| 🔬 **Research** | 임상시험매칭 | NLP + Rule Engine |

## 🛠 기술 스택

| 계층 | 기술 |
|:-----|:------|
| **CDC** | Debezium + Kafka |
| **표준화** | FHIR R4 + SNOMED CT / LOINC |
| **ML** | Prophet, XGBoost, ARIMA, LSTM, XGBoost |
| **LLM** | Claude Code CLI / Ollama |
| **Agent Framework** | LangChain / CrewAI 기반 |
| **Backend** | FastAPI + PostgreSQL + Redis |
| **Frontend** | React + TypeScript + D3.js |
| **Infra** | Docker + Kubernetes |

## 📂 구조

```
agnetic-hospital/
├── agent_framework/     # Agent Base Class, Event Bus, Orchestrator
├── agents/              # 6개 전담 Agent
├── models/              # 모델 저장소
├── shared/              # Feature Store, FHIR 변환기, CDC 파이프라인
├── api/                 # API Gateway
├── frontend/            # 통합 대시보드
├── docs/                # 설계 문서
└── plans/               # 20개 프로젝트 상세 구현 계획
```

## 🔗 링크

- GitHub: https://github.com/erotica/agnetic-hospital
- 상세 설계: [docs/multi-agent-system.md](docs/multi-agent-system.md)
- 국내외 사례: [docs/research-cases.md](docs/research-cases.md)
