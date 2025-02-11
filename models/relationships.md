# Relationships in Flask-SQLAlchemy

Relationships in Flask-SQLAlchemy help define associations between tables, making it easier to model complex data structures. Relationships are established using `db.relationship()` and `db.ForeignKey()`.

## Types of Relationships

### 1. **One-to-Many Relationship**
A single row in one table is associated with multiple rows in another table.

#### Example
```python
class Author(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), nullable=False)
    books = db.relationship('Book', backref='author', lazy=True)

class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    author_id = db.Column(db.Integer, db.ForeignKey('author.id'))
```
- The `books` attribute in `Author` defines the relationship.
- The `author_id` in `Book` is a foreign key linking to the `Author` table.
- The `backref` parameter allows reverse access (`book.author`).

### 2. **One-to-One Relationship**
A single row in one table is associated with a single row in another table.

#### Example
```python
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(50), unique=True, nullable=False)
    profile = db.relationship('Profile', backref='user', uselist=False)

class Profile(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    bio = db.Column(db.String(200))
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'))
```
- `uselist=False` ensures a one-to-one relationship.

### 3. **Many-to-Many Relationship**
Multiple rows in one table are associated with multiple rows in another table.

#### Example
```python
association_table = db.Table('association',
    db.Column('student_id', db.Integer, db.ForeignKey('student.id')),
    db.Column('course_id', db.Integer, db.ForeignKey('course.id'))
)

class Student(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50))
    courses = db.relationship('Course', secondary=association_table, backref='students')

class Course(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100))
```
- `association_table` is a helper table without its own model.
- `secondary=association_table` specifies the table managing the many-to-many relationship.

## Relationship Parameters

### 1. **`backref`**
Creates a reverse relationship.
```python
books = db.relationship('Book', backref='author')
```
Now you can access `book.author` or `author.books`.

### 2. **`lazy`**
Defines how related objects are loaded.
- `True`: Load all data immediately.
- `select`: Use a separate select statement (default).
- `joined`: Use a join query.
- `subquery`: Load with a subquery.

### 3. **`uselist`**
Controls whether the relationship returns a list (`True`) or a single item (`False`).

### 4. **`secondary`**
Specifies the table used for a many-to-many relationship.

## Example with Multiple Relationships
```python
class Department(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50), unique=True, nullable=False)
    employees = db.relationship('Employee', backref='department', lazy='dynamic')

class Employee(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    department_id = db.Column(db.Integer, db.ForeignKey('department.id'))
```

## Best Practices
1. Use `backref` to simplify reverse queries.
2. Always set foreign keys to maintain referential integrity.
3. Use meaningful relationship names for easier access.

By mastering relationships, you can effectively model complex database schemas with Flask-SQLAlchemy.

