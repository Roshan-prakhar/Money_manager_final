# 🚀 Money Manager - Render Deployment Guide

## Prerequisites
1. **GitHub Account** - Push your code to GitHub
2. **Render Account** - Sign up at [render.com](https://render.com)
3. **Git Repository** - Your code should be in a GitHub repository

## Step 1: Prepare Your Code

### Backend Changes Made:
- ✅ Updated `application.properties` to use environment variables
- ✅ Created `render.yaml` for backend configuration
- ✅ Configured database connection for production

### Frontend Changes Made:
- ✅ Updated `apiEndpoints.js` to use environment variables
- ✅ Created `render.yaml` for frontend configuration

## Step 2: Push to GitHub

```bash
# Navigate to your backend directory
cd "C:\Users\BIT\Downloads\money manager\moneymanager"

# Initialize git if not already done
git init
git add .
git commit -m "Prepare backend for Render deployment"

# Create GitHub repository and push
git remote add origin https://github.com/YOUR_USERNAME/moneymanager-backend.git
git push -u origin main
```

```bash
# Navigate to your frontend directory
cd "C:\Users\BIT\Downloads\money manager\moneymanagerwebapp"

# Initialize git if not already done
git init
git add .
git commit -m "Prepare frontend for Render deployment"

# Create GitHub repository and push
git remote add origin https://github.com/YOUR_USERNAME/moneymanager-frontend.git
git push -u origin main
```

## Step 3: Deploy Backend to Render

1. **Go to [render.com](https://render.com)** and sign in
2. **Click "New +"** → **"Web Service"**
3. **Connect your GitHub repository** (moneymanager-backend)
4. **Configure the service:**
   - **Name**: `moneymanager-backend`
   - **Environment**: `Java`
   - **Build Command**: `./mvnw clean package -DskipTests`
   - **Start Command**: `java -jar target/moneymanager.jar`
   - **Plan**: `Free`

5. **Add Environment Variables:**
   - `SPRING_PROFILES_ACTIVE` = `prod`
   - `JWT_SECRET` = (generate a random string)
   - `MONEY_MANAGER_FRONTEND_URL` = `https://moneymanager-frontend.onrender.com`
   - `APP_ACTIVATION_URL` = `https://moneymanager-backend.onrender.com/`

6. **Create Database:**
   - Click **"New +"** → **"PostgreSQL"**
   - **Name**: `moneymanager-db`
   - **Plan**: `Free`
   - **Database Name**: `moneymanager`
   - **User**: `moneymanager_user`

7. **Link Database to Backend:**
   - In your backend service settings
   - Add these environment variables:
     - `SPRING_DATASOURCE_URL` = (from database connection string)
     - `SPRING_DATASOURCE_USERNAME` = (from database)
     - `SPRING_DATASOURCE_PASSWORD` = (from database)

8. **Deploy**: Click **"Create Web Service"**

## Step 4: Deploy Frontend to Render

1. **Click "New +"** → **"Static Site"**
2. **Connect your GitHub repository** (moneymanager-frontend)
3. **Configure the service:**
   - **Name**: `moneymanager-frontend`
   - **Environment**: `Static Site`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
   - **Plan**: `Free`

4. **Add Environment Variables:**
   - `VITE_API_URL` = `https://moneymanager-backend.onrender.com`

5. **Deploy**: Click **"Create Static Site"**

## Step 5: Update Frontend URL in Backend

After frontend is deployed, update the backend environment variable:
- `MONEY_MANAGER_FRONTEND_URL` = `https://YOUR_FRONTEND_URL.onrender.com`

## Step 6: Test Your Deployment

1. **Backend Health Check**: `https://moneymanager-backend.onrender.com/status`
2. **Frontend**: `https://moneymanager-frontend.onrender.com`
3. **Test Registration/Login**
4. **Test Adding Expenses/Income**

## Troubleshooting

### Common Issues:
1. **Build Failures**: Check build logs in Render dashboard
2. **Database Connection**: Verify environment variables are set correctly
3. **CORS Issues**: Check SecurityConfig.java CORS settings
4. **Frontend API Calls**: Verify VITE_API_URL is set correctly

### Free Tier Limitations:
- **Sleep Mode**: Free services sleep after 15 minutes of inactivity
- **Cold Start**: First request after sleep takes ~30 seconds
- **Database**: 1GB storage limit

## Production Considerations

For production use, consider upgrading to:
- **Starter Plan** ($7/month) - No sleep mode
- **Database Plan** ($7/month) - More storage and performance

## URLs After Deployment

- **Backend**: `https://moneymanager-backend.onrender.com`
- **Frontend**: `https://moneymanager-frontend.onrender.com`
- **Database**: Managed by Render

---

🎉 **Your Money Manager app will be live!**
