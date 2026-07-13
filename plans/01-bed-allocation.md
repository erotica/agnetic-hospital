# 프로젝트 1: 병상 배치 최적화 (Bed Allocation Optimization)

> **Agent:** Patient Flow Agent
> **예상 기간:** 4주 (Phase 2)

---

## 1. 목표
EMR 데이터를 기반으로 일별/주별 입원 환자 수를 예측하여 병상 배치를 최적화하고 병상 가동률을 15% 향상시킨다.

## 2. 필요 데이터
| 데이터 | 출처 | 기간 |
|:-------|:-----|:----:|
| 입원 기록 (진료과/LOS/퇴원) | EMR | 3년 |
| 응급실 입원 의뢰 건수 | ED 시스템 | 1년 |
| 외래 예약 -> 입원 전환율 | EMR | 1년 |
| 수술 예정 건수 | OCS | 실시간 |
| 병상 정보 (등급/격리/성별) | 병동 | 현재 |

## 3. Feature Engineering
- 시간 특성: 요일, 월, 계절, 공휴일, 월초/월말
- 환자 특성: 진료과별 입원율, 응급실 경유율, 평균 LOS
- 외부 요인: 독감 유행 지수
- Lag Features: 전일/전주/전월 동일 요일 입원 수
- 이동 평균: 7일 MA, 28일 MA

## 4. 모델 아키텍처: 2-Stage (예측 + 최적화)

Stage 1: Prophet 기반 입원 수요 예측 (주/일 단위)
Stage 2: Integer Linear Programming 기반 병상 배치 최적화

제약 조건:
- 병상 등급별 (일반/격리/ICU) 가용 병상 수
- 성별 구분 병상
- 진료과별 전과 가능 여부

## 5. 출력물
- 일별/진료과별 예측 입원 환자 수 (신뢰구간 포함)
- 추천 병상 배치표 (병동별/등급별)
- 과밀 주의보 (가동률 85% 초과)
- 병동간 환자 전과 추천

## 6. 평가 지표
| 지표 | 목표 |
|:-----|:----:|
| 병상 가동률 | 85-90% |
| 입원 대기 시간 | 2시간 미만 |
| 예측 정확도 (MAPE) | 10% 미만 |
| 추천 수용률 | 80% 이상 |

## 7. 참고 사례
- CUBE 병상 배정 시스템 (서울아산병원) — https://www.fnnews.com/news/202310251355196160
- Bayesian DL 병상 점유율 예측 — https://pubmed.ncbi.nlm.nih.gov/41184292/
- GitHub: smart-hospital-bed-allocation — https://github.com/CPHettiarachchi/smart-hospital-bed-allocation
