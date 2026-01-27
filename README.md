# '난임 환자 데이터 기반 임신 성공 여부 예측' 데이터 분석 해커톤

> - **LG AIMERS 6기** 
> - 보조생식술(IVF, DI 등) 기록을 활용한 **임신 성공 여부** 이진 분류


## 프로젝트 개요

본 프로젝트는 **LG AIMERS 6기** 해커톤 과제로, 난임 환자의 **보조생식술 시술 데이터**를 바탕으로 **임신 성공 여부**를 예측합니다.

- **과제 유형**: 이진 분류 
- **평가**: 제출 형식은 probability이며, ROC-AUC 확률 기반 지표를 사용합니다.

## 프로젝트 구조

```
data-ai/
├── README.md
├── Baseline.ipynb          # 베이스라인 파이프라인 (결측치/인코딩/Random Forest)
├── PreProcessing.ipynb     # EDA 및 상세 전처리 (결측/이상치/다중공선성/정규화/인코딩)
└── Data/
    ├── train.csv           # 학습 데이터
    ├── test.csv            # 테스트 데이터
    ├── sample_submission.csv  # 제출 양식 (ID, probability)
    └── 데이터 명세.xlsx    # 컬럼 정의 (범주형/수치형 등)
```

## 데이터

### 구성

| 파일 | 설명 | 비고 |
|------|------|------|
| `train.csv` | 학습용 시술 기록 | `임신 성공 여부` 포함 |
| `test.csv` | 예측 대상 시술 기록 | `임신 성공 여부` 미포함 |
| `sample_submission.csv` | 제출 형식 | `ID`, `probability` |
| `데이터 명세.xlsx` | 컬럼별 설명, 범주형 여부 | 전처리 시 수치/범주 구분에 사용 |

### 주요 변수

- **시술·환자**: 시술 당시 나이, 시술 유형(IVF, DI), 특정 시술 유형(ICSI, IVF, IUI 등)
- **배란·이식**: 배란 자극 여부, 배란 유도 유형, 단일 배아 이식 여부, 착상 전 유전 검사/진단
- **불임 원인**: 남성/여성/부부 주·부 불임, 난관·남성·배란·여성·자궁·정자 등 세부 원인 (0/1)
- **이력·배아**: 총/IVF/DI 시술·임신·출산 횟수, 배아·난자 관련 수치(생성, 이식, 저장, 해동 등)
- **출처·기타**: 난자/정자 출처, 기증자 나이, 동결/신선/기증 배아 사용, 대리모, PGD/PGS, 경과일 등
- 
상세 정의는 `Data/데이터 명세.xlsx` 참고

## 사용 방법

### 1. 환경

- Python 3
- `pandas`, `numpy`, `scikit-learn`  
- `PreProcessing.ipynb` 사용 시: `plotly`, `openpyxl` (엑셀 읽기)

```bash
pip install pandas numpy scikit-learn openpyxl plotly
```

### 2. 노트북 실행

- **`Baseline.ipynb`**  
  - 데이터 로드 → ID·시술 시기 코드 제거 → 결측치 보간(수치: 중앙값, 범주: 최빈값) → Ordinal 인코딩 → train/validation 분할 → **Random Forest** 학습·평가  
  - 데이터 명세의 **범주형 여부**로 수치/범주 컬럼을 구분합니다.

- **`PreProcessing.ipynb`**  
  - 결측 80% 이상 컬럼 제거, `특정 시술 유형` 결측·Unknown·복합형 정리  
  - 시술 유형 **DI** 여부에 따른 배아·미세주입 결측 처리  
  - 불필요 컬럼 제거, **이상치 제거**, **다중공선성** 고려 피처 제거  
  - **StandardScaler** 정규화, **라벨/원핫 인코딩**  
  - `데이터 명세.xlsx`를 이용한 수치/범주 자동 구분

실행 시 `Data/` 폴더가 노트북과 같은 위치라고 가정합니다.  
(`./Data/train.csv` 등 상대 경로 사용)

## 베이스라인 성능

- **Baseline.ipynb** (Random Forest, validation set):  
  - Accuracy **약 71.49%**  
  - 제출이 `probability`이므로, **ROC-AUC** 등 확률 기반 지표 추가 활용을 권장합니다.

## 라이선스
본 저장소는 LG AIMERS 6기 해커톤 과제용이며, 데이터 및 과제에 대한 저작권과 이용 조건은 LG AI Research 및 대회 주최 측의 안내를 따릅니다.
