# Dacon E-Commerce 고객 분석

온라인 커머스 1년치(2019) 거래 데이터를 기반으로 한 **RFM 세그먼테이션 고객 분석** 포트폴리오.
ETL → EDA → 리텐션 → RFM 등급화 → 등급별 심층 분석으로 이어지는 분석 흐름을 노트북으로 정리했다.

데이터 출처: [Dacon — E-Commerce Dataset](https://dacon.io/competitions/official/236222/data)

## 분석 구조 (SQL · Python · Tableau)

역할을 나눠 구성했다 (상세: [`docs/sql_workflow.md`](docs/sql_workflow.md)).

- **SQL** — 추출·집계·분류·조인·윈도우. 노트북 02~07은 짝이 되는 [`sql/`](sql/) 파일(`-- name:` 마커)에서 쿼리를 호출한다. RFM 등급·세그먼트 분류는 `rfm_scored` VIEW(CASE WHEN)로 처리.
- **Python** — RFM 점수화(qcut·IQR·PCA·KMeans), 코호트, scipy 통계, Plotly 시각화.
- **Tableau** — 발표용 대시보드. [`tableau/`](tableau/)의 VIEW·`export_csv.py`로 CSV를 추출해 Tableau Desktop에서 제작 (가이드: [`tableau/BUILD_GUIDE.md`](tableau/BUILD_GUIDE.md)).

## 분석 흐름

`notebooks/`의 노트북은 번호 순서대로 읽으면 된다. 모든 그래프·출력은 노트북에 저장돼 있어 실행 없이 GitHub에서 바로 볼 수 있다.

| # | 노트북 | 내용 |
|---|--------|------|
| 00 | [`00_etl_pipeline`](notebooks/00_etl_pipeline.ipynb) | 5개 원본 테이블 정제·조인 → 마스터 테이블(`orders_master`) 생성, DB 적재 |
| 01 | [`01_eda`](notebooks/01_eda.ipynb) | 매출·카테고리·인구통계·쿠폰·마케팅(ARPPU)·지역·요일·장바구니 탐색 |
| 02 | [`02_retention`](notebooks/02_retention.ipynb) | 코호트 리텐션, 채널·카테고리·마케팅 비용별 재구매율 |
| 03 | [`03_rfm_segmentation`](notebooks/03_rfm_segmentation.ipynb) | RFM 점수화 → PCA 가중치 → 5등급 배정 ([방법론](docs/methodology.md)) |
| 04–07 | [`04`](notebooks/04_segment_bronze.ipynb)·[`05`](notebooks/05_segment_silver.ipynb)·[`06`](notebooks/06_segment_gold.ipynb)·[`07`](notebooks/07_segment_diamond_platinum.ipynb) | Bronze / Silver / Gold / Diamond·Platinum 등급별 가설–검증–제언 |

## 데이터셋

| 파일 | 행 수 | 주요 컬럼 |
|------|------|---------|
| `Onlinesales_info.csv` | 52,924 | 고객ID, 거래ID, 거래날짜, 제품카테고리, 수량, 평균금액, 배송료, 쿠폰상태 |
| `Customer_info.csv` | 1,468 | 고객ID, 성별, 고객지역, 가입기간 |
| `Discount_info.csv` | 204 | 월, 제품카테고리, 쿠폰코드, 할인율 |
| `Marketing_info.csv` | 365 | 날짜, 오프라인비용, 온라인비용 |
| `Tax_info.csv` | 20 | 제품카테고리, GST |

- 거래 기간: 2019-01-01 ~ 2019-12-31, 금액 단위는 달러($)
- **원본 CSV는 저장소에 포함하지 않는다.** 위 출처에서 내려받아 `data/`에 두면 된다.

## 주요 결과 (요약)

- **매출 편중**: Nest-USA 단일 카테고리가 전체 매출의 약 54%. 거래량(Apparel)과 매출 기여가 비례하지 않음
- **고객 구조**: 여성이 매출의 62% 기여(객단가는 남녀 동일, 차이는 고객 수). 25개월 이상 장기 고객이 53%
- **지역 집중**: Chicago·California 두 지역이 전체 매출의 약 66%
- **리텐션**: 단발성 구매 비중이 높아 초기 이탈이 지배적 — 마케팅 비용 효과는 장기에서만 유효
- **세그먼트**: RFM 점수 기반 5등급. 상위 2등급(Diamond·Platinum)이 전체 고객 14%로 매출의 48% 차지

## 기술 스택 / 재현

- Python: `pandas` · `numpy` · `scipy` · `scikit-learn` · `plotly` · `sqlalchemy` · `python-dotenv` · `mysql-connector-python`
- DB: MySQL — 노트북 02~07은 [`sql/`](sql/)의 쿼리를 `load_queries()`로 불러 실행
- BI: Tableau Desktop — [`tableau/export_csv.py`](tableau/export_csv.py)로 추출한 CSV를 데이터 소스로 사용
- 노트북 첫 셀이 `.env`에서 DB 접속 정보(`DB_USER`, `DB_PASSWORD`, `DB_HOST`, `DB_NAME`)와 `DATA_DIR`를 읽는다. 직접 실행하려면 `.env`와 MySQL이 필요하다. 읽기만 한다면 저장된 출력으로 충분하다.

```
dacon_e_commerce/
├── notebooks/   # 00~07 분석 노트북
├── sql/         # 02~07 분석 SQL (-- name: 마커)
├── tableau/     # VIEW·export_csv.py·BUILD_GUIDE (CSV는 재생성)
├── docs/        # methodology · sql_workflow
└── data/        # 원본 CSV (비공개, 별도 다운로드)
```
