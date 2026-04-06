# PayIt v3 - Agricultural Marketplace API

A FastAPI-based backend application that connects farmers with buyers in an agricultural marketplace. This platform enables farmers to list their agricultural products and buyers to browse and purchase items seamlessly.

## 🚀 Features

- **User Management**: Role-based authentication (Farmers/Buyers)
- **Product Catalog**: Comprehensive product management with categories
- **Order System**: Complete order lifecycle management
- **Authentication**: JWT-based auth with OAuth integration (Auth0)
- **File Upload**: Image upload for products with validation
- **Database**: MySQL with SQLAlchemy ORM and Alembic migrations
- **Docker Support**: Fully containerized deployment

## 📋 Prerequisites

- Docker and Docker Compose
- Python 3.13+ (for local development)
- MySQL 8.0+ (if running locally)
- Auth0 account (for OAuth integration)

## 🛠️ Technology Stack

- **Backend**: FastAPI 0.104.1
- **Database**: MySQL 8.0
- **ORM**: SQLAlchemy
- **Authentication**: JWT + Auth0 OAuth
- **File Storage**: Local filesystem with aiofiles
- **Containerization**: Docker & Docker Compose
- **Database Migrations**: Alembic

## 📦 Installation & Setup

### Using Docker (Recommended)

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd payit_v3
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the services**
   ```bash
   docker-compose up --build
   ```

4. **Access the application**
   - API: http://localhost:8000
   - API Documentation: http://localhost:8000/docs
   - phpMyAdmin: http://localhost:8080

### Local Development

1. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Set up environment variables**
   ```bash
   export DB_HOST=localhost
   export DB_PORT=3306
   export DB_USER=your_username
   export DB_PASSWORD=your_password
   export DB_DATABASE=payit_db
   export JWT_SECRET_KEY=your_secret_key
   export JWT_EXPIRATION_TIME=30
   export JWT_ALGORITHM=HS256
   export AUTH0_DOMAIN=your_auth0_domain
   export AUTH0_CLIENT_ID=your_auth0_client_id
   export AUTH0_CLIENT_SECRET=your_auth0_client_secret
   export AUTH0_CALLBACK_URL=http://localhost:8000/oauth/callback
   ```

3. **Run the application**
   ```bash
   uvicorn main:app --reload --host 0.0.0.0 --port 8000
   ```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `DB_HOST` | Database host | Yes |
| `DB_PORT` | Database port | Yes |
| `DB_USER` | Database username | Yes |
| `DB_PASSWORD` | Database password | Yes |
| `DB_DATABASE` | Database name | Yes |
| `JWT_SECRET_KEY` | JWT signing secret | Yes |
| `JWT_EXPIRATION_TIME` | Token expiration (minutes) | Yes |
| `JWT_ALGORITHM` | JWT algorithm | Yes |
| `AUTH0_DOMAIN` | Auth0 domain | Yes |
| `AUTH0_CLIENT_ID` | Auth0 client ID | Yes |
| `AUTH0_CLIENT_SECRET` | Auth0 client secret | Yes |
| `AUTH0_CALLBACK_URL` | OAuth callback URL | Yes |

### Database Setup

The application automatically creates database tables on startup. For production, consider using Alembic migrations:

```bash
# Generate migration
alembic revision --autogenerate -m "Initial migration"

# Apply migrations
alembic upgrade head
```

## 📚 API Documentation

### Authentication Endpoints

- `POST /auth/login` - User login with JWT
- `GET /oauth/login` - OAuth login with Auth0
- `GET /oauth/callback` - OAuth callback handler
- `GET /oauth/logout` - OAuth logout

### Product Endpoints

- `POST /products` - Create new product (Farmer only)
- `GET /products` - Get all products
- `GET /products/{id}` - Get specific product
- `PUT /products/{id}` - Update product (Owner only)
- `DELETE /products/{id}` - Delete product (Owner only)
- `GET /farmers/products/{farmer_id}` - Get farmer's products

### User Endpoints

- `POST /users` - Create new user
- `GET /users/{id}` - Get user details
- `PUT /users/{id}` - Update user profile
- `DELETE /users/{id}` - Delete user account

### Order Endpoints

- `POST /orders` - Create new order
- `GET /orders` - Get all orders
- `GET /orders/{id}` - Get specific order
- `PUT /orders/{id}` - Update order status

## 🏗️ Project Structure

```
payit_v3/
├── app/
│   ├── models/              # SQLAlchemy models
│   │   ├── users_model.py
│   │   ├── products_model.py
│   │   ├── orders_model.py
│   │   └── ...
│   ├── routes/              # API endpoints
│   │   ├── auth_routes.py
│   │   ├── products_routes.py
│   │   ├── users_routes.py
│   │   └── ...
│   ├── schemas/             # Pydantic models
│   ├── auth/                # JWT handling
│   ├── middlewares/         # Custom middleware
│   ├── config/              # Configuration
│   └── enums.py             # Application enums
├── docker-compose.yml       # Docker orchestration
├── Dockerfile              # Application container
├── requirements.txt        # Python dependencies
└── alembic.ini            # Database migration config
```

## 🎯 Product Categories

The platform supports the following agricultural categories:

- **Grains** - Wheat, rice, barley, etc.
- **Tubers** - Potatoes, yams, cassava, etc.
- **Cereals** - Corn, oats, sorghum, etc.
- **Fruits** - Various fruits and berries
- **Livestock** - Animals and animal products
- **Vegetables** - Fresh vegetables
- **Oils** - Cooking oils and fats
- **Latex** - Rubber and latex products

## 🔐 Authentication

### JWT Authentication
- Users can login with email/password
- JWT tokens are returned for authenticated sessions
- Tokens expire based on `JWT_EXPIRATION_TIME` setting

### OAuth Integration
- Auth0 integration for social login
- Supports Google, Facebook, and other Auth0 providers
- Automatic user creation for first-time OAuth users

## 📝 API Usage Examples

### Login with JWT
```bash
curl -X POST "http://localhost:8000/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "password123"
  }'
```

### Create Product (Authenticated)
```bash
curl -X POST "http://localhost:8000/products" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -F "name=Fresh Tomatoes" \
  -F "description=Organic fresh tomatoes" \
  -F "category=vegetables" \
  -F "unit_price=50" \
  -F "quantity=100" \
  -F "image=@product_image.jpg"
```

## 🧪 Testing

Run tests with pytest:
```bash
pytest tests/
```

## 🚀 Deployment

### Production Considerations

1. **Environment Variables**: Use secure values for production
2. **Database**: Configure connection pooling
3. **File Storage**: Consider cloud storage for images
4. **SSL**: Enable HTTPS in production
5. **Rate Limiting**: Implement API rate limiting
6. **Monitoring**: Add application monitoring

### Docker Production Deployment

```bash
# Build and deploy
docker-compose -f docker-compose.prod.yml up -d

# Check logs
docker-compose logs -f payit.api
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🐛 Known Issues

- OAuth callback has a syntax error (needs fixing)
- Some error handling inconsistencies
- Missing input validation in some endpoints

## 📞 Support

For support and questions:
- Create an issue in the repository
- Contact the development team

## 🔄 Version History

- **v3.0.0** - Current version with FastAPI and Docker support
- **v2.0.0** - Previous version with basic functionality
- **v1.0.0** - Initial release

---

**Built with ❤️ for the agricultural community**
