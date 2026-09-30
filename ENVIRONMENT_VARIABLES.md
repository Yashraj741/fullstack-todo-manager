# 🔐 Environment Variables & Secrets Reference

This file documents all environment variables needed for your MERN app.

**⚠️ IMPORTANT**: NEVER commit `.env` files to GitHub! They're already in `.gitignore`

---

## 📦 Backend Environment Variables

### Development (`Backend/.env`)

```env
# Server Configuration
PORT=3000

# Database
MONGO_URI=mongodb://localhost:27017/todoDB

# JWT Secret (Keep this safe!)
JWT_SECRET=your-super-secret-key-change-this
```

### Production (Set on Railway Dashboard)

```env
# Server Configuration
PORT=10000

# Database (MongoDB Atlas)
MONGO_URI=mongodb+srv://todouser:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/todoDB?retryWrites=true&w=majority&appName=Cluster0

# JWT Secret (Use a strong random string)
JWT_SECRET=your-production-secret-key-make-it-different
```

---

## 📦 Frontend Environment Variables

### Development (Optional)

`Frontend/.env.development` (optional - uses default):
```env
VITE_API_URL=http://localhost:3000/api
```

### Production (`Frontend/.env.production`)

```env
VITE_API_URL=https://api.yourdomain.com/api
```

---

## 🔑 How to Generate Strong Secrets

### Generate JWT_SECRET

**Option 1: Using Node.js**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

**Option 2: Using OpenSSL**
```bash
openssl rand -hex 32
```

**Option 3: Online Generator**
- Go to [randomkeygen.com](https://randomkeygen.com/)
- Copy the "CodeIgniter Encryption Key"

### Result Example
```
GENERATED: 7f8e9d0c1b2a3f4e5d6c7b8a9f0e1d2c3b4a5f6e7d8c9b0a1f2e3d4c5b6a7
```

---

## 🚀 Setting Environment Variables on Each Platform

### Railway Dashboard

1. Go to [railway.app](https://railway.app)
2. Click your project
3. Click **Backend** service
4. Click **Variables** tab
5. Click **Add Variable**
6. Enter:
   - **Key**: `MONGO_URI`
   - **Value**: (Your MongoDB Atlas connection string)
7. Click **Save**
8. Repeat for `PORT` and `JWT_SECRET`

### Vercel Dashboard

1. Go to [vercel.com/dashboard](https://vercel.com/dashboard)
2. Click your project
3. Click **Settings** → **Environment Variables**
4. Click **Add New**
5. Enter:
   - **Name**: `VITE_API_URL`
   - **Value**: `https://api.yourdomain.com/api`
   - **Environments**: Check **Production**
6. Click **Save**

---

## 📝 Getting MongoDB Connection String

### From MongoDB Atlas:

1. Go to [cloud.mongodb.com](https://cloud.mongodb.com)
2. Click **Database** → **Clusters**
3. Click **Connect**
4. Choose **Drivers**
5. Copy the connection string:
   ```
   mongodb+srv://todouser:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0
   ```
6. **Replace `<password>` with your actual password**
7. **Add database name** `/todoDB` before the `?`:
   ```
   mongodb+srv://todouser:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/todoDB?retryWrites=true&w=majority&appName=Cluster0
   ```

---

## 🔒 Security Best Practices

### DO ✅
- [ ] Use strong, unique secrets for each environment
- [ ] Regenerate secrets if compromised
- [ ] Use `.env` locally, environment variables on production
- [ ] Add `.env` to `.gitignore`
- [ ] Rotate JWT secrets periodically
- [ ] Use different secrets for dev/staging/production

### DON'T ❌
- [ ] Commit `.env` files to GitHub
- [ ] Share secrets in Slack/Discord/Email
- [ ] Use the same secret everywhere
- [ ] Use simple passwords like "123456"
- [ ] Hardcode secrets in code
- [ ] Put secrets in commit messages

---

## 🧪 Verifying Variables Are Set Correctly

### Backend Test

```javascript
// Add this to Backend/server.js temporarily
console.log("MONGO_URI:", process.env.MONGO_URI ? "✅ Set" : "❌ Missing");
console.log("JWT_SECRET:", process.env.JWT_SECRET ? "✅ Set" : "❌ Missing");
console.log("PORT:", process.env.PORT || "3000");
```

### Frontend Test

```javascript
// Add to Frontend/src/api.js temporarily
console.log("VITE_API_URL:", import.meta.env.VITE_API_URL);
```

---

## 🔄 Rotating Secrets (Important!)

### When to Rotate
- Suspected compromise
- Employee leaves company
- Every 3-6 months (best practice)
- After security audit

### How to Rotate JWT_SECRET

1. Generate new secret: `openssl rand -hex 32`
2. Update on Railway:
   - Dashboard → Variables → Edit `JWT_SECRET`
   - Railway auto-redeployes! ✅
3. Users will need to re-login (old tokens invalid)

### How to Rotate MongoDB Password

1. MongoDB Atlas → **Database Access**
2. Click user → **Edit Password**
3. Generate password
4. Update `MONGO_URI` on Railway
5. Railway auto-redeploys! ✅

---

## 📋 Environment Variables Checklist

### Backend (.env or Railway)
- [ ] `PORT` = `10000` (Railway) or `3000` (Local)
- [ ] `MONGO_URI` = MongoDB Atlas connection string
- [ ] `JWT_SECRET` = Strong random string (32+ characters)

### Frontend (.env.production or Vercel)
- [ ] `VITE_API_URL` = `https://api.yourdomain.com/api`

### Database (MongoDB Atlas)
- [ ] Username created (e.g., `todouser`)
- [ ] Password saved securely
- [ ] Database name set (e.g., `todoDB`)
- [ ] Network access allowed (0.0.0.0/0 for development)

---

## 💡 Quick Reference URLs

| Item | URL |
|------|-----|
| MongoDB Atlas | https://cloud.mongodb.com |
| Railway Dashboard | https://railway.app/dashboard |
| Vercel Dashboard | https://vercel.com/dashboard |
| Namecheap DNS | https://www.namecheap.com/myaccount/login/ |

---

## 🆘 Troubleshooting

### "Cannot connect to MongoDB"
- Check `MONGO_URI` format
- Verify username and password
- Check MongoDB network access allows your IP
- Confirm database name is in URI

### "API returns 401 Unauthorized"
- JWT_SECRET might be wrong
- Token might be expired
- Check Authorization header sent correctly

### "Frontend can't reach backend"
- Check `VITE_API_URL` is correct
- Ensure backend is running
- Verify CORS is enabled
- Check API URL includes `/api` at end

---

**Your secrets are safe! 🔐**
