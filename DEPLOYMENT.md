# 🚀 IIIT Bhubaneswar Mess Management System — Deployment Guide

This repository is **100% deployment-ready**. The application uses a unified architecture where the Node.js production server serves the optimized React frontend alongside the REST API, eliminating CORS issues and minimizing hosting costs.

---

## 📋 Prerequisites & Project Anatomy

| Component | Technology | Production Target |
|---|---|---|
| **Frontend** | React 19 + Tailwind v4 + Vite | Compiled into `client/dist/` |
| **Backend** | Node.js + Express.js | Listens on `PORT` (default `5000`) |
| **Database** | Persistent JSON store with atomic writes | Configurable via `DATA_DIR` |
| **Health Check** | `GET /api/health` | Returns HTTP 200 `ONLINE` |

---

## 🌟 Option 1: Render (Recommended — Free Tier Available)

Render detects the included [render.yaml](file:///c:/Users/tharu/OneDrive/Desktop/antigraviti/render.yaml) blueprint automatically.

### Steps:
1. Push this repository to **GitHub** or **GitLab**:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/iiit-mess-management.git
   git branch -M main
   git push -u origin main
   ```
2. Log in to [Render Dashboard](https://dashboard.render.com/).
3. Click **New +** → **Blueprint**.
4. Select your GitHub repository. Render reads `render.yaml` automatically:
   - **Build Command**: `npm run install:server && npm run build`
   - **Start Command**: `npm start`
   - **Health Check Path**: `/api/health`
5. Click **Apply**.
6. In ~2 minutes, your portal will be live at `https://iiit-bbsr-mess-portal.onrender.com`!

> [!TIP]
> **Persistent Disk on Render (Optional)**: If you want database changes to persist across redeploys on Render, add a **Disk** mounted at `/app/data` with environment variable `DATA_DIR=/app/data`.

---

## 🐳 Option 2: Docker & Docker Compose (Universal Cloud / VPS)

The project includes an optimized multi-stage [Dockerfile](file:///c:/Users/tharu/OneDrive/Desktop/antigraviti/Dockerfile) and [docker-compose.yml](file:///c:/Users/tharu/OneDrive/Desktop/antigraviti/docker-compose.yml).

### Quick Launch with Docker Compose:
```bash
docker compose up -d --build
```
Your application will be live at `http://localhost:5000` with persistent data stored in the Docker volume `mess-data`.

### Build & Run Standalone Container:
```bash
# 1. Build the production image
docker build -t iiit-mess-portal:latest .

# 2. Run container with persistent data volume
docker run -d \
  --name iiit-mess \
  -p 5000:5000 \
  -e PORT=5000 \
  -v mess_data:/app/data \
  --restart unless-stopped \
  iiit-mess-portal:latest
```

### Cloud Deployments using this Dockerfile:
- **Google Cloud Run**: Run `gcloud run deploy --source .`
- **AWS App Runner / ECS**: Point directly to this repository or Docker image.
- **DigitalOcean App Platform**: Select Dockerfile as build strategy.

---

## 🚂 Option 3: Railway

1. Go to [Railway.app](https://railway.app/).
2. Click **New Project** → **Deploy from GitHub repo**.
3. Select this repository.
4. Railway will automatically detect the `Dockerfile` or `package.json`.
5. Under **Variables**, add:
   - `NODE_ENV=production`
   - `PORT=5000`
6. Under **Volumes**, click **Add Volume** and mount it to `/app/data`.
7. Click **Deploy**. Generate a public domain under **Settings → Networking**.

---

## 🐧 Option 4: Self-Hosted Linux VPS (Ubuntu 22.04 / 24.04 with PM2 & Nginx)

For deployment on college infrastructure or an AWS EC2 / DigitalOcean Droplet:

### 1. Install Node.js 20+ & PM2 on the server:
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js 20 LTS
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs nginx git

# Install PM2 process manager
sudo npm install -g pm2
```

### 2. Clone and build the project:
```bash
# Clone to /var/www/
cd /var/www
sudo git clone https://github.com/YOUR_USERNAME/iiit-mess-management.git mess-portal
sudo chown -R $USER:$USER /var/www/mess-portal
cd mess-portal

# Install server & client dependencies and build the UI
npm run install:server
npm run build
```

### 3. Start with PM2:
```bash
# Start Node.js server with PM2
pm2 start server/index.js --name "iiit-mess-portal"

# Configure PM2 to start on system boot
pm2 startup
pm2 save
```

### 4. Configure Nginx Reverse Proxy with SSL:
Create `/etc/nginx/sites-available/mess.iiit-bh.ac.in`:
```nginx
server {
    listen 80;
    server_name mess.iiit-bh.ac.in; # Or your server IP

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Enable site & reload Nginx:
```bash
sudo ln -s /etc/nginx/sites-available/mess.iiit-bh.ac.in /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### 5. Enable HTTPS (Free Let's Encrypt SSL):
```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d mess.iiit-bh.ac.in
```

---

## ⚙️ Environment Variables Reference

| Variable | Default | Purpose |
|---|---|---|
| `PORT` | `5000` | Port for the HTTP Express server |
| `NODE_ENV` | `production` | Node environment flag |
| `DATA_DIR` | `./server` | Directory path where `database.json` is stored |
| `CLIENT_DIST` | `../client/dist` | Custom filesystem path for compiled frontend |

---

## 🔍 Post-Deployment Verification

Once deployed, run this check against your public domain:

1. **Health Check**:
   ```bash
   curl -i https://YOUR-APP-URL/api/health
   # Expected: HTTP 200 {"status":"ONLINE","system":"IIIT Bhubaneswar Mess Management & Billing System API",...}
   ```
2. **Web Portal UI**:
   Open `https://YOUR-APP-URL` in a browser:
   - Check that the navigation bar, live meal cards, and persona switcher load immediately.
   - Test a sample payment under the **Billing & Payments** tab.
   - Verify that receipts display and print cleanly.
