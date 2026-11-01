# LSMD_ADM_SECT_UMD_11_202609 코드북

## 데이터 개요

| 항목 | 내용 |
|---|---|
| 파일 | `LSMD_ADM_SECT_UMD_11_202609.shp` |
| 주제 | 서울특별시 법정 읍·면·동 경계 |
| 레코드 수 | 467개 (DBF 기준) |
| 도형 | 읍·면·동 경계 폴리곤 |
| 좌표계 | EPSG:5186, Korea 2000 / Central Belt 2010 (미터) |
| 속성 인코딩 | 한글이 깨지면 GeoPandas에서 `encoding="cp949"` 지정 |

`UMD`는 읍·면·동 경계 레이어를 가리킵니다. 이 자료는 법정동 기준 경계이므로 행정동 경계와 이름·경계가 다를 수 있습니다. 필드 자료형과 폭은 해당 Shapefile의 DBF 헤더에서 확인했습니다.

## 속성 필드

| 필드명 | 한글명 | DBF 형식 | 설명 |
|---|---|---:|---|
| `EMD_CD` | 읍면동코드 | 문자(8) | 법정 읍·면·동을 식별하는 코드. 코드값은 문자로 보존하세요. |
| `COL_ADM_SE` | 원천행정구역코드 | 문자(5) | 수집·제작 원천의 시군구 행정구역 코드로 사용되는 필드. 서울 지역 코드는 통상 `11`로 시작합니다. |
| `EMD_NM` | 읍면동명 | 문자(100) | 법정 읍·면·동 이름 |
| `SGG_OID` | 시군구 원천 객체 ID | 숫자(33, 소수 15자리) | 원천 공간 데이터의 객체 식별값. 행정구역 코드 대신 사용하지 마세요. |
| `geometry` | 공간도형 | Polygon/MultiPolygon | 읍·면·동 경계 공간도형 |

## 사용 시 참고

- `EMD_CD`와 `EMD_NM`을 함께 확인해 지역을 구분하세요. 같은 읍·면·동 이름이 다른 시군구에도 있을 수 있습니다.
- `COL_ADM_SE`는 DBF에 저장된 실제 필드명입니다. 배포처의 상세 메타데이터에 따라 정식 컬럼명이 `COL_ADM_SECT_CD` 등으로 표기될 수 있습니다.
- `SGG_OID`는 내부 객체 ID 성격의 값입니다. 자료 갱신 과정에서 달라질 수 있으므로 다른 데이터와 결합할 때는 법정동 코드 등 공식 지역 코드를 사용하세요.
- 법정동 경계를 행정동별 인구 등 행정동 단위 자료와 결합하려면 별도 법정동-행정동 매핑 자료가 필요합니다.

## 읽기 예시

```python
import geopandas as gpd

path = "shp/LSMD_ADM_SECT_UMD_서울/LSMD_ADM_SECT_UMD_11_202609.shp"
umd = gpd.read_file(path, encoding="cp949")

print(umd.shape)
print(umd.crs)
print(umd.dtypes)
umd.head()
```

## 참고 자료

- [주소정보누리집](https://www.juso.go.kr/) — 도로명주소·공간자료 안내
- [서울시 공간정보 레이어 목록](https://energyinfo.seoul.go.kr/gis/web/wicket/bookmarkable/org.geoserver.web.demo.MapPreviewPage?filter=false) — `LSMD_ADM_SECT_UMD` 레이어 확인
- 행정구역 코드의 형식과 필드 설명은 실제 배포 월의 원본 메타데이터·코드표와 대조해 사용하세요.
