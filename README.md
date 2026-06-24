# Data Analysis Portfolio

Python과 SQL을 활용해 고객 행동 데이터를 분석한 포트폴리오 저장소입니다.
구매 이력과 설문 응답을 바탕으로 리텐션, 세그먼트, 추천 의향, 채널 행동을 살펴보고, 분석 결과를 실제 액션으로 연결하는 데 초점을 두었습니다.

각 프로젝트는 단순 시각화보다 **문제 정의 → 데이터 정제 → SQL 기반 집계 → 탐색적 분석/통계 검정 → 세그먼트 해석 → 실행 가능한 제안**의 흐름으로 구성했습니다.
노트북은 분석 과정과 해석을 보여주고, 반복 가능한 집계·분류 로직은 SQL 또는 별도 문서로 분리했습니다.

## Projects

| Project | Focus | Stack | Data |
|---------|-------|-------|------|
| [E-Commerce 고객 분석](./dacon_e_commerce) | 구매 패턴, RFM 세그먼트, 코호트 리텐션, 마케팅 액션 제안 | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) | [Dacon](https://dacon.io/competitions/official/236222/data) |
| [패션 플랫폼 사용자 분석](./fashion_platform_analysis) | 설문 기반 사용자 행동, NPS 세그먼트, 재구매 의향, 채널·자유응답 분석 | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) | 직접 수집 |

## Project Notes

### E-Commerce 고객 분석

온라인 쇼핑몰 데이터를 활용해 고객의 구매 행동과 재구매 구조를 분석했습니다. 주문, 고객, 할인, 마케팅, 세금 데이터를 정리해 분석 가능한 형태로 만들고, 구매 빈도와 최근성, 매출 기여도를 기준으로 고객을 나누었습니다.

- ETL 노트북에서 원천 테이블을 정리하고 분석용 데이터셋을 구성
- EDA로 매출, 주문, 할인, 채널별 구매 패턴을 파악
- RFM 기반 고객 등급과 세그먼트를 정의해 고객군별 행동 차이를 비교
- 코호트 리텐션으로 첫 구매 이후 재구매·이탈 흐름을 확인
- 할인, 마케팅 채널, 구매 패턴을 바탕으로 세그먼트별 액션 아이템 제안

### 패션 플랫폼 사용자 분석

직접 수집한 설문 데이터를 바탕으로 패션 플랫폼 사용자의 만족도와 행동 패턴을 분석했습니다. 단순 만족도 평균보다 NPS, 구매 빈도, 최근 구매 시점, 채널 인지, 자유응답을 연결해 사용 지속과 불만 응답이 어떤 패턴을 보이는지 살펴보았습니다.

- 논리 모순 응답을 제외하고 사용자/구매자 기준 모수를 분리해 분석
- NPS 그룹별 재구매 이유, 계속 사용 의향, 불만족 경험을 비교
- 의향과 실제 구매 행동의 정합성을 검토하고 R×F/RFM 기반 세그먼트로 확장
- 인지 경로와 구매 영향 채널을 비교해 채널별 역할 차이를 확인
- 자유응답을 원문 기준으로 분류해 사이즈/핏, 추천/검색, 정보/리뷰 등 개선 포인트 도출
- 03–07 분석 로직은 SQL 파일로 분리해 노트북은 시각화·검정·해석에 집중하도록 구성

## Repository Structure

```text
data_analysis/
├── dacon_e_commerce/
└── fashion_platform_analysis/
```

각 프로젝트 폴더에서 세부 노트북과 분석 흐름을 확인할 수 있습니다.
