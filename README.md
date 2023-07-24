![alt text](logo-teal.png "FastAPI")

# FASTAPI BASE

## Introduction

FastAPI Base cung cấp sẵn các function basic nhất như CRUD User, Login, Register.

Project đã bao gồm migration DB và pytest để sẵn sàng sử dụng trong môi trường doanh nghiệp.

Phát triển dựa trên: https://github.com/Longdh57/fastapi-base

## Source Library

Đây là phiên bản Backend base từ framework FastAPI. Trong code base này đã cấu hình sẵn

- [FastAPI](https://fastapi.tiangolo.com/)
- [Postgresql(>=12)](https://www.postgresql.org/)
- Alembic
- API Login
- API CRUD User, API Get Me & API Update Me
- Pagination
- Authen/Authorize với JWT
- Permission_required & Login_required
- Logging
- Pytest

## Installation

**Cách 1:**

- Clone Project
- Cài đặt Postgresql & Create Database
- Cài đặt requirements.txt
- Run project ở cổng 8000

```
// Tạo postgresql Databases via CLI (Ubuntu 20.04)
$ sudo -u postgres psql
# CREATE DATABASE fastapi_base;
# CREATE USER db_user WITH PASSWORD 'secret123';
# GRANT ALL PRIVILEGES ON DATABASE fastapi_base TO db_user;

// Clone project & run
$ git clone ...
$ cd fastapi-postgre-celery
$ virtualenv -p python3 venv
$ source venv/bin/activate
$ pip install -r requirements.txt
$ cp env.example .env       // Recheck SQL_DATABASE_URL ở bước này
$ alembic upgrade head
$ uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

**Cách 2:** Dùng Docker & Docker Compose - đơn giản hơn nhưng cần có kiến thức Docker

- Clone Project
- Run docker-compose

```
$ git clone ...
$ cd fastapi-postgre-celery
$ docker-compose up --build -d
```

## Cấu trúc project

```
.
├── alembic
│   ├── versions    // thư mục chứa file migrations
│   └── env.py
├── app
│   ├── api         // các file api được đặt trong này, được tách ra gồm: controller, service, repo
│   ├── core        // chứa file config load các biến env & function tạo/verify JWT access-token
│   ├── db          // file cấu hình make DB session
│   ├── helpers     // các function hỗ trợ như login_manager, paging
│   ├── models      // Database model, tích hợp với alembic để auto generate migration
│   ├── schemas     // Pydantic Schema
│   ├── services    // Chứa logic CRUD giao tiếp với DB
│   └── main.py     // cấu hình chính của toàn bộ project
├── tests
│   ├── api         // chứa các file test cho từng api
│   ├── faker       // chứa file cấu hình faker để tái sử dụng
│   ├── .env        // config DB test
│   └── conftest.py // cấu hình chung của pytest
├── .gitignore      // danh sách các file không cần đẩy lên git
├── .gitlab-ci.yml      // cicd của gitlab-ci
├── .pre-commit-config.yaml      // danh sách các định dạng trước khi commit
├── ._config.yaml      // 
├── alembic.ini     //
├── docker-compose.yaml     // chứa các container phục vụ
├── Dockerfile      // Docker file cho fastapi
├── env.example     // chứa env dùng để chạy local
├── env.docker.example     // chứa env dùng để chạy cho docker
├── logging.ini     // cấu hình logging
├── pytest.ini      // file setup cho pytest
├── README.md       // đọc để biết
└── requirements.txt    // file chứa các thư viện để cài đặt qua pip install
```

## Migration

Migration là một tính năng cho phép bạn thay đổi cả cấu trúc và dữ liệu trong database.

Thay vì thay đổi trực tiếp vào database thì project FastAPI này sử dụng alembic để thực hiện việc thay đổi các table

```
$ alembic revision --autogenerate  # Create migration versions
$ alembic upgrade head  # Upgrade to last version migration
Tham khảo hướng dẫn ở document/CREATE_DB_GUIDE.md
```

Link [CREATE_DB_GUIDE.md](./document/CREATE_DB_GUIDE.md)

```
# alembic/versions/f9a075ca46e9_.py

...
def upgrade():
    # ### commands auto generated by Alembic - please adjust! ###
    op.create_table('user',
    sa.Column('id', sa.Integer(), autoincrement=True, nullable=False),
    sa.Column('full_name', sa.String(), nullable=True),
    sa.Column('email', sa.String(), nullable=True),
    sa.Column('hashed_password', sa.String(length=255), nullable=True),
    sa.PrimaryKeyConstraint('id')
    )
    op.create_index(op.f('ix_user_email'), 'user', ['email'], unique=True)
    op.create_index(op.f('ix_user_full_name'), 'user', ['full_name'], unique=False)
    # ### end Alembic commands ###
...
```

## PRE-COMMIT

Chạy ```pre-commit install``` để thiết lập tập lệnh git hook. nếu bạn muốn định dạng tất cả tệp trong dự án, bạn có thể
sử dụng lệnh này ```pre-commit run --all-files```

## FastAPI UI

Song song với FastAPI backend, đây là phiên bản Frontend tương thích với project này.

URL: [FastAPI Base Frontend](https://github.com/Longdh57/FastAPI-Base-Frontend)