# fastapi-project-tree

# 프로젝트 설명
FastAPI와 SQLite를 사용하여 사용자 관리 시스템을 구축하고, 프로젝트를 모범 사례에 맞게 모듈화하는 과정을 공유합니다.   

# 프로젝트 구조
```
app/
├── __init__.py
├── api/
│   ├── __init__.py
│   └── v1/
│       ├── __init__.py
│       └── endpoints/
│           ├── __init__.py
│           └── users.py
├── crud/
│   ├── __init__.py
│   └── user.py
├── db/
│   ├── __init__.py
│   └── session.py
├── models/
│   ├── __init__.py
│   └── user.py
├── schemas/
│   ├── __init__.py
│   └── user.py
└── main.py
```
각 디렉토리의 역할:  
    api/: API 라우터와 엔드포인트  
    core/: 설정 및 핵심 기능  
    crud/: 데이터베이스 CRUD 작업  
    db/: 데이터베이스 설정  
    models/: SQLAlchemy 모델  
    schemas/: Pydantic 모델 (데이터 검증)  
모든 디렉토리에 __init__.py 파일을 만들어야 Python이 해당 디렉토리를 패키지로 인식합니다.  

이메일 검증을 위한 패키지가 필요합니다. 다음 명령어로 설치해주세요:    
```
poetry add "pydantic[email]"
poetry add sqlalchemy
poetry add fastapi uvicorn
```   


# 데이터베이스 설정  
DATABASE_URL = "sqlite:///./users.db"  
engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})  
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)  
Base = declarative_base()  

# 의존성 주입(Dependency Injection)  
get_db() 함수를 통해 **의존성 주입(Dependency Injection)**을 사용하여 데이터베이스 세션을 주입합니다.    

# 데이터베이스 모델 정의  
테이블 정의는 models/user.py에 정의합니다.  

# Pydantic 스키마 정의  
입력과 출력을 검증하는 스키마 정의는 schemas/user.py에 정의합니다.  
 
# CRUD 함수 정의  
데이터베이스 작업을 수행하는 CRUD 함수 정의는 crud/user.py에 정의합니다.  

# API 엔드포인트 정의  
API 엔드포인트는 api/v1/endpoints/users.py에 정의합니다.  

# 메인 애플리케이션 설정  
메인 애플리케이션 설정은 main.py에 정의합니다.  

# 실행  
```
uvicorn main:app --reload  
``` 

# API 문서  
http://127.0.0.1:8000/docs  
  
# 사용자 목록 조회 테스트  
```
curl -X POST "http://127.0.0.1:8000/api/v1/users/" \
-H "Content-Type: application/json" \
-d '{
    "username": "testuser",
    "email": "testuser@example.com",
    "full_name": "Test User",
    "password": "testpassword123"
}'

```

# 사용자 조회  
```
curl -X GET "http://127.0.0.1:8000/api/v1/users/"

[
  {
    "username": "testuser",
    "email": "testuser@example.com",
    "full_name": "Test User",
    "id": 1
  }
]
``` 



md 파일 보는 방법 : cmd + shift + v  