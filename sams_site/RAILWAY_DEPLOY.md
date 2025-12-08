# SAMS - Railway Deployment Guide

## 🚀 Quick Deploy to Railway

Railway will automatically deploy your Django app from GitHub with these files in place.

### Step 1: Push to GitHub

```bash
cd F:\Desktop\WT\2025FA Web App Dev (CIDM-6325-70)\CIDM-Repo\CIDM6325\CIDM6325\sams_site
git add .
git commit -m "Add Railway deployment configuration"
git push origin project_django_app/feature/assignment-N-topic
```

### Step 2: Deploy on Railway

1. **Go to Railway**: https://railway.app
2. **Sign up/Login** with your GitHub account
3. **Click "New Project"**
4. **Select "Deploy from GitHub repo"**
5. **Choose** `zanehill01/CIDM6325` repository
6. **Select branch**: `project_django_app/feature/assignment-N-topic`
7. **Set root directory**: `/CIDM6325/sams_site` (since your Django project is nested)

### Step 3: Add PostgreSQL Database

1. In your Railway project, click **"+ New"**
2. Select **"Database"** → **"PostgreSQL"**
3. Railway will automatically set the `DATABASE_URL` environment variable

### Step 4: Configure Environment Variables

In Railway project settings → **Variables**, add:

```
SECRET_KEY=your-super-secret-key-generate-a-new-one-here
DEBUG=False
ALLOWED_HOSTS=.railway.app
```

**Generate a secure SECRET_KEY:**
```python
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

### Step 5: Run Migrations

In Railway project → **Settings** → **Deploy**, add a **deploy command**:

```bash
python manage.py migrate && python manage.py collectstatic --noinput
```

Or use the Railway CLI to run migrations manually:

```bash
railway run python manage.py migrate
railway run python manage.py createsuperuser
```

### Step 6: Create 20 Students (Production Data)

After deployment, use Railway shell:

```bash
railway run python manage.py shell
```

Then run:
```python
from sams.models import Student
students = ['Michael Johnson', 'Emily Davis', 'David Martinez', 'Sarah Wilson', 'James Brown', 'Jessica Taylor', 'Christopher Anderson', 'Ashley Thomas', 'Matthew Jackson', 'Amanda White', 'Joshua Harris', 'Samantha Martin', 'Andrew Thompson', 'Elizabeth Garcia', 'Daniel Rodriguez', 'Jennifer Lee', 'Ryan Walker', 'Megan Hall', 'Tyler Allen', 'Lauren Young']
for i, name in enumerate(students):
    Student.objects.get_or_create(name=name, student_id=f'S{str(i+1).zfill(3)}')
print(f'Created {Student.objects.count()} students')
```

### Step 7: Access Your Site

Railway will provide a URL like: `https://your-app-name.railway.app`

---

## 🔧 Troubleshooting

### Static Files Not Loading
- Make sure `python manage.py collectstatic` runs in deploy command
- Check `STATIC_ROOT` is set in settings.py ✓
- WhiteNoise middleware is installed ✓

### Database Connection Error
- Ensure PostgreSQL addon is added
- Check `DATABASE_URL` environment variable exists
- Run migrations: `railway run python manage.py migrate`

### 500 Error
- Set `DEBUG=True` temporarily to see error details
- Check Railway logs: **Deployments** → **View Logs**
- Ensure `ALLOWED_HOSTS` includes `.railway.app`

### CSRF Token Error
- Add your Railway domain to `ALLOWED_HOSTS`
- Check `CSRF_TRUSTED_ORIGINS` if needed:
  ```python
  CSRF_TRUSTED_ORIGINS = ['https://*.railway.app']
  ```

---

## 📝 Files Created for Deployment

- ✅ `requirements.txt` - Python dependencies
- ✅ `Procfile` - Tells Railway how to run your app
- ✅ `runtime.txt` - Specifies Python version
- ✅ `.env.example` - Template for environment variables
- ✅ Updated `settings.py` - Production-ready configuration

---

## 🔄 Continuous Deployment

Railway automatically redeploys when you push to GitHub:

```bash
git add .
git commit -m "Update feature"
git push
```

Railway detects the push and deploys automatically! 🎉

---

## 🔐 Security Checklist

- ✅ `DEBUG=False` in production
- ✅ `SECRET_KEY` from environment variable
- ✅ `ALLOWED_HOSTS` properly configured
- ✅ PostgreSQL instead of SQLite
- ✅ WhiteNoise for static files
- ✅ `.env` in `.gitignore` (don't commit secrets!)

---

## 💰 Railway Pricing

- **Free Tier**: $5 credit/month (enough for small apps)
- **Hobby Plan**: $5/month for more resources
- Your SAMS app should run fine on free tier for demo/class purposes

---

## Alternative: One-Click Deploy

Add this button to your README for instant deployment:

```markdown
[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/new/template?template=https://github.com/zanehill01/CIDM6325)
```

---

**Your app is now ready to deploy! 🚀**

Need help? Check Railway docs: https://docs.railway.app/
