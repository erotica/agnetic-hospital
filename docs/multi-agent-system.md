# 🏥 인하대병원 Multi-Agent 시스템 — 상세 설계

> 20개 병원 AI 프로젝트를 하나의 통합 **Multi-Agent 시스템**으로 운영하는 아키텍처

---

## 1. 시스템 개요

### 1.1 왜 Multi-Agent인가?

| 단일 모델 접근법 | Multi-Agent 접근법 |
|:----------------|:-------------------|
| 각 프로젝트가 독립적 실행 | Agent 간 **데이터/예측 공유** |
| 중복 데이터 수집 | **공유 Knowledge Layer** |
| 결과물 단일 출력 | **협업 추론** (예: 병상배치 + 퇴원예측) |
| 수동 통합 필요 | **자동 오케스트레이션** |

### 1.2 핵심 원칙

```
공유 데이터 레이어 (EMR/OCS/LIS)
        ↓
 전담 Agent 들 (20개 프로젝트)
        ↓
 오케스트레이터 (협업 + 충돌 해결)
        ↓
 통합 대시보드 + Action Engine
```

---

## 2. Agent 구성

### 2.1 Agent 목록 (20개 → 8개 그룹)

```
┌─────────────────────────────────────────────────────────────┐
│                    Orchestrator Agent                        │
│  (전체 조정, 우선순위 결정, 자원 배분)                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │ Patient     │  │ Resource    │  │ Clinical    │         │
│  │ Flow Agent  │  │ Agent       │  │ Quality     │         │
│  │             │  │             │  │ Agent       │         │
│  │• 입원예측   │  │• 수술실     │  │• 처방검토   │         │
│  │• 응급실    │  │• 병상       │  │• 약품상호   │         │
│  │• 퇴원예측  │  │• 장비정비   │  │• 감염탐지   │         │
│  │• 재입원    │  │• 검체이송   │  │• 기록요약   │         │
│  │• No-Show   │  │• 간호스케줄 │  │• 처방패턴   │         │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘         │
│         │                │                │                  │
│  ┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐         │
│  │ Financial   │  │ Supply      │  │ Research    │         │
│  │ Agent       │  │ Chain Agent │  │ Agent       │         │
│  │             │  │             │  │             │         │
│  │• 진료비심사 │  │• 재고예측   │  │• 임상시험   │         │
│  │• 폐기물    │  │• 구매추천   │  │• 환자매칭   │         │
│  │• 만족도    │  │• 검사량예측 │  │• 문헌분석   │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                              │
├─────────────────────────────────────────────────────────────┤
│                    Knowledge Layer                           │
│  (통합 Feature Store + Model Registry + Event Bus)          │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 Agent별 상세

#### 🧑‍⚕️ Patient Flow Agent (환자 흐름)
| 담당 프로젝트 | Agent Role | 핵심 협업 대상 |
|:-------------|:-----------|:--------------|
| 1. 병상 배치 | 입원 수요 예측 → 병상 할당 | Resource Agent |
| 2. 응급실 도착 | ED 도착 예측 → 의료진 요청 | Resource Agent |
| 6. 퇴원 예측 | 퇴원 시점 예측 → 병상 회전 | Resource Agent |
| 9. No-Show | No-Show 위험 예측 → 오버부킹 | Financial Agent |
| 13. 재입원 | 재입원 위험 평가 → 퇴원 관리 | Clinical Quality Agent |

**Agent 행동 예시:**
```python
class PatientFlowAgent:
    """환자 흐름 관리 Agent"""
    
    def predict_bed_demand(self, date) -> dict:
        """입원 수요 예측 및 병상 부족 시 알림"""
        admissions = self.predict_admissions(date)
        discharges = self.call_agent('resource', 'get_planned_discharges', date)
        bed_gap = admissions - discharges
        if bed_gap > 0:
            self.alert(f"🛏️ {bed_gap}개 병상 부족 예상")
            self.call_agent('resource', 'suggest_bed_reallocation', bed_gap)
        return {'admissions': admissions, 'discharges': discharges}
    
    def on_event(self, event: Event):
        """실시간 이벤트 처리"""
        if event.type == 'ED_ARRIVAL_SURGE':
            self.call_agent('resource', 'request_extra_staff', 
                          dept='ED', count=event.data['count'])
```

#### 🏗️ Resource Agent (자원 관리)
| 담당 프로젝트 | Agent Role | 핵심 협업 대상 |
|:-------------|:-----------|:--------------|
| 3. 수술실 스케줄 | 수술실 배정 최적화 | Patient Flow |
| 14. 검체 이송 | 로봇 경로 최적화 | Supply Chain |
| 16. 간호사 근무표 | 인력 배치 최적화 | Patient Flow |
| 19. 장비 예측 정비 | 장비 가용성 관리 | 모든 Agent |

#### 🩺 Clinical Quality Agent (임상 질 관리)
| 담당 프로젝트 | Agent Role | 핵심 협업 대상 |
|:-------------|:-----------|:--------------|
| 4. 처방 검토 | 처방 안전성 검증 | 약품 상호작용 Agent |
| 7. 감염 탐지 | 감염 조기 경보 | Patient Flow |
| 8. 기록 요약 | 의무 기록 자동화 | 모든 Agent |
| 11. 약품 상호작용 | 약물 안전 관리 | 처방 검토 Agent |
| 18. 처방 패턴 | 진료 행태 분석 | Quality Agent |

---

## 3. 통신 및 협업 프로토콜

### 3.1 Event Bus 아키텍처

```python
# agent_framework/event_bus.py
from enum import Enum
from dataclasses import dataclass, field
from typing import Any, Callable
import asyncio

class EventType(Enum):
    PATIENT_ADMITTED = "patient_admitted"
    PATIENT_DISCHARGED = "patient_discharged"
    ED_ARRIVAL_SURGE = "ed_arrival_surge"
    BED_SHORTAGE = "bed_shortage"
    STAFF_REQUEST = "staff_request"
    SUPPLY_LOW = "supply_low"
    INFECTION_ALERT = "infection_alert"
    PREDICTION_UPDATED = "prediction_updated"
    ANOMALY_DETECTED = "anomaly_detected"

@dataclass
class Event:
    type: EventType
    source: str  # agent_id
    data: dict
    priority: int = 0  # 0=normal, 1=high, 2=critical
    timestamp: float = field(default_factory=time.time)

class EventBus:
    """중앙 이벤트 버스 — Agent 간 통신"""
    
    def __init__(self):
        self.subscribers: dict[EventType, list[Callable]] = {}
        self.event_log: list[Event] = []
    
    def subscribe(self, event_type: EventType, callback: Callable):
        """이벤트 구독"""
        if event_type not in self.subscribers:
            self.subscribers[event_type] = []
        self.subscribers[event_type].append(callback)
    
    async def emit(self, event: Event):
        """이벤트 발행"""
        self.event_log.append(event)
        for callback in self.subscribers.get(event.type, []):
            await callback(event)
    
    def get_recent_events(self, minutes: int = 60) -> list:
        """최근 이벤트 조회"""
        cutoff = time.time() - minutes * 60
        return [e for e in self.event_log if e.timestamp > cutoff]
```

### 3.2 Agent 간 협업 시나리오

#### 시나리오: 수술실 과밀 → 전파

```
수술실 Agent: "내일 오전 3개실 부족 예상"
     ↓ (Event: OR_OVERBOOKED)
Resource Agent: "수술 연기 가능 목록 생성 중..."
     ↓
Patient Flow Agent: "연기된 수술 환자 → 다음 주 재예약 필요"
     ↓
Supply Chain Agent: "연기된 수술 재고 → 타 수술실로 재배치"
     ↓
Orchestrator: 최종 결정 → 관련 부서 알림
```

#### 시나리오: 감염 의심 → 통합 대응

```
Clinical Agent: "3병동 발열 환자 5명 → 감염 의심"
     ↓ (Event: INFECTION_ALERT)
Patient Flow Agent: "3병동 신규 입원 중단 권고"
Resource Agent: "격리실 가동률 80% → 2개 추가 준비"
Supply Chain Agent: "방역 물품 재고 확인 → 부족 시 발주"
     ↓
Orchestrator: "감염관리위원회 소집 필요 → 자동 보고서 생성"
```

---

## 4. 데이터 아키텍처 (CDC + FHIR 기반 실시간 COPY)

### 4.1 핵심 원칙: 운영 DB 직접 조회 ❌ → CDC 실시간 COPY ✅

```
                    병원 운영 시스템
┌─────────────────────────────────────────────────────────────┐
│  ⚕️ EMR DB     💊 OCS DB     🔬 LIS DB     🏥 행정 DB     │
│  (Oracle)      (Oracle)      (MSSQL)       (PostgreSQL)    │
└──────┬───────────┬─────────────┬──────────────┬────────────┘
       │           │             │              │
       │    CDC (Change Data Capture) — Debezium / Oracle GoldenGate
       │           │             │              │
       ▼           ▼             ▼              ▼
┌─────────────────────────────────────────────────────────────┐
│              Kafka Event Bus (실시간 변경 이벤트)             │
└─────────────────────────────────────────────────────────────┘
       │
       │    FHIR R4 변환 (표준화)
       ▼
┌─────────────────────────────────────────────────────────────┐
│              FHIR 표준화 레이어                               │
│                                                              │
│  • Patient → FHIR Patient Resource                           │
│  • Encounter → FHIR Encounter Resource                       │
│  • Observation → FHIR Observation Resource                   │
│  • MedicationRequest → FHIR MedicationRequest Resource       │
│  • Procedure → FHIR Procedure Resource                       │
│  • DiagnosticReport → FHIR DiagnosticReport Resource         │
│  • SNOMED CT / LOINC 매핑 (Snowstorm)                        │
└─────────────────────────────────────────────────────────────┘
       │
       │    실시간 COPY
       ▼
┌─────────────────────────────────────────────────────────────┐
│              Analytics Data Platform (읽기 전용 COPY)         │
│                                                              │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐            │
│  │ PostgreSQL │  │Elasticsearch│  │  Redis     │            │
│  │ (구조화)   │  │ (검색)     │  │ (캐싱)     │            │
│  └────────────┘  └────────────┘  └────────────┘            │
│                                                              │
│  ← 모든 Agent는 이 COPY만 READ (운영 DB 직접조회 ❌)          │
└─────────────────────────────────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────────────────────────┐
│              Feature Store + ML Engine                        │
│                                                              │
│  • 예측 Feature 자동 생성                                     │
│  • Model Inference (Prophet/XGBoost/LSTM)                    │
│  • Agent 추론 (LLM)                                          │
│  • 통합 대시보드                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 CDC 파이프라인 상세

```yaml
# docker-compose.cdc.yml
# CDC (Change Data Capture) — 운영 DB → 실시간 COPY
services:
  # 1. Kafka — CDC 이벤트 버스
  kafka:
    image: confluentinc/cp-kafka:latest
    environment:
      KAFKA_TOPICS: "emr.patient,emr.encounter,ocs.prescription,lis.result"

  # 2. Debezium Connectors — DB 변경 캡처
  debezium-emr:
    image: debezium/connector-oracle:latest
    environment:
      DB_HOST: ${EMR_HOST}
      DB_PORT: 1521
      DB_USER: ${EMR_CDC_USER}
      DB_PASSWORD: ${EMR_CDC_PASS}
      TOPIC_PREFIX: "emr"

  debezium-ocs:
    image: debezium/connector-oracle:latest
    environment:
      DB_HOST: ${OCS_HOST}
      DB_PORT: 1521
      TOPIC_PREFIX: "ocs"

  debezium-lis:
    image: debezium/connector-sqlserver:latest
    environment:
      DB_HOST: ${LIS_HOST}
      DB_PORT: 1433
      TOPIC_PREFIX: "lis"

  # 3. FHIR 변환기 — CDC 이벤트 → FHIR Resource
  fhir-transformer:
    image: hapi-fhir/hapi-validator:latest
    command: python fhir_transform.py
    volumes:
      - ./fhir/mapping:/app/mapping   # SNOMED/LOINC 매핑 파일

  # 4. 실시간 COPY — FHIR Resource → Analytics DB
  cdc-sync:
    image: postgres:15
    command: |
      pg_ctl start
      && pg_dump -h ${EMR_HOST} --data-only > /tmp/initial_copy.sql
      && psql -d analytics_db < /tmp/initial_copy.sql
      && LISTEN cdc_kafka FOR INSERT/UPDATE/DELETE
```

### 4.3 FHIR R4 표준화

```python
# shared/fhir/fhir_transformer.py

class FHIRTransformer:
    """원시 EMR 데이터 → FHIR R4 Resource 변환"""

    SNOMED_MAP = {
        'CBC': {'code': '26604007', 'system': 'http://snomed.info/sct'},
        'LFT': {'code': '34557009', 'system': 'http://snomed.info/sct'},
        'BUN': {'code': '59565001', 'system': 'http://snomed.info/sct'},
        'CRP': {'code': '71415007', 'system': 'http://snomed.info/sct'},
    }

    LOINC_MAP = {
        'CBC': '58410-2',
        'GLU': '2345-7',
        'HBA1C': '4548-4',
    }

    def to_patient_resource(self, emr_row: dict) -> dict:
        """EMR 환자 정보 → FHIR Patient Resource"""
        return {
            'resourceType': 'Patient',
            'id': emr_row['patient_id'],
            'identifier': [{
                'system': 'urn:oid:1.2.410.XXXX',
                'value': emr_row['patient_id']
            }],
            'name': [{
                'use': 'official',
                'family': emr_row['name_last'],
                'given': [emr_row['name_first']]
            }],
            'gender': emr_row['gender'],
            'birthDate': emr_row['birth_date'],
        }

    def to_observation_resource(self, lab_result: dict) -> dict:
        """LIS 검사 결과 → FHIR Observation Resource"""
        loinc = self.LOINC_MAP.get(lab_result['test_code'])
        snomed = self.SNOMED_MAP.get(lab_result['test_code'])
        return {
            'resourceType': 'Observation',
            'id': lab_result['result_id'],
            'status': 'final',
            'code': {
                'coding': [
                    {'system': 'http://loinc.org', 'code': loinc},
                    {'system': 'http://snomed.info/sct', 'code': snomed['code']},
                ],
                'text': lab_result['test_name']
            },
            'subject': {'reference': f"Patient/{lab_result['patient_id']}"},
            'valueQuantity': {
                'value': lab_result['result_value'],
                'unit': lab_result['unit'],
            },
            'effectiveDateTime': lab_result['result_date'],
        }

    def to_encounter_resource(self, visit: dict) -> dict:
        """진료 방문 → FHIR Encounter Resource"""
        return {
            'resourceType': 'Encounter',
            'id': visit['visit_id'],
            'status': 'finished',
            'class': {
                'system': 'http://terminology.hl7.org/CodeSystem/v3-ActCode',
                'code': 'IMP' if visit['type'] == '입원' else 'AMB',
                'display': visit['type'],
            },
            'subject': {'reference': f"Patient/{visit['patient_id']}"},
            'period': {
                'start': visit['admission_date'],
                'end': visit.get('discharge_date'),
            },
            'diagnosis': [{
                'condition': {'reference': f"Condition/{d['code']}"},
            } for d in visit.get('diagnoses', [])],
        }
```

### 4.4 실시간 COPY 프로세스

```
[운영 DB 변경 발생]
    │
    ▼
[Oracle Redo Log / MSSQL Transaction Log]
    │ ← Debezium이 변경분 캡처
    ▼
[Kafka Topic: emr.patient, emr.encounter, ocs.prescription, lis.result]
    │
    ▼
[FHIR Transformer] ← 원시 데이터를 FHIR R4로 변환
    │
    ├──▶ [FHIR Validator] ← Snowstorm(SNOMED CT)으로 의미 검증
    │       │
    │       ├── ✅ 통과 → Analytics DB에 INSERT/UPDATE
    │       └── ❌ 실패 → Dead Letter Queue + 운영자 알림
    │
    └──▶ [Elasticsearch] ← 검색 인덱싱
    │
    └──▶ [Feature Store] ← ML Feature 자동 갱신
```

### 4.5 초기 전체 COPY (첫 실행)

```python
# shared/data_layer/initial_load.py

class InitialDataLoader:
    """운영 DB → Analytics DB 최초 전체 COPY"""

    TABLES = [
        # EMR
        ('emr', 'patients', 'SELECT * FROM patients'),
        ('emr', 'admissions', 'SELECT * FROM admissions WHERE adm_date >= DATE_SUB(NOW(), INTERVAL 3 YEAR)'),
        ('emr', 'diagnoses', 'SELECT * FROM diagnoses'),
        ('emr', 'procedures', 'SELECT * FROM procedures'),
        ('emr', 'vitals', 'SELECT * FROM vitals WHERE record_date >= DATE_SUB(NOW(), INTERVAL 1 YEAR)'),

        # OCS
        ('ocs', 'prescriptions', 'SELECT * FROM prescriptions WHERE order_date >= DATE_SUB(NOW(), INTERVAL 2 YEAR)'),
        ('ocs', 'appointments', 'SELECT * FROM appointments WHERE apt_date >= DATE_SUB(NOW(), INTERVAL 1 YEAR)'),

        # LIS
        ('lis', 'lab_results', 'SELECT * FROM lab_results WHERE result_date >= DATE_SUB(NOW(), INTERVAL 2 YEAR)'),
        ('lis', 'blood_bank', 'SELECT * FROM blood_bank'),

        # 행정
        ('admin', 'staff', 'SELECT * FROM staff'),
        ('admin', 'departments', 'SELECT * FROM departments'),
        ('admin', 'inventory', 'SELECT * FROM inventory'),
    ]

    def run_initial_load(self):
        """운영 DB → Analytics DB 전체 복제"""
        for source, table, query in self.TABLES:
            print(f"📥 Copying {source}.{table}...")

            # CDC 중단
            self.pause_cdc()

            # 전체 COPY (PostgreSQL Foreign Data Wrapper or pg_dump)
            self.copy_table(source, table, query)

            # CDC 재개 + 초기 스냅샷 이후 변경분 동기화
            self.resume_cdc_with_lsn(table)

        print("✅ 초기 전체 COPY 완료!")
        print("🔁 CDC 실시간 동기화 시작")

    def validate_copy(self) -> dict:
        """복제 정합성 검증"""
        results = {}
        for source, table, _ in self.TABLES:
            source_count = self.count_operational(source, table)
            target_count = self.count_analytics(table)
            match = '✅' if source_count == target_count else '❌'
            results[table] = {'source': source_count, 'target': target_count, 'status': match}
        return results
```

### 4.6 CDC 모니터링

```python
# shared/data_layer/cdc_monitor.py

class CDCMonitor:
    """CDC 상태 모니터링"""

    def get_status(self) -> dict:
        return {
            'kafka_lag': self.get_kafka_consumer_lag(),
            'debezium_uptime': self.check_connector_health(),
            'last_sync_at': self.get_last_sync_time(),
            'sync_lag_seconds': self.calculate_lag(),
            'fhir_validation_errors': self.get_validation_error_count(),
            'topics': {
                'emr': {'messages_24h': self.topic_count('emr.*', '24h')},
                'ocs': {'messages_24h': self.topic_count('ocs.*', '24h')},
                'lis': {'messages_24h': self.topic_count('lis.*', '24h')},
            }
        }

    def alert_on_lag(self, threshold_seconds: int = 60):
        """CDC 지연 알림"""
        lag = self.calculate_lag()
        if lag > threshold_seconds:
            self.send_alert(
                severity='WARNING',
                message=f"CDC 동기화 지연: {lag}s (허용: {threshold_seconds}s)"
            )
```

### 4.7 보안: 운영 DB 직접 접근 금지

```
┌────────────────────────────────────────────────────────┐
│                    🚫 절대 금지                          │
│  Agent가 운영 EMR/OCS/LIS DB를 직접 SELECT ❌          │
│  Agent가 운영 DB에 직접 INSERT/UPDATE/DELETE ❌         │
│                                                         │
│  모든 Agent는 Analytics COPY DB만 READ ✅               │
│  Agent의 액션(발주/알림)은 API Gateway를 통해서만 ✅     │
└────────────────────────────────────────────────────────┘
```

| 구분 | 운영 DB | Analytics COPY DB |
|:-----|:-------:|:-----------------:|
| **목적** | 환자 진료/처방 | 분석/예측/AI |
| **조회 주체** | EMR/OCS/LIS 사용자 | Agent 시스템 |
| **데이터 기간** | 현재 + 최근 3개월 | 전체 (최대 3년) |
| **변경 방식** | 실시간 트랜잭션 | Kafka CDC → READ ONLY |
| **장애 영향** | Agent 장애가 운영에 영향 ❌ | COPY DB 장애 = 운영 영향 없음 ✅ |

---

## 5. FEATURE STORE (통합 Feature 관리)

### 5.1 FHIR 기반 Feature 정의

```python
# shared/feature_store/fhir_features.py

class FHIRFeatureProvider:
    """FHIR Resource → ML Feature 변환"""

    def get_patient_features(self, patient_id: str) -> dict:
        """FHIR Patient + Observation → Feature"""
        patient = self.fhir_client.read('Patient', patient_id)
        observations = self.fhir_client.search('Observation', {
            'subject': f'Patient/{patient_id}',
            '_sort': '-date',
            '_count': 100
        })

        return {
            # Patient Resource 기반
            'age': self._calc_age(patient.birthDate),
            'gender': patient.gender,
            'marital_status': patient.maritalStatus,

            # Observation Resource 기반
            'lab_count_30d': self._count_recent(observations, 30),
            'abnormal_lab_ratio': self._abnormal_ratio(observations),

            # Encounter Resource 기반
            'visit_count_1y': self._count_encounters(patient_id, 365),
            'hospitalization_days_1y': self._sum_los(patient_id, 365),
        }
```

### 5.2 실시간 Feature 갱신

```python
# shared/feature_store/streaming.py

class StreamingFeatureUpdater:
    """CDC 이벤트 → Feature Store 실시간 갱신"""

    def __init__(self, kafka_consumer, feature_store):
        self.consumer = kafka_consumer
        self.store = feature_store

    async def start(self):
        """Kafka CDC 이벤트 구독 → Feature 자동 갱신"""
        async for message in self.consumer:
            topic = message.topic
            operation = message.key  # 'c'=CREATE, 'u'=UPDATE, 'd'=DELETE
            resource = json.loads(message.value)

            if topic == 'emr.encounter':
                await self._update_encounter_features(resource)
            elif topic == 'lis.result':
                await self._update_lab_features(resource)
            elif topic == 'ocs.prescription':
                await self._update_prescription_features(resource)

    async def _update_lab_features(self, observation: dict):
        """LIS 결과 → 환자 Feature 갱신"""
        patient_id = observation['subject']['reference'].split('/')[1]
        feature_key = f"patient:{patient_id}:lab_last_24h"

        # 24시간 내 검사 횟수 Feature 갱신
        count = self.store.increment(feature_key, 1, ttl=86400)
        if count > 10:  # 24시간 10건 초과 = 과다검사 플래그
            await self.event_bus.emit(Event(
                type=EventType.ANOMALY_DETECTED,
                source='feature_store',
                data={
                    'type': 'excessive_testing',
                    'patient_id': patient_id,
                    'count_24h': count,
                },
                priority=1
            ))


### 5.3 Agent Base Class

```python
# agent_framework/base_agent.py

class BaseAgent(ABC):
    """모든 Agent의 기본 클래스"""
    
    def __init__(self, agent_id: str, name: str, 
                 llm_backend: str = "claude"):
        self.agent_id = agent_id
        self.name = name
        self.event_bus = EventBus()
        self.memory = AgentMemory()
        self.llm = self._init_llm(llm_backend)
        self.projects: list[str] = []
        self.status = AgentStatus.IDLE
    
    @abstractmethod
    async def analyze(self, context: dict) -> dict:
        pass
    
    @abstractmethod
    async def handle_event(self, event: Event):
        pass
    
    async def call_agent(self, target: str, action: str, **kwargs):
        return await self.event_bus.rpc_call(target, action, kwargs)

### 5.4 Orchestrator

```python
# agent_framework/orchestrator.py

class Orchestrator:
    """Multi-Agent 오케스트레이터"""
    
    def __init__(self):
        self.agents: dict[str, BaseAgent] = {}
        self.event_bus = EventBus()
        self.scheduler = BackgroundScheduler()
        self.conflict_resolver = ConflictResolver()
    
    def register_agent(self, agent: BaseAgent):
        self.agents[agent.agent_id] = agent
    
    async def run_daily_cycle(self):
        self.log("🔄 일일 Agent 사이클 시작")
        results = {}
        for agent in self.agents.values():
            results[agent.agent_id] = await agent.analyze({
                'date': today(), 'previous_results': results
            })
        conflicts = self.detect_conflicts(results)
        for conflict in conflicts:
            await self.conflict_resolver.resolve(conflict)
        report = await self.generate_integrated_report(results)
        actions = self.prioritize_actions(report['actions'])
        for action in actions[:5]:
            await self.execute_action(action)
        self.log("✅ 일일 사이클 완료")
        return report
    
    def detect_conflicts(self, results: dict) -> list:
        conflicts = []
        or_schedule = results.get('resource', {}).get('or_schedule', {})
        bed_plan = results.get('patient_flow', {}).get('bed_plan', {})
        return conflicts
```

## 6. 배포 아키텍처

### 6.1 컨테이너 구성

```yaml
# docker-compose.yml
version: '3.8'
services:
  orchestrator:
    build: .
    command: python -m agent_framework.orchestrator
  
  agent-patient-flow:
    build: .
    command: python -m agents.patient_flow.main
  
  agent-resource:
    build: .
    command: python -m agents.resource.main
  
  agent-clinical:
    build: .
    command: python -m agents.clinical_quality.main
  
  agent-financial:
    build: .
    command: python -m agents.financial.main
  
  agent-supply-chain:
    build: .
    command: python -m agents.supply_chain.main
  
  agent-research:
    build: .
    command: python -m agents.research.main
  
  event-bus:
    image: redis:7-alpine
  
  feature-store:
    image: postgres:15
  
  dashboard:
    build: frontend
    ports: ["3000:3000"]
  
  api-gateway:
    build: .
    command: uvicorn api.gateway:app --host 0.0.0.0 --port 8000
    ports: ["8000:8000"]
```

### 6.2 Agent 간 통신

```
┌──────────┐     RPC Calls     ┌──────────┐
│ Agent A  │◄─────────────────►│ Agent B  │
└──────────┘                   └──────────┘
     │                              │
     │     Event Bus (Redis)        │
     ├──────────────────────────────┤
     │     Feature Store (PG)        │
     ├──────────────────────────────┤
     │     Model Registry (MLflow)   │
     └──────────────────────────────┘
```

---

## 7. 구현 로드맵

| Phase | 기간 | 내용 | 산출물 |
|:------|:----:|:------|:--------|
| **P0** | 2주 | Agent Framework + Event Bus | Base Agent, 통신 인프라 |
| **P1** | 4주 | Patient Flow + Resource Agent | 2개 Agent + Feature Store |
| **P2** | 4주 | Clinical + Supply Chain Agent | 4개 Agent |
| **P3** | 3주 | Financial + Research Agent | 6개 Agent 완료 |
| **P4** | 3주 | Orchestrator + Dashboard | 통합 대시보드 + API |
| **P5** | 2주 | Kubernetes 배포 + 모니터링 | 운영 환경 |

**총 예상 기간: 18주**

---

## 8. 기대 효과

| 항목 | 단일 프로젝트 | Multi-Agent 통합 |
|:-----|:------------:|:----------------:|
| 데이터 중복 | ⚠️ 각 프로젝트 별도 수집 | ✅ **Feature Store 1회 수집** |
| 예측 정확도 | 70-80% | ✅ **85-95%** (상호 보완) |
| 의사결정 속도 | 수동 통합 | ✅ **실시간 자동 결정** |
| 확장성 | 프로젝트 추가 시 재구축 | ✅ **Agent만 추가 등록** |
| 유지보수 | 20개 시스템 개별 관리 | ✅ **통합 플랫폼** |
| 총 예상 비용 | 각 2-5천만원 | ✅ **1억원 (통합 60% 절감)** |

---

> 📍 **저장소:** inhwa-agent 프로젝트에 통합 예정
