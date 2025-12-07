# political-tracker
정치인 추적 프로젝트
# Political Tracker

> 정치인의 발언과 입장 변화를 추적하여 정치적 투명성을 높이는 시스템

## 📋 프로젝트 소개

정치인들의 발언과 입장 변화를 체계적으로 추적하고 분석하여, 유권자들이 정치인의 일관성을 쉽게 확인할 수 있도록 돕는 플랫폼입니다.

### 프로젝트 배경
정치인들이 시간이 지남에 따라 입장을 바꾸는 경우가 많지만, 이를 체계적으로 추적하고 비교하기 어려운 문제를 해결하고자 시작되었습니다.

### 핵심 가치
- ✅ **정치적 투명성**: 정치인의 과거 발언과 현재 입장을 한눈에 비교
- ✅ **데이터 기반**: 뉴스, 입법활동, 국회 기록 등 객관적 자료 활용
- ✅ **추적 가능성**: 모든 발언의 출처와 시점을 명확히 기록

---

## 🎯 주요 기능

### 1. 뉴스 발언 추적
```
뉴스 크롤링 → 저장 → 태그 추출 → 발언 분석 → 태도 비교 → 웹 게시
```
- 네이버 뉴스 등 주요 언론사 기사 자동 수집
- KeyBERT를 활용한 키워드 자동 추출
- 정치인별, 주제별 발언 분류

### 2. 입법 활동 추적
```
국회 입법 크롤링 → 저장 → 요약 → 태도 비교 → 변화 게시
```
- 법안 발의, 찬반 투표 기록 수집
- 기권 → 찬성/반대 변화 감지
- 입법 활동 이력 관리

### 3. 정치 활동 추적
```
활동 크롤링 → 저장 → 태그 추출 → 태도 비교 → 변화 게시
```
- SNS 활동 모니터링 (선택적)
- 공식 보도자료 수집
- 국회 의정활동 기록

### 4. 태도 변화 감지
- AI 보조 + 수동 검증 방식
- 주제별 입장 변화 타임라인
- 180도 전환, 점진적 변화 등 분류

---

## 🛠 기술 스택

### Backend
- **Language**: Java 17+
- **Framework**: Spring Boot 3.x
- **ORM**: JPA/Hibernate
- **Database**: MySQL 8.0+
- **Build Tool**: Gradle/Maven

### Frontend
- **Framework**: React 18+
- **Language**: JavaScript/TypeScript
- **Styling**: Tailwind CSS / Material-UI

### Crawling & NLP
- **Language**: Python 3.9+
- **Crawling**: 
  - BeautifulSoup4
  - Requests
  - Newspaper3k (선택적)
- **NLP/AI**:
  - KeyBERT (키워드 추출)
  - KoBERT (개체명 인식)
  - OpenAI GPT-4 (발언 분석 보조)
  - Konlpy (형태소 분석)

### Search
- **Engine**: Elasticsearch 8.x + Nori Analyzer
- **Alternative**: SQLite FTS5 (초기 단계)

---

## 🏗 시스템 아키텍처

```
┌─────────────────────────────────────────────────────────┐
│                   사용자 인터페이스                        │
│                  (React Web App)                        │
└──────────────────┬──────────────────────────────────────┘
                   │
┌──────────────────▼──────────────────────────────────────┐
│               Backend API Server                        │
│              (Spring Boot + JPA)                        │
└──────────────────┬──────────────────────────────────────┘
                   │
     ┌─────────────┼─────────────┐
     │             │             │
┌────▼────┐  ┌────▼────┐  ┌────▼────────┐
│  MySQL  │  │Elastic  │  │   Python    │
│Database │  │ search  │  │Crawler+NLP  │
└─────────┘  └─────────┘  └─────────────┘
```

---

## 💾 데이터베이스 설계

### 핵심 테이블 구조

#### 1. 정치인 (politicians)
```sql
- 정치인_id (PK)
- 이름
- 생년월일
- 현재_소속_정당
- 활동_상태 (현역/은퇴/사망)
- SNS URLs
```

#### 2. 뉴스 (news)
```sql
- 뉴스_id (PK)
- 제목, 본문
- URL (UNIQUE)
- 언론사, 발행_날짜
- 상태 (정상/삭제됨/수정됨)
```

#### 3. 발언_입장 (statements)
```sql
- 입장_id (PK)
- 정치인_id (FK)
- 주제_id (FK)
- 태도 (강한찬성/찬성/중립/반대/강한반대/기권)
- 발언_요약
- 발언_날짜
- 근거_뉴스_id (FK)
```

#### 4. 주제 (topics)
```sql
- 주제_id (PK)
- 주제_이름
- 상위_주제_id (FK, 계층 구조)
- 계층_레벨
- 동의어
```

#### 5. 태도_변화 (stance_changes)
```sql
- 변화_id (PK)
- 정치인_id (FK)
- 주제_id (FK)
- 이전_입장_id (FK)
- 현재_입장_id (FK)
- 변화_강도 (180도전환/중대변화/미묘한변화)
- 공개_여부
```

### ERD 주요 관계
- 정치인 ↔ 뉴스: **Many-to-Many** (정치인_뉴스_매핑)
- 뉴스 ↔ 태그: **Many-to-Many** (뉴스_태그)
- 정치인 ↔ 입법: **Many-to-Many** (정치인_입법_참여)
- 주제: **Self-Referencing** (계층 구조)

---

## 🚀 설치 및 실행

### 1. 사전 요구사항
```bash
- Java 17+
- Python 3.9+
- MySQL 8.0+
- Node.js 18+
```

### 2. 데이터베이스 설정
```sql
CREATE DATABASE political_tracker CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 3. Python 크롤러 설정
```bash
cd crawler
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt --break-system-packages
```

### 4. Backend 실행
```bash
cd backend
./gradlew bootRun
```

### 5. Frontend 실행
```bash
cd frontend
npm install
npm start
```

---

## 📊 개발 로드맵

### Phase 1: MVP (2-3개월) ✅ 진행 중
- [x] 데이터베이스 설계 완료
- [x] 네이버 뉴스 크롤러 개발
- [ ] KeyBERT 키워드 추출 구현
- [ ] 기본 웹 UI
- [ ] 수동 태도 라벨링 시스템

### Phase 2: 자동화 확대 (3-6개월)
- [ ] 정치인 자동 인식 (NER)
- [ ] 감성 분석 (긍정/부정)
- [ ] 주제 자동 분류
- [ ] Elasticsearch 검색 엔진

### Phase 3: 고도화 (6개월+)
- [ ] GPT-4 기반 태도 분석
- [ ] 문맥 이해 및 모순 탐지
- [ ] 입법 활동 자동 분석
- [ ] 사용자 제보 시스템

---

## 🔍 핵심 설계 결정

### 1. 반자동 시스템 선택
**완전 자동화가 아닌 이유:**
- 정치적 뉘앙스와 맥락 이해는 AI만으로 부족
- 오판 시 법적 리스크 (명예훼손)
- 사람의 최종 검증이 정확도와 신뢰성 보장

**워크플로우:**
```
AI 분석 → 인간 검증 → 공개
```

### 2. 주제 계층 구조
```
경제 (최상위)
└── 노동
    └── 최저임금
        ├── 최저임금 인상
        └── 최저임금 동결
```

**장점:**
- 유연한 검색 (상위 주제 검색 시 하위 모두 포함)
- 통계 집계 용이
- 의미적 그룹핑

**단점:**
- 다중 상위 주제 필요 시 복잡
- 순환 참조 방지 필요
- 성능 고려 (재귀 쿼리)

### 3. Many-to-Many 설계
한 뉴스에 여러 정치인 등장 가능 → 매핑 테이블 필수

---

## 📝 라이선스

MIT License

---

## 🤝 기여 방법

이슈 제보와 Pull Request를 환영합니다!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## ⚠️ 면책 조항

본 프로젝트는 공개된 자료를 기반으로 정치인의 발언을 추적하며, 모든 내용은 출처가 명시됩니다. 
정치적 중립을 유지하며, 특정 정당이나 정치인을 지지/비판하지 않습니다.

---

## 📧 문의

프로젝트에 대한 문의사항이 있으시면 Issue를 열어주세요.

---

**Made with ❤️ for Political Transparency**
