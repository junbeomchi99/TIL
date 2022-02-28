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

## 필수 설치 요소
```python
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
- routes = URL 경로에 따라 api 함수들을 나누어주는 역확
- main.py = __FastAPI를 실행__ 하고 routes폴더들의 다른 라우트들을 불러와 포함시키는 역활
- __init__.py = __해당 폴더가 패키지의 일부라는 것을 나타낸다__. 해당 파일에 작성 되어야 할 내용은 없음. (Python 3.3 버전 이후에는 해당 파일이 존재하지 않아도 패키지 일부로 인식할수 있지만 하위 번전은 Python과 호환을 위해 만들어주는것 권장)

