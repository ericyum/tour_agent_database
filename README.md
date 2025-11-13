# Tour Agent Database

FestMoment 데이터베이스 - 축제, 문화시설, 여행코스 데이터

## Quick Start

```bash
# 1. Clone all three projects in the same directory
git clone <frontend-repo> tour_agent_frontend
git clone <backend-repo> tour_agent_backend
git clone <database-repo> tour_agent_database

# 2. The database is ready to use!
# Backend will automatically detect and use this database
# No additional setup required if you follow the directory structure above
```

## 개요

이 프로젝트는 Tour Agent 시스템의 모든 데이터를 관리합니다:
- SQLite 데이터베이스
- CSV 원본 데이터
- 축제 메타데이터 (JSON)
- 분류 및 조건 데이터

## 디렉토리 구조

```
tour_agent_database/
├── tour.db                              # SQLite 데이터베이스 파일
├── festival_final_classification.csv   # 축제 분류 데이터
│
├── data/                                # 원본 CSV 데이터
│   ├── 축제공연행사csv.CSV
│   ├── 문화시설csv.CSV
│   ├── 여행코스csv.CSV
│   ├── festival_condition_split.csv
│   └── festivals_camera_angle_all.csv
│
├── festivals/                           # 축제 카테고리별 메타데이터
│   ├── festivals_type_계절과_자연.json
│   ├── festivals_type_도시와_커뮤니티.json
│   ├── festivals_type_레저와_스포츠.json
│   ├── festivals_type_문화와_예술.json
│   ├── festivals_type_미식과_특산물.json
│   ├── festivals_type_전통과_역사.json
│   ├── festivals_type_종교와_영성.json
│   └── festivals_type_체험과_교육.json
│
├── assets/                              # 정적 리소스
│   ├── seasons/                         # 계절별 워드클라우드 마스크
│   │   ├── mask_spring.png
│   │   ├── mask_summer.png
│   │   ├── mask_fall.png
│   │   └── mask_winter.png
│   └── themes/                          # 테마별 아이콘
│
└── best_images_and_icons/               # 축제 이미지 및 아이콘
    ├── best_images/                     # 축제별 대표 이미지
    ├── icons/                           # 축제 아이콘
    ├── icon_map.json                    # 축제명 → 아이콘 매핑
    └── best_images_map.json             # 축제명 → 이미지 매핑
```

## 데이터베이스 스키마

### festivals 테이블
축제 및 행사 정보

주요 컬럼:
- `title`: 축제명
- `addr1`, `addr2`: 주소
- `mapx`, `mapy`: 좌표
- `eventstartdate`, `eventenddate`: 행사 기간
- `cat1`, `cat2`, `cat3`: 카테고리 코드
- `overview`: 개요
- `tel`: 연락처
- `homepage`: 홈페이지

### facilities 테이블
문화시설 정보

주요 컬럼:
- `title`: 시설명
- `addr1`, `addr2`: 주소
- `mapx`, `mapy`: 좌표
- `cat1`, `cat2`, `cat3`: 카테고리 코드
- `overview`: 개요
- `usefee`: 이용요금
- `usetimeculture`: 이용시간

### courses 테이블
여행코스 정보

주요 컬럼:
- `title`: 코스명
- `addr1`, `addr2`: 주소
- `mapx`, `mapy`: 좌표
- `cat1`, `cat2`, `cat3`: 카테고리 코드
- `overview`: 개요
- `distance`: 코스 거리
- `taketime`: 소요시간

## 데이터 출처

- 한국관광공사 Tour API
- Naver 블로그 리뷰
- 자체 수집 및 가공 데이터

## 데이터베이스 초기화

Backend 프로젝트에서 자동으로 데이터베이스를 초기화합니다.

수동으로 초기화하려면:

```python
from src.infrastructure.persistence.database import init_db

init_db()
```

## 설치

### 프로젝트 Clone

**중요**: Frontend 및 Backend 프로젝트와 함께 같은 부모 디렉토리에 clone 하세요.

```bash
# 같은 디렉토리에 3개 프로젝트 clone
cd /your/projects/folder

git clone <frontend-repo-url> tour_agent_frontend
git clone <backend-repo-url> tour_agent_backend
git clone <database-repo-url> tour_agent_database
```

올바른 디렉토리 구조:
```
/your/projects/folder/
├── tour_agent_frontend/
├── tour_agent_backend/
└── tour_agent_database/  ← 이 프로젝트
```

### 데이터베이스 연결 설정

**자동 탐지 (권장)**:
- Backend가 자동으로 형제 디렉토리 `../tour_agent_database`를 찾습니다
- 위의 디렉토리 구조대로 설치하면 추가 설정이 필요 없습니다

**수동 설정 (다른 위치에 설치한 경우)**:
Backend 프로젝트의 `.env` 파일에서 경로를 지정하세요:

```env
DATABASE_PATH=/path/to/tour_agent_database
```

## 데이터 업데이트

### CSV 데이터 업데이트

1. `data/` 폴더의 CSV 파일을 새 데이터로 교체
2. Backend 프로젝트에서 `init_db()` 실행

### 축제 메타데이터 업데이트

`festivals/` 폴더의 JSON 파일을 편집하여 축제 카테고리 정보를 업데이트할 수 있습니다.

## 데이터 통계

- **축제**: 약 3,000개 이상
- **문화시설**: 약 1,000개 이상
- **여행코스**: 약 500개 이상

## 관련 프로젝트

- [Backend](../tour_agent_backend) - 이 데이터베이스를 사용하는 FastAPI 서버
- [Frontend](../tour_agent_frontend) - 웹 인터페이스

## 주의사항

- `tour.db` 파일은 SQLite 데이터베이스로, 직접 수정하지 마세요
- CSV 파일은 UTF-8 또는 CP949 인코딩을 사용합니다
- 데이터베이스 파일이 손상된 경우, 삭제 후 Backend에서 자동 재생성됩니다
