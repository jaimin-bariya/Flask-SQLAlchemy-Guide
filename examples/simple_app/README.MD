# Simple Flask App with SQLAlchemy

## app.py
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

# Initialize Flask App
app = Flask(__name__)

# Configure the SQLite database
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# Initialize the database
db = SQLAlchemy(app)

# Define a simple User model
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def __repr__(self):
        return f'<User {self.username}>'

# Create the database and tables
with app.app_context():
    db.create_all()

@app.route('/')
def home():
    return "Hello, Flask with SQLAlchemy!"

if __name__ == '__main__':
    app.run(debug=True)
```

## Instructions to Run the Example

1. **Install Dependencies:**
    ```bash
    pip install Flask Flask-SQLAlchemy
    ```

2. **Run the Application:**
    ```bash
    python app.py
    ```

3. **Open in Browser:**
    Visit [http://127.0.0.1:5000](http://127.0.0.1:5000) to see the app running.

## Explanation

- This example demonstrates setting up a simple Flask app with SQLAlchemy.
- We define a `User` model with `id`, `username`, and `email` columns.
- The database and tables are created using `db.create_all()`.
- The home route returns a simple welcome message.

## Next Steps
- Extend the app with CRUD operations.
- Connect to more advanced databases like PostgreSQL.
- Handle migrations with `Flask-Migrate`.
