# Migrations in Flask-SQLAlchemy

Database migrations allow you to update the database schema over time as your application evolves. Instead of manually creating or updating tables, migrations handle schema changes automatically. Flask-Migrate, which is built on Alembic, provides a simple and powerful way to manage migrations in Flask applications.

## Why Use Migrations?
- **Schema Updates:** Easily add or modify columns, tables, and constraints.
- **Version Control:** Track database changes over time.
- **Consistency:** Ensure all environments have the same schema.

## Installing Flask-Migrate

To get started, install Flask-Migrate:

```bash
pip install Flask-Migrate
```

## Setting Up Migrations

### 1. **Initialize Flask-Migrate**

Modify your `app.py` to include Flask-Migrate:

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'

# Initialize the database
db = SQLAlchemy(app)

# Initialize Flask-Migrate
migrate = Migrate(app, db)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
```

### 2. **Initialize the Migration Directory**

Run the following command to create the migration directory:

```bash
flask db init
```
This creates a `migrations/` folder to store migration files.

### 3. **Generate a Migration Script**

Whenever you make changes to your models, generate a migration script:

```bash
flask db migrate -m "Initial migration."
```
This command detects changes in your models and generates a migration script.

### 4. **Apply the Migration**

Run the following command to apply the migration and update the database schema:

```bash
flask db upgrade
```

### 5. **Downgrade the Migration (Optional)**

If you need to revert the last migration, use:

```bash
flask db downgrade
```

## Common Commands

| Command | Description |
|---------|-------------|
| `flask db init` | Initialize the migration directory. |
| `flask db migrate` | Generate a new migration script. |
| `flask db upgrade` | Apply the migration changes. |
| `flask db downgrade` | Revert the last migration. |

## Best Practices

1. **Always Add Descriptive Migration Messages:** Use `-m "message"` when generating migrations.
2. **Review Migration Scripts:** Check the generated script before upgrading to ensure it matches your intended changes.
3. **Keep Migrations in Version Control:** Commit migration files to your Git repository.
4. **Avoid Manual Schema Changes:** Always use migrations to keep the schema consistent.

## Example Workflow

1. **Add a New Column to the User Model:**

```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    bio = db.Column(db.String(250))  # New column added
```

2. **Generate and Apply Migration:**

```bash
flask db migrate -m "Add bio column to User table"
flask db upgrade
```

## Troubleshooting

### Migration Command Errors
Ensure that your Flask application is properly configured with the correct database URI and that the environment is activated.

### Inconsistent Database Schema
Verify that migrations have been applied using `flask db upgrade` and that all environments are up to date.

By following this guide, you can manage your database schema efficiently and confidently handle schema changes in your Flask applications using migrations.

