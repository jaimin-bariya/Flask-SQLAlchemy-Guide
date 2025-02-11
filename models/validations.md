# Validations in Flask-SQLAlchemy

Validations are essential in web applications to ensure data integrity and correctness. Flask-SQLAlchemy doesn't have built-in validation mechanisms for models, but you can implement validations using several techniques.

## 1. **Using Model Methods for Custom Validations**

You can define custom validation methods within your model class to check specific constraints.

### Example:
```python
from flask_sqlalchemy import SQLAlchemy
from flask import Flask

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def validate_username(self):
        if len(self.username) < 3:
            raise ValueError("Username must be at least 3 characters long.")
        return True

    def validate_email(self):
        if "@" not in self.email:
            raise ValueError("Invalid email format.")
        return True

    def save(self):
        self.validate_username()
        self.validate_email()
        db.session.add(self)
        db.session.commit()

# Usage Example
new_user = User(username='JP', email='invalidemail.com')
try:
    new_user.save()
except ValueError as e:
    print(f"Validation Error: {e}")
```

## 2. **Using WTForms for Form-Level Validations**

WTForms is a powerful library for form handling and validation in Flask.

### Example:
```python
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired, Email, Length

class UserForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired(), Length(min=3, message="Username must be at least 3 characters long.")])
    email = StringField('Email', validators=[DataRequired(), Email()])
    submit = SubmitField('Submit')
```

## 3. **Using SQLAlchemy Constraints for Database-Level Validations**

### Unique Constraint
To ensure a field is unique:
```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
```

### NOT NULL Constraint
To ensure a field cannot be `NULL`:
```python
class User(db.Model):
    email = db.Column(db.String(120), nullable=False)
```

## 4. **Validation with Event Listeners**

SQLAlchemy provides event listeners that can be used to validate data before it is committed.

### Example:
```python
from sqlalchemy import event

class Product(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(80), nullable=False)
    price = db.Column(db.Float, nullable=False)

@event.listens_for(Product, 'before_insert')
def validate_price(mapper, connect, target):
    if target.price <= 0:
        raise ValueError("Price must be greater than zero.")
```

## 5. **Raising Validation Errors in Routes**

Example in a route:
```python
@app.route('/add_user', methods=['POST'])
def add_user():
    username = request.form['username']
    email = request.form['email']
    user = User(username=username, email=email)
    try:
        user.save()
        return "User added successfully"
    except ValueError as e:
        return f"Error: {e}", 400
```

## Best Practices
- Combine **form-level validations** with **database constraints** for a robust validation strategy.
- Use **custom model methods** for complex, business-rule-specific validations.
- Validate as early as possible (form validation before reaching the database).
- Log validation errors for debugging.

This approach ensures that your Flask-SQLAlchemy applications maintain data integrity and provide a better user experience.

