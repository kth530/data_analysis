# Data Analysis Portfolio

Python과 SQL을 활용해 고객 행동, 리텐션, 세그먼트, 설문 데이터를 분석한 프로젝트를 정리한 저장소입니다.  
각 프로젝트는 데이터 정제부터 탐색적 분석, 세그먼트 해석, 실행 가능한 제안까지 하나의 흐름으로 구성했습니다.

## Projects

| Project | Focus | Stack | Data |
|---------|-------|-------|------|
| [E-Commerce 고객 분석](./dacon_e_commerce) | 구매 패턴, RFM 세그먼트, 코호트 리텐션, 마케팅 액션 제안 | Python · SQL | [Dacon](https://dacon.io/competitions/official/236222/data) |
| [패션 플랫폼 사용자 분석](./fashion_platform_analysis) | 설문 기반 사용자 행동, NPS 세그먼트, 재구매 의향, 채널·자유응답 분석 | Python · SQL · KoNLPy | 직접 수집 |

## Project Notes

### E-Commerce 고객 분석

온라인 쇼핑몰 데이터를 활용해 고객의 구매 행동과 재구매 구조를 분석했습니다.

- RFM 기반 고객 등급 및 세그먼트 분류
- 코호트 리텐션으로 재구매·이탈 흐름 확인
- 할인, 마케팅 채널, 구매 패턴을 바탕으로 액션 아이템 제안

### 패션 플랫폼 사용자 분석

직접 수집한 설문 데이터를 바탕으로 패션 플랫폼 사용자의 만족도와 행동 패턴을 분석했습니다.

- NPS 그룹별 재구매 이유와 불만족 경험 비교
- 의향과 실제 행동의 정합성 검증
- 인지 경로, 구매 영향 채널, 자유응답 기반 개선 포인트 도출

## Repository Structure

```text
data_analysis/
├── dacon_e_commerce/
└── fashion_platform_analysis/
```

각 프로젝트 폴더에서 세부 노트북과 분석 흐름을 확인할 수 있습니다.
