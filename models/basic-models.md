# Basic Models in Flask-SQLAlchemy

In Flask-SQLAlchemy, models are Python classes that represent database tables. Below is a step-by-step guide on how to create and define models.

## Step 1: Create a Model Class
- Models in Flask-SQLAlchemy inherit from `db.Model`.
- Each attribute of the class corresponds to a column in the database table.

### Example Model Definition
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def __repr__(self):
        return f'<User {self.username}>'

if __name__ == '__main__':
    db.create_all()
    print("Database and tables created!")
```

## Key Components Explained
### 1. **`db.Model` Inheritance**
   - All models inherit from `db.Model` to connect them to the SQLAlchemy instance.

### 2. **Columns (`db.Column`)**
   - Define the fields of the table.
   - Each column has a data type and optional constraints.

#### Common Data Types
- `db.Integer`: Integer values.
- `db.String`: Variable-length string.
- `db.Float`: Floating-point numbers.
- `db.Boolean`: Boolean values.
- `db.DateTime`: Date and time values.
- `db.Text`: Large text fields.

#### Constraints
- `primary_key=True`: Marks the column as the primary key.
- `unique=True`: Ensures unique values for the column.
- `nullable=False`: Makes the column non-nullable.

## Creating the Database
To create the database and tables, use:
```python
if __name__ == '__main__':
    db.create_all()
```
This will generate the necessary tables in the database.

## Conclusion
This is a basic example of setting up models in Flask-SQLAlchemy. In the next guide, we will cover column attributes in detail, including default values, indexing, and relationships between tables.
