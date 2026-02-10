# 📝 회의록 요약 및 맞춤형 뉴스 추천 시스템  
### Contextual News Recommender  
**2024 소프트웨어 아키텍처 10조**

[발표자료](https://github.com/zrp0x0/Contextual-News-Recommender/blob/main/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98%20%EA%B8%B0%EB%A7%90%20FULL%20%EB%B0%9C%ED%91%9C%20%EC%9E%90%EB%A3%8C%2010%ED%8C%80%20.pdf)

---

## 👥 팀 구성

- **팀장**: 신홍규  
  - Backend Architecture & Implementation

- **팀원**: 곽인경  
  - UI/UX, Frontend Integration

- **팀원**: 추정민  
  - UI/UX, Frontend Integration

---

## 📖 프로젝트 개요 (Overview)

**Contextual News Recommender**는 기업 회의록을 분석하여  
AI 기반 요약 및 키워드 추출을 수행하고,  
이를 바탕으로 가장 연관성 높은 최신 뉴스를 추천하는 **지능형 시스템**입니다.

단순 키워드 매칭이 아닌,  
**S-BERT(Sentence-BERT)** 기반 문맥 유사도 분석을 통해  
회의 내용과 실제 비즈니스 인사이트 간의 연결을 제공합니다.

---

## 🎯 핵심 목표

- **회의록 자동화**
  - LLM(Google Gemini)을 활용한 핵심 요약 및 주요 키워드 추출

- **문맥 기반 뉴스 추천**
  - 회의 문맥(Context)과 뉴스 본문 간 코사인 유사도 분석을 통한 정밀 추천

- **비동기 처리**
  - 대용량 크롤링 및 AI 연산 병목 방지를 위한  
    Celery & Redis 기반 분산 처리 아키텍처

---

## 🚀 핵심 기능 (Key Features)

### 1️⃣ 회의록 분석 및 요약 (Meeting Analysis)

- 사용자가 입력한 회의록을 **Google Gemini Pro** 모델을 통해 요약
- 회의를 관통하는 **Top 5 핵심 키워드 자동 추출**

---

### 2️⃣ 문맥 기반 뉴스 추천 (Contextual News Recommendation)

1. **Google Custom Search API**
   - 키워드 기반 최신 뉴스 URL 1차 수집

2. **Newspaper3k**
   - 뉴스 본문 크롤링

3. **S-BERT (paraphrase-multilingual-MiniLM-L12-v2)**
   - 회의 요약본과 뉴스 본문 임베딩 생성
   - **Cosine Similarity** 계산
   - 가장 연관성 높은 뉴스 **상위 5개 추천**

---

### 3️⃣ 고성능 비동기 아키텍처 (High Performance Architecture)

- **FastAPI**
  - Async / Await 기반 비동기 API 처리

- **Celery + Redis**
  - 무거운 AI 연산(GPU)과 I/O 작업(크롤링)을 큐로 분리
  - CPU / GPU 작업 분리 처리 (Queue Separation)

---

### 4️⃣ 사용자 관리 (User Management)

- 이메일 기반 회원가입
- 로그인 / 로그아웃 / 세션 관리
- 사용자별 회의록 저장 및 관리 (CRUD)

---

## 🛠 기술 스택 (Tech Stack)

### Backend
- FastAPI
- Python 3.10+
- SQLAlchemy (Async)

### Infrastructure & DevOps
- Celery
- Redis
- MySQL

### AI & Core Logic
- Google Gemini API
- S-BERT (Sentence Transformers)
- Newspaper3k
- Google Custom Search API

---

## 🏛 시스템 아키텍처 (System Architecture)

본 프로젝트는 **유지보수성과 확장성**을 고려하여  
**Layered Architecture**와 다양한 디자인 패턴을 적용했습니다.

---

### 🏗 Layered Architecture

- **Presentation Layer**
  - API Router
  - 요청 / 응답 처리

- **Service Layer**
  - 비즈니스 로직
  - 트랜잭션 관리
  - Celery 작업 호출

- **Domain Layer**
  - 핵심 엔티티(Model)
  - 인터페이스 정의

- **Infrastructure Layer**
  - 외부 API (Gemini, Google Search)
  - DB, AI 모델 구현체
  - Adapter Pattern 적용

---

### 🧩 적용된 디자인 패턴

- **Adapter Pattern**
  - 외부 서비스(LLM, Search) 교체 용이

- **Singleton Pattern**
  - DB Connection Pool
  - S-BERT 모델 로딩 최적화

- **Repository Pattern**
  - 데이터 접근 로직과 비즈니스 로직 분리

---

## 💻 설치 및 실행 (Installation & Run)

### 1️⃣ 환경 변수 설정 (.env)

```ini
# Database
DB_CONN=mysql+aiomysql://root:password@localhost:3306/meetings_db

# Redis
REDIS_URL=redis://localhost:6379/0

# API Keys
GEMINI_API_KEY=your_gemini_api_key
CUSTOM_SEARCH_JSON_API_KEY=your_google_search_api_key
CUSTOM_SEARCH_ENGINE_API_KEY=your_search_engine_id

# Security
SECRET_KEY=your_secret_key
```

---

## 프로젝트 구조
```
📦 Contextual-News-Recommender
├── 📂 api               # API Routers & Dependencies
├── 📂 core              # DB Connection, Template Config
├── 📂 domain            # Models(DTO), Interfaces
├── 📂 infrastructure    # DB Repositories, External Adapters
├── 📂 services          # Business Logic
├── 📂 templates         # Jinja2 HTML Templates
├── 📂 utils             # Middleware, Exception Handlers
├── 📜 celery_worker.py  # Celery Task Definitions
├── 📜 main.py           # Application Entry Point
└── 📜 requirements.txt  # Project Dependencies
```
