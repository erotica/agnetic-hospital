# Agnetic Hospital — Agent Architecture

> 6개 전담 Agent의 역할 정의, 업무 경계, 협업 프로토콜, 충돌 해결

---

## 1. Agent 개요

```
                    ┌─────────────────────────────────────┐
                    │         Orchestrator Agent           │
                    │  (조정 / 충돌 해결 / 우선순위 결정)   │
                    └─────────────────────────────────────┘
                                   │
        ┌──────────────┬───────────┼───────────┬──────────────┐
        │              │           │           │              │
   ┌────▼────┐   ┌────▼────┐ ┌───▼───┐  ┌───▼───┐     ┌────▼────┐
   │Patient  │   │Resource │ │Clinical│  │Finan- │     │ Supply  │
   │Flow     │   │         │ │Quality │  │cial   │     │ Chain   │
   └────┬────┘   └────┬────┘ └───┬───┘  └───┬───┘     └────┬────┘
        │              │           │           │              │
        └──────────────┴───────────┼───────────┴──────────────┘
                                   │
                              ┌────▼────┐
                              │ Research │
                              │ Agent    │
                              └─────────┘
```

### Agent 요약

| Agent | ID | 담당 도메인 | 프로세스 수 | 핵심 데이터 |
|:------|:---|:-----------|:---------:|:-----------|
| 🧑‍⚕️ **Patient Flow** | `patient_flow` | 환자 흐름 관리 | 5 | 입원/퇴원/예약/ED |
| 🏗️ **Resource** | `resource` | 자원/시설/인력 | 4 | 수술실/로봇/간호사/장비 |
| 🩺 **Clinical Quality** | `clinical_quality` | 임상 질/안전 | 5 | 처방/감염/기록/약품 |
| 💰 **Financial** | `financial` | 재무/원가/만족도 | 3 | 청구/폐기물/설문 |
| 📦 **Supply Chain** | `supply_chain` | 공급망/재고 | 3 | 재고/발주/검사량 |
| 🔬 **Research** | `research` | 연구/임상시험 | 1 | 시험프로토콜/환자 |

---

## 2. Agent별 상세 정의

### 2.1 Patient Flow Agent (환자 흐름)

**목적:** 병원 전체 환자 흐름을 예측하고 최적화하여 병상 가동률을 극대화하고 대기 시간을 최소화한다.

**담당 프로세스:**

| 프로세스 | 설명 | 입력 데이터 | 출력 |
|:---------|:-----|:----------|:------|
| 병상 배치 최적화 | 입원 수요 예측 → 병상 할당 | EMR 입원/퇴원 | 병상 배치표 |
| 응급실 도착 예측 | ED 도착 패턴 예측 | ED 로그/119 | 시간대별 예측 |
| 퇴원 예측 | 퇴원 시점 예측 | 임상 데이터 | 예상 퇴원일 |
| 재입원 위험 예측 | 30일 재입원 위험 | 퇴원 데이터 | 위험 점수 |
| No-Show 예측 | 외래 No-Show 위험 | 예약 이력 | No-Show 확률 |

**협업 포인트:**
- → **Resource Agent**: "내일 병상 N개 부족 → 병동 재배치 요청"
- → **Clinical Quality Agent**: "퇴원 예정 환자 중 재입원 고위험 N명 → 집중 관리 요청"
- ← **Resource Agent**: "수술실 스케줄 변경 → 입원 일정 영향"

**상태:**
```python
class PatientFlowAgent(BaseAgent):
    agent_id = "patient_flow"
    
    # 구독 이벤트
    subscriptions = [
        EventType.PATIENT_ADMITTED,      # 입원 발생
        EventType.PATIENT_DISCHARGED,    # 퇴원 발생
        EventType.ED_ARRIVAL_SURGE,      # 응급실 쏠림
        EventType.OR_SCHEDULE_CHANGED,   # 수술 일정 변경
    ]
    
    # 발행 이벤트
    emissions = [
        EventType.BED_SHORTAGE,          # 병상 부족 예상
        EventType.DISCHARGE_WINDOW,      # 퇴원 추천 시간
        EventType.READMISSION_RISK,      # 재입원 고위험
    ]
```

---

### 2.2 Resource Agent (자원 관리)

**목적:** 병원의 물리적 자원(수술실, 장비, 로봇, 인력)을 최적으로 배분하고 가동률을 극대화한다.

**담당 프로세스:**

| 프로세스 | 설명 | 입력 데이터 | 출력 |
|:---------|:-----|:----------|:------|
| 수술실 스케줄링 | 수술 시간 예측 + 배정 | 수술 이력/의사 프로파일 | 스케줄표 |
| 검체 이송 로봇 | 로봇 경로 최적화 | 검체 발생/로봇 위치 | 이동 경로 |
| 간호사 근무표 | 인력 배치 최적화 | 간호사/환자 중증도 | 근무표 |
| 장비 예측 정비 | 장비 고장 예측 | 센서/로그 | 정비 일정 |

**협업 포인트:**
- ← **Patient Flow Agent**: "병상 부족 → 재배치 요청" → 병동 재배치 실행
- ← **Supply Chain Agent**: "재고 부족 → 수술 재료 확보 요청"
- → **Patient Flow Agent**: "수술실 스케줄 변경 → 입원 일정 조정 필요"
- → **Clinical Quality Agent**: "장비 정비 예정 → 해당 검사 일정 조정"

---

### 2.3 Clinical Quality Agent (임상 질 관리)

**목적:** 환자 안전과 임상 질을 향상시키기 위해 처방/감염/기록을 실시간 모니터링하고 이상을 탐지한다.

**담당 프로세스:**

| 프로세스 | 설명 | 입력 데이터 | 출력 |
|:---------|:-----|:----------|:------|
| 처방전 자동 검토 | 처방 이상 실시간 경고 | OCS 처방/환자 정보 | WARN/STOP |
| 감염병 조기 탐지 | 원내 감염 실시간 감시 | LIS/EMR 발열/검사 | 감염 경보 |
| 의무 기록 요약 | LLM 기반 기록 요약 | 진료 기록 텍스트 | 요약문/코드 |
| 약품 상호작용 경고 | 약물-약물/약물-질병 | 처방/환자 진단 | 경고/대체안 |
| 처방 패턴 분석 | 의사별 처방 패턴 피드백 | 처방 통계 | 프로파일 리포트 |

**협업 포인트:**
- → **Patient Flow Agent**: "감염 의심 병동 → 신규 입원 중단 권고"
- → **Supply Chain Agent**: "감염 의심 → 방역 물품 추가 확보 요청"
- ← **Financial Agent**: "처방 패턴 이상 → 심사 강화 요청"

---

### 2.4 Financial Agent (재무 관리)

**목적:** 병원 재무 건전성을 확보하고 청구 효율성과 비용 절감을 추진한다.

**담당 프로세스:**

| 프로세스 | 설명 | 입력 데이터 | 출력 |
|:---------|:-----|:----------|:------|
| 진료비 심사 자동화 | 청구 내역 자동 검토 | 청구/심사 데이터 | PASS/REVIEW |
| 의료 폐기물 예측 | 폐기물 발생량 예측 | 환자/검사/수술 데이터 | 처리 계획 |
| 환자 만족도 예측 | 만족도 예측 및 분석 | 설문/진료 데이터 | 개선 리포트 |

**협업 포인트:**
- → **Clinical Quality Agent**: "처방 패턴 이상 감지 → 임상 검토 요청"
- ← **Supply Chain Agent**: "재고 비용 분석 → 예산 조정 요청"
- ← **Resource Agent**: "장비 정비 비용 → 예산 반영 요청"

---

### 2.5 Supply Chain Agent (공급망 관리)

**목적:** 병원 전체 공급망의 재고를 최적화하고 자동 발주를 통해 품절을 방지하고 재고 비용을 절감한다.

**담당 프로세스:**

| 프로세스 | 설명 | 입력 데이터 | 출력 |
|:---------|:-----|:----------|:------|
| 재고 예측 및 구매 추천 | 검사/시술 기반 재고 예측 | LIS/OCS/재고 DB | 구매 추천 목록 |
| 검사량 예측 | 검사량 기반 시약 예측 | LIS/예약/의사패턴 | 시약 필요량 |
| 발주 자동화 | 자동 발주 + 승인 워크플로우 | 예측/재고/예산 | 발주서 |

**협업 포인트:**
- ← **Patient Flow Agent**: "입원 환자 증가 예상 → 소모품 추가 확보"
- ← **Resource Agent**: "수술 건수 증가 → 수술 재료 추가 발주"
- → **Financial Agent**: "월별 재고 비용 리포트 전송"

---

### 2.6 Research Agent (연구 관리)

**목적:** 임상시험 환자 모집을 자동화하고 연구 데이터를 분석하여 연구 효율성을 높인다.

**담당 프로세스:**

| 프로세스 | 설명 | 입력 데이터 | 출력 |
|:---------|:-----|:----------|:------|
| 임상시험 환자 매칭 | 조건-환자 자동 매칭 | EMR/프로토콜 | 적격 환자 리스트 |

**협업 포인트:**
- ← **Clinical Quality Agent**: "환자 진단/검사 정보 조회"
- ← **Patient Flow Agent**: "입원/외래 환자 명단 조회"

---

## 3. Agent 간 협업 프로토콜

### 3.1 통신 방식: Event Bus + RPC Call

```
┌──────────────────────────────────────────────────────────┐
│                    Event Bus (Kafka)                      │
│                                                           │
│  Topics:                                                  │
│    agent.patient_flow.events   ──┐                        │
│    agent.resource.events        │ │                       │
│    agent.clinical.events        │ │  모든 Agent가          │
│    agent.financial.events       ├─┤  발행/구독             │
│    agent.supply_chain.events    │ │                       │
│    agent.research.events       ─┘                        │
│    system.alerts                (전체 알림)               │
│    system.anomaly               (이상 탐지)               │
└──────────────────────────────────────────────────────────┘
         │                              ▲
         │ Publish                      │ Subscribe
    ┌────▼────┐                    ┌────┴────┐
    │ Agent A │──── RPC Call ─────▶│ Agent B │
    │         │◁─── Response ──────│         │
    └─────────┘                    └─────────┘
```

### 3.2 이벤트 유형 정의

```python
class EventType(Enum):
    # 환자 흐름 (Patient Flow)
    PATIENT_ADMITTED = "patient_admitted"         # 입원 발생
    PATIENT_DISCHARGED = "patient_discharged"     # 퇴원 발생
    ED_CROWDING = "ed_crowding"                   # 응급실 혼잡
    BED_SHORTAGE = "bed_shortage"                 # 병상 부족
    READMISSION_HIGH_RISK = "readmission_high_risk"  # 재입원 고위험
    NO_SHOW_PREDICTED = "no_show_predicted"       # No-Show 예측

    # 자원 (Resource)
    OR_OVERBOOKED = "or_overbooked"               # 수술실 과밀
    EQUIPMENT_FAILURE_RISK = "equipment_failure"   # 장비 고장 위험
    STAFF_SHORTAGE = "staff_shortage"             # 인력 부족
    ROBOT_BATTERY_LOW = "robot_battery_low"       # 로봇 배터리

    # 임상 질 (Clinical Quality)
    PRESCRIPTION_ALERT = "prescription_alert"     # 처방 경고
    INFECTION_SUSPECTED = "infection_suspected"   # 감염 의심
    RECORD_READY = "record_ready"                 # 기록 요약 완료
    DRUG_INTERACTION = "drug_interaction"         # 약물 상호작용

    # 재무 (Financial)
    CLAIM_ANOMALY = "claim_anomaly"               # 청구 이상
    SATISFACTION_DROP = "satisfaction_drop"       # 만족도 하락
    WASTE_OVER_LIMIT = "waste_over_limit"         # 폐기물 초과

    # 공급망 (Supply Chain)
    STOCK_LOW = "stock_low"                       # 재고 부족
    ORDER_RECOMMENDED = "order_recommended"       # 구매 추천
    PRICE_SURGE = "price_surge"                   # 단가 급등

    # 시스템
    PREDICTION_UPDATED = "prediction_updated"     # 예측 갱신
    MODEL_ACCURACY_DROP = "model_accuracy_drop"  # 모델 정확도 하락
    CDC_LAG = "cdc_lag"                           # CDC 지연
```

### 3.3 Agent 간 RPC 인터페이스

각 Agent는 표준 RPC 엔드포인트를 통해 다른 Agent의 기능을 직접 호출할 수 있다.

```python
# agent_framework/rpc_registry.py

RPC_REGISTRY = {
    'patient_flow': {
        'get_bed_occupancy': {'args': ['date'], 'returns': 'dict'},
        'get_predicted_admissions': {'args': ['start_date', 'end_date'], 'returns': 'list'},
        'get_discharge_candidates': {'args': ['date', 'count'], 'returns': 'list'},
        'get_readmission_risk': {'args': ['patient_id'], 'returns': 'float'},
    },
    'resource': {
        'get_or_schedule': {'args': ['date'], 'returns': 'list'},
        'get_available_beds': {'args': ['ward', 'type'], 'returns': 'int'},
        'get_staff_schedule': {'args': ['date', 'dept'], 'returns': 'list'},
        'request_bed_reallocation': {'args': ['count', 'from_ward', 'to_ward'], 'returns': 'bool'},
    },
    'clinical_quality': {
        'get_infection_status': {'args': ['ward'], 'returns': 'dict'},
        'get_prescription_alerts': {'args': ['date'], 'returns': 'list'},
        'request_prescription_review': {'args': ['prescription_id'], 'returns': 'dict'},
    },
    'financial': {
        'get_claim_stats': {'args': ['date'], 'returns': 'dict'},
        'get_waste_forecast': {'args': ['date'], 'returns': 'float'},
    },
    'supply_chain': {
        'get_inventory_status': {'args': ['item_id'], 'returns': 'dict'},
        'get_stock_alert': {'args': [], 'returns': 'list'},
        'request_urgent_order': {'args': ['item_id', 'quantity'], 'returns': 'bool'},
    },
    'research': {
        'get_eligible_patients': {'args': ['trial_id'], 'returns': 'list'},
        'get_trial_status': {'args': ['trial_id'], 'returns': 'dict'},
    },
}
```

---

## 4. 협업 시나리오 (6가지)

### 시나리오 1: 병상 부족 → 통합 자원 재배치

```
Trigger: Patient Flow Agent가 내일 병상 5개 부족 예측
         ↓ (publish: BED_SHORTAGE)
Orchestrator: 충돌 검사 시작
         ↓ (RPC: get_discharge_candidates)
Patient Flow: 퇴원 예정 환자 8명 중 5명 조기 퇴원 가능
         ↓ (RPC: get_or_schedule)
Resource: 수술실 일정 변경 없음, 병동 간 재배치 가능
         ↓
Clinical Quality: 재입원 위험 평가 → 퇴원 가능 환자 승인
         ↓
Orchestrator: 최종 결정 → Action 실행
         ├── Patient Flow: 퇴원 추천 리스트 전송
         ├── Resource: 병동 재배치 실행
         └── Supply Chain: 퇴원/전과에 따른 소모품 재조정
```

### 시나리오 2: 응급실 쏠림 → 전사 대응

```
Trigger: ED 도착 급증 (평시 대비 3배)
         ↓ (publish: ED_CROWDING, priority=HIGH)
Patient Flow: 예측 갱신, 입원 필요 환자 분류
         ↓
Resource: ED 의료진 추가 배치, 검사실 연장 운영
         ↓
Clinical Quality: 중증도 재평가 → 입원 우선순위 결정
         ↓
Supply Chain: ED 소모품 긴급 보충
         ↓
Financial: 초과 근무 비용 추정 → 예산 조정
```

### 시나리오 3: 감염 의심 → 격리 및 방역

```
Trigger: Clinical Quality가 3병동 발열 환자 집중 탐지
         ↓ (publish: INFECTION_SUSPECTED, priority=CRITICAL)
Resource: 격리실 확보 (현재 80% → 2개 추가)
Patient Flow: 3병동 신규 입원 중단, 전과 금지
Supply Chain: 방역 물품 재고 확인 → 추가 발주
         ↓
Orchestrator: 감염관리위원회 자동 보고서 생성
         └── 감염 경로 분석 → 확산 예측 리포트
```

### 시나리오 4: 재고 부족 → 긴급 발주

```
Trigger: Supply Chain이 주요 시약 재고 부족 감지
         ↓ (publish: STOCK_LOW)
Patient Flow: 향후 3일 검사량 예측 전달
Resource: 대체 장비/방법 가동 가능 여부 확인
         ↓
Orchestrator: 긴급 발주 결정
└── Supply Chain: 자동 발주 실행 + 공급사 협상
└── Financial: 예산 변동 내역 기록
```

### 시나리오 5: 수술실 과밀 → 일정 재조정

```
Trigger: Resource가 내일 수술실 3개 부족 예상
         ↓ (publish: OR_OVERBOOKED)
Patient Flow: 연기 가능한 수술 환자 명단 + 입원 일정 조정
Clinical Quality: 연기된 수술 환자 위험도 평가
         ↓
Orchestrator: 최종 조정안 승인
└── Resource: 수술실 재배정표 업데이트
└── Patient Flow: 환자별 입원/수술 일정 재통보
```

### 시나리오 6: 청구 패턴 이상 탐지

```
Trigger: Financial이 특정 진료과 청구 패턴 이상 감지
         ↓ (publish: CLAIM_ANOMALY)
Clinical Quality: 처방 패턴 분석 의뢰 → 적정성 평가
         ↓
Orchestrator: 심층 분석 후 피드백
└── Financial: 심사 강화 대상 지정
└── Clinical Quality: 처방 행태 개선 권고 리포트
```

---

## 5. 충돌 감지 및 해결

### 5.1 충돌 유형

| 충돌 유형 | 예시 | 관련 Agent |
|:----------|:-----|:-----------|
| **자원 경합** | Patient Flow가 병상 A 요청 ↔ Resource가 병상 A를 다른 용도로 배정 | Patient Flow ↔ Resource |
| **우선순위 충돌** | Financial이 No-Show 감소 제안 ↔ Clinical Quality가 진료 질 우선 | Financial ↔ Clinical |
| **일정 충돌** | Resource가 수술 연장 ↔ Patient Flow가 퇴원 예정 | Resource ↔ Patient Flow |
| **데이터 불일치** | 두 Agent의 동일 Feature 값이 다름 | 모든 Agent |
| **정책 충돌** | Supply Chain이 최소 재고 요청 ↔ Financial이 예산 삭감 | Supply ↔ Financial |

### 5.2 충돌 해결 매트릭스

```python
# agent_framework/conflict_matrix.py

CONFLICT_MATRIX = {
    # (요청Agent, 대상Agent, 자원유형) -> 해결방식
    ('patient_flow', 'resource', 'bed'): {
        'resolver': 'orchestrator',  # 오케스트레이터 중재
        'priority_rule': 'clinical_urgency_first',  # 임상적 긴급도 우선
        'fallback': 'escalate_to_human',  # 사람에게 에스컬레이션
    },
    ('resource', 'patient_flow', 'discharge'): {
        'resolver': 'negotiation',  # 양측 협상
        'priority_rule': 'balanced',  # 균형
    },
    ('clinical_quality', 'financial', 'prescription'): {
        'resolver': 'clinical_over_financial',  # 임상 우선
        'priority_rule': 'safety_first',  # 안전 최우선
    },
    ('supply_chain', 'financial', 'budget'): {
        'resolver': 'orchestrator',
        'priority_rule': 'cost_benefit_analysis',
    },
}
```

### 5.3 우선순위 규칙

```
CRITICAL (P0) → 즉시 실행, 사람 개입 불필요
  - 감염 의심, 응급실 쏠림, 장비 고장 임박

HIGH (P1) → 1시간 내 실행, Agent간 협의 후 자동 결정
  - 병상 부족, 재고 부족, 수술실 과밀

MEDIUM (P2) → 24시간 내 실행, 오케스트레이터 승인
  - 청구 이상, 처방 패턴, 간호사 스케줄

LOW (P3) → 주간 계획에 반영
  - 만족도 분석, 폐기물 최적화, 기록 품질
```

---

## 6. 데이터 공유 및 Feature Store

### 6.1 Agent 간 공유 Feature

| Feature | 생성 Agent | 소비 Agent | 갱신 주기 |
|:--------|:----------|:----------|:---------:|
| 병상 가동률 | Patient Flow | Resource, Supply Chain | 실시간 |
| ED 혼잡도 | Patient Flow | Resource, Clinical | 실시간 |
| 수술실 가동률 | Resource | Patient Flow, Supply Chain | 매시간 |
| 감염 의심 지수 | Clinical Quality | Patient Flow, Resource | 실시간 |
| 재고 부족 품목 | Supply Chain | Resource, Financial | 실시간 |
| 청구 오류율 | Financial | Clinical Quality | 매일 |
| 재입원 위험도 | Patient Flow | Clinical Quality, Financial | 실시간 |

### 6.2 Feature Store 스키마

```sql
-- Agent 간 공유 Feature 테이블
CREATE TABLE shared_features (
    feature_key    VARCHAR(100) PRIMARY KEY,  -- e.g. 'bed:ward_3:occupancy'
    feature_value  JSONB NOT NULL,            -- e.g. {"rate": 0.87, "total": 50, "used": 44}
    source_agent   VARCHAR(50) NOT NULL,      -- 생성 Agent ID
    updated_at     TIMESTAMP NOT NULL,
    ttl_seconds    INTEGER DEFAULT 3600,      -- 자동 만료
    confidence     FLOAT DEFAULT 1.0          -- 신뢰도 (0~1)
);

-- Feature 변경 이력 (감사/디버깅)
CREATE TABLE feature_change_log (
    log_id         BIGSERIAL PRIMARY KEY,
    feature_key    VARCHAR(100),
    old_value      JSONB,
    new_value      JSONB,
    changed_by     VARCHAR(50),
    changed_at     TIMESTAMP DEFAULT NOW()
);
```

---

## 7. Orchestrator (통합 조정)

### 7.1 오케스트레이터 역할

1. **일일 사이클 관리**: 매일 00:30 모든 Agent 분석 실행 → 결과 취합
2. **충돌 감지 및 해결**: Agent 간 추천 충돌 탐지 → 해결
3. **우선순위 결정**: CRITICAL/HIGH/MEDIUM/LOW 분류
4. **통합 보고서 생성**: 병원장/부서장용 대시보드 데이터
5. **사람 개입 필요 시 에스컬레이션**: Agent가 결정 불가 시 담당자 호출

### 7.2 실행 사이클

```
00:00  CDC 동기화 상태 확인
00:15  Feature Store 일괄 갱신
00:30  Agent 일일 분석 시작 (병렬)
       ├── Patient Flow: 입원/퇴원/ED 예측
       ├── Resource: 수술실/장비/간호사 최적화
       ├── Clinical Quality: 처방/감염 탐지
       ├── Financial: 청구/폐기물 분석
       ├── Supply Chain: 재고/발주 예측
       └── Research: 임상시험 매칭
00:45  충돌 감지 + 해결
01:00  통합 대시보드 업데이트
01:15  알림 발송 (필요 시)
```

### 7.3 에스컬레이션 규칙

```python
ESCALATION_RULES = {
    'bed_crisis': {
        'condition': '가용 병상 < 5% AND 입원 대기 > 50명',
        'target': '의료원장/간호부장',
        'channel': 'SMS + Email + Dashboard Alert',
        'template': '긴급: 병상 부족 위기 - 현재 가용률 {rate}%',
    },
    'infection_outbreak': {
        'condition': '동일 병동 발열 환자 > 30%',
        'target': '감염관리실장',
        'channel': 'SMS + 전화',
        'template': '감염 의심: {ward}병동 발열률 {rate}%',
    },
    'budget_overrun': {
        'condition': '월 예산 대비 지출 > 120%',
        'target': '기획조정실장',
        'channel': 'Email + 리포트',
        'template': '예산 초과 경고: {dept} {rate}%',
    },
}
```

---

## 8. Agent 수명 주기 및 상태 관리

### 8.1 Agent 상태

```
INIT → IDLE → ANALYZING → WAITING → ACTION → IDLE
                ↓                      ↑
             ERROR ───→ RECOVERING ────┘
```

| 상태 | 설명 |
|:-----|:------|
| `INIT` | 초기화 중 (모델 로드, Feature Store 연결) |
| `IDLE` | 대기 중 (다음 실행 or 이벤트 대기) |
| `ANALYZING` | 분석 실행 중 (예측/최적화) |
| `WAITING` | 다른 Agent 응답 대기 중 |
| `ACTION` | 액션 실행 중 (알림/발주/API 호출) |
| `ERROR` | 오류 발생 → 자동 복구 시도 |
| `RECOVERING` | 복구 중 |

### 8.2 Agent 건강 모니터링

```python
# 각 Agent는 5초마다 Heartbeat 전송
HEARTBEAT_INTERVAL = 5  # seconds
MISSED_HEARTBEAT_LIMIT = 3  # 15초 이상 무응답 → 장애

# agent_framework/monitor.py
class AgentMonitor:
    def check_health(self) -> dict:
        results = {}
        for agent in self.agents.values():
            last = agent.last_heartbeat
            lag = time.time() - last
            results[agent.agent_id] = {
                'status': 'healthy' if lag < 15 else 'unreachable',
                'last_heartbeat': last,
                'lag_seconds': lag,
                'memory_usage': agent.get_memory_usage(),
                'predictions_today': agent.memory.predictions_today,
            }
        return results
```

---

## 9. 보안 및 거버넌스

| 원칙 | 설명 |
|:-----|:------|
| **운영 DB 직접 접근 금지** | 모든 Agent는 COPY DB만 READ |
| **Agent 간 인증** | RPC 호출 시 JWT 토큰 검증 |
| **감사 로그** | 모든 결정/액션은 DB에 기록 |
| **사람 승인 필요 액션** | 발주 > 1천만원, 병상 강제 이동 등 |
| **모델 버전 관리** | MLflow로 모든 모델 버전 추적 |
| **데이터 마스킹** | 환자 식별 정보는 Feature Store에서 마스킹 |

---

## 10. 구현 로드맵

| Phase | 기간 | 내용 | 산출물 |
|:------|:----:|:------|:--------|
| **P0** | 2주 | Agent Framework (Base Class, Event Bus, RPC) | 통신 인프라 |
| **P1** | 4주 | Patient Flow + Resource Agent | 2개 Agent |
| **P2** | 4주 | Clinical Quality + Supply Chain Agent | 4개 Agent |
| **P3** | 3주 | Financial + Research Agent | 6개 Agent 완료 |
| **P4** | 3주 | Orchestrator + Conflict Resolution | 통합 조정 |
| **P5** | 2주 | Dashboard + Monitoring | 통합 대시보드 |
| **P6** | 2주 | CDC + FHIR 파이프라인 연동 | 전체 통합 |

**총 예상 기간: 20주**
