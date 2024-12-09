---
title: Recommendation Structure Folder For a Python FastAPI Projects
date: 2024-12-09
draft: false
tags:
  - mitoyus
  - blog
  - python
  - english
  - chatgpt
  - fastapi
---
When structuring a folder for a **Python FastAPI** project, it's essential to maintain modularity and scalability, especially as your project grows. A good folder structure helps to separate concerns, making it easier to manage and maintain the codebase.

Here's a recommended folder structure for a typical FastAPI project:

### Recommended Folder Structure

```
my_fastapi_project/
│
├── app/                        # Main application code
│   ├── __init__.py
│   ├── main.py                 # Entry point for the FastAPI application
│   ├── api/                    # API routes and business logic
│   │   ├── __init__.py
│   │   ├── v1/                 # Versioned API (optional, but good for future scalability)
│   │   │   ├── __init__.py
│   │   │   ├── endpoints/      # Routes for version 1 API
│   │   │   │   ├── __init__.py
│   │   │   │   ├── user.py     # Example: user-related routes
│   │   │   │   ├── item.py     # Example: item-related routes
│   │   ├── v2/                 # (Optional) Future version of the API
│   │   ├── dependencies.py     # Shared dependencies (e.g., DB session)
│   ├── core/                   # Core components like settings, config, security
│   │   ├── __init__.py
│   │   ├── config.py           # Configuration settings (e.g., environment variables)
│   │   ├── security.py         # Security-related utilities (e.g., JWT)
│   │   ├── settings.py         # Settings management (e.g., loading env variables)
│   ├── models/                 # ORM models or Pydantic schemas
│   │   ├── __init__.py
│   │   ├── user.py             # Example: user model
│   │   ├── item.py             # Example: item model
│   ├── schemas/                # Pydantic schemas for validation
│   │   ├── __init__.py
│   │   ├── user.py             # Example: user schema
│   │   ├── item.py             # Example: item schema
│   ├── services/               # Business logic and service layer
│   │   ├── __init__.py
│   │   ├── user_service.py     # Example: user service
│   │   ├── item_service.py     # Example: item service
│   ├── db/                     # Database-related code (e.g., SQLAlchemy models, migrations)
│   │   ├── __init__.py
│   │   ├── session.py          # Database session management
│   │   ├── init_db.py          # Database initialization code (e.g., creating tables)
│   ├── tests/                  # Unit and integration tests
│   │   ├── __init__.py
│   │   ├── test_user.py        # Example test file for user-related functionality
│   │   ├── test_item.py        # Example test file for item-related functionality
│   ├── utils/                  # Utility functions that are used across the app
│   │   ├── __init__.py
│   │   ├── date_utils.py       # Example utility: date-related functions
│
├── .env                         # Environment variables for sensitive data (e.g., DB URI, API keys)
├── .gitignore                   # Git ignore file to exclude unnecessary files
├── requirements.txt             # Project dependencies (including FastAPI and Uvicorn)
├── alembic.ini                  # Alembic configuration file (if using migrations with SQLAlchemy)
├── Dockerfile                   # Dockerfile for containerizing the app (optional)
├── docker-compose.yml           # Docker Compose file for multi-container setup (optional)
└── README.md                    # Project documentation
```

### Explanation of the Structure:

1. **`app/`**: This is where the majority of your code resides. It contains all your app-specific logic and modules.
    
2. **`main.py`**: The entry point of the FastAPI app. It includes the FastAPI instance, router inclusion, middleware setup, etc.
    
3. **`api/`**: This folder holds all your API-related logic. If your project has different versions or groups of endpoints, you can group them by version (e.g., `v1`, `v2`). Each version can have its own set of routes, controllers, and dependencies.
    
4. **`core/`**: Contains core functionality, such as app settings, security mechanisms (e.g., authentication), and other foundational components. This is where configuration files (e.g., environment variables, secrets) are loaded.
    
5. **`models/`**: If you are using an ORM like SQLAlchemy, this folder would contain your ORM models. These models define the structure of your database tables.
    
6. **`schemas/`**: This is where you define Pydantic models that are used for request validation and response formatting. Pydantic schemas ensure data consistency and provide automatic validation for input data.
    
7. **`services/`**: This folder is for the business logic layer. You can encapsulate your core logic here, such as interacting with the database or performing specific actions related to your resources (e.g., user service, item service).
    
8. **`db/`**: Contains all the database-related code. This includes database session management, migrations, and any helpers needed for interacting with the database.
    
9. **`tests/`**: This is where your tests live. Typically, you will write unit tests for individual components (e.g., services) and integration tests for end-to-end validation of API routes.
    
10. **`utils/`**: A folder for utility functions that don’t fit neatly into the core, services, or other categories. For example, helper functions for date formatting, file handling, etc.
    
11. **`requirements.txt`**: A file listing all dependencies required to run your FastAPI app, including `fastapi`, `uvicorn`, `sqlalchemy`, etc.
    
12. **`Dockerfile` & `docker-compose.yml`** (optional): If you're containerizing your application, these files are necessary for setting up the containerized environment.
    
13. **`.env`**: This file holds environment variables for sensitive data like API keys, database credentials, and other settings that should not be hardcoded.
    
14. **`README.md`**: The documentation for your project, explaining the purpose, how to set it up, and how to contribute.
    

### Example of `main.py`:

```python
from fastapi import FastAPI
from app.api.v1 import endpoints

app = FastAPI()

# Include the routes from the API version 1
app.include_router(endpoints.router, prefix="/v1", tags=["v1"])

# You can add more versioned routers as your app grows
```

### Example of `endpoints/user.py`:

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

### Conclusion

This folder structure provides clear separation of concerns, modularity, and maintainability. It scales well for larger applications while keeping the codebase organized. You can easily extend the API by adding new versions in the `api/` folder, handle configuration separately in `core/`, and structure your database logic in `db/`.