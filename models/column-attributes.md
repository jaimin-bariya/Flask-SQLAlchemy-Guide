# Column Attributes in Flask-SQLAlchemy

When defining models in Flask-SQLAlchemy, each field is defined using `db.Column` with specific attributes. These attributes control the behavior and constraints of the database columns.

## Common Column Attributes

### 1. **Primary Key (**``**)**

Defines the primary key of the table.

```python
id = db.Column(db.Integer, primary_key=True)
```

### 2. **Unique (**``**)**

Ensures that all values in this column are unique.

```python
email = db.Column(db.String(120), unique=True)
```

### 3. **Nullable (**``**)**

Specifies whether the column can have `NULL` values. By default, columns are nullable.

```python
username = db.Column(db.String(80), nullable=False)
```

### 4. **Default (**``**)**

Sets a default value for the column.

```python
is_active = db.Column(db.Boolean, default=True)
```

### 5. **Index (**``**)**

Creates an index on the column to speed up queries.

```python
created_at = db.Column(db.DateTime, index=True)
```

### 6. **Data Types (**``**, **``**, etc.)**

Defines the type of data that the column will store.

- `db.String`: Variable-length string.
- `db.Integer`: Integer values.
- `db.Float`: Floating-point numbers.
- `db.Boolean`: Boolean values.
- `db.DateTime`: Date and time values.
- `db.Text`: Large text fields.

### 7. **Foreign Key (**``**)**

Establishes relationships between tables.

```python
profile_id = db.Column(db.Integer, db.ForeignKey('profile.id'))
```

### 8. **Server Default (**``**)**

Specifies a default value at the database level rather than application level.

```python
status = db.Column(db.String(50), server_default='active')
```

### 9. **Check Constraint (**``**)**

Adds constraints to validate data.

```python
__table_args__ = (
    db.CheckConstraint('age >= 18', name='check_age_positive'),
)
```

### 10. **Relationships (**``**)**

Defines relationships between tables.

```python
profile = db.relationship('Profile', backref='user', lazy=True)
```

## Example Model with Attributes

```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    created_at = db.Column(db.DateTime, index=True, default=datetime.utcnow)
    is_active = db.Column(db.Boolean, default=True)

    def __repr__(self):
        return f'<User {self.username}>'
```

## Conclusion

Understanding and using column attributes properly allows you to build robust and efficient databases. In the next section, we will cover relationships and more advanced features in Flask-SQLAlchemy.

