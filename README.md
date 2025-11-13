# data-analysis

Jupyter Notebook 기반으로 여러 데이터셋을 분석하고, 회귀 모델을 적용해 보는 연습용 레포지토리입니다.  

현재는 아래 두 가지 프로젝트가 포함되어 있습니다.

1. **Supermarket Store Sales 분석** – 지점별 매출 데이터를 활용한 매출 예측
2. **House Price 분석** – 주택 특성 정보를 활용한 집값 예측 (Kaggle House Prices)

---

## 1. 프로젝트 개요

### 1-1. Supermarket Store Sales 분석 (`supermarket_sales_analysis.ipynb`)

**데이터 설명**  
`Stores.csv`에는 지점 단위로 다음과 같은 정보가 포함됩니다.

- `Store ID` : 각 지점의 고유 ID (Index)
- `Store_Area` : 매장의 실제 면적 (제곱야드)
- `Items_Available` : 해당 지점에서 판매하는 서로 다른 상품의 개수
- `Daily_Customer_Count` : 한 달 평균 일일 방문 고객 수
- `Store_Sales` : 지점 매출 (미국 달러, 예측 대상)

**분석/모델링 흐름**

- 데이터 로딩 및 기본 탐색 (`head`, `info`, `describe`)
- 분포 및 상관 관계 분석  
  - `seaborn.distplot`을 활용한 각 변수의 분포 시각화  
  - 상관 행렬 및 히트맵(`data.corr()`, `sns.heatmap`)을 통한 변수 간 관계 파악
- 회귀 모델링
  - **Train/Validation/Test 분리** (`train_test_split`)
  - **Linear Regression** 기본 모델 학습 및 RMSE, R² 평가
  - **Polynomial Regression** (다항 특성 생성 – `PolynomialFeatures`)  
  - **Ridge / Lasso Regression**  
  - **StandardScaler + Ridge** 파이프라인을 이용한 스케일링 & 정규화 회귀
  - **교차 검증** (`cross_val_score`)을 이용한 RMSE 평균 비교
- 여러 모델의 성능(RMSE)을 비교하며, 단순 선형 회귀 대비 정규화/다항 회귀의 효과를 확인

---

### 1-2. House Price 분석 (`house_price.ipynb`)

**데이터 설명**  
Kaggle **House Prices – Advanced Regression Techniques** 데이터셋을 사용합니다.  
대표적인 컬럼(일부)은 다음과 같습니다.

- `SalePrice` : 예측해야 할 집 판매 가격 (타깃 변수)
- `MSSubClass` : 건물 클래스
- `MSZoning` : 토지 용도 구분
- `LotFrontage` : 도로와 맞닿은 길이
- `LotArea` : 대지 면적
- `Neighborhood` : Ames 시 내 물리적 위치(동네)
- `OverallQual`, `OverallCond` : 전반적인 자재/마감 품질 및 상태
- `YearBuilt`, `YearRemodAdd` : 건축 연도, 리모델링 연도
- `1stFlrSF`, `2ndFlrSF`, `GrLivArea` : 층/지상 생활 면적
- 그 외 수많은 범주형/수치형 특성

**분석/모델링 흐름**

- 데이터 로딩 및 기초 탐색
- 결측치 확인 및 처리
  - `LotFrontage`, `MasVnrArea`, `GarageYrBlt` 등 연속형 변수의 결측을 **중앙값(median)** 으로 대체
- 연속형 / 범주형 변수 분리
- 이상치 탐지 & 제거
  - `LotArea` 등 일부 연속형 변수에 대해 **IQR(사분위 범위)** 를 이용한 이상치 제거
- 범주형 변수 인코딩
  - `pd.get_dummies()`를 사용한 원-핫 인코딩
- 모델링
  - 불필요/희소한 피처 제거 (ex. `PoolArea`, `MiscVal` 등)
  - **Linear Regression**
  - **Ridge / Lasso Regression**
  - **Polynomial Regression (PolynomialFeatures + LinearRegression)**
  - **StandardScaler**를 포함한 파이프라인 구성
- 평가
  - `cross_val_score`를 활용한 **K-Fold 교차 검증**
  - RMSE를 중심으로 여러 모델의 일반화 성능 비교

---

## 2. 기술 스택 (Tech Stack)

- Python 3.x
- 라이브러리
  - `numpy`
  - `pandas`
  - `matplotlib`
  - `seaborn`
  - `scikit-learn`
  - `jupyter` / `jupyterlab` 또는 Google Colab

---

## 3. 레포지토리 구조

```bash
data-analysis/
├── supermarket_sales_analysis.ipynb  # 슈퍼마켓 지점 매출 분석 & 예측
├── house_price.ipynb                 # 주택 가격 분석 & 예측
└── README.md

## 4. 실행 방법

### 4-1. 로컬 환경에서 실행

1. 레포지토리 클론

   ```bash
   git clone https://github.com/<YOUR_ID>/data-analysis.git
   cd data-analysis

2. (선택) 가상환경 생성 & 활성화
python -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate

3. 필요한 패키지 설치
pip install -U pip
pip install numpy pandas matplotlib seaborn scikit-learn jupyter

4. Jupyter Notebook 실행
jupyter notebook

5. 브라우저에서 아래 파일들을 열어 셀을 순서대로 실행합니다.
- supermarket_sales_analysis.ipynb
- house_price.ipynb

