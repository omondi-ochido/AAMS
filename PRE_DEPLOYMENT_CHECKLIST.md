# Pre-Deployment Checklist

Use this checklist before deploying to GitHub and Render.

## ✅ Code Cleanup

- [x] Deleted irrelevant documentation files
- [x] Removed development-only scripts (.bat files)
- [x] Updated .gitignore to exclude unnecessary files
- [x] Removed sensitive data from code

## ✅ Configuration Files

- [x] Created `render.yaml` for Render deployment
- [x] Created `.env.example` template
- [x] Updated `requirements.txt` with production dependencies
- [x] Updated `settings.py` for production (PostgreSQL, WhiteNoise, Security)
- [x] Created `build.sh` for build process
- [x] Created `DEPLOYMENT.md` with detailed instructions

## ✅ Security

- [x] SECRET_KEY will be set via environment variable
- [x] DEBUG set to False in production
- [x] Security headers configured (HSTS, SSL redirect, etc.)
- [x] CSRF and Session cookies secure in production
- [x] .env file excluded from git

## ✅ Database

- [x] Database configuration supports PostgreSQL via DATABASE_URL
- [x] Migrations are included in repository
- [x] Database backup strategy documented

## ✅ Static Files

- [x] WhiteNoise middleware configured
- [x] STATIC_ROOT and STATIC_URL configured
- [x] collectstatic command in build script

## ✅ Documentation

- [x] README.md updated with deployment instructions
- [x] DEPLOYMENT.md created with step-by-step guide
- [x] .env.example created with all required variables

## 📋 Before Pushing to GitHub

1. **Review .gitignore**
   ```bash
   cat .gitignore
   ```

2. **Check for sensitive data**
   ```bash
   # Search for potential secrets
   grep -r "SECRET_KEY" --exclude-dir=venv --exclude=".env*"
   grep -r "password" --exclude-dir=venv --exclude=".env*" | grep -i "="
   ```

3. **Test locally**
   ```bash
   python manage.py check --deploy
   python manage.py collectstatic --noinput
   python manage.py migrate
   ```

4. **Commit changes**
   ```bash
   git add .
   git commit -m "Prepare for deployment to Render"
   ```

5. **Push to GitHub**
   ```bash
   git push origin main
   ```

## 📋 Before Deploying to Render

1. **Create GitHub repository**
   - Create new repo on GitHub
   - Push code to repository

2. **Set up Render services**
   - Create PostgreSQL database
   - Create Redis instance (optional)
   - Create Web Service

3. **Configure environment variables**
   - Set SECRET_KEY (generate new one)
   - Set DEBUG=False
   - Set ALLOWED_HOSTS
   - Set DATABASE_URL (from PostgreSQL)
   - Set Redis variables (if using)

4. **Deploy and verify**
   - Check build logs
   - Verify application starts
   - Test admin panel
   - Create superuser

## 🚨 Important Notes

- **Never commit `.env` file** - It contains sensitive data
- **Generate new SECRET_KEY** for production
- **Use Render's Internal Database URL** for DATABASE_URL
- **Test all functionality** after deployment
- **Monitor logs** for any errors

## 📞 Need Help?

Refer to [DEPLOYMENT.md](DEPLOYMENT.md) for detailed instructions.

