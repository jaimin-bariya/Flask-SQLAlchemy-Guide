# Advanced Features Example with Flask-SQLAlchemy

## app.py
```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy.orm import relationship

# Initialize Flask App
app = Flask(__name__)

# Configure the PostgreSQL database
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://username:password@localhost:5432/advanced_db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

# Initialize the database
db = SQLAlchemy(app)

# Define models with relationships and validations
class Author(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    books = db.relationship('Book', backref='author', lazy=True)

class Book(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200), nullable=False)
    genre = db.Column(db.String(50))
    author_id = db.Column(db.Integer, db.ForeignKey('author.id'), nullable=False)

# Create the database and tables
with app.app_context():
    db.create_all()

@app.route('/authors', methods=['POST'])
def add_author():
    data = request.get_json()
    new_author = Author(name=data['name'])
    db.session.add(new_author)
    db.session.commit()
    return jsonify({'message': 'Author added successfully'}), 201

@app.route('/books', methods=['POST'])
def add_book():
    data = request.get_json()
    new_book = Book(title=data['title'], genre=data['genre'], author_id=data['author_id'])
    db.session.add(new_book)
    db.session.commit()
    return jsonify({'message': 'Book added successfully'}), 201

@app.route('/authors/<int:author_id>/books', methods=['GET'])
def get_books_by_author(author_id):
    books = Book.query.filter_by(author_id=author_id).all()
    return jsonify([{'id': book.id, 'title': book.title, 'genre': book.genre} for book in books])

if __name__ == '__main__':
    app.run(debug=True)
```

## Instructions to Run the Example

1. **Install Dependencies:**
    ```bash
    pip install Flask Flask-SQLAlchemy psycopg2-binary
    ```

2. **Create PostgreSQL Database:**
    - Create a PostgreSQL database named `advanced_db`.

3. **Run the Application:**
    ```bash
    python app.py
    ```

4. **API Endpoints:**
    - Add Author: `POST /authors` with JSON `{"name": "Author Name"}`
    - Add Book: `POST /books` with JSON `{"title": "Book Title", "genre": "Fiction", "author_id": 1}`
    - Get Books by Author: `GET /authors/1/books`

## Explanation

- Demonstrates advanced features like relationships between models and dynamic queries.
- Validates and handles complex requests.
- Integrates with PostgreSQL for production-grade database management.

## Next Steps
- Add error handling and validation checks.
- Secure the API with authentication.
- Implement pagination for large datasets.
