# RoadSense

### 다중 교통사고 데이터를 활용한 치명 사고 분류 및 위험 요인 분석

사고·장소·인적·차량 정보를 사고 단위로 통합하고, 치명 사고와 연관된 조건을 탐색한 개인 데이터 사이언스 프로젝트입니다. 데이터의 의미를 고려한 전처리와 피처 설계를 수행하고, 불균형 분류 환경에서 모델 및 임계값에 따른 탐지 성능을 비교했습니다.

[포트폴리오 PDF](./RoadSense_ML_Project_포트폴리오%20최종정리.pdf) · [데이터 출처](https://www.kaggle.com/competitions/etiq-roadsense)

## 1. 프로젝트 개요

| 항목 | 내용 |
| --- | --- |
| 수행 형태 | 개인 프로젝트 |
| 문제 유형 | 불균형 이진 분류 |
| 예측 대상 | 사고 단위 치명 여부: NonLethal = 0, Lethal = 1 |
| 데이터 구성 | Accidents · Places · Users · Vehicles |
| 클래스 분포 | NonLethal 약 94.4% · Lethal 약 5.6% |
| 최종 데이터 | 47,821건: Train 38,256건 · 내부 평가용 Test 9,565건 |
| 모델 입력 | 192개 피처, 식별자와 타깃 제외 |
| 비교 모델 | RandomForest · XGBoost |
| 검증 방법 | Train의 Stratified 5-Fold CV · OOF 기반 임계값 선정 · 내부 Test 평가 |

Kaggle에서 제공한 Test에는 타깃이 없으므로, 제공된 Train을 사고 ID 기준으로 분할하여 내부 평가용 Test를 구성했습니다. 아래 성능은 내부 Test 평가 결과입니다.

## 2. 주요 분석 및 설계

| 단계 | 핵심 내용 |
| --- | --- |
| 데이터 품질 점검 | 날짜·언어·범주 표기 표준화, 오염값 및 결측 처리 |
| Places 중복 통합 | 동일 사고 내 비결측값 충돌 여부를 확인한 후 정보를 보완하여 한 행으로 통합 |
| EDA 및 변수 설계 | 월·요일·시간대별 사고 건수와 치명 비율 분석, 범주 재정의 및 파생변수 생성 |
| RoadUserType 생성 | 좌석·보행자 정보와 사고 내 차량별 관계를 활용하여 도로 이용자 역할을 규칙 기반으로 추론 |
| 사고 단위 집계 | 참여자 구성, 고유 차량 수, 차종 포함 여부, 차량 특성별 개수를 사고당 한 행으로 구성 |
| 모델 평가 | DummyClassifier 기준선, 클래스 가중치, OOF 임계값 조정, RandomizedSearchCV 비교 |
| 추가 실험 | SMOTENC 성능 및 합성 표본의 의미적 타당성 검토 — 포트폴리오 PDF에 정리 |

## 3. 폴더 및 파일 구성

아래 경로는 `RoadSense/` 기준입니다.

| 경로 | 역할 |
| --- | --- |
| [`README.md`](./README.md) | 프로젝트 개요와 탐색 안내 |
| [`01_RoadSense_DataPreprocessing.ipynb`](./01_RoadSense_DataPreprocessing.ipynb) | 원본 데이터 점검, 표기 표준화, 중복 및 오염값 처리 |
| [`02_RoadSense_EDA_and_Preprocessing.ipynb`](./02_RoadSense_EDA_and_Preprocessing.ipynb) | 사고 단위 Train/Test 분할, EDA, 범주 재정의 및 파생변수 생성 |
| [`03_RoadSense_Feature_Engineering.ipynb`](./03_RoadSense_Feature_Engineering.ipynb) | Users·Vehicles 집계, 네 테이블 결합, 인코딩 및 모델 입력 생성 |
| [`04_RoadSense_Modeling.ipynb`](./04_RoadSense_Modeling.ipynb) | 기준 모델, 교차검증, 임계값 조정, 모델 비교 및 튜닝 |
| [`RoadSense_ML_Project_포트폴리오 최종정리.pdf`](./RoadSense_ML_Project_포트폴리오%20최종정리.pdf) | 분석 과정과 결과를 정리한 18쪽 포트폴리오 |
| [`data/`](./data/) | 원본 CSV: accidents·places·users·vehicles의 train/test 파일 |
| [`data/preprocessed/`](./data/preprocessed/) | 01번 노트북에서 저장한 표준화 데이터 |
| [`data/Cleaning/`](./data/Cleaning/) | 02번 노트북에서 저장한 내부 Train/Test 및 피처 설계 결과 |

**실행 시 생성하는 경로:** `data/Finaldata/`에는 03번 노트북의 출력인 `train_final.csv`와 `test_final.csv`를 저장하며, 04번 노트북에서 이를 불러옵니다. 현재 저장소의 폴더 목록에는 이 경로가 포함되어 있지 않으므로 실행 전에 생성해야 합니다.

## 4. 포트폴리오 목차

| 페이지 | 섹션 | 주요 내용 |
| --- | --- | --- |
| 1 | 표지 | 프로젝트명 및 사용 기술 |
| 2 | Project Overview | 문제 정의, 목표, 데이터 및 성능 요약 |
| 3 | Multi-table Data Structure | 테이블 관계와 사고 단위 분석의 필요성 |
| 4–6 | Data Quality & Preprocessing | 날짜·범주 표준화, Places 오염값 및 중복 처리 |
| 7–10 | EDA & Feature Engineering | Train/Test 분할, 타깃 불균형, 시간 관련 변수 분석 |
| 11–13 | EDA & Feature Engineering | RoadUserType 생성, 동승자 수 및 보행자 포함 여부 분석 |
| 14 | Accident-Level Aggregation | 참여자·차량 정보 집계와 최종 데이터 결합 |
| 15–17 | Model Training & Evaluation | 모델 선정, 기준선, 교차검증, 임계값 조정 및 튜닝 |
| 18 | 추가 실험 및 결론 | 오버샘플링 검토, 종합 결론, 활용 방향 및 한계 |

## 5. 사용 환경 및 라이브러리

| 구분 | 도구·라이브러리 | 활용 |
| --- | --- | --- |
| 분석 환경 | Python · Jupyter Notebook | 단계별 분석 및 실험 기록 |
| 데이터 처리 | pandas · NumPy | 정제, 범주 변환, 집계, 병합 및 수치 연산 |
| 시각화 | Matplotlib · Seaborn | 분포, 치명 비율 및 임계값별 지표 시각화 |
| 통계 분석 | SciPy | 카이제곱 통계량을 이용한 Cramér's V 산출 |
| 전처리·모델링·평가 | scikit-learn | 데이터 분할, OneHotEncoder, DummyClassifier, RandomForest, 교차검증, 튜닝 및 평가지표 |
| 부스팅 모델 | XGBoost | 클래스 불균형을 고려한 비교 모델 학습 |

추가 오버샘플링 실험에는 `imbalanced-learn`의 SMOTENC를 사용했습니다. 해당 실험 코드는 별도로 정리합니다.

## 6. 모델 평가 결과

각 모델의 Train OOF 예측으로 F1을 최대화하는 임계값을 선정한 후, 내부 Test 9,565건에서 평가했습니다. Precision·Recall·F1·F2는 양성 클래스인 Lethal 기준이며, AP는 Average Precision입니다.

| 모델 | 임계값 | Precision | Recall | F1 | F2 | ROC-AUC | AP |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| **RandomForest 기본** | **0.2800** | **0.2350** | **0.4192** | **0.3011** | **0.3624** | **0.8224** | **0.2326** |
| RandomForest 튜닝 | 0.5826 | 0.2380 | 0.4004 | 0.2985 | 0.3523 | 0.8202 | 0.2426 |
| XGBoost 기본 | 0.3686 | 0.1989 | 0.4154 | 0.2690 | 0.3412 | 0.7750 | 0.1938 |

클래스 가중치를 적용한 튜닝 전 RandomForest와 임계값 0.2800을 최종 모델 및 분류 기준으로 선택했습니다. 튜닝 모델은 AP와 Precision이 소폭 높았으나, 선정된 임계값에서 Recall과 F1의 개선은 확인하지 못했습니다.

최종 모델은 실제 치명 사고 532건 중 223건을 탐지하고 309건을 놓쳤습니다. 현재 데이터와 실험 범위에서는 예측 결과만으로 개별 사고의 치명 여부를 안정적으로 판정하기 어렵다고 판단했습니다.

### 활용 방향과 한계

- 변수별 치명 비율에서 확인한 조건은 안전관리의 우선 조사·점검 대상을 선정하는 참고자료로 활용할 수 있습니다.
- 관측된 연관성이 인과관계를 의미하지는 않으며, 개선 조치의 효과는 별도 검증이 필요합니다.
- 충돌 유형 등 사고 이후 확인되는 변수가 포함되어 있습니다. 사고 전 위험 예측에는 사전에 확보 가능한 변수로 모델을 다시 구성해야 합니다.
- 내부 Test를 후보 모델 비교에도 활용했으므로, 최종 선택 모델의 일반화 성능은 추가적인 독립 데이터에서 검증할 필요가 있습니다.

## 7. 실행 순서

1. Python 환경에 pandas, NumPy, Matplotlib, Seaborn, SciPy, scikit-learn, XGBoost 및 Jupyter를 설치합니다.
2. 작업 디렉터리를 `RoadSense/`로 설정하고 원본 CSV가 `data/`에 있는지 확인합니다.
3. `data/preprocessed/`, `data/Cleaning/`, `data/Finaldata/` 디렉터리를 준비합니다.
4. **01 → 02 → 03 → 04** 순서로 각 노트북을 처음부터 실행합니다.

노트북의 한글 시각화는 Windows의 `Malgun Gothic` 폰트를 기준으로 설정되어 있습니다. 다른 운영체제에서는 설치된 한글 폰트로 변경해야 합니다.
