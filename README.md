# FestMoment Database 💾✨

> "축제의 순간을 AI로 재해석하다"
>
> **Team FestMoment** | 염정운, 최가윤

**FestMoment Database**는 전국의 축제, 문화시설, 여행코스 데이터와 AI 분석에 필요한 모든 정적 리소스를 관리합니다. 공공 데이터와 자체 수집 데이터를 융합하여 AI 축제 가이드의 기반을 제공합니다.

---

## 📑 목차

1. [데이터 개요](#-데이터-개요)
2. [디렉토리 구조](#-디렉토리-구조)
3. [데이터베이스 스키마](#-데이터베이스-스키마)
4. [데이터 출처](#-데이터-출처)
5. [Quick Start](#-quick-start)
6. [상세 설치 가이드](#-상세-설치-가이드)
7. [데이터 관리](#-데이터-관리)

---

## 📊 데이터 개요

### 데이터 규모
- **축제**: 약 3,000개 이상 (TourAPI)
- **문화시설**: 약 1,000개 이상 (TourAPI)
- **여행코스**: 약 500개 이상 (TourAPI)
- **축제 이미지**: 수백 개의 대표 이미지 및 아이콘
- **계절 마스크**: 4계절 워드클라우드 마스크
- **테마 아이콘**: 7가지 축제 테마별 아이콘

### 데이터 특징

1. **공공 + 민간 데이터 융합**
   - TourAPI 공공 데이터 (정형 정보)
   - 자체 수집 메타데이터 (분류, 특성)

2. **AI 친화적 구조**
   - 계층적 카테고리 (대분류 → 중분류 → 소분류)
   - 좌표 기반 지리 데이터
   - 이미지 메타데이터 매핑

3. **확장 가능한 설계**
   - CSV → SQLite 자동 변환
   - JSON 메타데이터 분리 관리
   - 신규 데이터 추가 용이

---

## 📁 디렉토리 구조

```
tour_agent_database/
├── tour.db                              # 🔵 SQLite 데이터베이스 (자동 생성)
├── festival_final_classification.csv   # 🟢 축제 상세 분류 데이터
│
├── data/                                # 🟢 원본 CSV 데이터
│   ├── 축제공연행사csv.CSV              # TourAPI 축제 데이터
│   ├── 문화시설csv.CSV                  # TourAPI 문화시설 데이터
│   ├── 여행코스csv.CSV                  # TourAPI 여행코스 데이터
│   ├── festival_condition_split.csv    # AI 렌더링용 조건 데이터
│   └── festivals_camera_angle_all.csv  # AI 렌더링용 카메라 앵글
│
├── festivals/                           # 🟡 축제 카테고리별 메타데이터 (JSON)
│   ├── festivals_type_계절과_자연.json
│   ├── festivals_type_도시와_커뮤니티.json
│   ├── festivals_type_레저와_스포츠.json
│   ├── festivals_type_문화와_예술.json
│   ├── festivals_type_미식과_특산물.json
│   ├── festivals_type_전통과_역사.json
│   ├── festivals_type_종교와_영성.json
│   └── festivals_type_체험과_교육.json
│
├── assets/                              # 🎨 정적 리소스
│   ├── seasons/                         # 계절별 워드클라우드 마스크
│   │   ├── mask_spring.png             # 봄 마스크
│   │   ├── mask_summer.png             # 여름 마스크
│   │   ├── mask_fall.png               # 가을 마스크
│   │   └── mask_winter.png             # 겨울 마스크
│   ├── themes/                          # 테마별 아이콘 이미지
│   │   ├── 계절과_자연.png
│   │   ├── 도시와_커뮤니티.png
│   │   ├── 레저와_스포츠.png
│   │   ├── 문화와_예술.png
│   │   ├── 미식과_특산물.png
│   │   ├── 전통과_역사.png
│   │   └── 종교와_영성.png
│   └── 워드클라우드_7개테마_명칭.txt
│
└── best_images_and_icons/               # 🖼️ 축제 이미지 및 아이콘
    ├── best_images/                     # 축제별 대표 이미지
    │   ├── 축제명1.jpg
    │   ├── 축제명2.jpg
    │   └── ...
    ├── icons/                           # 축제 아이콘
    │   ├── 축제명1_icon.png
    │   ├── 축제명2_icon.png
    │   └── ...
    ├── icon_map.json                    # 축제명 → 아이콘 파일 매핑
    └── best_images_map.json             # 축제명 → 이미지 파일 매핑
```

---

## 🗄️ 데이터베이스 스키마

### festivals 테이블
**축제 및 행사 정보** (약 3,000개)

| 컬럼명 | 타입 | 설명 |
|--------|------|------|
| `id` | INTEGER | 고유 ID (자동 증가) |
| `title` | TEXT | 축제명 |
| `addr1`, `addr2` | TEXT | 주소 (기본, 상세) |
| `areacode` | INTEGER | 지역 코드 |
| `sigungucode` | INTEGER | 시군구 코드 |
| `mapx`, `mapy` | REAL | 좌표 (경도, 위도) |
| `eventstartdate` | TEXT | 행사 시작일 (YYYYMMDD) |
| `eventenddate` | TEXT | 행사 종료일 (YYYYMMDD) |
| `cat1`, `cat2`, `cat3` | TEXT | 카테고리 코드 (대/중/소) |
| `firstimage` | TEXT | 대표 이미지 URL |
| `overview` | TEXT | 개요 (HTML 포함) |
| `tel` | TEXT | 연락처 |
| `homepage` | TEXT | 홈페이지 URL |
| `contentid` | TEXT | TourAPI Content ID |
| ... | ... | 기타 30+ 컬럼 |

### facilities 테이블
**문화시설 정보** (약 1,000개)

| 컬럼명 | 타입 | 설명 |
|--------|------|------|
| `id` | INTEGER | 고유 ID |
| `title` | TEXT | 시설명 |
| `addr1`, `addr2` | TEXT | 주소 |
| `mapx`, `mapy` | REAL | 좌표 |
| `cat1`, `cat2`, `cat3` | TEXT | 카테고리 |
| `usefee` | TEXT | 이용 요금 |
| `usetimeculture` | TEXT | 이용 시간 |
| `restdateculture` | TEXT | 휴무일 |
| `parkingculture` | TEXT | 주차 정보 |
| ... | ... | 기타 시설 정보 |

### courses 테이블
**여행코스 정보** (약 500개)

| 컬럼명 | 타입 | 설명 |
|--------|------|------|
| `id` | INTEGER | 고유 ID |
| `title` | TEXT | 코스명 |
| `addr1`, `addr2` | TEXT | 주소 |
| `mapx`, `mapy` | REAL | 좌표 |
| `distance` | TEXT | 코스 거리 |
| `schedule` | TEXT | 일정 |
| `taketime` | TEXT | 소요 시간 |
| `theme` | TEXT | 테마 |
| `subcontentid` | TEXT | 하위 코스 ID (JSON) |
| `subname` | TEXT | 하위 코스명 (JSON) |
| ... | ... | 기타 코스 정보 |

---

## 🌐 데이터 출처

### 공공 데이터
- **한국관광공사 TourAPI** (https://api.visitkorea.or.kr)
  - 축제공연행사 정보 조회
  - 문화시설 정보 조회
  - 여행코스 정보 조회
  - 이미지 정보 조회

### 자체 수집 데이터
- **축제 분류**: 7가지 테마 × 3단계 계층 분류
- **AI 렌더링 메타데이터**: 계절/시간/앵글 조건
- **베스트 이미지**: Naver 블로그 크롤링 + AI 선정
- **아이콘**: Gemini Vision 자동 생성

### 데이터 갱신 주기
- **기본 정보**: 분기별 (TourAPI)
- **이미지/아이콘**: 필요 시 수동 갱신
- **카테고리**: 반년 주기 재분류

---

## 🚀 Quick Start

```bash
# 1. Clone all three projects in the same directory
git clone <frontend-repo> tour_agent_frontend
git clone <backend-repo> tour_agent_backend
git clone <database-repo> tour_agent_database

# 2. The database is ready to use!
# Backend will automatically detect and use this database
# No additional setup required if you follow the directory structure above
```

**자동 설정**:
- Backend가 자동으로 `tour.db` 생성
- CSV 데이터 자동 로드
- JSON 메타데이터 자동 읽기

---

## 📚 상세 설치 가이드

### 프로젝트 Clone

**⚠️ 중요**: Frontend 및 Backend 프로젝트와 함께 같은 디렉토리에 clone 하세요.

```bash
cd /your/projects/folder

git clone <frontend-repo-url> tour_agent_frontend
git clone <backend-repo-url> tour_agent_backend
git clone <database-repo-url> tour_agent_database
```

**올바른 디렉토리 구조**:
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

### 데이터베이스 초기화

Backend 서버를 처음 실행하면 자동으로 초기화됩니다:

```bash
cd ../tour_agent_backend
python api_server.py
```

초기화 과정:
1. `tour.db` 파일 생성
2. 3개 테이블 생성 (festivals, facilities, courses)
3. `data/` 폴더의 CSV 파일 읽기
4. 데이터 로드 및 인덱싱

---

## 🔧 데이터 관리

### 데이터 업데이트

#### CSV 데이터 업데이트

1. `data/` 폴더의 CSV 파일을 새 데이터로 교체
2. 기존 `tour.db` 파일 삭제
3. Backend 서버 재시작 (자동 재생성)

```bash
# 1. 새 CSV 파일을 data/ 폴더에 복사
cp new_data.csv data/축제공연행사csv.CSV

# 2. 기존 DB 삭제
rm tour.db

# 3. Backend 재시작 (자동으로 DB 재생성)
cd ../tour_agent_backend
python api_server.py
```

#### 축제 메타데이터 업데이트

`festivals/` 폴더의 JSON 파일을 직접 편집:

```json
{
  "계절과 자연": {
    "봄 축제": {
      "벚꽃 축제": [
        "진해군항제",
        "여의도 봄꽃축제",
        "경주 벚꽃마라톤"
      ]
    }
  }
}
```

Backend 재시작 없이 즉시 반영됩니다.

#### 이미지 및 아이콘 추가

1. `best_images_and_icons/best_images/`에 이미지 추가
2. `best_images_and_icons/icons/`에 아이콘 추가
3. `best_images_map.json` / `icon_map.json` 업데이트

```json
// best_images_map.json
{
  "축제명": "image_filename.jpg"
}

// icon_map.json
{
  "축제명": "icon_filename.png"
}
```

### 데이터베이스 백업

```bash
# SQLite 데이터베이스 백업
cp tour.db tour.db.backup

# CSV 데이터 백업
tar -czf data_backup.tar.gz data/ festivals/
```

### 데이터 검증

Python으로 데이터 확인:

```python
import sqlite3

conn = sqlite3.connect('tour.db')
cursor = conn.cursor()

# 축제 개수 확인
cursor.execute("SELECT COUNT(*) FROM festivals")
print(f"총 축제 수: {cursor.fetchone()[0]}")

# 카테고리별 축제 수
cursor.execute("""
    SELECT cat1, COUNT(*)
    FROM festivals
    WHERE cat1 IS NOT NULL
    GROUP BY cat1
""")
for row in cursor.fetchall():
    print(f"{row[0]}: {row[1]}개")

conn.close()
```

---

## 📊 데이터 통계

### 지역별 분포
- **서울**: 약 400개
- **경기**: 약 500개
- **강원**: 약 200개
- **기타 지역**: 약 1,900개

### 카테고리별 분포
- **문화와 예술**: 약 800개
- **전통과 역사**: 약 600개
- **계절과 자연**: 약 500개
- **기타**: 약 1,100개

### 시기별 분포
- **봄 (3-5월)**: 약 900개
- **여름 (6-8월)**: 약 700개
- **가을 (9-11월)**: 약 800개
- **겨울 (12-2월)**: 약 600개

---

## 🔒 데이터 사용 시 주의사항

### 저작권
- TourAPI 데이터는 한국관광공사 저작권 정책 준수
- 이미지는 출처 표기 필수
- 상업적 이용 시 별도 승인 필요

### 개인정보
- 연락처, 홈페이지 등은 공개 정보만 포함
- 개인 블로그 리뷰는 익명 처리

### 데이터 품질
- NULL 값 존재 가능 (선택 항목)
- 좌표 정확도 ±50m
- 이미지 URL은 외부 링크 (변경 가능)

---

## 🤝 관련 프로젝트

- [**Frontend**](../tour_agent_frontend) - React 기반 웹 인터페이스
- [**Backend**](../tour_agent_backend) - FastAPI 기반 AI 엔진

---

## 📄 라이선스

이 프로젝트는 교육 및 연구 목적으로 개발되었습니다.

**데이터 출처**:
- 한국관광공사 TourAPI (공공 데이터)
- 자체 수집 메타데이터 (Team FestMoment)

---

## 👥 개발팀

**Team FestMoment**
- 염정운
- 최가윤

**문의**: [GitHub Issues](https://github.com/your-repo/issues)

---

**FestMoment** - AI로 축제의 감성을 재해석합니다 ✨
