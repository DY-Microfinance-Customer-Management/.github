# DY Microfinance Customer Management System

미얀마 마이크로파이낸스 고객 관리 시스템 개발 프로젝트입니다.

---

## 프로젝트 개요

미얀마 마이크로파이낸스 시장의 특수성을 반영한 대출 계산 및 고객 관리 시스템입니다.

### 핵심 특징

- **유연한 상환 주기**: 월별/4주/2주/주간 상환 방식을 모두 지원
- **정확한 이자 계산**: 실제 일수 기반의 정밀한 대출 이자 계산
- **3가지 상환 방식**: 원리금 균등상환, 원금 균등상환, 만기일시상환
- **풀스택 구조**: Next.js 프론트엔드 + Nest.js 백엔드 + PyQt5 알고리즘 엔진

---

## 레포지토리 구조

### 🎨 [dymf-front](https://github.com/DY-Microfinance-Customer-Management/dymf-front)
**Frontend - Next.js + TypeScript**

- 고객 및 대출 정보 조회·입력·관리 화면
- 대출 상환 스케줄 시각화
- Vercel 배포: [dymf-front.vercel.app](https://dymf-front.vercel.app)

**기술 스택:**
- Next.js 14 (App Router)
- TypeScript
- Tailwind CSS
- React Query

**주요 기능:**
- 고객 정보 CRUD
- 대출 신청 및 승인 프로세스
- 상환 스케줄 조회 및 출력
- 대시보드 및 통계 리포트

---

### ⚙️ [dymf-back](https://github.com/DY-Microfinance-Customer-Management/dymf-back)
**Backend - Nest.js + PostgreSQL**

- RESTful API 서버
- 고객/대출 데이터 관리
- 인증 및 권한 관리
- 대출 알고리즘 API 통합

**기술 스택:**
- Nest.js
- PostgreSQL
- TypeORM
- JWT Authentication
- Docker

**API 엔드포인트:**
```
GET    /customers              # 고객 목록 조회
POST   /loans                  # 대출 신청
GET    /loans/:id/schedule     # 상환 스케줄 조회
PATCH  /loans/:id/payment      # 상환 처리
```

---

### 🧮 [dymf-algorithm](https://github.com/DY-Microfinance-Customer-Management/dymf-algorithm)
**Algorithm Engine - Python + PyQt5**

미얀마 금융 시장 특성을 반영한 대출 이자 계산 알고리즘 구현

**핵심 구현: cycle과 period 분리 계산**

일반적인 대출 시스템은 월별 상환만 지원하지만, 본 시스템은:

```python
cycle: str = ['month', '4week', '2week', 'week']

# 연간 상환 횟수 자동 계산
if cycle == 'month':
    cycle_cnt = 12
elif cycle == '4week':
    cycle_cnt = 13  # 52주 / 4주
elif cycle == '2week':
    cycle_cnt = 26
elif cycle == 'week':
    cycle_cnt = 52

# 실제 상환 횟수는 일수 기반으로 정확히 계산
total_period = math.ceil(total_days / cycle_days)

# 주기별 이자율 자동 변환
period_interest_rate = annual_interest_rate / cycle_cnt
```

**지원 알고리즘:**

| 알고리즘 | 영문명 | 특징 |
|---------|--------|------|
| 원리금 균등상환 | Equal Payment Loan | 매 주기 동일 금액 상환 |
| 원금 균등상환 | Equal Principal Payment | 원금 고정, 이자 감소 |
| 만기일시상환 | Bullet Payment | 이자만 납부 후 만기 일시 상환 |

**기술 스택:**
- Python 3.10+
- PyQt5 (Desktop UI)
- Pandas (데이터 처리)
- Jupyter Notebook (알고리즘 검증)

**주요 파일:**
- `algorithm/loan.ipynb`: 알고리즘 정의 및 수학적 증명
- `app/main.py`: PyQt5 애플리케이션 진입점

---

## 팀 구성

| 역할 | 이름 | 담당 |
|------|------|------|
| Backend | 모진영 | 프론트 개발, DB 설계 |
| Backend | 박훈일 | 인증/권한, 알고리즘 통합 |

---

## 시스템 아키텍처

```
[Client Browser]
       ↓
[dymf-front (Next.js)]
       ↓ REST API
[dymf-back (Nest.js)]
       ↓
[PostgreSQL Database]
       ↓
[dymf-algorithm (Python)]
```

---

## 주요 기능

### ✅ 고객 관리
- 고객 정보 등록/수정/삭제
- 고객별 대출 이력 조회
- CSV 파일 일괄 등록

### ✅ 대출 관리
- 대출 신청 및 심사
- 상환 방식 선택 (3가지 알고리즘)
- 상환 주기 선택 (월/4주/2주/주간)
- 자동 상환 스케줄 생성

### ✅ 상환 관리
- 회차별 상환 처리
- 연체 관리 및 추적
- 조기 상환 처리

### ✅ 통계 및 리포트
- 대출 현황 대시보드
- 상환율 통계
- 연체율 분석

---

## 개발 환경 설정

### 전체 시스템 실행

```bash
# 1. Backend 실행
cd dymf-back
npm install
docker-compose up -d  # PostgreSQL 시작
npm run start:dev

# 2. Frontend 실행
cd dymf-front
npm install
npm run dev

# 3. Algorithm 검증 (선택사항)
cd dymf-algorithm
pip install -r requirements.txt
jupyter notebook
```

---

## 기술적 하이라이트

### 1. 유연한 상환 주기 지원

기존 시스템들은 월별 상환만 지원하지만, 본 시스템은 미얀마 시장 특성을 반영하여:
- **일수 기반 정확한 계산**: `math.ceil(total_days / cycle_days)`
- **주기별 이자율 자동 변환**: `annual_rate / cycle_cnt`
- **동적 날짜 증가**: `relativedelta(months=1)` 또는 `relativedelta(weeks=n)`

### 2. 타입 안전성

- Frontend: TypeScript 100% 적용
- Backend: TypeScript + TypeORM entity 타입 정의
- API 통신: DTO 기반 타입 검증

### 3. 확장 가능한 아키텍처

- Backend: Nest.js 모듈 시스템
- Frontend: Next.js App Router + Server Components
- Algorithm: Python 클래스 기반 OOP 설계

---

## 배포

- **Frontend**: Vercel (자동 배포)
- **Backend**: Docker Compose (Production 환경)
- **Database**: PostgreSQL (Docker)

---

## 라이선스

Copyright 2024 DY Microfinance. All rights reserved.
