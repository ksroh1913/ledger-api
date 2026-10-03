# W4 PostgreSQL · Supabase 실습

- GitHub: https://github.com/ksroh1913/ledger-api
- Render: https://ledger-api-r4zp.onrender.com
- Swagger UI: https://ledger-api-r4zp.onrender.com/docs

## 1. 실습 결과 확인

Render에 배포한 FastAPI와 Supabase PostgreSQL을 연결

Render Swagger UI에서 계좌를 생성한 후 Supabase Table Editor를 확인한 결과,
`월급통장` 계좌가 실제 `accounts` 테이블에 저장된 것을 확인

### Supabase 저장 결과

![Supabase accounts](supabase_accounts.png)

또한 계좌와 거래의 1:N 관계를 확인하기 위해
`GET /accounts/1/detail`을 실행

`월급통장` 계좌에 다음 두 거래가 연결되어 있는 것을 확인

- 식비: 점심 -12,000원
- 교통: 지하철 -1,500원

### 계좌 상세 조회 결과

![Account detail](account_detail.png)

`GET /stats/by-category`를 실행하여 카테고리별 지출을 집계

- 식비: -12,000원 / 1건
- 교통: -1,500원 / 1건

### 카테고리별 집계 결과

![Category stats](stats_by_category.png)

## 2. 핵심 개념 되새김

### 계좌와 거래를 두 테이블로 나눈 이유

하나의 계좌에는 여러 거래가 발생할 수 있으므로,
계좌와 거래를 별도의 테이블로 만들고 `account_id`를 이용하여
1:N 관계로 연결

### SQLAlchemy 모델과 실제 테이블의 관계

SQLAlchemy의 모델 클래스는 데이터베이스의 테이블 구조를
파이썬 코드로 정의한 것
FastAPI에서는 파이썬 객체를 사용하지만,
SQLAlchemy가 이를 SQL로 변환하여 PostgreSQL에서 실행

### DB 접속 문자열을 .env로 분리하는 이유

DB 접속 문자열에는 서버 주소와 인증정보가 포함될 수 있으므로
소스 코드에 직접 저장하지 않음
로컬에서는 `.env`, Render에서는 Environment Variables를 사용하여
코드와 접속정보를 분리

## 3. 자유 로그

이번 실습에서는 Render에 배포된 FastAPI가
Supabase PostgreSQL에 실제 데이터를 저장하고 조회하는 과정을 확인

처음에는 Swagger UI가 단순한 테스트 화면이라고 생각했지만,
`POST /accounts`를 실행하면 실제 FastAPI API가 호출되고
Supabase 데이터베이스에 데이터가 저장된다는 점을 이해

또한 `accounts`, `transactions`, `categories`를 각각 분리하고
기본키와 외래키를 이용하여 서로 연결하는 관계형 데이터베이스의 구조를
직접 확인

### AI 활용 및 검증

ChatGPT를 이용하여 W4 과제의 구조와 실습 순서를 확인함
AI가 안내한 내용은 Render Swagger UI의 실제 응답과
Supabase Table Editor에 저장된 데이터를 직접 확인하여 검증함
