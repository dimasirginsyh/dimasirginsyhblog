---
title: Contoh File Dari Rekomendasi Folder Python FastAPI Project
date: 2024-12-09
draft: false
tags:
  - mitoyus
  - blog
  - python
  - indonesia
  - chatgpt
  - fastapi
---
Tentu! Berikut adalah contoh lengkap untuk setiap file dan folder dalam struktur folder FastAPI yang telah saya jelaskan sebelumnya. Saya akan memberikan contoh kode untuk setiap file utama, termasuk contoh `main.py`, API endpoints, model, schema, dan sebagainya. Ini akan memberikan gambaran lebih jelas tentang bagaimana mengorganisasi aplikasi FastAPI yang skalabel.

### 1. **`my_fastapi_project/app/main.py`**

```python
from fastapi import FastAPI
from app.api.v1 import endpoints

app = FastAPI()

# Include the routes from the API version 1
app.include_router(endpoints.router, prefix="/v1", tags=["v1"])
```

### 2. **`my_fastapi_project/app/api/v1/endpoints/user.py`**

```python
from fastapi import APIRouter, Depends
from app.schemas.user import User, UserCreate
from app.services.user_service import create_user, get_user

router = APIRouter()

@router.post("/users/", response_model=User)
def create_user_endpoint(user: UserCreate):
    return create_user(user)

@router.get("/users/{user_id}", response_model=User)
def get_user_endpoint(user_id: int):
    return get_user(user_id)
```

### 3. **`my_fastapi_project/app/api/v1/endpoints/item.py`**

```python
from fastapi import APIRouter, Depends
from app.schemas.item import Item, ItemCreate
from app.services.item_service import create_item, get_item

router = APIRouter()

@router.post("/items/", response_model=Item)
def create_item_endpoint(item: ItemCreate):
    return create_item(item)

@router.get("/items/{item_id}", response_model=Item)
def get_item_endpoint(item_id: int):
    return get_item(item_id)
```

### 4. **`my_fastapi_project/app/core/config.py`**

```python
import os
from dotenv import load_dotenv

load_dotenv()  # Load environment variables from .env

class Config:
    API_KEY = os.getenv("API_KEY")
    DB_URL = os.getenv("DATABASE_URL")
    SECRET_KEY = os.getenv("SECRET_KEY")

config = Config()
```

### 5. **`my_fastapi_project/app/core/security.py`**

```python
from passlib.context import CryptContext

# Password hashing
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)
```

### 6. **`my_fastapi_project/app/models/user.py`**

```python
from sqlalchemy import Column, Integer, String
from app.db.session import Base

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    username = Column(String, unique=True, index=True)
    hashed_password = Column(String)
```

### 7. **`my_fastapi_project/app/models/item.py`**

```python
from sqlalchemy import Column, Integer, String
from app.db.session import Base

class Item(Base):
    __tablename__ = "items"

    id = Column(Integer, primary_key=True, index=True)
    name = Column(String, index=True)
    description = Column(String)
```

### 8. **`my_fastapi_project/app/schemas/user.py`**

```python
from pydantic import BaseModel

class User(BaseModel):
    id: int
    username: str

    class Config:
        orm_mode = True

class UserCreate(BaseModel):
    username: str
    password: str
```

### 9. **`my_fastapi_project/app/schemas/item.py`**

```python
from pydantic import BaseModel

class Item(BaseModel):
    id: int
    name: str
    description: str

    class Config:
        orm_mode = True

class ItemCreate(BaseModel):
    name: str
    description: str
```

### 10. **`my_fastapi_project/app/services/user_service.py`**

```python
from app.models.user import User
from app.schemas.user import UserCreate
from app.db.session import SessionLocal
from app.core.security import hash_password

def create_user(user_create: UserCreate):
    db = SessionLocal()
    hashed_password = hash_password(user_create.password)
    db_user = User(username=user_create.username, hashed_password=hashed_password)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    db.close()
    return db_user

def get_user(user_id: int):
    db = SessionLocal()
    db_user = db.query(User).filter(User.id == user_id).first()
    db.close()
    return db_user
```

### 11. **`my_fastapi_project/app/services/item_service.py`**

```python
from app.models.item import Item
from app.schemas.item import ItemCreate
from app.db.session import SessionLocal

def create_item(item_create: ItemCreate):
    db = SessionLocal()
    db_item = Item(name=item_create.name, description=item_create.description)
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    db.close()
    return db_item

def get_item(item_id: int):
    db = SessionLocal()
    db_item = db.query(Item).filter(Item.id == item_id).first()
    db.close()
    return db_item
```

### 12. **`my_fastapi_project/app/db/session.py`**

```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"  # Replace with your actual database URL

engine = create_engine(SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

# To initialize the database (create tables)
def init_db():
    Base.metadata.create_all(bind=engine)
```

### 13. **`my_fastapi_project/app/tests/test_user.py`**

```python
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_create_user():
    response = client.post("/v1/users/", json={"username": "testuser", "password": "password123"})
    assert response.status_code == 200
    assert response.json()["username"] == "testuser"

def test_get_user():
    response = client.post("/v1/users/", json={"username": "testuser", "password": "password123"})
    user_id = response.json()["id"]
    
    response = client.get(f"/v1/users/{user_id}")
    assert response.status_code == 200
    assert response.json()["id"] == user_id
```

### 14. **`my_fastapi_project/app/utils/date_utils.py`**

```python
from datetime import datetime

def current_time():
    return datetime.utcnow().strftime("%Y-%m-%d %H:%M:%S")
```

### 15. **`my_fastapi_project/.env`**

```
API_KEY=your_api_key_here
DATABASE_URL=sqlite:///./test.db  # Replace with your actual database URL
SECRET_KEY=your_secret_key_here
```

### 16. **`my_fastapi_project/requirements.txt`**

```
fastapi
uvicorn
sqlalchemy
pydantic
passlib
python-dotenv
```

### 17. **`my_fastapi_project/Dockerfile`** (optional)

```Dockerfile
# Use official Python image
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Copy requirements and install
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application
COPY . .

# Expose port 8000 for FastAPI
EXPOSE 8000

# Run the application with Uvicorn
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 18. **`my_fastapi_project/docker-compose.yml`** (optional)

```yaml
version: "3.8"
services:
  app:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=sqlite:///./test.db
    volumes:
      - .:/app
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000
```

### Conclusion

Dengan struktur ini, Anda memiliki aplikasi FastAPI yang terorganisir dengan baik, memisahkan setiap bagian dari aplikasi (API, model, schema, logika bisnis, dll). Ini memudahkan pengembangan dan pemeliharaan proyek, serta mendukung pengembangan lebih lanjut saat aplikasi Anda tumbuh.