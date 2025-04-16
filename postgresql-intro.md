# 📚 PostgreSQL란?

PostgreSQL은 오픈소스 관계형 데이터베이스 관리 시스템(RDBMS)입니다.

관계형 데이터베이스는 데이터를 테이블 형식으로 저장하고, 테이블들 간에 관계를 설정하여 데이터를 연결합니다. 예를 들어, 웹 사이트에 가입할 때 "회원"과 "주문" 정보가 따로 저장되지만, 각 테이블은 서로 관계를 맺어서 정보를 효율적으로 연결하고 활용할 수 있습니다.

PostgreSQL은 ACID(원자성, 일관성, 독립성, 지속성) 속성을 지원하는 안정적이고 성능 좋은 데이터베이스 시스템으로 많은 웹 애플리케이션에서 사용됩니다.

---

# 📚 PostgreSQL 설치 방법

### 1) Homebrew 설치

Homebrew는 Mac에서 프로그램을 쉽게 설치할 수 있는 패키지 관리자입니다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2) PostgreSQL 설치

Homebrew를 이용하여 PostgreSQL 15 버전을 설치합니다.

```bash
brew install postgresql@15
```

### 3) PostgreSQL 실행

```bash
brew services start postgresql@15
```

### +) PostgreSQL 종료

```bash
brew services stop postgresql@15
```

### 4) 상태 확인

```bash
brew services list
```

실행 중이라면 위와 같이 started라는 상태가 표시됩니다.

---

# 📚 데이터베이스 사용 방법

### 1) 데이터베이스 접속

```bash
psql postgres
```

Postgres를 설치하면, 기본적으로 postgres라는 db가 생성되기 때문에 바로 접속이 가능합니다.

만약, psql 명령어를 찾을 수 없다는 오류가 발생하면 아래와 같이 환경 변수 설정을 해줍니다.

```bash
export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"
```

### 2) 새로운 사용자 계정 생성

'test0' 위치에 'YOUR_USER_NAME'을 삽입합니다.

```bash
createuser -s test0
```

### 3) 사용자 계정으로 접속

```bash
psql -U test0 postgres
```

### 4) 새로운 데이터베이스 생성

```sql
CREATE DATABASE test_db0;
```

### 5) 데이터베이스 생성 확인

```bash
\l
```

### 6) 데이터베이스 접속

```bash
\c test_db0
```

---

# 📚 테이블 사용 방법

### 1) 테이블 생성

```sql
CREATE TABLE test_table0 (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    email VARCHAR(100)
);
```

### 2) 테이블 생성 확인

```bash
\d test_table0
```

### 3) 데이터 삽입

```sql
INSERT INTO test_table0 (name, age, email) VALUES
('Tom', 25, 'tom@example.com'),
('Jeffrey', 30, 'jeffrey@example.com'),
('Gracie', 28, 'gracie@example.com');
```

### 4) 데이터 조회

```sql
SELECT * FROM test_table0;
```

---

# 📚 PostgreSQL과 Python 연결하기

psycopg2 라이브러리를 설치하여, Python에서 PostgreSQL을 사용할 수 있습니다.

### 1) 패키지 설치

```bash
pip install psycopg2
# 에러 발생 시
pip install psycopg2-binary
```

### 2) Python 코드 작성

```python
# insert_data.py

import psycopg2

# PostgreSQL 연결
conn = psycopg2.connect(
    dbname="test_db0",
    user="test0",
    # password="yourpassword",  # 비밀번호 설정했으면 입력
    host="localhost",
    port="5432"
)

cur = conn.cursor()

# test_table0 테이블에 샘플 데이터 삽입
data = [
    ("Chris", 27, "chris@example.com"),
    ("Emily", 24, "emily@example.com")
]

for user in data:
    cur.execute("INSERT INTO test_table0 (name, age, email) VALUES (%s, %s, %s)", user)

conn.commit()  # 변경 사항 저장

# 데이터 조회
cur.execute("SELECT * FROM test_table0;")
rows = cur.fetchall()

for row in rows:
    print(row)

cur.close()
conn.close()
```

---

# 📚 PostgreSQL을 활용한 미니 프로젝트 (Feat. 인기 검색어 분석 시스템)

구현 할 기능 : 사용자의 검색 기능, 인기 검색어 목록 조회 기능  
사용 할 기술 : PostgreSQL, FastAPI, psycopg2

---

### 1) 서버 시작

```bash
brew services start postgresql@15
brew services list
```

### 2) 새로운 데이터베이스 생성

```bash
psql postgres
CREATE DATABASE trend_db;
\l
```

### 3) 새로운 데이터베이스 사용

```bash
\c trend_db
```

### 4) 새로운 테이블 생성

```sql
CREATE TABLE searches (
    id SERIAL PRIMARY KEY,
    keyword VARCHAR(100) NOT NULL,
    searched_at TIMESTAMP DEFAULT NOW()
);
```

```bash
\d searches
```

### 5) Python 파일 생성

```python
# trend_db.py

import psycopg2

# PostgreSQL 연결 설정 (trend_db 사용)
DB_NAME = "trend_db"
DB_USER = "test0"
# DB_PASSWORD = "yourpassword"
DB_HOST = "localhost"
DB_PORT = "5432"

def get_connection():
    return psycopg2.connect(
        dbname=DB_NAME,
        user=DB_USER,
        # password=DB_PASSWORD,
        host=DB_HOST,
        port=DB_PORT
    )
```

---

### 6) FastAPI 설치

```bash
pip install fastapi
```

### 7) FastAPI와 PostgreSQL 연결

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

### 8) uvicorn 설치

```bash
pip install uvicorn
```

### 9) uvicorn 실행

```bash
uvicorn app:app --reload
```

### 10) 요청 테스트

```bash
curl -X 'POST' 'http://127.0.0.1:8000/search?keyword=Python'
curl -X 'GET' 'http://127.0.0.1:8000/trending'
```

---

### 11) 저장 기능과 중복 저장 방지 기능 추가 (기존 main.py 수정)

```python
# main.py

from fastapi import FastAPI

app = FastAPI()

# 인기 검색어 목록을 저장할 리스트
trending_keywords = ["Python", "FastAPI", "PostgreSQL"]

# 검색어 추가 API (POST)
@app.post("/search")
def add_search(keyword: str):
    if keyword not in trending_keywords:
        trending_keywords.append(keyword)  # 중복된 검색어가 없으면 추가
    return {"message": f"검색어 '{keyword}'이 저장되었습니다."}

# 인기 검색어 조회 API (GET)
@app.get("/trending")
def get_trending():
    return {"trending_keywords": trending_keywords}
```

### 12) 수정 후 재실행

```bash
uvicorn main:app --reload
```

### 13) 'JavaScript' 저장 테스트

```bash
curl -X 'POST' 'http://127.0.0.1:8000/search?keyword=JavaScript'
curl -X 'GET' 'http://127.0.0.1:8000/trending'
```

이번에는 'JavaScript'가 정상적으로 저장되었습니다.
