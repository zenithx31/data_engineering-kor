# 📊 PostgreSQL 설치 및 활용 가이드 (feat. 인기 검색어 분석 시스템)

## 📌 PostgreSQL이란?

**PostgreSQL**은 오픈소스 관계형 데이터베이스 관리 시스템(RDBMS)입니다.  
데이터를 테이블 형식으로 저장하고, 테이블 간 관계를 설정하여 효율적으로 데이터를 연결합니다.  
안정적이고 성능 좋은 ACID 지원 시스템으로, 다양한 웹 애플리케이션에서 사용됩니다.

---

## ⚙️ PostgreSQL 설치 방법 (macOS 기준)

### 1. Homebrew 설치

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. PostgreSQL 설치

```bash
brew install postgresql@15
```

### 3. PostgreSQL 실행

```bash
brew services start postgresql@15
```

### ➕ 종료 / 상태 확인

```bash
brew services stop postgresql@15     # 종료
brew services list                   # 상태 확인
```

---

## 🧭 PostgreSQL 기본 사용법

### 1. DB 접속

```bash
psql postgres
```

> 접속 오류 시 환경 변수 설정:

```bash
export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"
```

### 2. 사용자 생성 및 접속

```bash
createuser -s test0              # 사용자 생성
psql -U test0 postgres           # 해당 사용자로 접속
```

### 3. DB 생성 및 접속

```sql
CREATE DATABASE test_db0;
\l          -- DB 리스트 확인
\c test_db0 -- DB 접속
```

---

## 🧱 테이블 생성 및 사용

### 1. 테이블 생성

```sql
CREATE TABLE test_table0 (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    email VARCHAR(100)
);
```

### 2. 데이터 삽입

```sql
INSERT INTO test_table0 (name, age, email) VALUES
('Tom', 25, 'tom@example.com'),
('Jeffrey', 30, 'jeffrey@example.com'),
('Gracie', 28, 'gracie@example.com');
```

### 3. 데이터 조회

```sql
SELECT * FROM test_table0;
```

---

## 🐍 PostgreSQL과 Python 연동

### 1. 패키지 설치

```bash
pip install psycopg2
# 또는
pip install psycopg2-binary
```

### 2. 데이터 삽입 및 조회 예제

```python
# insert_data.py

import psycopg2

conn = psycopg2.connect(
    dbname="test_db0",
    user="test0",
    host="localhost",
    port="5432"
)

cur = conn.cursor()

data = [
    ("Chris", 27, "chris@example.com"),
    ("Emily", 24, "emily@example.com")
]

for user in data:
    cur.execute("INSERT INTO test_table0 (name, age, email) VALUES (%s, %s, %s)", user)

conn.commit()

cur.execute("SELECT * FROM test_table0;")
rows = cur.fetchall()
for row in rows:
    print(row)

cur.close()
conn.close()
```

---

## 🚀 미니 프로젝트: 인기 검색어 분석 시스템

### 사용 기술

- PostgreSQL
- FastAPI
- psycopg2

---

### 📦 데이터베이스 준비

```bash
psql postgres
CREATE DATABASE trend_db;
\c trend_db
```

```sql
CREATE TABLE searches (
    id SERIAL PRIMARY KEY,
    keyword VARCHAR(100) NOT NULL,
    searched_at TIMESTAMP DEFAULT NOW()
);
```

---

### 🧩 DB 연결 함수

```python
# trend_db.py

import psycopg2

DB_NAME = "trend_db"
DB_USER = "test0"
DB_HOST = "localhost"
DB_PORT = "5432"

def get_connection():
    return psycopg2.connect(
        dbname=DB_NAME,
        user=DB_USER,
        host=DB_HOST,
        port=DB_PORT
    )
```

---

### 🌐 FastAPI 앱

```bash
pip install fastapi uvicorn
```

```python
# main.py

from fastapi import FastAPI

app = FastAPI()

trending_keywords = ["Python", "FastAPI", "PostgreSQL"]

@app.post("/search")
def add_search(keyword: str):
    if keyword not in trending_keywords:
        trending_keywords.append(keyword)
    return {"message": f"검색어 '{keyword}'이 저장되었습니다."}

@app.get("/trending")
def get_trending():
    return {"trending_keywords": trending_keywords}
```

---

### ▶️ 실행

```bash
uvicorn main:app --reload
```

---

### ✅ 테스트

```bash
curl -X 'POST' 'http://127.0.0.1:8000/search?keyword=JavaScript'
curl -X 'GET' 'http://127.0.0.1:8000/trending'
```

---

## ✅ 마무리

> 위 예제는 로컬 메모리에 저장되며, 추후 PostgreSQL `searches` 테이블을 활용하도록 확장할 수 있습니다.  
> 실시간 인기 검색어 분석, 조회수 기반 순위, 시간별 트렌드 분석 등 다양한 기능으로 발전시켜 보세요.