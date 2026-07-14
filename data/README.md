# 데이터 안내

이 프로젝트는 아래 Kaggle 데이터셋을 사용한다.

- **출처**: [Credit Card Transactions Dataset](https://www.kaggle.com/datasets/priyamchoksi/credit-card-transactions-dataset)
- **파일명**: `credit.csv`
- **용량**: 약 350MB (약 130만 행)

파일 용량이 커서 이 저장소에는 포함하지 않았다.
직접 실행하려면 위 링크에서 다운로드 후 이 폴더(`data/`)에 `credit.csv`로 저장하면 된다.

## 주요 컬럼

| 컬럼명 | 설명 |
|---|---|
| `cc_num` | 사용자 식별자 |
| `merchant` | 가맹점명 |
| `category` | 거래 카테고리 |
| `amt` | 거래 금액 |
| `trans_date_trans_time` | 거래 발생 시점 |
| `lat`, `long` | 사용자 위치 |
| `merch_lat`, `merch_long` | 가맹점 위치 |
| `is_fraud` | 사기 거래 여부 (0/1) |
