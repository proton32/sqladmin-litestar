<p align="center">
<a href="https://github.com/aminalaee/sqladmin">
    <img width="400px" src="https://raw.githubusercontent.com/aminalaee/sqladmin/main/docs/assets/images/banner.png" alt"SQLAdmin">
</a>
</p>

<p align="center">
<a href="https://github.com/aminalaee/sqladmin/actions">
    <img src="https://github.com/aminalaee/sqladmin/workflows/Test%20Suite/badge.svg" alt="Build Status">
</a>
<a href="https://github.com/aminalaee/sqladmin/actions">
    <img src="https://github.com/aminalaee/sqladmin/workflows/Publish/badge.svg" alt="Publish Status">
</a>
<a href="https://codecov.io/gh/aminalaee/sqladmin">
    <img src="https://codecov.io/gh/aminalaee/sqladmin/branch/main/graph/badge.svg" alt="Coverage">
</a>
<a href="https://pypi.org/project/sqladmin/">
    <img src="https://badge.fury.io/py/sqladmin.svg" alt="Package version">
</a>
<a href="https://pypi.org/project/sqladmin" target="_blank">
    <img src="https://img.shields.io/pypi/pyversions/sqladmin.svg?color=%2334D058" alt="Supported Python versions">
</a>
</p>

---



# SQLAlchemy Admin for Litestar

SQLAdmin is a flexible Admin interface for SQLAlchemy models.

Main features include:

* [SQLAlchemy](https://github.com/sqlalchemy/sqlalchemy) sync/async engines
* [Litestar](https://github.com/litestar-org/litestar) integration
* [WTForms](https://github.com/wtforms/wtforms) form building
* [SQLModel](https://github.com/tiangolo/sqlmodel) support
* UI using [Tabler](https://github.com/tabler/tabler)

---

**Documentation**: [https://aminalaee.dev/sqladmin](https://aminalaee.dev/sqladmin)

**Source Code**: [https://github.com/cemrehancavdar/sqladmin-litestar](https://github.com/cemrehancavdar/sqladmin-litestar)

**Original Source Code**: [https://github.com/aminalaee/sqladmin](https://github.com/aminalaee/sqladmin)

**Online Demo**: [Demo](https://sqladmin-demo.aminalaee.dev/admin/)

---

## Installation

Install using `pip`:

```shell
$ pip install sqladmin-litestar
```

This will install the full version of sqladmin with optional dependencies:

```shell
$ pip install "sqladmin-litestar[full]"
```

---

## Screenshots

<img width="1492" alt="sqladmin-1" src="https://user-images.githubusercontent.com/19784933/208232730-0114a155-2740-4e89-9d73-64a4e51a5cf5.png">
<img width="1492" alt="sqladmin-2" src="https://user-images.githubusercontent.com/19784933/208232731-6d783dde-b93e-41c0-911b-3d1c3c73f1d5.png">

## Quickstart

Let's define an example SQLAlchemy model:

```python
# db.py
from sqlalchemy import Column, Integer, String, create_engine
from sqlalchemy.orm import declarative_base


Base = declarative_base()
engine = create_engine(
    "sqlite:///example.db",
    connect_args={"check_same_thread": False},
)


class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    name = Column(String)


Base.metadata.create_all(engine)  # Create tables
```

If you want to use `SQLAdmin` with `Litestar`:

```python
# app.py
from litestar import Litestar
from sqladmin import Admin, ModelView

from db import User, engine


admin = Admin(engine)


class UserAdmin(ModelView, model=User):
    column_list = [User.id, User.name]


admin.add_view(UserAdmin)

app = Litestar(plugins=[admin], debug=True)
```

Now visiting `/admin` on your browser you can see the `SQLAdmin` interface.


## Differences between SQLAdmin and SQLAdmin-Litestar

1) Working with Custom Views
   
 SQLAdmin-Litestar templates works sync, and handlers should be annotated.
   ```
    @expose("/report", methods=["GET"])
    async def report_page(self, request):
        return await self.templates.TemplateResponse(request, "report.html")
   ```
    should be changed to
   ```
    @expose("/report", methods=["GET"])
    async def report_page(self, request) -> str:
        return self.templates.TemplateResponse(request, "report.html")
   ```


## Related projects and inspirations
* [SQLAdmin](https://github.com/aminalaee/sqladmin) The original SQLAdmin Repository
* [Flask-Admin](https://github.com/flask-admin/flask-admin) Admin interface for Flask supporting different database backends and ORMs. This project has inspired SQLAdmin extensively and most of the features and configurations are implemented the same.
* [FastAPI-Admin](https://github.com/fastapi-admin/fastapi-admin) Admin interface for FastAPI which works with `TortoiseORM`.
* [Dashboard](https://github.com/encode/dashboard) Admin interface for ASGI frameworks which works with the `orm` package.
