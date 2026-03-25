# 🚀 Belimaa — Complete Deployment Guide
## Full-Stack: Node.js + SQLite + HTML Frontend

---

## 📁 Project Structure

```
belimaa-project/
├── backend/
│   ├── server.js          ← Node.js API server (ALL backend logic)
│   └── package.json       ← Dependencies
├── database/
│   └── belimaa.db         ← SQLite database (auto-created on first run)
└── public/
    ├── index.html         ← Your main website
    └── admin.html         ← Admin control panel
```

---

## ⚡ OPTION 1 — Run Locally (Test on Your Computer)

### Step 1: Install Node.js
Download from https://nodejs.org (choose LTS version 18+)
Verify: open terminal and type `node -v`

### Step 2: Install Dependencies
```bash
cd belimaa-project/backend
npm install
```

### Step 3: Start the Server
```bash
node server.js
```

You'll see:
```
🚀 Belimaa server running at http://localhost:3000
📋 Admin panel: http://localhost:3000/admin.html
🔑 Admin login: admin@belimaa.in / Belimaa@Admin2025
```

### Step 4: Open in Browser
- **Website:** http://localhost:3000
- **Admin:** http://localhost:3000/admin.html

---

## 🌐 OPTION 2 — Deploy to Railway (FREE, Recommended)

Railway is the easiest free Node.js host. Takes 5 minutes.

### Step 1: Create Account
Go to https://railway.app → Sign up with GitHub

### Step 2: Push to GitHub
```bash
# In your belimaa-project folder:
git init
git add .
git commit -m "Belimaa launch"
# Create a repo on github.com, then:
git remote add origin https://github.com/YOUR_USERNAME/belimaa.git
git push -u origin main
```

### Step 3: Deploy on Railway
1. Click **"New Project"** → **"Deploy from GitHub repo"**
2. Select your `belimaa` repository
3. Railway auto-detects Node.js
4. Set **Root Directory** to `backend`
5. Set **Start Command** to `node server.js`

### Step 4: Add Environment Variables (on Railway)
```
JWT_SECRET=your-random-secret-here-change-this
PORT=3000
```

### Step 5: Add Custom Domain
1. In Railway → Settings → Domains
2. Add `belimaa.in` (your domain)
3. Point your domain's DNS to Railway's provided CNAME

---

## 🌐 OPTION 3 — Deploy to Render (Also FREE)

### Steps:
1. Go to https://render.com → Sign up
2. Click **"New Web Service"** → Connect GitHub
3. Select your repo
4. Settings:
   - **Root Directory:** `backend`
   - **Build Command:** `npm install`
   - **Start Command:** `node server.js`
5. Add environment variable: `JWT_SECRET=change-me-to-random-string`
6. Deploy!

---

## 🌐 OPTION 4 — Deploy to VPS (DigitalOcean / Hostinger)

If you have a VPS or Hostinger hosting:

### Upload Files
Use FileZilla (FTP) or SSH to upload the entire `belimaa-project` folder.

### On the Server (SSH):
```bash
# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2 (keeps server running)
npm install -g pm2

# Go to backend folder
cd belimaa-project/backend
npm install

# Start with PM2
pm2 start server.js --name belimaa
pm2 startup
pm2 save
```

### Setup Nginx (to serve on port 80):
```nginx
server {
    listen 80;
    server_name belimaa.in www.belimaa.in;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Add SSL (Free HTTPS):
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d belimaa.in -d www.belimaa.in
```

---

## 🔐 Admin Panel Login

Default credentials (change after first login!):
- **URL:** https://belimaa.in/admin.html
- **Email:** admin@belimaa.in
- **Password:** Belimaa@Admin2025

### Admin Features:
- 📊 **Dashboard** — Live stats (orders, revenue, customers, enquiries)
- 📦 **Orders** — View & update order status
- 🛍️ **Products** — Add, edit, delete products (saved to database)
- ✉️ **Enquiries** — All contact form submissions with reply tracking
- 👥 **Customers** — Registered user accounts
- 📈 **Analytics** — Charts and reports

---

## 🔗 How Main Site Links to Admin

The main site (`index.html`) connects to the backend at `/api`:

| Feature | What it does |
|---------|-------------|
| **Login/Signup** | Real accounts saved in SQLite database |
| **Contact Form** | Enquiries saved, appear in admin panel |
| **Admin Button** | Appears in navbar if user is admin role |

---

## 🗄️ Database (SQLite)

All data is stored in `database/belimaa.db` — no setup needed, auto-creates.

**Tables:**
- `users` — Customer accounts
- `admins` — Admin accounts  
- `enquiries` — Contact form submissions
- `orders` — Customer orders
- `products` — Product catalogue

---

## 🔧 Changing Admin Password

After deploying, SSH into your server:
```bash
cd belimaa-project
node -e "
const Database = require('better-sqlite3');
const bcrypt = require('bcryptjs');
const db = new Database('./database/belimaa.db');
const hash = bcrypt.hashSync('YOUR_NEW_PASSWORD', 10);
db.prepare('UPDATE admins SET password=? WHERE email=?').run(hash, 'admin@belimaa.in');
console.log('Password updated!');
"
```

---

## ❓ Troubleshooting

| Problem | Solution |
|---------|----------|
| `Cannot find module 'express'` | Run `npm install` in `backend/` folder |
| Login says "Connection error" | Server not running — run `node server.js` |
| Admin shows "—" stats | Backend API not reachable, check URL |
| Contact form not saving | Check CORS settings in server.js, add your domain |
| Port 3000 in use | Change `PORT=3001` in env or kill other process |

---

## 📞 Quick Start Checklist

- [ ] Install Node.js 18+
- [ ] Run `npm install` in `backend/`
- [ ] Run `node server.js`
- [ ] Open http://localhost:3000 — website works
- [ ] Open http://localhost:3000/admin.html — login with default creds
- [ ] Submit contact form on website — check it appears in Admin → Enquiries
- [ ] Change admin password after first login

---

*Built with Node.js + Express + SQLite + Belimaa HTML*
