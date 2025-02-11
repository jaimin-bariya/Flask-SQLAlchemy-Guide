# Flask-SQLAlchemy Comprehensive Guide Repository

This repository provides a structured and detailed guide on using Flask-SQLAlchemy to manage models and interact with databases in Flask applications. The goal is to share knowledge about different attributes, column options, and best practices.

---

## **Project Structure**
```
flask-sqlalchemy-guide/
├── README.md                # Project overview and introduction
├── setup/                    # Environment setup instructions
│   └── environment.md
├── models/                   # Guides on defining models
│   ├── basic-models.md       # Basic model creation
│   ├── column-attributes.md  # Column options and attributes
│   ├── relationships.md      # Defining relationships (One-to-Many, Many-to-Many)
│   └── validations.md        # Model constraints and validation
├── database/                 # Database creation and management
│   ├── creating-tables.md    # How to create tables
│   └── migrations.md         # Database migrations with Flask-Migrate
└── examples/                 # Practical examples and best practices
    ├── simple-app/           # Simple Flask app with SQLAlchemy
    └── advanced-features/    # Advanced configurations
```

---

## **1. Setting Up the Environment**

### **[setup/environment.md](setup/environment.md)**
Instructions to set up the environment:
- Installing Flask-SQLAlchemy
- Creating a virtual environment
- Connecting to different databases

**Example Configuration:**
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///database.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
```

---

## **2. Defining Models**

### **[models/basic-models.md](models/basic-models.md)**
Basic structure of defining models and mapping them to tables.

**Example:**
```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(80), nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
```

### **[models/column-attributes.md](models/column-attributes.md)**
Comprehensive list of column attributes:
- **Data Types:** `db.Integer`, `db.String`, `db.Float`, `db.Boolean`, etc.
- **Common Options:**
  - `primary_key=True`
  - `nullable=False`
  - `unique=True`
  - `default=value`

**Example:**
```python
price = db.Column(db.Float, default=0.0, nullable=False)
created_at = db.Column(db.DateTime, default=db.func.now())
```

### **[models/relationships.md](models/relationships.md)**
Guide on creating relationships between tables:
- One-to-Many
- Many-to-Many

**Example:**
```python
class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'))
    user = db.relationship('User', backref='posts')
```

### **[models/validations.md](models/validations.md)**
How to apply model-level constraints and validation.

---

## **3. Database Management**

### **[database/creating-tables.md](database/creating-tables.md)**
Steps to create tables and manage the database lifecycle.

**Creating Tables:**
```python
with app.app_context():
    db.create_all()
```

### **[database/migrations.md](database/migrations.md)**
How to handle migrations using `Flask-Migrate`.

---

## **4. Examples and Best Practices**

### **[examples/simple-app/](examples/simple-app/)**
A minimal example of a Flask app with a User model.

### **[examples/advanced-features/](examples/advanced-features/)**
Examples demonstrating advanced configurations such as:
- Custom Query Methods
- Database Indexing
- Performance Optimization

---

## **How to Contribute**
Feel free to open issues or submit pull requests to improve this guide.

---

## **License**
MIT License
