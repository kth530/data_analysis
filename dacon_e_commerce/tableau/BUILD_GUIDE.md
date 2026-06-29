# Tableau 대시보드 제작 가이드

Dacon E-Commerce 고객 분석 대시보드를 Tableau Desktop에서 재현하기 위한 가이드.
데이터는 `export_csv.py`가 생성한 CSV 4종을 사용한다 (Tableau Public은 MySQL 라이브 연결 불가).

## 0. 준비

```bash
python tableau/export_csv.py   # MySQL 뷰 생성 + CSV 4종 추출
```

| CSV | 그레인 | 행수 | 용도 |
|-----|--------|------|------|
| `v_tableau_orders.csv` | 거래라인 | 52,659 | 매출·카테고리·쿠폰·요일 |
| `v_tableau_customer.csv` | 고객 | 1,468 | 세그먼트·등급 프로파일, R×F |
| `v_tableau_monthly.csv` | 월 | 12 | 매출 추세·마케팅·ARPPU |
| `cohort_retention.csv` | 코호트×경과월 | 78 | 리텐션 히트맵 |

Tableau에서 각 CSV를 **개별 데이터 소스(텍스트 파일 연결)**로 추가한다. 4개는 조인하지 않고 시트별로 알맞은 소스를 선택한다.

## 1. 색상 팔레트 (노트북과 통일)

| 등급 | HEX | | 성별 | HEX |
|------|-----|---|------|-----|
| Diamond | `#B9F2FF` | | 여 | `#e74c3c` |
| Platinum | `#E0B0FF` | | 남 | `#3498db` |
| Gold | `#FFD700` | | | |
| Silver | `#C0C0C0` | | 강조 | `#2ecc71` |
| Bronze | `#CD7F32` | | 중립 | `#95a5a6` |

등급 정렬 순서(수동): Diamond → Platinum → Gold → Silver → Bronze.

## 2. 대시보드 구성 (5섹션 스토리)

세로 1장 스크롤(1200 × ~2600px) 권장. 섹션별 시트:

### ① 매출 개요 — `v_tableau_monthly`
- **KPI 카드**: 총매출 `SUM(매출)`, 총거래 `SUM(거래수)`, 연 ARPPU `SUM(매출)/SUM(고객수)`
- **월별 매출 추세**: 막대(월) + ARPPU 라인 (이중 축). 11~12월 피크 강조
- **마케팅비용 vs 매출**: 월 기준 온라인비용·오프라인비용(막대) + 매출(라인)

### ② 카테고리 — `v_tableau_orders`
- **카테고리별 매출**: `SUM(세후금액)` 내림차순 막대 (Nest-USA 편중 확인)
- **카테고리 × 등급 매출 비중**: 누적 막대 (행=제품카테고리, 색=등급)
- **쿠폰 사용률**: `쿠폰상태`별 거래 비중 (Used/Clicked/Not Used)

### ③ 고객 / 세그먼트 — `v_tableau_customer`
- **세그먼트 분포**: `세그먼트`별 `COUNT(고객ID)` 막대 (휴면·신규·잠재충성 상위)
- **R × F 산점도**: x=R, y=F, 색=세그먼트, 크기=`총지출`
- **성별·지역**: 성별 도넛 + 고객지역별 매출(`SUM(총지출)`) 막대

### ④ 리텐션 — `cohort_retention`
- **코호트 히트맵**: 행=`코호트월`, 열=`경과월`, 색=`리텐션율`(빨강→초록), 레이블=리텐션율
- 경과월 0열은 100%(기준) — 색 스케일에서 제외하거나 필터 `경과월 > 0`

### ⑤ 등급 프로파일 — `v_tableau_customer`
- **등급별 고객수·매출**: `COUNT(고객ID)` + `SUM(총지출)` (이중 축 또는 나란히)
- **등급별 재방문율**: `AVG(재방문여부) * 100` 막대 (Bronze 26% → Diamond 95%)
- **등급별 1인당지출**: `AVG(총지출)` 막대

## 3. 핵심 계산식 (Tableau)

```
재방문율(%)      = AVG([재방문여부]) * 100
1인당지출        = AVG([총지출])
매출비중(%)      = SUM([세후금액]) / TOTAL(SUM([세후금액]))
ARPPU            = SUM([매출]) / SUM([고객수])      // monthly 소스
```

## 4. 검증 체크리스트 (제작 후 확인)

- 고객 수 합계 = **1,468**
- 거래라인 합계 = **52,659**
- 등급 분포 = Bronze 699 / Silver 314 / Gold 243 / Platinum 154 / Diamond 58
- 등급별 재방문율 ≈ Bronze 26.3 / Silver 57.0 / Gold 74.1 / Platinum 88.3 / Diamond 94.8
- 상위 2등급(Diamond+Platinum) 매출 비중 ≈ 48.6%
- 1월 코호트 +1개월 리텐션 = 6.0%

이 수치들은 노트북 03·07 출력과 일치해야 한다.
