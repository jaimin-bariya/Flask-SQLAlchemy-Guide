# Environment Setup for Flask-SQLAlchemy Project

## Prerequisites
Before setting up the environment, ensure you have the following installed:

- **Python (>=3.7)**: [Download Python](https://www.python.org/downloads/)
- **Virtual Environment**: Comes built-in with Python (`venv`).
- **Node.js (Optional)**: Required if using TailwindCSS or other frontend tools.
- **Database System (Optional)**: PostgreSQL, MySQL, or SQLite (default).

---

## Step 1: Create Project Directory
```bash
mkdir flask_sqlalchemy_project
cd flask_sqlalchemy_project
```

---

## Step 2: Set Up Virtual Environment
```bash
python -m venv venv
```
Activate the virtual environment:
- On Windows:
  ```bash
  .\venv\Scripts\activate
  ```
- On Mac/Linux:
  ```bash
  source venv/bin/activate
  ```

---

## Step 3: Install Dependencies
```bash
pip install Flask Flask-SQLAlchemy Flask-Migrate
```

Optional tools:
```bash
pip install psycopg2-binary  # For PostgreSQL
pip install mysql-connector-python  # For MySQL
```

To freeze dependencies for version control:
```bash
pip freeze > requirements.txt
```

---

## Step 4: Initialize Flask Project Structure
```bash
mkdir app
cd app
```
Create the initial files:
```bash
touch __init__.py models.py routes.py
```
Sample directory structure:
```
flask_sqlalchemy_project/
├── venv/
├── requirements.txt
├── app/
│   ├── __init__.py
│   ├── models.py
│   └── routes.py
```

---

## Step 5: Environment Variables
Create a `.env` file in the root directory:
```
FLASK_APP=app
FLASK_ENV=development
DATABASE_URL=sqlite:///database.db
SECRET_KEY=supersecretkey
```

Install `python-dotenv` to load environment variables:
```bash
pip install python-dotenv
```
Update `__init__.py` to load environment variables:
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from dotenv import load_dotenv
import os

load_dotenv()

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = os.getenv('DATABASE_URL')
app.config['SECRET_KEY'] = os.getenv('SECRET_KEY')

# Initialize extensions
db = SQLAlchemy(app)
```

---

## Step 6: Run Initial Application
Create a `main.py` at the root level:
```python
from app import app

if __name__ == '__main__':
    app.run(debug=True)
```
Run the application:
```bash
flask run
```

---

## Optional: TailwindCSS Setup for Styling
Install TailwindCSS using Node.js:
```bash
npm install -D tailwindcss
npx tailwindcss init
```

Configure the input/output paths in `tailwind.config.js`, and watch for changes:
```bash
npx tailwindcss -i ./static/css/tailwind.css -o ./static/css/output.css --watch
```

---

## Best Practices
- Keep secrets secure using environment variables.
- Freeze and track dependencies with `requirements.txt`.
- Separate configuration logic into a `config.py` file for better maintainability.
- Use version control systems like Git for collaboration.

Now you're all set up to start building a Flask-SQLAlchemy project!

