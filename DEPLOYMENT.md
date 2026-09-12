# Deployment Guide for GridPlus

This guide covers deploying GridPlus to various cloud platforms.

## Prerequisites

- MongoDB Atlas account and cluster (free tier available)
- Git repository (GitHub, GitLab, or Bitbucket)
- Environment variables ready

## Environment Variables Required

Make sure you have these environment variables set in your deployment platform:

```
ATLAS_DB=mongodb+srv://username:password@cluster.mongodb.net/database?retryWrites=true&w=majority
SESSION_SECRET=your_random_session_secret_here
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password_here
NODE_ENV=production
PORT=3000 (or let the platform set it automatically)
```

---

## Option 1: Deploy to Render (Recommended - Free Tier Available)

### Steps:

1. **Sign up/Login to Render**
   - Go to https://render.com
   - Sign up with GitHub/GitLab/Bitbucket

2. **Create a New Web Service**
   - Click "New +" → "Web Service"
   - Connect your Git repository
   - Select the GridPlus repository

3. **Configure the Service**
   - **Name:** gridplus (or your preferred name)
   - **Environment:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Plan:** Free (or choose a paid plan)

4. **Set Environment Variables**
   - Go to "Environment" tab
   - Add all required environment variables:
     - `ATLAS_DB`
     - `SESSION_SECRET`
     - `EMAIL_USER`
     - `EMAIL_PASS`
     - `NODE_ENV=production`
   - Render will automatically set `PORT`

5. **Deploy**
   - Click "Create Web Service"
   - Render will build and deploy your app
   - Your app will be available at: `https://your-app-name.onrender.com`

### Notes:
- Free tier services spin down after 15 minutes of inactivity
- First request after spin-down may take 30-60 seconds
- Upgrade to paid plan for always-on service

---

## Option 2: Deploy to Railway

### Steps:

1. **Sign up/Login to Railway**
   - Go to https://railway.app
   - Sign up with GitHub

2. **Create a New Project**
   - Click "New Project"
   - Select "Deploy from GitHub repo"
   - Choose your GridPlus repository

3. **Configure Environment Variables**
   - Go to "Variables" tab
   - Add all required environment variables
   - Railway automatically sets `PORT`

4. **Deploy**
   - Railway will automatically detect Node.js and deploy
   - Your app will be available at: `https://your-app-name.up.railway.app`

### Notes:
- Free tier includes $5 credit monthly
- Automatic HTTPS
- Easy database integration

---

## Option 3: Deploy to Heroku

### Steps:

1. **Install Heroku CLI**
   ```bash
   # Windows (using Chocolatey)
   choco install heroku-cli
   
   # Or download from https://devcenter.heroku.com/articles/heroku-cli
   ```

2. **Login to Heroku**
   ```bash
   heroku login
   ```

3. **Create Heroku App**
   ```bash
   heroku create your-app-name
   ```

4. **Set Environment Variables**
   ```bash
   heroku config:set ATLAS_DB="your_mongodb_connection_string"
   heroku config:set SESSION_SECRET="your_session_secret"
   heroku config:set EMAIL_USER="your_email@gmail.com"
   heroku config:set EMAIL_PASS="your_app_password"
   heroku config:set NODE_ENV="production"
   ```

5. **Deploy**
   ```bash
   git push heroku main
   # or
   git push heroku master
   ```

6. **Open Your App**
   ```bash
   heroku open
   ```

### Notes:
- Heroku free tier is no longer available
- Paid plans start at $5/month
- Easy to scale and manage

---

## Option 4: Deploy to AWS EC2 (As Previously Used)

### Steps:

1. **Launch EC2 Instance**
   - Go to AWS Console → EC2
   - Launch Ubuntu t3.micro instance
   - Configure security group (open ports 22, 80, 443, 3000)

2. **Connect to Instance**
   ```bash
   ssh -i your-key.pem ubuntu@your-ec2-ip
   ```

3. **Install Dependencies**
   ```bash
   sudo apt update
   sudo apt install nodejs npm -y
   sudo npm install -g pm2
   ```

4. **Clone and Setup**
   ```bash
   git clone your-repo-url
   cd GridPlus
   npm install
   ```

5. **Create .env file**
   ```bash
   nano .env
   # Add all environment variables
   ```

6. **Run with PM2**
   ```bash
   pm2 start app.js --name gridplus
   pm2 save
   pm2 startup
   ```

7. **Setup Nginx (Optional - for reverse proxy)**
   ```bash
   sudo apt install nginx
   # Configure nginx to proxy to localhost:3000
   ```

### Notes:
- More control but requires server management
- Need to handle SSL certificates manually
- Can use CloudFront for CDN (as mentioned in README)

---

## Option 5: Deploy to Vercel

### Steps:

1. **Install Vercel CLI**
   ```bash
   npm i -g vercel
   ```

2. **Login**
   ```bash
   vercel login
   ```

3. **Deploy**
   ```bash
   vercel
   ```

4. **Set Environment Variables**
   - Go to Vercel Dashboard → Your Project → Settings → Environment Variables
   - Add all required variables

### Notes:
- Great for serverless deployments
- Free tier available
- Automatic HTTPS

---

## Post-Deployment Checklist

- [ ] Verify MongoDB connection is working
- [ ] Test user registration/login
- [ ] Test meter creation and management
- [ ] Verify email notifications (if configured)
- [ ] Check cron jobs are running (meter simulation, payment requests)
- [ ] Test alert system
- [ ] Verify HTTPS is enabled
- [ ] Set up custom domain (optional)

---

## Troubleshooting

### MongoDB Connection Issues
- Verify `ATLAS_DB` connection string is correct
- Check MongoDB Atlas IP whitelist (add `0.0.0.0/0` for all IPs)
- Ensure cluster is not paused

### Port Issues
- Most platforms set `PORT` automatically
- If using custom port, ensure it matches platform requirements

### Email Not Working
- For Gmail, use App Password instead of regular password
- Verify `EMAIL_USER` and `EMAIL_PASS` are correct
- Check platform's outbound email restrictions

### Session Issues
- Ensure `SESSION_SECRET` is set and is a strong random string
- Verify MongoDB connection for session storage

---

## Security Recommendations

1. **Never commit `.env` file** (already in .gitignore)
2. **Use strong SESSION_SECRET** (generate with: `openssl rand -base64 32`)
3. **Enable MongoDB Atlas IP whitelist** (restrict to your deployment IPs)
4. **Use HTTPS** (most platforms provide this automatically)
5. **Keep dependencies updated** (`npm audit` and `npm update`)

---

## Need Help?

- Check platform-specific documentation
- Review application logs in platform dashboard
- Verify all environment variables are set correctly
- Test locally with production environment variables


