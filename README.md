# 금융 플랫폼 데이터 분석 시뮬레이션

> 금융 플랫폼의 실무 상황을 가정하고, 그로스 분석 → A/B 테스트 → ML 기반 의사결정까지 하루 안에 일련의 분석 시뮬레이션을 수행한 프로젝트입니다.

## 프로젝트 개요

금융 플랫폼 데이터 분석가의 관점에서 실무에서 마주할 만한 상황을 가정한 시뮬레이션합니다.  
각 노트북은 독립적인 분석이면서도, **유저 유입 → 전환 최적화 → 서비스 기여도 분석**이라는 하나의 흐름으로 연결됩니다.

## 분석 흐름

### 1. CC & AARRR — 거래 플랫폼 유저 성장 분석
`CC_AARRR.ipynb`

**시나리오:** 거래 플랫폼의 유저 성장이 둔화되고 있다. 현재 성장의 한계(천장)가 어디인지 진단하고, 개선 포인트를 찾아야 한다.

**분석 내용:**
- 주간 코호트 리텐션 히트맵 시각화 및 해석
- 초기 코호트 vs 후기 코호트 리텐션 품질 비교
- 전체 유저 기준 첫 주 리텐션율 계산
- 주평균 신규 유입(Inflow) 산출
- **Carrying Capacity** 계산 → 플랫폼의 균형 유저 수 추정

**핵심 인사이트:**
- Week 0→1 구간에서 가장 큰 이탈이 발생하며, 초기 온보딩 개선이 핵심 과제
- 후기 코호트일수록 리텐션이 낮아 유입 품질 저하 가능성 존재
- CC를 높이기 위해서는 유입 확대보다 리텐션 개선이 효율적

**사용 기술:** `pandas` · `seaborn` · Cohort Analysis · Carrying Capacity

---

### 2. A/B Test — 대출 중개 플랫폼 퍼널 개선 실험
`ABTest.ipynb`

**시나리오:** 대출 중개 플랫폼에서 특정 제휴사의 대출 실행률이 급감했다. 퍼널 분석으로 병목을 찾고, UX 개선 후 A/B 테스트로 효과를 검증해야 한다.

**분석 내용:**
- 대출 실행 퍼널(본인인증 → 신분증인증 → 개인정보입력 → 대출조건 → 심사 → 실행) 분석
- **B(신분증 인증) → C(개인정보 입력)** 구간에서 심각한 이탈 발견
- UX 개선안(실험군 A) vs 기존 화면(대조군 B) 비교 실험 설계
- 전환율 차이 계산 및 카이제곱 검정

**실험 결과:**

| 구분 | 전환율 |
|------|--------|
| A안 (UX 개선) | 51.93% |
| B안 (기존) | 34.56% |
| 개선율 | +50.27% |

- p-value ≈ 0 → UX 개선이 C 도달률을 통계적으로 유의하게 높임

**사용 기술:** `pandas` · `scipy.stats` · Funnel Analysis · Chi-squared Test

---

### 3. ML & SHAP — 보험 서비스 기여도 분석
`ML.ipynb`

**시나리오:** 보험 중개 제품에 5가지 서비스 지면이 있다. 어떤 서비스를 제품 전면에 배치해야 보험 가입 전환이 극대화되는지, 데이터 기반으로 의사결정해야 한다.

**서비스 지면:**
| 변수 | 서비스 | 성격 |
|------|--------|------|
| bnft_user_cnt | 혜택/리워드 지면 | 혜택성 |
| lck_comp_user_cnt | 보험 가성비 평가 지면 | 정보성 |
| bridge_user_cnt | 외부 마케팅 랜딩 지면 | 혜택성 |
| detail_user_cnt | 보험 정보 조회 지면 | 정보성 |
| r_fee_user_cnt | 보험금 간편청구 지면 | 서비스 |

**분석 내용:**
- XGBoost 모델로 보험 가입 수(cm_user_cnt) 예측
- SHAP Summary Plot으로 서비스별 기여도(중요도) 확인
- SHAP Beeswarm Plot으로 변수 값 크기에 따른 영향 방향 분석
- Dependence Plot으로 **수확 체감(Diminishing Returns)** 패턴 관찰
- Waterfall Chart로 최고/최저 성과일의 기여 분해 → 비개발 직군 대상 커뮤니케이션

**핵심 인사이트:**
- 기여도 순위: 브릿지(혜택성) > 베네핏(혜택성) > 디테일(정보성)
- 혜택성 지면에서는 성장 체감(수확 체감)이 관찰됨
- 반면 정보성 지면(디테일)은 성장 체감이 적음
- **정보성 화면을 제품 전면에 배치하는 것이 적절한 의사결정**

**사용 기술:** `XGBoost` · `SHAP` · Dependence Plot · Waterfall Chart

---

## AAR 대시보드 완성
-  Tableau 대시보드 시각화
- [Tableau Public 링크](https://public.tableau.com/views/1_17745041197640/1_1?:language=ko-KR&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
---

## 기술 스택

| 구분 | 사용 기술 |
|------|----------|
| 언어 | Python |
| 데이터 처리 | pandas, numpy |
| 시각화 | matplotlib, seaborn, SHAP |
| 통계 검정 | scipy.stats (Chi-squared Test) |
| 머신러닝 | XGBoost, scikit-learn |
| 설명 가능 AI | SHAP (Summary, Dependence, Waterfall) |
| 대시보드 | Tableau |

