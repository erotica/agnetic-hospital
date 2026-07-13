# 프로젝트 3: 수술실 스케줄 최적화 (OR Scheduling Optimization)

> **Agent:** Resource Agent
> **예상 기간:** 5주 (Phase 2)

---

## 1. 목표
수술 시간 예측 기반 최적 스케줄링으로 수술실 가동률 20%↑, 초과 근무 시간 15%↓

## 2. 필요 데이터
| 데이터 | 출처 | 기간 |
|:-------|:-----|:----:|
| 수술 건별 소요 시간 (마취+수술+회복) | OCS | 2년 |
| 수술실별/진료과별 사용률 | OR 시스템 | 1년 |
| 응급 수술 발생 패턴 | EMR | 2년 |
| 의사별 수술 시간 프로파일 | OCS | 2년 |
| 수술 재고 현황 | 재고 시스템 | 실시간 |

## 3. Feature Engineering
- 수술 코드, 집도의 ID, 마취 종류
- 환자 나이/BMI/ASA score
- 진료과, 수술실 유형, 시간대
- 수술 시간 (목표 변수): 마취 + 수술 + 회복

## 4. 모델: XGBoost Regressor + Constraint Programming

```python
# 수술 시간 예측 (XGBoost)
model = XGBRegressor(n_estimators=300, max_depth=6, learning_rate=0.05)
model.fit(X_train, y_train)  # y_train = surgery_duration_minutes

# 최적 스케줄링 (CP-SAT)
from ortools.sat.python import cp_model
model = cp_model.CpModel()
# 제약: 수술실 중복 금지, 의사 시간 중복 금지, 응급 수술 여유
# 목적: 전체 수술 시간 최소화 + 우선순위 최대화
```

## 5. 출력물
- 수술 건별 예상 소요 시간 (신뢰구간)
- 일별/주간 최적 수술실 배정표
- 응급 수술 여유 시간 확보 알림
- 지연 예측 및 사전 조정

## 6. 평가 지표
| 지표 | 목표 |
|:-----|:----:|
| 수술 시간 예측 MAPE | < 15% |
| 수술실 가동률 | > 85% |
| 스케줄 준수율 | > 90% |
| 응급 수술 대응 시간 | < 30분 |

## 7. 참고 사례
- ASTP 연구 (한국) — https://pubmed.ncbi.nlm.nih.gov/42286083/
- RL + GA 하이브리드 — https://pubmed.ncbi.nlm.nih.gov/41946121/
