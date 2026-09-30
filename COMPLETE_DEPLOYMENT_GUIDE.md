# 🚀 Complete Deployment Guide - Vercel + Railway + MongoDB Atlas + Namecheap

---

## 📋 Overview

```
┌─────────────────────────────────────────────┐
│  Frontend: Vercel (yourdomain.com)         │
└────────────┬─────────────────────────────────┘
             │ API Calls
             ↓
┌──────────────────────────────────────────────┐
│  Backend: Railway (api.yourdomain.com)      │
└────────────┬──────────────────────────────────┘
             │ Database Connection
             ↓
┌──────────────────────────────────────────────┐
│  MongoDB Atlas (Cloud Database)              │
└──────────────────────────────────────────────┘
```

---

# STEP 1: Push Code to GitHub 📤

## 1.1 Create GitHub Repository

1. Go to [github.com](https://github.com)
2. Sign in → Click **+** → **New repository**
3. Fill in:
   - **Repository name**: `fullstack-todo-manager`
   - **Description**: `MERN Todo App with Authentication`
   - **Public** (choose based on preference)
   - Click **Create repository**

## 1.2 Push Your Local Code to GitHub

Open terminal in your project folder:

```bash
git init
git add .
git commit -m "Initial commit: MERN Todo App"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/fullstack-todo-manager.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username.

✅ Your code is now on GitHub!

---

# STEP 2: MongoDB Atlas Setup ✅ (Already Done)

Verify your `.env` file has:
```env
MONGO_URI=mongodb+srv://todouser:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/todoDB?retryWrites=true&w=majority
```

✅ MongoDB is ready!

---

# STEP 3: Deploy Backend on Railway 🚂

Railway is MUCH better than Render for this! It auto-deploys from GitHub and has better performance.

### **3.1 Sign Up to Railway**

1. Go to [railway.app](https://railway.app)
2. Click **Sign up** → Choose **GitHub**
3. Authorize GitHub access
4. Click **+ New Project**

### **3.2 Connect GitHub Repository**

1. Click **Deploy from GitHub repo**
2. Select your `fullstack-todo-manager` repository
3. Click **Connect**

### **3.3 Configure Environment Variables**

After connecting, Railway will show a configuration screen:

1. Click **Add Variables**
2. Add these environment variables:
   - `PORT`: `10000`
   - `MONGO_URI`: (Your MongoDB Atlas connection string from `.env`)
   - `JWT_SECRET`: (Your JWT secret from `.env`)

### **3.4 Configure Build & Start Commands**

1. Click on the **Backend** service
2. Go to **Deploy** tab
3. Set:
   - **Build Command**: `cd Backend && npm install`
   - **Start Command**: `cd Backend && npm start`

OR create a **Procfile** in root:

```
web: cd Backend && npm start
```

### **3.5 Deploy!**

1. Click **Deploy** button
2. Wait 2-3 minutes for deployment to complete
3. Check logs for any errors
4. Your backend URL will be: `https://backend-production-xxxx.up.railway.app`

✅ **Copy this URL** - you'll need it for the frontend!

---

# STEP 4: Deploy Frontend on Vercel 🎨

### **4.1 Update Frontend Configuration**

Update [Frontend/src/api.js](Frontend/src/api.js) to handle dynamic API URL:

```javascript
// Determine API base URL based on environment
const API_BASE_URL = import.meta.env.VITE_API_URL || "http://localhost:3000/api";

const API = axios.create({
  baseURL: API_BASE_URL,
});
```

Create [Frontend/.env.production](Frontend/.env.production):

```env
VITE_API_URL=https://backend-production-xxxx.up.railway.app/api
```

Replace the URL with your actual Railway backend URL!

### **4.2 Sign Up to Vercel**

1. Go to [vercel.com](https://vercel.com)
2. Click **Sign up** → Choose **GitHub**
3. Authorize GitHub

### **4.3 Import Project**

1. Click **Add New** → **Project**
2. Click **Import Git Repository**
3. Select `fullstack-todo-manager`
4. Click **Import**

### **4.4 Configure Vercel Project**

1. **Project Name**: `fullstack-todo-manager`
2. **Framework Preset**: `Vite`
3. **Root Directory**: `Frontend`
4. Click **Environment Variables** (if not shown, skip to next step)
5. Add environment variable:
   - Key: `VITE_API_URL`
   - Value: `https://backend-production-xxxx.up.railway.app/api`

### **4.5 Deploy!**

1. Click **Deploy**
2. Wait 2-3 minutes
3. Your frontend URL: `https://fullstack-todo-manager.vercel.app`

✅ Test it: Open the URL in browser and check if it works!

---

# STEP 5: Configure CORS for Backend 🔒

Update [Backend/server.js](Backend/server.js):

```javascript
const allowedOrigins = [
    "http://localhost:5173",
    "http://localhost:3000",
    "https://fullstack-todo-manager.vercel.app"
];

app.use(cors({
    origin: function (origin, callback) {
        if (!origin || allowedOrigins.includes(origin)) {
            callback(null, true);
        } else {
            callback(new Error("Not allowed by CORS"));
        }
    },
    credentials: true
}));
```

Push this change to GitHub:

```bash
git add Backend/server.js
git commit -m "Update CORS for Vercel frontend"
git push origin main
```

Railway will auto-deploy! ✅

---

# STEP 6: Buy Domain from Namecheap 🌐

### **6.1 Choose & Buy Domain**

1. Go to [namecheap.com](https://namecheap.com)
2. Search for your desired domain (e.g., `mytodoapp.com`)
3. Click **Add to Cart** → **Checkout**
4. Sign up/Login with GitHub or email
5. Complete payment

### **6.2 Namecheap Dashboard Setup**

After purchase:

1. Go to **Dashboard** → **Domain List**
2. Click **Manage** next to your domain
3. Click **Advanced DNS**
4. You'll see DNS records here (keep this tab open for next steps)

---

# STEP 7: Connect Domain to Vercel 🔗

### **7.1 Add Domain to Vercel Project**

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click your `fullstack-todo-manager` project
3. Go to **Settings** → **Domains**
4. Click **Add Domain**
5. Type your domain name (e.g., `mytodoapp.com`)
6. Click **Add**

### **7.2 Update Namecheap DNS**

Vercel will show DNS records like:

```
Type: A
Name: @
Value: 76.76.19.195
```

Go back to Namecheap:

1. **Advanced DNS** tab
2. Find the **A** record (or create if missing)
3. Set:
   - **Type**: `A`
   - **Host**: `@`
   - **Value**: `76.76.19.195` (from Vercel)
   - **TTL**: `Automatic`
4. Click ✅ Save

Wait **5-10 minutes** for DNS to propagate.

✅ Now `https://mytodoapp.com` points to your Vercel frontend!

---

# STEP 8: Add Subdomain for Backend on Vercel 🔗

### **8.1 Create API Subdomain DNS Record**

Go to Namecheap **Advanced DNS**:

1. Click **Add Record**
2. Set:
   - **Type**: `CNAME`
   - **Host**: `api`
   - **Value**: `cname.vercel-dns.com.`
   - **TTL**: `Automatic`
3. Click ✅ Save

Wait 5-10 minutes.

### **8.2 Tell Vercel About the Subdomain**

1. Go to Vercel project **Settings** → **Domains**
2. Click **Add Domain**
3. Type: `api.mytodoapp.com`
4. It should auto-verify! ✅

---

# STEP 9: Update Backend URL for Production 🔄

### **9.1 Update Railway to Use Custom Domain**

1. Go to [railway.app](https://railway.app)
2. Click your project
3. Click the **Backend** service
4. Go to **Settings** → **Domain**
5. Click **Generate Domain** or add custom domain: `api.mytodoapp.com`

### **9.2 Update Frontend for Production Domain**

Edit [Frontend/.env.production](Frontend/.env.production):

```env
VITE_API_URL=https://api.mytodoapp.com/api
```

Push to GitHub:

```bash
git add Frontend/.env.production
git commit -m "Update API URL to custom domain"
git push origin main
```

Vercel auto-deploys! ✅

---

# STEP 10: Final Testing 🧪

### **Test Everything**

1. **Open your domain**: `https://mytodoapp.com`
2. **Register** a new account
3. **Create** a todo
4. **Edit** it
5. **Delete** it
6. **Logout** and verify session clears
7. **Login** again and verify todos persist
8. **Change Password** and login with new password

✅ If all works, you're DONE! 🎉

---

# STEP 11: Enable HTTPS Certificate 🔐

Both Vercel and Railway include **FREE SSL certificates** automatically!

Verify:
- Visit `https://mytodoapp.com` ✅ (Green lock 🔒)
- Visit `https://api.mytodoapp.com` ✅ (Green lock 🔒)

---

# STEP 12: Set Up Email Notifications (Optional) 📧

### **Railway Alerts**

1. Go to [railway.app/account/notifications](https://railway.app/account/notifications)
2. Enable deployment alerts
3. Get notified when your app deploys!

### **Vercel Alerts**

1. Go to Vercel project **Settings** → **Notifications**
2. Enable deployment notifications

---

# 🔧 Troubleshooting

## **Frontend Can't Connect to Backend**

**Check:**
1. Backend URL in `Frontend/.env.production` is correct
2. Railway deployment is successful (check logs)
3. CORS is enabled in `Backend/server.js`
4. API URL includes `/api` at the end

**Test:**
```bash
# In browser console
fetch('https://api.mytodoapp.com/api/auth/me')
  .then(r => r.json())
  .then(console.log)
```

---

## **Domain Not Working**

**Check:**
1. DNS records are correct in Namecheap
2. Wait 10-15 minutes for DNS propagation
3. Use [whatsmydns.net](https://whatsmydns.net) to check DNS status

**Flush DNS cache:**
```bash
# Windows
ipconfig /flushdns

# Mac
sudo dscacheutil -flushcache
```

---

## **Railway Deployment Fails**

**Check logs:**
1. Go to Railway project → Backend service
2. Click **Logs** tab
3. Look for error messages
4. Common issues:
   - Missing environment variables
   - Wrong build command
   - Database connection error

---

## **MongoDB Connection Timeout**

1. Go to [MongoDB Atlas](https://cloud.mongodb.com)
2. Click **Network Access**
3. Ensure `0.0.0.0/0` is whitelisted (Allow from anywhere)
4. Check `MONGO_URI` format is correct

---

# 📊 Service Comparison

| Service | Free Tier | Auto Deploy | Performance |
|---------|-----------|------------|-------------|
| **Vercel** | ✅ Unlimited | ✅ Git push | ⭐⭐⭐⭐⭐ |
| **Railway** | ✅ $5/month credit | ✅ Git push | ⭐⭐⭐⭐ |
| **MongoDB Atlas** | ✅ M0 | ✅ Always on | ⭐⭐⭐⭐ |
| **Namecheap** | ❌ Domain (paid) | N/A | ⭐⭐⭐⭐⭐ |

---

# 💰 Cost Breakdown

| Service | Monthly Cost | Notes |
|---------|-------------|-------|
| Vercel | **$0** | Free for hobby projects |
| Railway | **$0-5** | $5 free credit/month |
| MongoDB Atlas | **$0** | Free M0 tier |
| Namecheap Domain | **$8-15** | One-time + renewal |
| **Total** | **~$8-15/year** | Very affordable! |

---

# 🎯 Success Checklist

- ✅ Code pushed to GitHub
- ✅ MongoDB Atlas connected
- ✅ Backend deployed on Railway
- ✅ Frontend deployed on Vercel
- ✅ CORS configured
- ✅ Domain bought from Namecheap
- ✅ Root domain (yourdomain.com) → Vercel
- ✅ API subdomain (api.yourdomain.com) → Railway
- ✅ HTTPS working on both
- ✅ Can register/login/create todos
- ✅ Data persists on refresh
- ✅ Session timeout works
- ✅ Logout clears data

---

# 🚀 Next Steps After Deployment

1. **Monitor Performance**: Check Railway and Vercel dashboards
2. **Set Up Analytics**: Enable on Vercel dashboard
3. **Add Email Verification**: Enhance security
4. **Set Up Backups**: Enable in MongoDB Atlas
5. **Custom Email**: Add MX records to Namecheap for custom email
6. **SEO Optimization**: Add meta tags to frontend
7. **Performance Monitoring**: Set up error tracking

---

# 📱 Quick Commands Reference

```bash
# Push changes to GitHub (auto-deploys)
git add .
git commit -m "Your message"
git push origin main

# Check Railway logs
# Go to railway.app → Project → Service → Logs

# Check Vercel deployment logs
# Go to vercel.com/dashboard → Project → Deployments

# Test backend API
curl https://api.yourdomain.com/
```

---

**🎉 Congratulations! Your MERN app is now live globally with a custom domain!**

For support: Check service documentation or ask in their community forums.
