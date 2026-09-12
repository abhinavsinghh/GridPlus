# Quick Deployment Guide

## Fastest Way: Deploy to Render (Free)

### Step-by-Step:

1. **Push your code to GitHub** (if not already)
   ```bash
   git add .
   git commit -m "Prepare for deployment"
   git push origin main
   ```

2. **Go to Render.com**
   - Sign up/login: https://render.com
   - Click "New +" → "Web Service"

3. **Connect Repository**
   - Connect your GitHub account
   - Select the GridPlus repository

4. **Configure**
   - **Name:** gridplus
   - **Environment:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Plan:** Free

5. **Add Environment Variables** (in Render dashboard)
   ```
   ATLAS_DB = mongodb+srv://username:password@cluster.mongodb.net/database?retryWrites=true&w=majority
   SESSION_SECRET = (generate a random string)
   EMAIL_USER = your_email@gmail.com
   EMAIL_PASS = your_gmail_app_password
   NODE_ENV = production
   ```

6. **Deploy**
   - Click "Create Web Service"
   - Wait 5-10 minutes for first deployment
   - Your app will be live at: `https://gridplus.onrender.com`

### Generate SESSION_SECRET:
```bash
# On Windows PowerShell:
[Convert]::ToBase64String((1..32 | ForEach-Object { Get-Random -Maximum 256 }))
```

### Gmail App Password:
1. Go to Google Account → Security
2. Enable 2-Step Verification
3. Go to App Passwords
4. Generate password for "Mail"
5. Use that password in EMAIL_PASS

---

## Alternative: Railway (Also Free)

1. Go to https://railway.app
2. Sign up with GitHub
3. New Project → Deploy from GitHub
4. Select GridPlus repo
5. Add environment variables
6. Deploy automatically!

---

## Your App is Ready! 🚀

After deployment, visit your app URL and test:
- User registration/login
- Create a meter
- View dashboard
- Check alerts


