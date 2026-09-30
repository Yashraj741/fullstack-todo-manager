# ⚡ Quick Start Deployment Checklist

## 🎯 Your Stack
- **Frontend**: Vercel (`yourdomain.com`)
- **Backend**: Railway (`api.yourdomain.com`)
- **Database**: MongoDB Atlas
- **Code**: GitHub
- **Domain**: Namecheap

---

## ✅ PRE-DEPLOYMENT (Do These Now)

- [ ] MongoDB Atlas account created
- [ ] MongoDB cluster created (M0 free tier)
- [ ] Database user created (username & password)
- [ ] Network Access set to "Allow from anywhere"
- [ ] Connection string copied to `.env`
- [ ] CORS updated in `Backend/server.js`
- [ ] `start` script added to `Backend/package.json`
- [ ] `.env` added to `.gitignore`
- [ ] GitHub repository created

---

## 🔴 STEP 1: Push Code to GitHub (5 mins)

```bash
# In project root
git init
git add .
git commit -m "Initial commit: MERN Todo App"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/fullstack-todo-manager.git
git push -u origin main
```

**Verify**: Check your GitHub repository has all files ✅

---

## 🟠 STEP 2: Deploy Backend on Railway (10 mins)

1. Go to [railway.app](https://railway.app)
2. Sign up with GitHub
3. **New Project** → **Deploy from GitHub repo**
4. Select `fullstack-todo-manager`
5. Configure environment variables:
   - `PORT=10000`
   - `MONGO_URI=mongodb+srv://todouser:PASSWORD@cluster0.xxxxx/todoDB?retryWrites=true&w=majority`
   - `JWT_SECRET=your-secret`
6. Railway auto-detects Node.js
7. Set Build/Start from Railway config or keep defaults
8. Click **Deploy** and wait 2-3 minutes

**Note**: Copy your Railway URL → `https://backend-production-xxxx.up.railway.app`

**Verify**: Open `https://backend-production-xxxx.up.railway.app/` → Should see "Todo API Running 🚀" ✅

---

## 🟡 STEP 3: Deploy Frontend on Vercel (10 mins)

1. Update `Frontend/src/api.js` → Use environment variable for API URL
2. Create `Frontend/.env.production`:
   ```env
   VITE_API_URL=https://backend-production-xxxx.up.railway.app/api
   ```
3. Go to [vercel.com](https://vercel.com)
4. Sign up with GitHub
5. **Add New** → **Project**
6. Select `fullstack-todo-manager`
7. **Framework**: Vite
8. **Root Directory**: `Frontend`
9. Add environment variable: `VITE_API_URL=YOUR_RAILWAY_URL/api`
10. Click **Deploy** and wait 2-3 minutes

**Note**: Copy your Vercel URL → `https://fullstack-todo-manager.vercel.app`

**Verify**: Open Vercel URL → Should see your app ✅

---

## 🟢 STEP 4: Buy Domain from Namecheap (5 mins)

1. Go to [namecheap.com](https://namecheap.com)
2. Search for domain (e.g., `mytodoapp.com`)
3. Add to cart and checkout
4. Complete payment
5. Go to **Dashboard** → **Domain List**
6. Click **Manage** → **Advanced DNS** (keep this open)

**Note**: Save your domain name for next steps

---

## 🔵 STEP 5: Connect Domain to Vercel (5 mins)

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click your project
3. **Settings** → **Domains**
4. Add Domain: `mytodoapp.com`
5. Choose **Add** and verify
6. Vercel shows DNS records like:
   ```
   Type: A
   Name: @
   Value: 76.76.19.195
   ```
7. Go back to Namecheap **Advanced DNS**
8. Update A record:
   - Host: `@`
   - Value: `76.76.19.195`
   - TTL: Automatic
9. Click ✅ Save

**⏳ Wait 5-15 minutes for DNS to propagate**

**Verify**: Open `https://mytodoapp.com` → Should show your Vercel app ✅

---

## 🟣 STEP 6: Add API Subdomain (5 mins)

### Link to Railway:

1. Namecheap **Advanced DNS** → Click **Add Record**
2. Create CNAME record:
   - Type: `CNAME`
   - Host: `api`
   - Value: `cname.vercel-dns.com.`
   - TTL: Automatic
3. Click ✅ Save

### Configure in Vercel:

1. Vercel project → **Settings** → **Domains**
2. Add Domain: `api.mytodoapp.com`
3. Should auto-verify ✅

**⏳ Wait 5-10 minutes for DNS**

---

## 🟡 STEP 7: Connect API Subdomain to Railway (5 mins)

1. Go to [railway.app](https://railway.app)
2. Click your project → Backend service
3. **Settings** → **Domain**
4. Add custom domain: `api.mytodoapp.com`
5. Click **Add Custom Domain**
6. Railway will show DNS instructions (already done in Namecheap!)

**Verify**: `https://api.mytodoapp.com/` → Should show "Todo API Running 🚀" ✅

---

## ⚪ STEP 8: Update Frontend for Production Domain (2 mins)

1. Edit `Frontend/.env.production`:
   ```env
   VITE_API_URL=https://api.mytodoapp.com/api
   ```
2. Push to GitHub:
   ```bash
   git add Frontend/.env.production
   git commit -m "Update API URL to custom domain"
   git push origin main
   ```
3. Vercel auto-deploys automatically! ✅

---

## 🎯 STEP 9: Test Everything (5 mins)

- [ ] Open `https://mytodoapp.com` → Frontend loads
- [ ] Click Register → Form appears
- [ ] Enter email, password → Submit
- [ ] Account created → Redirects to login
- [ ] Login with credentials → Dashboard shows
- [ ] Create a todo → Appears in list
- [ ] Edit todo → Changes saved
- [ ] Delete todo → Removed from list
- [ ] Logout → Session clears
- [ ] Refresh page → Logged out
- [ ] Login again → Previous todos visible
- [ ] Change password → Login with new password works

**All tests pass?** 🎉 **YOU'RE DONE!**

---

## 🔧 If Something Breaks

### Frontend won't load
- Check Vercel deployment logs: `vercel.com/dashboard` → Deployments
- Verify `VITE_API_URL` in `.env.production`

### Can't connect to backend
- Check Railway logs: `railway.app` → Project → Logs
- Test API directly: `https://api.mytodoapp.com/`
- Verify CORS in `Backend/server.js`

### Database connection fails
- MongoDB Atlas → Network Access → Check IP whitelist
- Verify `MONGO_URI` format
- Check connection string has correct password

### Domain not working
- Wait 10-15 minutes for DNS
- Use [whatsmydns.net](https://whatsmydns.net) to check DNS status
- Verify DNS records in Namecheap match requirements

---

## 📊 Final Architecture

```
www.mytodoapp.com → Vercel Frontend
                    ↓ (API calls)
                api.mytodoapp.com → Railway Backend
                                    ↓ (DB queries)
                                  MongoDB Atlas
```

---

## 💾 Keep These Credentials Safe

- MongoDB URI (with password)
- JWT Secret
- Namecheap username/password
- GitHub token (if used)

**NEVER commit these to GitHub!** Use environment variables on each platform.

---

## 🚀 You're Live!

Your MERN app is now accessible worldwide at:
- **Frontend**: `https://yourdomain.com`
- **Backend API**: `https://api.yourdomain.com`

**Share your domain with friends! 🎉**

---

## 📞 Support Links

- Railway: [railway.app/support](https://railway.app/support)
- Vercel: [vercel.com/support](https://vercel.com/support)
- MongoDB: [docs.mongodb.com](https://docs.mongodb.com)
- Namecheap: [namecheap.com/support](https://www.namecheap.com/support/)
