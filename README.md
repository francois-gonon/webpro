# Library Management System API

A modern, RESTful API for library management built with FastAPI, SQLAlchemy, and SQLite. This system provides comprehensive book and user management capabilities with authentication, loan tracking, and administrative statistics.

## 📚 Features

### Core Functionality
- **Book Management**: Add, update, delete, and search books
- **User Management**: User registration, profiles, and role-based access
- **Loan System**: Book borrowing and returning with due date tracking
- **Authentication**: JWT-based authentication with role-based permissions
- **Statistics**: Administrative dashboard with loan and book statistics
- **RESTful API**: Well-documented API endpoints with OpenAPI/Swagger documentation

### Technical Features
- **FastAPI Framework**: High-performance, modern Python web framework
- **SQLAlchemy ORM**: Database abstraction with migrations via Alembic
- **JWT Authentication**: Secure token-based authentication
- **CORS Support**: Cross-origin resource sharing configuration
- **Database Migrations**: Version-controlled database schema changes
- **Repository Pattern**: Clean architecture with service and repository layers
- **Comprehensive Testing**: Unit tests for core functionality

## 🛠️ Technology Stack

- **Backend Framework**: FastAPI
- **Database**: SQLite (easily configurable for PostgreSQL/MySQL)
- **ORM**: SQLAlchemy 2.0
- **Authentication**: JWT with python-jose
- **Password Hashing**: bcrypt
- **Migrations**: Alembic
- **Testing**: pytest
- **ASGI Server**: Uvicorn

## 📁 Project Structure

```
library_app/
├── src/
│   ├── api/                    # API layer
│   │   ├── routes/            # API endpoints
│   │   │   ├── auth.py        # Authentication routes
│   │   │   ├── books.py       # Book management routes
│   │   │   ├── loans.py       # Loan management routes
│   │   │   ├── stats.py       # Statistics routes
│   │   │   └── users.py       # User management routes
│   │   ├── schemas/           # Pydantic models for request/response
│   │   └── dependencies.py    # Dependency injection
│   ├── models/                # SQLAlchemy database models
│   │   ├── books.py           # Book model
│   │   ├── loans.py           # Loan model
│   │   └── users.py           # User model
│   ├── repositories/          # Data access layer
│   ├── services/              # Business logic layer
│   ├── utils/                 # Utility functions
│   │   └── security.py        # Authentication utilities
│   ├── db/                    # Database configuration
│   ├── config.py              # Application configuration
│   └── main.py                # FastAPI application setup
├── alembic/                   # Database migrations
├── tests/                     # Test suite
├── requirements.txt           # Python dependencies
├── alembic.ini               # Alembic configuration
└── run.py                    # Application entry point
```

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- pip package manager

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd library_app
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up the database**
   ```bash
   # Run database migrations
   alembic upgrade head
   ```

5. **Start the development server**
   ```bash
   python run.py
   ```

The API will be available at `http://localhost:8000`

### API Documentation

Once the server is running, you can access:
- **Interactive API Documentation (Swagger UI)**: `http://localhost:8000/docs`
- **Alternative API Documentation (ReDoc)**: `http://localhost:8000/redoc`
- **OpenAPI JSON Schema**: `http://localhost:8000/api/v1/openapi.json`

## 📖 API Endpoints

### Authentication
- `POST /api/v1/auth/login` - User login (get access token)

### Books
- `GET /api/v1/books/` - List all books (paginated)
- `POST /api/v1/books/` - Create a new book (admin only)
- `GET /api/v1/books/{id}` - Get book details
- `PUT /api/v1/books/{id}` - Update book (admin only)
- `DELETE /api/v1/books/{id}` - Delete book (admin only)

### Users
- `GET /api/v1/users/` - List all users (admin only)
- `POST /api/v1/users/` - Create a new user
- `GET /api/v1/users/me` - Get current user profile
- `PUT /api/v1/users/{id}` - Update user
- `DELETE /api/v1/users/{id}` - Delete user (admin only)

### Loans
- `GET /api/v1/loans/` - List loans
- `POST /api/v1/loans/` - Create a new loan
- `GET /api/v1/loans/{id}` - Get loan details
- `PUT /api/v1/loans/{id}/return` - Return a book
- `DELETE /api/v1/loans/{id}` - Delete loan (admin only)

### Statistics
- `GET /api/v1/stats/` - Get library statistics (admin only)

## 🔐 Authentication

The API uses JWT (JSON Web Tokens) for authentication. To access protected endpoints:

1. **Login** to get an access token:
   ```bash
   curl -X POST "http://localhost:8000/api/v1/auth/login" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        -d "username=user@example.com&password=yourpassword"
   ```

2. **Use the token** in subsequent requests:
   ```bash
   curl -X GET "http://localhost:8000/api/v1/books/" \
        -H "Authorization: Bearer your-access-token"
   ```

## 🗄️ Database Schema

### Users
- `id`: Primary key
- `email`: Unique user email
- `hashed_password`: Bcrypt hashed password
- `full_name`: User's full name
- `is_active`: Account status
- `is_admin`: Administrator privileges

### Books
- `id`: Primary key
- `title`: Book title
- `author`: Book author
- `isbn`: Unique ISBN-13
- `publication_year`: Year of publication
- `description`: Book description
- `quantity`: Available copies

### Loans
- `id`: Primary key
- `user_id`: Foreign key to users
- `book_id`: Foreign key to books
- `loan_date`: When the book was borrowed
- `due_date`: When the book should be returned
- `return_date`: When the book was actually returned (nullable)

## ⚙️ Configuration

Configuration is managed through environment variables and the `src/config.py` file:

- `PROJECT_NAME`: Application name
- `API_V1_STR`: API version prefix (`/api/v1`)
- `SECRET_KEY`: JWT signing secret
- `ACCESS_TOKEN_EXPIRE_MINUTES`: Token expiration time
- `DATABASE_URL`: Database connection string
- `BACKEND_CORS_ORIGINS`: Allowed CORS origins

Create a `.env` file in the project root to override default settings:

```env
SECRET_KEY=your-super-secret-key-here
DATABASE_URL=sqlite:///./library.db
ACCESS_TOKEN_EXPIRE_MINUTES=11520
```

## 🧪 Testing

Run the test suite:

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src

# Run specific test file
pytest tests/services/test_users.py
```

## 🔄 Database Migrations

The project uses Alembic for database migrations:

```bash
# Create a new migration
alembic revision --autogenerate -m "Description of changes"

# Apply migrations
alembic upgrade head

# Rollback to previous migration
alembic downgrade -1

# View migration history
alembic history
```

## 📦 Deployment

### Production Setup

1. **Environment Variables**: Set production environment variables
2. **Database**: Configure production database (PostgreSQL recommended)
3. **ASGI Server**: Use Gunicorn with Uvicorn workers:
   ```bash
   pip install gunicorn
   gunicorn src.main:app -w 4 -k uvicorn.workers.UvicornWorker
   ```

### Docker Deployment

Create a `Dockerfile`:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## 📞 Support

For support, please open an issue in the GitHub repository or contact the development team.

---

**Built with ❤️ using FastAPI and modern Python practices**