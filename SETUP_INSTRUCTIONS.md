# WaaS Portal - Setup Instructions

## Prerequisites
- Python 3.8 or higher
- PostgreSQL 12 or higher
- pip (Python package manager)

## Database Setup

1. **Install PostgreSQL** (if not already installed)
   ```bash
   # Ubuntu/Debian
   sudo apt update
   sudo apt install postgresql postgresql-contrib
   
   # macOS with Homebrew
   brew install postgresql
   
   # Windows: Download from https://www.postgresql.org/download/windows/
   ```

2. **Create Database and User**
   ```bash
   sudo -u postgres psql
   ```
   
   ```sql
   CREATE DATABASE waas_db;
   CREATE USER waas_user WITH PASSWORD 'your_secure_password';
   GRANT ALL PRIVILEGES ON DATABASE waas_db TO waas_user;
   ALTER USER waas_user CREATEDB;
   \q
   ```

## Project Setup

1. **Clone/Download the project** and navigate to the project directory

2. **Create Virtual Environment**
   ```bash
   python -m venv venv
   
   # Activate virtual environment
   # On Windows:
   venv\Scripts\activate
   
   # On macOS/Linux:
   source venv/bin/activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Environment Configuration**
   ```bash
   # Copy the example environment file
   cp .env.example .env
   
   # Edit .env file with your actual values
   # Make sure to update:
   # - SECRET_KEY (generate a new one for production)
   # - DATABASE_URL with your actual database credentials
   # - DEBUG=False for production
   ```

5. **Database Migration**
   ```bash
   # Create and apply migrations
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create Superuser**
   ```bash
   python manage.py createsuperuser
   ```

7. **Load Initial Data (Optional)**
   ```bash
   # You can create some initial subscription plans via Django admin or shell
   python manage.py shell
   ```
   
   ```python
   from subscriptions.models import SubscriptionPlan
   
   # Create sample subscription plans
   SubscriptionPlan.objects.create(name="1 Hour Pass", duration_hours=1, price_kes=10.00)
   SubscriptionPlan.objects.create(name="3 Hour Pass", duration_hours=3, price_kes=25.00)
   SubscriptionPlan.objects.create(name="Daily Pass", duration_hours=24, price_kes=50.00)
   SubscriptionPlan.objects.create(name="Weekly Pass", duration_hours=168, price_kes=200.00)
   ```

8. **Run Development Server**
   ```bash
   python manage.py runserver
   ```

## Application Structure

```
WaaSPortal/
├── manage.py
├── requirements.txt
├── .env.example
├── WaaSPortal/          # Main project settings
│   ├── settings.py
│   ├── urls.py
│   └── ...
├── core_app/            # General utilities and settings
├── users/               # User authentication and profiles
├── subscriptions/       # Wi-Fi plans and subscriptions
├── payments/            # Payment transactions (M-Pesa integration)
└── sessions/            # Wi-Fi session tracking
```

## API Endpoints

### Authentication
- `POST /api/auth/register/` - User registration
- `POST /api/auth/login/` - User login
- `GET/PUT /api/auth/profile/` - User profile

### Subscriptions
- `GET /api/subscriptions/plans/` - List active subscription plans
- `GET /api/subscriptions/plans/{id}/` - Get specific plan details

### Payments
- `POST /api/payments/initiate/` - Initiate M-Pesa payment
- `GET /api/payments/transactions/` - List user's payment transactions
- `POST /api/payments/mpesa-callback/` - M-Pesa callback endpoint

### Sessions
- `GET /api/sessions/` - List user's Wi-Fi sessions
- `POST /api/sessions/create/` - Create new Wi-Fi session
- `GET /api/sessions/active/` - Get current active session
- `POST /api/sessions/{id}/end/` - End active session

## Next Steps

1. **M-Pesa Integration**: Implement actual M-Pesa STK Push integration in the payments app
2. **RADIUS Integration**: Connect with your RADIUS server for Wi-Fi authentication
3. **Frontend Development**: Build a user-friendly frontend interface
4. **Production Deployment**: Set up proper production environment with security considerations

## Admin Interface

Access the Django admin at `http://localhost:8000/admin/` using your superuser credentials to:
- Manage users
- Create/edit subscription plans
- Monitor payment transactions
- Track Wi-Fi sessions

## Important Notes

- Always use environment variables for sensitive information
- Ensure proper database backups in production
- Implement proper logging and monitoring
- Consider rate limiting for API endpoints
- Use HTTPS in production
- Implement proper error handling and validation