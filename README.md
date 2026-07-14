# 놓치는 사기 vs 오해하는 고객, 카드사는 어디에 선을 그어야 하는가
### 비용 기반 이상거래 탐지 Threshold 최적화

## 프로젝트 개요

카드사의 이상거래 탐지(FDS)는 두 가지 실수를 할 수 있다.

1. **사기를 놓치는 실수 (False Negative)** → 카드사가 거래 금액만큼 손실을 그대로 떠안는다.
2. **정상 거래를 사기로 오해하는 실수 (False Positive)** → 고객이 불편을 겪고, 카드사는 고객 대응 비용과 이탈 리스크를 진다.

대부분의 분류 모델은 이 두 실수를 동일하게 취급하는 `threshold = 0.5` 기준을 기본값으로 사용한다.
이 기준은 "두 실수의 비용이 같다"는 가정 위에서만 최적이며, 실제 이 데이터(사기 거래의 평균 금액이 정상 거래보다 훨씬 큼)에서는 이 가정이 성립하지 않는다.

이 프로젝트는 **분류 성능 자체가 아니라, "어떤 기준으로 거래를 막을 것인가"라는 의사결정 문제**를 다룬다.

## 핵심 질문

- 0.5라는 기본 threshold는 왜, 어떤 조건에서만 타당한가?
- 이 데이터의 실제 비용 구조(사기 손실액 vs 오탐 비용)를 반영하면 최적 threshold는 어떻게 달라지는가?
- 그 결과 예상되는 총 비용은 기본 threshold 대비 얼마나 줄어드는가?

## 프로젝트 구조 (4막)

| 단계 | 노트북 | 내용 |
|---|---|---|
| 1막. 문제 제기 | `01_eda.ipynb` | 사기 거래 vs 정상 거래의 금액 분포 비교, 0.5 threshold의 수학적 전제 검증 |
| 2막. 탐색 | `02_clustering.ipynb` | 클러스터링을 통한 사기 밀집 패턴 탐색 및 feature 반영 |
| 3막. 모델링 | `03_modeling.ipynb` | 지도학습(XGBoost 등) + Isolation Forest(비지도) 비교, PR-AUC 기반 평가 |
| 4막. 비용 최적화 | `04_cost_optimization.ipynb` | 비용 매트릭스 정의, threshold별 총 비용 곡선, 최적 threshold 도출 및 검증 |

## 데이터

- 출처: [Kaggle - Credit Card Transactions Dataset](https://www.kaggle.com/datasets/priyamchoksi/credit-card-transactions-dataset)
- 약 130만 건의 미국 신용카드 거래 기록 (2019.01 ~ 2020.06)
- 핵심 컬럼: `cc_num`, `merchant`, `category`, `amt`, `trans_date_trans_time`, `is_fraud`
- 용량 문제로 원본 CSV는 리포지토리에 포함하지 않음 (`data/README.md` 참고)

## 비교 기준 (Baseline 정의)

이 프로젝트에서 "성능 개선"은 **실제 카드사의 내부 탐지율과 비교한 결과가 아니다.**
카드사의 실제 FDS 탐지율/오탐율은 보안상 공개되지 않는 정보이며, 외부에서 검증이 불가능하다.

따라서 이 프로젝트의 비교 기준은 다음과 같이 명확히 한정한다.

> **기본 threshold(0.5)를 사용했을 때 vs 비용 매트릭스 기반으로 재계산한 threshold를 사용했을 때, 동일한 모델·동일한 데이터 안에서의 비교**

FP 비용(오탐 1건당 고객 대응/이탈 비용)은 데이터에 존재하지 않는 값이므로, 가정치를 명시적으로 설정하고 그 가정을 README와 노트북에 투명하게 기록한다.

## 핵심 결과

> (분석 완료 후 작성 예정)
> - Baseline(threshold=0.5) 대비 비용 최적화 threshold 적용 시 예상 총 비용 변화: TBD
> - 탐지율/오탐율 변화: TBD

## 한계점

- FP 비용은 실측치가 아닌 가정치이며, 실제 카드사 내부 데이터와는 다를 수 있다.
- 데이터는 시뮬레이션된 공개 데이터셋으로, 실제 프로덕션 환경의 거래 분포와 차이가 있을 수 있다.
- 실시간 처리(스트리밍) 관점은 다루지 않으며, 배치 단위 분석에 한정한다.

## 사용 기술

Python, pandas, scikit-learn, XGBoost, matplotlib/seaborn

## 실행 방법

```bash
pip install -r requirements.txt
# data/README.md 안내에 따라 원본 데이터를 data/ 폴더에 위치시킨 뒤
jupyter notebook notebooks/01_eda.ipynb
```
