# Database Access

`[Mid]`

Most backend services store data in a database. You need to know SQL basics and how to interact with a database from Python.

## SQL Basics

SQL (Structured Query Language) is how you talk to relational databases (PostgreSQL, MySQL, SQLite). Four operations cover most of what you'll do:

```sql
-- Create a table
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Insert a row
INSERT INTO users (name, email) VALUES ('Ada', 'ada@example.com');

-- Read rows
SELECT * FROM users;
SELECT name, email FROM users WHERE id = 1;

-- Update a row
UPDATE users SET name = 'Ada Lovelace' WHERE id = 1;

-- Delete a row
DELETE FROM users WHERE id = 1;
```

| Operation | SQL Keyword | Example |
|-----------|-------------|---------|
| Create | `INSERT INTO` | `INSERT INTO users (name) VALUES ('Ada')` |
| Read | `SELECT` | `SELECT * FROM users WHERE id = 1` |
| Update | `UPDATE ... SET` | `UPDATE users SET name = 'Bob' WHERE id = 1` |
| Delete | `DELETE FROM` | `DELETE FROM users WHERE id = 1` |

Common clauses:

```sql
-- Filter
SELECT * FROM users WHERE email LIKE '%@example.com';

-- Sort
SELECT * FROM users ORDER BY created_at DESC;

-- Limit
SELECT * FROM users ORDER BY created_at DESC LIMIT 10;

-- Count
SELECT COUNT(*) FROM users;
```

## SQLAlchemy ORM

Writing raw SQL in Python is error-prone and doesn't integrate well with type checking. SQLAlchemy is the standard ORM (Object-Relational Mapper) for Python. It lets you interact with the database using Python objects instead of raw SQL strings.

### Define Models

```python
from sqlalchemy import String
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(100))
    email: Mapped[str] = mapped_column(String(255), unique=True)
```

Step by step:
- `Base` -- the base class all models inherit from. SQLAlchemy tracks all subclasses.
- `__tablename__` -- the database table name.
- `id: Mapped[int]` -- type-annotated column. `Mapped[int]` means this is an integer column.
- `primary_key=True` -- this column is the primary key.
- `unique=True` -- no two users can have the same email.

### Query with Sessions

```python
from sqlalchemy import select
from sqlalchemy.orm import Session

# Create a user
new_user = User(name="Ada", email="ada@example.com")
session.add(new_user)
session.commit()

# Read all users
stmt = select(User)
users = session.execute(stmt).scalars().all()

# Read one user by ID
stmt = select(User).where(User.id == 1)
user = session.execute(stmt).scalar_one_or_none()

# Update
user.name = "Ada Lovelace"
session.commit()

# Delete
session.delete(user)
session.commit()
```

### Integrate with FastAPI

```python
from fastapi import FastAPI, Depends
from sqlalchemy import create_engine
from sqlalchemy.orm import Session

engine = create_engine("sqlite:///app.db")

def get_db():
    with Session(engine) as session:
        yield session

app = FastAPI()

@app.get("/users/{user_id}")
async def get_user(user_id: int, db: Session = Depends(get_db)):
    stmt = select(User).where(User.id == user_id)
    user = db.execute(stmt).scalar_one_or_none()
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return {"id": user.id, "name": user.name, "email": user.email}
```

`Depends(get_db)` is FastAPI's dependency injection. It creates a database session for each request and closes it when the request finishes.

## Migrations with Alembic

When you change your models (add a column, create a new table), you need to update the database schema. Alembic handles this:

```bash
# Set up Alembic
uv add alembic
uv run alembic init migrations

# Generate a migration from your models
uv run alembic revision --autogenerate -m "add users table"

# Apply the migration
uv run alembic upgrade head
```

Alembic compares your models to the current database state and generates SQL to bridge the gap. This lets you evolve your schema safely over time without manual SQL.

## The Flow

```
HTTP Request  -->  FastAPI endpoint  -->  SQLAlchemy query  -->  Database
                                                            <--  Results
              <--  JSON response   <--  Python objects
```

Your FastAPI endpoint receives a request, uses SQLAlchemy to query the database, converts the result to JSON, and returns it. The ORM handles translating between Python objects and database rows.
