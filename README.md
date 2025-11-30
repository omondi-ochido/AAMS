# Athlete & Athletics Management System (AAMS)

A unified, cloud-based digital platform designed to digitize the entire athletics value chain—from initial talent registration to advanced risk prevention.

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com)

## 🚀 Quick Start

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/aams.git
   cd aams
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your settings
   ```

5. **Run migrations**
   ```bash
   python manage.py migrate
   ```

6. **Create superuser**
   ```bash
   python manage.py createsuperuser
   ```

7. **Run development server**
   ```bash
   python manage.py runserver
   ```

8. **Access the application**
   - Web: http://localhost:8000
   - Admin: http://localhost:8000/admin

### Production Deployment

See [DEPLOYMENT.md](DEPLOYMENT.md) for detailed instructions on deploying to Render.

## 🎯 Core Functionality

The AAMS is structured into modules that ensure every operational aspect is seamlessly covered:

### 1. 🏅 Athlete & Talent Onboarding

This module replaces paper-based processes with digital efficiency.

**Features:**
- Secure online registration
- Digital issuance of a permanent Unique Athlete ID (UAID)
- Digital workflow for submitting and approving camp accreditation applications

**Models:**
- `Athlete` - Athlete profiles with UAID
- `AthleticsCamp` - Camp information and management
- `CampAccreditation` - Camp application and approval workflow

### 2. 🚨 Performance & Alert Coordination

This module ensures real-time communication and logistical precision.

**Features:**
- Centralized calendar for training, travel, and competition
- Automated push and SMS notifications for schedule changes
- Integrated attendance tracking using geo-fencing at training venues

**Models:**
- `CalendarEvent` - Training, competition, and travel events
- `Alert` - Notifications and alerts
- `Attendance` - Attendance tracking with geo-fencing

### 3. 🩹 Health, Risk, & Wellness (AI-Powered)

This is the system's most crucial and innovative component, focused on predictive prevention.

**Features:**
- **Digital Medical Passport**: Secure history of all injuries, treatments, and return-to-play clearances linked to the athlete's UAID
- **Wearable Integration**: Connects with smart devices to ingest physiological data (sleep, heart rate variability, training load)
- **AI-Driven Risk Engine**: Uses Machine Learning to analyze real-time and historical data, generating a Predictive Injury Risk Score for every athlete

**Models:**
- `MedicalPassport` - Medical history and information
- `Injury` - Injury tracking and management
- `WearableDevice` - Wearable device integration
- `PhysiologicalData` - Data from wearable devices
- `InjuryRiskScore` - AI-generated risk scores
- `RiskAlert` - Automated risk alerts
- `NutritionPlan` - Nutrition plans for athletes
- `NutritionLog` - Daily nutrition tracking

## 🧠 The Competitive Edge: AI Integration

The integration of the Predictive Injury Risk Engine sets AAMS apart by transforming management from reactive to predictive:

- **Prediction**: The AI model continuously processes data to assess the likelihood of a non-contact injury (e.g., stress fracture, hamstring strain)
- **Actionable Alerts**: When an athlete's risk score exceeds a threshold, the system automatically sends specific, actionable alerts to the appropriate users:
  - Coaches receive suggestions for immediate training load reduction
  - Athletes receive personalized prompts regarding nutrition, hydration, or sleep optimization required to mitigate the risk
  - Medical Teams are flagged with the top risk factors identified by the AI

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- PostgreSQL (optional, SQLite used by default)
- Redis (for Celery and Channels)
- Virtual environment (recommended)

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd AAMS
   ```

2. **Create and activate virtual environment**
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

4. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

5. **Run migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create superuser**
   ```bash
   python manage.py createsuperuser
   ```

7. **Run development server**
   ```bash
   python manage.py runserver
   ```

8. **Run Celery worker** (in a separate terminal)
   ```bash
   celery -A aams worker -l info
   ```

9. **Run Celery beat** (for scheduled tasks, in a separate terminal)
   ```bash
   celery -A aams beat -l info
   ```

## 📁 Project Structure

```
AAMS/
├── aams/                  # Main project directory
│   ├── settings.py        # Django settings
│   ├── urls.py           # Main URL configuration
│   ├── wsgi.py           # WSGI configuration
│   ├── asgi.py           # ASGI configuration
│   └── celery.py         # Celery configuration
├── core/                  # Core app (User model, authentication)
├── onboarding/            # Athlete onboarding module
├── coordination/          # Performance & alert coordination module
├── health_risk/           # Health, risk & wellness module
├── manage.py             # Django management script
├── requirements.txt      # Python dependencies
└── README.md            # This file
```

## 🔌 API Endpoints

### Authentication
- `POST /api/auth/register/` - User registration
- `POST /api/auth/login/` - User login
- `GET /api/auth/users/me/` - Get current user profile

### Onboarding
- `GET /api/onboarding/athletes/` - List athletes
- `POST /api/onboarding/athletes/` - Register athlete
- `GET /api/onboarding/athletes/me/` - Get current athlete profile
- `GET /api/onboarding/camps/` - List camps
- `POST /api/onboarding/camps/` - Create camp
- `GET /api/onboarding/accreditations/` - List accreditations
- `POST /api/onboarding/accreditations/` - Apply for camp accreditation

### Coordination
- `GET /api/coordination/events/` - List calendar events
- `POST /api/coordination/events/` - Create event
- `GET /api/coordination/alerts/` - List alerts
- `POST /api/coordination/alerts/` - Create alert
- `POST /api/coordination/attendance/check-in/<event_id>/` - Check in to event
- `GET /api/coordination/attendance/` - List attendance records

### Health & Risk
- `GET /api/health-risk/medical-passports/me/` - Get medical passport
- `GET /api/health-risk/injuries/` - List injuries
- `POST /api/health-risk/injuries/` - Create injury record
- `GET /api/health-risk/wearable-devices/` - List wearable devices
- `POST /api/health-risk/wearable-devices/` - Register wearable device
- `GET /api/health-risk/physiological-data/` - List physiological data
- `POST /api/health-risk/physiological-data/` - Add physiological data
- `GET /api/health-risk/risk-scores/latest/` - Get latest risk score
- `POST /api/health-risk/risk-scores/calculate/<athlete_id>/` - Calculate risk score
- `GET /api/health-risk/risk-alerts/` - List risk alerts
- `GET /api/health-risk/nutrition-plans/` - List nutrition plans
- `POST /api/health-risk/nutrition-plans/` - Create nutrition plan
- `GET /api/health-risk/nutrition-logs/` - List nutrition logs
- `POST /api/health-risk/nutrition-logs/` - Log nutrition

## 🔐 Authentication

The API uses token-based authentication. Include the token in the Authorization header:

```
Authorization: Token <your-token>
```

## 🤖 AI Risk Engine

The AI Risk Engine analyzes multiple factors to generate injury risk scores:

1. **Sleep Quality** (20% weight) - Sleep duration and quality scores
2. **Training Load** (25% weight) - Training load trends and absolute values
3. **Heart Rate Variability** (15% weight) - HRV indicators
4. **Injury History** (20% weight) - Recent injuries and recovery status
5. **Recovery Indicators** (20% weight) - Resting heart rate, training stress, etc.

Risk levels:
- **Low**: 0-40
- **Moderate**: 40-60
- **High**: 60-80
- **Critical**: 80-100

## 📊 Database Models

### Core Models
- `User` - Custom user model with roles (athlete, coach, medical, admin, staff)

### Onboarding Models
- `Athlete` - Athlete profile with UAID
- `AthleticsCamp` - Camp information
- `CampAccreditation` - Camp applications

### Coordination Models
- `CalendarEvent` - Events (training, competition, travel)
- `Alert` - Notifications and alerts
- `Attendance` - Attendance tracking

### Health & Risk Models
- `MedicalPassport` - Medical information
- `Injury` - Injury records
- `WearableDevice` - Wearable device integration
- `PhysiologicalData` - Physiological metrics
- `InjuryRiskScore` - AI-generated risk scores
- `RiskAlert` - Risk alerts
- `NutritionPlan` - Nutrition plans
- `NutritionLog` - Nutrition logs

## 🔧 Configuration

### Environment Variables

See `.env.example` for all available environment variables.

Key settings:
- `SECRET_KEY` - Django secret key
- `DEBUG` - Debug mode (set to False in production)
- `ALLOWED_HOSTS` - Allowed hosts
- `DB_*` - Database configuration
- `CELERY_BROKER_URL` - Celery broker URL
- `TWILIO_*` - Twilio configuration for SMS

### Database

By default, the project uses SQLite. For production, use PostgreSQL:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST'),
        'PORT': config('DB_PORT'),
    }
}
```

## 🧪 Testing

```bash
python manage.py test
```

## 📝 License

[Your License Here]

## 👥 Contributors

[Your Name/Team]

## 📞 Support

For support, email [your-email] or open an issue in the repository.

