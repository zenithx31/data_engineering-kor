
# [ Neo4j ] Docker에 Neo4j 설치하고 Python과 연결하는 방법

---

## 📚 Docker에 Neo4j 설치하기

아래는 Docker 위에 Neo4j를 설치하는 방법입니다.

로컬 환경에서 설치를 하려는 경우, Neo4j 공식 홈페이지에서 설치 프로그램을 다운로드 할 수 있습니다.

```bash
$ docker pull neo4j
$ docker run --publish=7474:7474 --publish=7473:7473 --publish=7687:7687 --volume=$HOME/neo4j/data:/data neo4j
```

설치를 완료한 후, **Docker Desktop**을 열면 Neo4j 컨테이너가 생성된 것을 확인할 수 있습니다.

Neo4j 브라우저는 [http://localhost:7474/](http://localhost:7474/) 에서 접속하여 확인할 수 있습니다.

- 초기 로그인 아이디: `neo4j`
- 초기 비밀번호: `neo4j`

---

## 📚 Python과 연결하기

다음으로 Python과 Neo4j를 연결합니다.

```bash
$ pip install neo4j
```

설치가 완료되면 아래 코드를 통해 설치된 버전을 확인할 수 있습니다.

```python
from neo4j import __version__ as neo4j_version
print(neo4j_version)
# 출력 예시: 5.7.0
```

> (2023년 4월 기준 최신 버전: 5.7.0)

---

다음은 Python 코드에서 Neo4j에 연결하는 예시입니다.

```python
from neo4j import GraphDatabase

driver = GraphDatabase.driver('bolt://localhost:7687', auth=('neo4j', '0000'))
# driver = GraphDatabase.driver(uri, auth=(user, password))
session = driver.session()
```

- `bolt://`는 Neo4j에서 사용하는 통신 방법인 **Bolt 프로토콜**을 의미합니다.
- `7687`은 Neo4j의 기본 포트 번호입니다.

코드를 실행한 후 [Neo4j 브라우저](http://localhost:7474/)에 접속하면 접속 정보를 확인할 수 있습니다.
