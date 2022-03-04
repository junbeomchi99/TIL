# FastAPI - DB 연동 (ORM)

# ORM
> Object Relational Mapping의 약자. 
> = 데이터베이스의 내용 (table, row)등을 프로그래밍 언어의 객채로 취급하여 다루는 개념
- = 객체와 관계현 데이터베이스를 자동으로 매핑(연결) 해주는 프레임워크
- (= Entity와 table들의 관계를 연결시켜주는 툴)
- 예: SQL 테이블 `pets` 를 파이썬 클래스 `Pet` 으로 맵핑시
    - `pet = Pet()` 이라고 가정하고,
    - `pet.type` -> SQL pets 테이블의 `type` 컬럼 접근 가능
    - `pet.owner` -> SQL pets 테이블의 `owner` 컬럼 접근 가능

# Python 대표적 ORM 라이브러리
> Django-ORM, SQLAlchemy, TOrtoiseORM, Peewee가 있음
- `Django-ORM`: 장고 프레임워크에 내장된 ORM 으로 FastAPI 개발시 사용할 일 X
- `SQLAlchemy`: 파이썬의 가장 표준적인 ORM (FastAPI와 제일 자주 사용되는 것 같음)
- `Peewee`: 가볍고 간단한 기능을 제공하는 ORM, sqlite/ mysql / postgresql / cockroachdb 만 지원함
- `Tortoise ORM`: 등장한지 얼마 안된 ORM이지만 [비동기 처리](https://github.com/junbeomchi99/TIL/blob/main/%EB%8F%99%EA%B8%B0%20vs%20%EB%B9%84%EB%8F%99%EA%B8%B0.md) 에 적합
    - SQLAlchemy에서는 아직 비동기 처리가 알파단계 (= 제한적인 기능 제공)
    - DB와 비동기 통신을 위해서는 Tortoise ORM 사용 추천 

[SQLAlchemy Tutorial (한국어)](https://soogoonsoogoonpythonists.github.io/sqlalchemy-for-pythonist/tutorial/1.%20%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC%20%EA%B0%9C%EC%9A%94.html#%E1%84%80%E1%85%A2%E1%84%8B%E1%85%AD)

[SqlAlchemy Docs (영어 자료)](https://docs.sqlalchemy.org/en/14/orm/index.html)

[FastAPI SQL Databases Tutorial (영어)](https://fastapi.tiangolo.com/tutorial/sql-databases/)


# SQLAlchemy를 이용한 FastAPI DB Connection
링크: https://velog.io/@___pepper/Fast-api-SqlAlchemy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-DB-%EC%97%B0%EA%B2%B0
## 필수 설치 요소
```python
pip3 install fastapi
# FastAPI 사용을 위한 설치

pip3 install uvicorn
# 파이썬 서버실행기인 uvicorn 설치
# 순수 파이썬으로 작성된 uvicorn이 설치된다

pip3 install 'uvicorn[standard]'
# Python과 C / C++ 로 작성된 uvicorn이 설치된다

pip3 install sqlalchemy
# ORM을 통하여 DB 쿼리문을 작성하기 위해 설치

pip3 install python-dotenv
# DB관련 정보를 입력할 때, 환경변수를 통하여 내용을 입력하기 위해 dotenv 설치
```

## 폴더 구조
```
└── app
    ├── __init__.py
    ├── apis
    │   ├── __init__.py
    │   └── test.py
    ├── core
    │   ├── __init__.py
    │   └── config.py
    ├── crud
    │   ├── __init__.py
    │   └── crud_test.py
    ├── db
    │   ├── __init__.py
    │   ├── connection.py
    │   ├── models
    │   │   ├── __init__.py
    │   │   └── test_model.py
    │   └── session.py
    ├── main.py
    └── routes
        ├── __init__.py
        └── test.py

```
- apis = __메인 로직__ 을 작성하는 부분 (MVC 패턴의 Controller 같은 역활)
- core = core의 config.py에 DB URL를 반환하는 클래스가 포함되어 있음
- crud = ORM 형식으로 DB 쿼리문이 작성된 파일들의 모여있는 폴더
- db = __DB 연결, DB 세션 관리, 모델들을 정의__ 한 폴더
- routes = URL 경로에 따라 api 함수들을 나누어주는 역활
- main.py = __FastAPI를 실행__ 하고 routes폴더들의 다른 라우트들을 불러와 포함시키는 역활
- __init__.py = __해당 폴더가 패키지의 일부라는 것을 나타낸다__. 해당 파일에 작성 되어야 할 내용은 없음. (Python 3.3 버전 이후에는 해당 파일이 존재하지 않아도 패키지 일부로 인식할수 있지만 하위 번전은 Python과 호환을 위해 만들어주는것 권장)


# 연결 생성하기

1. connection 생성 (mapping 선언)
``` python
from sqlalchemy.orm import declarative_base
base = declarative_base()
```

2. 연결
- session 생성 -> engine 연결

``` python
from sqlalchemy import create_engine
# echo를 true로 설정하면 command창에 실행된 sql문이 뜨게 됨
engine = create_engine([사용할 sql의 engine(dsn)], echo = True)
# dsn? 데이터 원본 이름 : 데이터베이스에 대한 연결을 설정하도록 함
# echo를 True로 설정하면 sql log가 찍히게 됨
```
- postgreSQL 사용 시 engine (psycopg2-binary(sql engine) 설치가 필요함)

``` python
dsn = f"postgresql+psycopg2://{db_user}:{db_password}@{db_host}:{db_port}/{db_database}"
```
- sqlite 사용 시 engine

``` python
dsn = "sqlite://db.sqlite"
```

3. 생성할 모델 (테이블) 만들기
- 생성한 Base를 기반으로 모델을 만들어냄
- __tablename__ 을 필수로 선언해야됨

``` python
from sqlalchemy import Column, Integer, String, DateTime, ForeignKey, Boolean
from sqlalchemy.orm import relationship

from board.domain.board import PostInfo
from board.repositories.base import Base

from datetime import datetime

from board.utils import ToEntity


class DBUser(Base):
    __tablename__ = 'users' # 필수적으로 선언 -> table의 이름

    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)
    email = Column(String, nullable=False)
    password = Column(String, nullable=False)
    inactivate = Column(Boolean, default=False)
```

- relationship 생성
= relationship이 작성된 쪽이 다른 쪽을 참조

```python
from sqlalchemy.orm import relationship

#relation을 사용할 model 내부에
relationship(연결할 model, [options,,,,,])
#options 
#ex. back_populate : relation에서 연결된 부분에서 접근할시의 이름
#order by : model명.id -> id순으로 정렬
```

4. schema 생성
- 데이터베이스 초기 생성시 create_all 을 사용하여 생성
- 최초 한 번만 create_all 을 사용
```python 
Base.metadata.create_all(engine) #1.에서 만든 base와 2.에서 만든 engine을 사용
```
- 이미 존재하는 database와 연결시 create_all이 아닌 bind를 사용
```python
Base.metadata.bind = engine
```

5. session 생성

```python
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)
#혹은
#Session = sessionmaker()
#Session.configure(bind=engine)
```
- context class를 사용하면 session의 관리를 더 수월하게 할 수 있음
- - __enter__ : with문이 시작될 때 실행되고 return하는 값이 변수에 들어감
- - __exit__: with문 종료 시 실행 된다.
``` python
class SessionContext:
	session = None
    
    def __enter__(self):
    	self.session = Session()
        return self.session
        
        
   def __exit__(self):
   	self.session.close()
    
    
with SessionContext() as session:
	# DB 작업 작성
```

# 사용하기

(위에 생성한 모델을) with문으로 session 생성 후 실행

CRUD API (create, read, update, delete)
- create: table에 정보 넣기
```python
def create():
	ed_user = User(name='ed', fullname='Ed Jones', nickname='edsnickname')
	session.add(ed_user) #값 add하고
	#commit을 하면 data가 table에 삽입됨
	session.commit()

def create_all():
	session.add_all([User(....), User(....), ......]) #한 번에 여러값을 삽입
```

- read: table에 있는 정보 불러오기
```python
def get():
	user1.session.query(User).first() #User table의 첫번째 값을 불러옴

	#filter를 사용해 원하는 값을 불러올 수 있음
	session.query(User).filter(User.name.in_(['Edwardo', 'fakeuser'])).all()

def get_all():
	session.query(User, User.name).all() #User 객체와 User 중 이름만을 select함
```
- update: table에 있는 정보 update
```python
def update(item_id):
	# 해당하는 아이템 찾아오기
	item = session.query(User).filter(User.id==item_id).first()
    	# 값 변경 진행
        item = .....
        # DB에 갱신 값 저장
        session.add(item)
        session.commit()
```
- delete: table에 있는 정보 삭제
```python
def delete(item_id):
	session.query(User).filter(User.id==item_id).delete()
    session.commit
```

# filter_by vs filter
두개 함수 모두 filtering을 위해 사용됨
- filter_by: 결과물 filtering
- filter: filter_by보다 좀 더 유연한 SQL 표현을 사용할 수 있음
- - filter 연산자 사용가능
- - - equals: ==
- - - not equals: !=
- - - like: User.name.like('%ed%')
- - - match: User.name.match('wendy')

# pagination
offset과 limit을 이용한 paging

```python
with MakeSession() as session:
	query = session.query(DBPost)
    
    # pagination 진행 안하는 경우
    if not page:
    	posts = query.all()
    
    # pagination 진행
    else: 
    	offset = (page - 1) * page당 개체의 수
        posts = query.offset(offset).limit(page당 개체의 수).all()
```
- offset: 몇번째 페이지 인지 지정 (0부터 시작)
- limit: 몇 개의 값을 불러올 것진이 지정함

