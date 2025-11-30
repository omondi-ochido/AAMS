# Deployment Guide - AAMS to Render

This guide will help you deploy the AAMS (Athlete & Athletics Management System) to Render.

## Prerequisites

1. A GitHub account
2. A Render account (sign up at [render.com](https://render.com))
3. Git installed on your local machine

## Step 1: Prepare Your Repository

### 1.1 Initialize Git (if not already done)

```bash
git init
git add .
git commit -m "Initial commit - AAMS ready for deployment"
```

### 1.2 Create GitHub Repository

1. Go to [GitHub](https://github.com) and create a new repository
2. Name it `aams` (or your preferred name)
3. **Do NOT** initialize with README, .gitignore, or license
4. Copy the repository URL

### 1.3 Push to GitHub

```bash
git remote add origin https://github.com/YOUR_USERNAME/aams.git
git branch -M main
git push -u origin main
```

## Step 2: Deploy to Render

### 2.1 Create New Web Service

1. Log in to [Render Dashboard](https://dashboard.render.com)
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub account if not already connected
4. Select your repository: `aams`

### 2.2 Configure Web Service

**Basic Settings:**
- **Name**: `aams-web` (or your preferred name)
- **Region**: Choose closest to your users
- **Branch**: `main`
- **Root Directory**: Leave empty (root of repo)
- **Runtime**: `Python 3`
- **Build Command**: 
  ```bash
  pip install -r requirements.txt && python manage.py collectstatic --noinput && python manage.py migrate
  ```
- **Start Command**: 
  ```bash
  gunicorn aams.wsgi:application --bind 0.0.0.0:$PORT
  ```

### 2.3 Create PostgreSQL Database

1. In Render Dashboard, click **"New +"** → **"PostgreSQL"**
2. Configure:
   - **Name**: `aams-db`
   - **Database**: `aams`
   - **User**: `aams_user` (or auto-generated)
   - **Region**: Same as web service
   - **Plan**: Starter (free tier available)

3. Copy the **Internal Database URL** from the database dashboard

### 2.4 Create Redis Instance (Optional but Recommended)

1. In Render Dashboard, click **"New +"** → **"Redis"**
2. Configure:
   - **Name**: `aams-redis`
   - **Region**: Same as web service
   - **Plan**: Starter (free tier available)

### 2.5 Configure Environment Variables

In your Web Service settings, go to **"Environment"** and add:

**Required Variables:**
```
SECRET_KEY=<generate-a-secure-random-key>
DEBUG=False
ALLOWED_HOSTS=your-app-name.onrender.com
DATABASE_URL=<from-postgres-database-internal-url>
```

**Optional Variables (if using Redis):**
```
REDIS_HOST=<from-redis-instance>
REDIS_PORT=<from-redis-instance>
CELERY_BROKER_URL=<from-redis-instance>
CELERY_RESULT_BACKEND=<from-redis-instance>
```

**Optional Variables (if using Twilio):**
```
TWILIO_ACCOUNT_SID=<your-twilio-sid>
TWILIO_AUTH_TOKEN=<your-twilio-token>
TWILIO_PHONE_NUMBER=<your-twilio-number>
```

### 2.6 Generate SECRET_KEY

You can generate a secure secret key using Python:

```python
from django.core.management.utils import get_random_secret_key
print(get_random_secret_key())
```

Or use an online generator: [Django Secret Key Generator](https://djecrety.ir/)

## Step 3: Deploy

1. Click **"Create Web Service"**
2. Render will automatically:
   - Clone your repository
   - Install dependencies
   - Run migrations
   - Collect static files
   - Start your application

3. Wait for deployment to complete (usually 2-5 minutes)

## Step 4: Post-Deployment Setup

### 4.1 Create Superuser

1. Go to your service dashboard
2. Click **"Shell"** tab
3. Run:
   ```bash
   python manage.py createsuperuser
   ```
4. Follow the prompts to create an admin account

### 4.2 Verify Deployment

1. Visit your app URL: `https://your-app-name.onrender.com`
2. Test the login page
3. Access admin panel: `https://your-app-name.onrender.com/admin`

## Step 5: Continuous Deployment

Render automatically deploys when you push to the `main` branch. To deploy:

```bash
git add .
git commit -m "Your commit message"
git push origin main
```

## Troubleshooting

### Build Fails

1. Check build logs in Render dashboard
2. Ensure all dependencies are in `requirements.txt`
3. Verify Python version compatibility

### Database Connection Issues

1. Ensure `DATABASE_URL` is set correctly
2. Use **Internal Database URL** (not external)
3. Check database is running

### Static Files Not Loading

1. Verify `collectstatic` runs in build command
2. Check `STATIC_ROOT` setting
3. Ensure WhiteNoise middleware is enabled

### 500 Internal Server Error

1. Check application logs in Render dashboard
2. Verify all environment variables are set
3. Check database migrations completed successfully

## Environment Variables Reference

| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| `SECRET_KEY` | Yes | Django secret key | `django-insecure-...` |
| `DEBUG` | Yes | Debug mode | `False` |
| `ALLOWED_HOSTS` | Yes | Allowed hostnames | `your-app.onrender.com` |
| `DATABASE_URL` | Yes | PostgreSQL connection string | `postgresql://...` |
| `REDIS_HOST` | No | Redis host | `redis-xxx.render.com` |
| `REDIS_PORT` | No | Redis port | `6379` |
| `CELERY_BROKER_URL` | No | Celery broker URL | `redis://...` |
| `CELERY_RESULT_BACKEND` | No | Celery result backend | `redis://...` |
| `TWILIO_ACCOUNT_SID` | No | Twilio account SID | `AC...` |
| `TWILIO_AUTH_TOKEN` | No | Twilio auth token | `...` |
| `TWILIO_PHONE_NUMBER` | No | Twilio phone number | `+1234567890` |

## Additional Resources

- [Render Documentation](https://render.com/docs)
- [Django Deployment Checklist](https://docs.djangoproject.com/en/4.2/howto/deployment/checklist/)
- [WhiteNoise Documentation](https://whitenoise.readthedocs.io/)

## Support

For issues specific to this deployment, check:
1. Render build logs
2. Render application logs
3. Django error logs in Render shell

