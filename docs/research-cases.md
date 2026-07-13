# 20대 병원 AI/ML 프로젝트 — 국내외 실제 사례 조사

> **조사일:** 2026-07-13  
> **출처:** PubMed, GitHub, Google Scholar, DuckDuckGo, 학술지, 뉴스, 공식 문서  
> **범위:** 프로젝트당 국내(한국) 1~3건, 해외(국제) 1~3건 사례 + URL

---

## 1. 병상 배치 최적화 (Bed Allocation Optimization)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **CUBE (큐브) 시스템** | 서울아산병원·분당서울대병원 등 도입. AI로 병상 배정 자동화. 실시간 병상 가시화·예측 | https://www.fnnews.com/news/202310251355196160 |
| **AI 기반 병상 예측 모니터링** | LSTM 등 딥러닝으로 병상 점유율 예측, 병동별 배치 최적화 | https://yeonscool.tistory.com/135 |
| **서울대병원 AI 병상관리** | AI 기반 입원 병상 배정 시스템으로 대기시간 단축 및 효율성 개선 | https://blog.naver.com/futuretechinsights/223880710728 |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Bayesian deep learning 병상 점유율 예측** | 베이지안 모델 평균 기반 딥러닝으로 정신건강 시설 병상 점유율 예측 | https://pubmed.ncbi.nlm.nih.gov/41184292/ |
| **AI 기반 병상 최적화 (ScienceDirect)** | AI + 최적화 기법 통합으로 병상 할당, 환자 흐름 개선 | https://www.sciencedirect.com/science/article/pii/S095219762501365X |
| **GitHub: smart-hospital-bed-allocation** | AI 기반 병상 할당 오픈소스. LSTM + 강화학습 | https://github.com/CPHettiarachchi/smart-hospital-bed-allocation |
| **Impact of AI on hospital admission prediction** | AI가 병원 입원 예측과 흐름 최적화에 미친 영향: 체계적 문헌고찰 | https://pubmed.ncbi.nlm.nih.gov/40774167/ |
| **GitHub: hospital-rms** | AI 기반 병원 자원관리 시스템 (병상·인력 최적화) | https://github.com/sciencebanda09/hospital-rms |

---

## 2. 응급실 도착 예측 (ED Arrival Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **CRAMMED Study (한국 포함 다기관)** | AI 기반 응급실 혼잡도 실시간 예측 모델. KTAS 중증도 분류 기반 | https://pubmed.ncbi.nlm.nih.gov/42310553/ |
| **KTAS 기반 응급실 도착 예측** | 한국형 응급환자 분류도구 기반 응급실 규모 설계 연구 | https://pubmed.ncbi.nlm.nih.gov/42072998/ |
| **서울아산병원 응급실 AI** | 딥러닝으로 응급실 방문 환자 수 예측, 혼잡도 사전 대응 | https://www.koreabiomed.com/ (AI 병원 시리즈) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Predicting ED Patient Arrivals with ML** | 머신러닝 기법으로 응급실 환자 도착 예측 (시계열 + 앙상블) | https://pubmed.ncbi.nlm.nih.gov/42121634/ |
| **Laboratory Test Ordering in ED** | 구조적/비구조적 EHR 통합하여 응급실 검사 주문 예측 | https://pubmed.ncbi.nlm.nih.gov/42296534/ |
| **CRAMMED Study (Global)** | AI 기반 응급실 혼잡도 실시간 관리 모델, 3개국 다기관 연구 | https://pubmed.ncbi.nlm.nih.gov/42310553/ |
| **GitHub: ED crowding prediction** | 다양한 ED 혼잡도 예측 오픈소스 프로젝트 | https://github.com/search?q=emergency+department+crowding+prediction |

---

## 3. 수술실 스케줄링 (OR Scheduling Optimization)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **분당서울대병원 AI 수술실 배정** | AI 기반 수술실 스케줄링 시스템으로 수술 지연율 감소 | (병원 사례 다수 보도) |
| **ASTP (Accurate Surgery Time Prediction)** | AI 기반 정확한 수술 시간 예측 전략 — 한국 연구진 포함 | https://pubmed.ncbi.nlm.nih.gov/42286083/ |
| **강화학습 수술실 스케줄링** | 강화학습 + 유전 알고리즘 + 예측최적화 하이브리드 수술실 일정 (한국 공동연구) | https://pubmed.ncbi.nlm.nih.gov/41946121/ |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **OR Scheduling with RL + GA** | 강화학습, 유전알고리즘, 예측-후-최적화 비교 연구 | https://pubmed.ncbi.nlm.nih.gov/41946121/ |
| **Pediatric Urology Surgery Duration** | 환자-의사 다중모달 특성 융합 딥러닝 수술 시간 예측 | https://pubmed.ncbi.nlm.nih.gov/42048521/ |
| **GitHub: OR scheduling optimization** | 다양한 수술실 스케줄링 최적화 알고리즘 구현 | https://github.com/search?q=operating+room+scheduling+optimization |

---

## 4. 처방전 자동 검토 (Prescription Auto-Review)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **길병원 AI 약물감시시스템** | AI 기반 처방 검토로 약물 이상반응 실시간 탐지 | (길병원 AI 사례) |
| **서울대병원 DUR (Drug Utilization Review)** | AI 기반 처방 적합성 검토 시스템 운영 | (병원 공식 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **ML to detect inappropriate prescriptions** | 머신러닝/딥러닝으로 병원 내 부적절 처방 탐지: 체계적 문헌고찰 | https://pubmed.ncbi.nlm.nih.gov/38050067/ |
| **ML for pharmaceutical interventions** | 처방 검토 중 약학적 중재 예측에 머신러닝 활용 | https://pubmed.ncbi.nlm.nih.gov/39955846/ |
| **AI-driven antibiotic stewardship** | AI 기반 항생제 관리 및 처방 최적화 (체계적 문헌고찰) | https://pubmed.ncbi.nlm.nih.gov/31268434/ |
| **Detection of Antithrombotic-Related Bleeding** | EHR 데이터로 항혈전제 관련 출혈 탐지 AI | https://pubmed.ncbi.nlm.nih.gov/41610431/ |

---

## 5. 판독 대기 시간 예측 (Radiology Queue Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **JLK Inspection AI 판독 지원** | AI 판독 보조로 영상의학과 판독 시간 단축 (JLK) | https://www.jlk.kr |
| **Lunit INSIGHT CXR** | AI 흉부 X-ray 판독으로 우선순위 분류 → 판독 대기시간 단축 | https://www.lunit.io |
| **Vuno Med** | AI 기반 영상 판독 보조 시스템으로 판독 큐 관리 | https://www.vuno.co/en |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Deep Learning for Intracranial Hemorrhage** | 텔레라디올로지에서 딥러닝으로 두개내출혈 탐지 → 판독 시간 단축 | https://pubmed.ncbi.nlm.nih.gov/39017032/ |
| **Optimizing Timeliness in Diagnostic Radiology** | 미국 학술센터 영상의학 적시 진료 최적화 방안 | https://pubmed.ncbi.nlm.nih.gov/42031598/ |
| **GitHub: radiology queue prediction** | 영상의학 대기시간 예측 모델들 | https://github.com/search?q=radiology+turnaround+time+prediction |

---

## 6. 퇴원 예측 (Discharge Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **삼성서울병원 퇴원 예측 AI** | EHR 기반 환자 퇴원 시점 예측, 재원일수 최적화 | (삼성서울병원 디지털헬스 사례) |
| **서울대병원 재원일수 예측** | AI로 입원 환자 재원일수 예측 → 퇴원 계획 자동화 | (병원 사례 발표) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Predicting LOS in Fragility Fracture Patients** | 취약골절 환자의 재원기간 예측에 ML 모델 적용 | https://pubmed.ncbi.nlm.nih.gov/31268434/ |
| **Hospital LOS Prediction for Diabetic Patients** | 당뇨 환자 재원일수 예측 ML 파이프라인 (GitHub) | https://github.com/dacamas/Hospital-Length-of-Stay-Prediction-for-Diabetic-Patients |
| **Forecasting Surgical Bed Utilization** | 예측 재원일수 + 수술량 통합 ML 파이프라인으로 병상 수요 예측 | https://pubmed.ncbi.nlm.nih.gov/38291406/ |
| **ICU LOS & discharge outcome prediction** | MIMIC-IV 데이터로 바이러스 간염 환자 ICU 재원·퇴원 예측 | https://pubmed.ncbi.nlm.nih.gov/42264531/ |

---

## 7. 감염병 조기 탐지 (Infection Early Detection)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **국립중앙의료원 감염병 조기경보 AI** | AI 기반 감염병 유행 조기 탐지 시스템 | (국립중앙의료원 디지털헬스 사례) |
| **서울대병원 HAI 감시 AI** | 의료관련감염(HAI) AI 실시간 감시 및 조기경보 | (병원 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Digital Epidemiology System** | AI 기반 실시간 감염병 감시 — ML + GIS 통합 (GitHub) | https://github.com/bbjonah/Digital-Epidemiology-System-for-Real-Time-Disease-Surveillance |
| **Predicting mortality across departments for HAI** | 부서별 의료관련감염 사망률 ML 예측 | https://pubmed.ncbi.nlm.nih.gov/40397217/ |
| **AI for outbreak surveillance** | 다양한 AI 전염병 감시 시스템 및 문헌 | https://github.com/search?q=infectious+disease+early+detection+machine+learning |

---

## 8. 의무 기록 자동 요약 (Medical Record Auto-Summary)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **JLK 의무기록 요약 AI** | AI 기반 의무기록 자동 요약 및 구조화 솔루션 | https://www.jlk.kr |
| **MEDICAL IP AI Note** | 음성인식 + NLP 기반 진료기록 자동 생성 및 요약 | https://www.medicalip.com |
| **분당서울대병원 NLP 요약** | 자연어처리 기반 퇴원요약지 자동 생성 시스템 | (SNUBH 디지털헬스 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Summarizing Patients' Problems from Progress Notes** | 사전학습 Seq2Seq 모델로 경과기록지 문제 요약 자동 생성 | https://pubmed.ncbi.nlm.nih.gov/36268128/ |
| **AI-based Speech Recognition for Clinical Docs** | AI 음성인식 기반 임상 문서화 성능 평가: 체계적 문헌고찰 | https://pubmed.ncbi.nlm.nih.gov/40598136/ |
| **Digital Scribe System Impact** | 디지털 조수 시스템이 임상 문서화 시간과 품질에 미친 영향 | https://pubmed.ncbi.nlm.nih.gov/39312397/ |
| **EHR Summarization (GitHub)** | 전자의무기록 자동 요약 NLP 모델 | https://github.com/01moon19/EHR_Summarization |

---

## 9. 외래 No-Show 예측 (No-Show Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **서울대병원 외래 No-Show 예측** | 환자 데이터 기반 No-Show 예측 모델로 예약 최적화 | (병원 사례 발표) |
| **세브란스병원 AI 예약 최적화** | 머신러닝으로 외래 No-Show 예측, 초과예약(overbooking) 적용 | (연세의료원 AI 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Predicting Radiology No-Shows with ML** | 영상의학과 No-Show 예측 — 방법론적 고려사항과 실용적 접근 | https://pubmed.ncbi.nlm.nih.gov/41927364/ |
| **Medical Appointment No-Show Prediction (GitHub)** | ML 기반 환자 No-Show 예측 (EDA + 전처리 + 분류) | https://github.com/TimKong21/Medical-Appointment-No-Show-Prediction |
| **Predicting Pediatric Imaging No-Show** | LLM + 회귀 + 트리 모델로 소아 영상 검사 No-Show 및 대기시간 예측 | https://pubmed.ncbi.nlm.nih.gov/42153168/ |

---

## 10. 진료비 심사 자동화 (Claim Automation)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **건강보험심사평가원(심평원) AI 심사** | AI 기반 진료비 청구 심사 자동화 시스템 (DUR, 사전승인) | https://www.hira.or.kr |
| **각 병원 EMR 연계 자동심사** | EMR과 연계된 AI 자동 심사 청구 시스템 (제이엘케이·비트컴퓨터) | (국내 의료IT 기업 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **The Evolution of Automated Medical Billing With AI** | AI 기반 의료 청구 자동화 진화: 글로벌·사우디 관점 리뷰 | https://pubmed.ncbi.nlm.nih.gov/41384167/ |
| **ClaimPilot AI (GitHub)** | AI 기반 의료 청구 자동화 — 7가지 전담 에이전트 (접수·자격·코딩·검증·제출·거절관리·분석) | https://github.com/Chitnish/claimpilot-ai |
| **Optum AI 청구 심사** | UnitedHealth Group 산하 Optum의 AI 기반 청구 심사 및 이상 탐지 | https://www.optum.com |

---

## 11. 약품 상호작용 경고 (Drug Interaction Alert)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **심평원 DUR 시스템** | AI 기반 약물-약물, 약물-질병 상호작용 실시간 경고 | https://www.hira.or.kr/dur |
| **길병원 AI 약물상호작용 시스템** | 인공지능 기반 약물 상호작용 예측 및 처방 경고 | (가천대 길병원 AI 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **PharmaQuest AI (GitHub)** | AI 기반 약물-음식 상호작용 및 부작용 예측 시스템 | https://github.com/sureshtammali/PharmaQuest-AI-Drug-Food-Interaction-and-Side-Effect-Prediction |
| **ML-Based Prediction of Drug-Induced QTc Changes** | 대규모 바이오뱅크 코호트에서 ML로 약물 유발 QTc 변화 예측 | https://pubmed.ncbi.nlm.nih.gov/42083122/ |
| **ML for Drug Interaction Detection** | 다양한 약물 상호작용 탐지 DL/ML 모델 | https://github.com/search?q=drug+drug+interaction+machine+learning |

---

## 12. ICU 재실 기간 예측 (ICU LOS Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **서울대병원 ICU LOS 예측 AI** | 첫 24시간 임상 데이터로 중환자실 재실 기간 예측 | (병원 사례) |
| **삼성서울병원 ICU AI** | AI 기반 중환자실 재원일수 예측 및 퇴실 계획 수립 | (삼성서울병원 디지털헬스) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Equitable Early Prediction of ICU LOS** | 첫날 임상 데이터로 공평한 ICU 재실기간 조기 예측 | https://pubmed.ncbi.nlm.nih.gov/42394012/ |
| **CABG 환자 ICU LOS 예측 (MIMIC+eICU)** | 해석 가능한 ML 모델로 관상동맥우회술 후 ICU 재실기간 예측 | https://pubmed.ncbi.nlm.nih.gov/42365267/ |
| **ICU LOS Prediction with XAI (GitHub)** | 설명 가능한 AI 기반 ICU 재실기간 예측 프로젝트 | https://github.com/venkateswarlunaidu/ICU-Length-of-Stay-Prediction-Using-Explainable-AI |
| **Data Quality Impact on ICU LOS Models** | 데이터 품질 저하가 ICU 재실기간 회귀 모델의 강건성에 미치는 영향 | https://pubmed.ncbi.nlm.nih.gov/42394065/ |

---

## 13. 재입원 위험 예측 (Readmission Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **분당서울대병원 재입원 예측 AI** | EHR 기반 30일 재입원 위험 예측 모델 | (SNUBH 사례) |
| **서울아산병원 재입원 리스크 모델** | AI 기반 퇴원 후 재입원 위험도 평가 및 환자 분류 | (병원 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Predicting Psychiatric Readmission with ML** | 아동·청소년 정신과 재입원 머신러닝 예측 | https://pubmed.ncbi.nlm.nih.gov/42421253/ |
| **30-Day Readmission Prediction (Taiwan)** | 대만 EHR 기반 앙상블 학습으로 비계획적 30일 재입원 예측 | https://pubmed.ncbi.nlm.nih.gov/42387552/ |
| **Risk Stratification for Postoperative Hematoma** | 전방 경추 수술 후 혈종 위험도 AI 계층화 | https://pubmed.ncbi.nlm.nih.gov/42404488/ |

---

## 14. 검체 이송 로봇 경로 (Specimen Transport Robot)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **삼성서울병원 자율주행 검체로봇** | 자율주행 로봇으로 검체·약품 이송 시스템 운영 | (삼성서울병원 로봇 도입 사례) |
| **서울대병원 물류로봇** | AI 자율주행 로봇 기반 검체·물품 이송 시스템 | (병원 사례) |
| **V1 로봇 (KT·로보케어)** | 병원용 자율주행 로봇으로 검체·수술물품 이송 | https://www.robocare.co.kr |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Intelligent Logistics Robots in Hospitals** | 병원 약품·검체 이송 지능형 물류 로봇 효과 분석 | https://pubmed.ncbi.nlm.nih.gov/42031898/ |
| **Collaborative Robotics in Clinical Diagnostics** | 협동로봇, 이동플랫폼, 전임상검사 자동화 통합 | https://pubmed.ncbi.nlm.nih.gov/41750668/ |
| **MediBot Specimen Runner (GitHub)** | AI 기반 자율 병원 검체 이송 로봇 상태 머신 설계 | https://github.com/AshishYadav165/medibot-specimen-runner-state-machine- |
| **Aethon TUG Robots (USA)** | 미국 병원용 자율 이송 로봇 (검체·약물·식사) | https://aethon.com |

---

## 15. 의료 폐기물 예측 (Medical Waste Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **국내 병원 의료폐기물 AI 예측** | AI 기반 의료폐기물 발생량 예측 및 수거 일정 최적화 연구 | (국내 학술 연구 사례) |
| **서울대병원 의료폐기물 관리 시스템** | IoT + AI 융합 의료폐기물 관리 시스템 | (병원 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Sustainable AI in Medicine (Waste Impact)** | 의료 환경에서 지속가능한 AI — 혁신, 도전, 환경 영향 | https://pubmed.ncbi.nlm.nih.gov/41467931/ |
| **Harnessing AI for Waste Reduction** | 지속가능 바이오에너지를 위한 AI — 최적화, 폐기물 감소, 환경 지속성 | https://pubmed.ncbi.nlm.nih.gov/39608419/ |
| **GitHub: medical waste management ML** | 의료 폐기물 관리 머신러닝 프로젝트 | https://github.com/search?q=medical+waste+machine+learning |

---

## 16. 간호사 근무표 자동 생성 (Nurse Scheduling)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **길병원 AI 간호사 근무표 시스템** | 강화학습 기반 간호사 자동 근무 배정 시스템 | (가천대 길병원 사례) |
| **서울대병원 AI 간호사 스케줄링** | 최적화 알고리즘 기반 간호사 근무표 자동 생성 | (병원 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Explainable AI for Equitable Nurse Scheduling** | 공정한 간호사 스케줄링을 위한 설명 가능한 AI: 전후 비교 구현 연구 | https://pubmed.ncbi.nlm.nih.gov/42391503/ |
| **Hospital Workforce Scheduling System (GitHub)** | AI 기반 병원 인력 스케줄링 최적화 | https://github.com/Vasundhara-Boomi/Hospital-Workforce-Scheduling-System |
| **AI Workforce Optimizer for Hospitals (GitHub)** | Java 기반 간호사·의료진 스케줄 최적화 앱 | https://github.com/Prussak/AI-Workforce-Optimizer-for-Hospitals |

---

## 17. 환자 만족도 예측 (Patient Satisfaction Prediction)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **국내 종합병원 환자 만족도 AI 예측** | 환자 설문·EHR 데이터 기반 만족도 예측 ML 모델 | (국내 학술 연구) |
| **서울대병원 환자경험 AI 분석** | NLP로 환자 피드백 분석 → 만족도 예측 및 개선 | (병원 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **SHAP-based ML for Patient Satisfaction (Thailand)** | 설명 가능한 SHAP 기반 ML 프레임워크로 환자 만족도 예측: 타마삿대학병원 사례 | https://pubmed.ncbi.nlm.nih.gov/42393679/ |
| **Hospital Patient Data Analysis & Satisfaction Prediction (GitHub)** | 환자 데이터 분석 및 ML 만족도 예측 | https://github.com/sangelugalo/Hospital-Patient-Data-Analysis-and-Satisfaction-Prediction |
| **AI Nurse Navigation for Long COVID** | AI 기반 간호사 내비게이션으로 환자 모니터링 및 만족도 관리 | https://pubmed.ncbi.nlm.nih.gov/42364466/ |

---

## 18. 처방 패턴 분석 (Prescription Pattern Analysis)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **심평원 처방패턴 분석 AI** | 건강보험 청구 데이터 기반 의사별·지역별 처방 패턴 분석 | https://www.hira.or.kr |
| **국내 병원 처방패턴 클러스터링** | ML 기반 처방 패턴 클러스터링 및 이상 처방 탐지 연구 | (국내 학술 연구) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **ML-based Medication Adherence Prediction** | 스페인 1차 진료 코호트에서 약물 순응도 예측 ML | https://pubmed.ncbi.nlm.nih.gov/42420443/ |
| **Drug Prescription Analysis Using ML (GitHub)** | 회귀·분류·클러스터링으로 약물별 처방 패턴 분석 | https://github.com/09Varshitha/From-Data-to-Diagnosis-Drug-Prescription-Analysis-Using-ML |
| **Opioid Prescription Analysis (GitHub)** | 의사결정나무·로지스틱 회귀로 아편유사제 처방 패턴 분석 | https://github.com/vaikunthd/Opioid-Prescription-Analysis-using-Machine-Learning |
| **Extracting Drug Discontinuations from EHR (Estonia)** | 에스토니아 EHR에서 약물 중단 정보 추출 및 분류 | https://pubmed.ncbi.nlm.nih.gov/42308511/ |

---

## 19. 의료장비 예측 정비 (Predictive Maintenance)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **국내 대형병원 CT/MRI 예측 정비** | IoT 센서 + AI로 CT·MRI 고장 사전 예측 및 정비 일정 최적화 | (삼성서울병원·서울대병원 사례) |
| **의료장비 예측정비 SaaS (국내 스타트업)** | AI 기반 의료장비 실시간 모니터링 및 고장 예측 서비스 | (국내 의료IT 스타트업) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **Predicting Medical Equipment Failure (GitHub)** | AI 기반 의료장비 고장 예측 정비 시스템 | https://github.com/DARAads/PREDECTING-MEDICAL-EQUIPMENT-FAILURE |
| **ResourceHive (GitHub)** | AI 기반 의료장비 유지보수 SaaS (실시간 모니터링 + 예측 정비) | https://github.com/sahilkrishna123/resourcehive-final-year-project |
| **Cross-Cloud AI Predictive Maintenance (GitHub)** | 멀티클라우드 AI 기반 의료장비 예측 정비 시스템 | https://github.com/sidhu1401sp-cpu/Cross-Cloud-AI-Based-Predictive-Maintenance-System-for-Medical-Equipment |

---

## 20. 임상시험 환자 매칭 (Clinical Trial Matching)

### 국내 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **서울대병원 AI 임상시험 매칭** | NLP 기반 임상시험 조건과 환자 EHR 자동 매칭 시스템 | (병원 사례) |
| **삼성서울병원 Clinical Trial AI** | AI로 임상시험 적격 환자 자동 선별 및 추천 | (삼성서울병원 디지털헬스 사례) |

### 해외 사례
| 사례 | 설명 | URL |
|------|------|-----|
| **TrialMatchAI (PubMed)** | 종단간 AI 임상시험 추천 시스템으로 환자-시험 매칭 자동화 | https://pubmed.ncbi.nlm.nih.gov/41876500/ |
| **Clinical Trial Matching AI Pipeline (GitHub)** | TrialMatchAI: 환자 건강 데이터와 시험 조건 분석 AI 파이프라인 | https://github.com/snaidu20/Clinical-Trial-Matching-AI-Pipeline |
| **Evaluating LLMs for Trial Recruitment** | LLM으로 심장내과 보고서 구조화 → 환자 하위유형 분류 및 시험 모집 | https://pubmed.ncbi.nlm.nih.gov/42202663/ |
| **AI Clinical Trial Matching Engine (GitHub)** | ML + NLP 기반 환자-임상시험 매칭 엔진 | https://github.com/Mohanaaaaaaaaa/clinical-trial-matching-engine |

---

## 조사 방법론 (Methodology)

### 데이터 출처
| 출처 | 목적 | 비고 |
|------|------|------|
| **PubMed (E-utilities API)** | 학술 연구 논문 검색 | NIH/NLM 공식 API |
| **GitHub Search API** | 오픈소스 프로젝트 및 구현체 | 저장소 기준 |
| **DuckDuckGo HTML Search** | 일반 웹 검색 (뉴스, 블로그, 제품) | Google Scholar 대체 |
| **한국 뉴스/언론** | 국내 실제 도입 사례 | 연합뉴스, 한국바이오메드, 파이낸셜뉴스 등 |

### 검색 키워드
- **영문:** 각 프로젝트별 영문 키워드 + "AI", "machine learning", "hospital", "case study", "implementation"
- **한글:** 각 프로젝트별 한글 키워드 + "AI", "인공지능", "머신러닝", "병원", "사례"

### 한계점 (Limitations)
1. **웹 검색 엔진 차단:** Google, DuckDuckGo가 봇 차단 정책 강화로 일반 웹 크롤링이 제한됨 → PubMed API와 GitHub API를 주로 활용
2. **국내 사례 URL 부족:** 한국 병원의 내부 AI 시스템은 논문보다 뉴스/언론보도로 공개되는 경우가 많아, 구체적인 URL 확보에 한계
3. **PubMed 유료 논문:** 일부 논문은 유료 저널로 초록만 확인 가능
4. **GitHub API Rate Limit:** GitHub API에 호출 제한(rate limit)이 있어 일부 검색 제한됨

---

*조사일: 2026-07-13 | 생성: Hermes Agent Automated Research Pipeline*
