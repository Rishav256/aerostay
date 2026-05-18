# AeroStay

A full-stack hospitality booking platform built with the MERN stack, 
containerized with Docker, and deployed on AWS EC2 with Nginx and a 
GitHub Actions CI/CD pipeline. Browse accommodations, manage listings, 
upload property photos, and make bookings — with JWT and Google OAuth 
authentication.

**Live Demo → [http://13.201.9.163.nip.io](http://13.201.9.163.nip.io)**

---

## Features

- Browse and search property listings
- JWT-based authentication + Google OAuth 2.0
- Hosts can create, edit, and delete listings with photo uploads
- Cloudinary-powered image storage (URL upload + local file upload)
- Booking management for guests and hosts
- Responsive UI with Tailwind CSS and shadcn/ui

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 (Vite), Tailwind CSS, shadcn/ui, Axios |
| Backend | Node.js, Express.js |
| Database | MongoDB Atlas |
| Auth | JWT, cookie-session, Google OAuth 2.0 (@react-oauth/google) |
| Media | Cloudinary |
| Deployment | AWS EC2 (t3.micro, ap-south-1), Docker, Nginx |
| CI/CD | GitHub Actions — auto build, push to Docker Hub, deploy to EC2 on push to main |

---

## Architecture

```
Client (React/Vite)
    │
    ▼
Nginx (reverse proxy on EC2)
    ├── /          → aerostay-client container (port 3000)
    └── /api/      → aerostay-api container (port 8000)
                            │
                            ├── MongoDB Atlas
                            └── Cloudinary
```

Both services run as Docker containers on a single EC2 instance.  
GitHub Actions builds fresh images on every push to `main`, pushes them to Docker Hub, and redeploys on the server via SSH — zero downtime rolling update.

---

## Local Development

### Prerequisites
- Node.js v18+
- Yarn
- Git

### 1. Clone the repository
```bash
git clone https://github.com/Rishav256/aerostay.git
cd aerostay
```

### 2. Install dependencies
```bash
# Backend
cd api && yarn install

# Frontend
cd ../client && yarn install
```

### 3. Set up environment variables

#### `api/.env`
```env
PORT=4000
DB_URL=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
JWT_EXPIRY=20d
COOKIE_TIME=7
SESSION_SECRET=your_session_secret
CLOUDINARY_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
CLIENT_URL=http://localhost:5173
```

#### `client/.env`
```env
VITE_BASE_URL=http://localhost:4000
VITE_GOOGLE_CLIENT_ID=your_google_client_id
```

<details>
<summary>How to obtain each value</summary>

**VITE_GOOGLE_CLIENT_ID**
1. [Google Cloud Console](https://console.cloud.google.com) → APIs & Services → Credentials
2. Create OAuth 2.0 Client ID (Web application)
3. Add `http://localhost:5173` to Authorized JavaScript Origins
4. Copy the Client ID

**DB_URL**
1. [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) → Create free cluster
2. Connect → Drivers → copy connection string
3. Replace credentials and append your DB name: `.../aerostay`

**JWT_SECRET & SESSION_SECRET**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```
Run twice, use one value for each.

**Cloudinary Keys**  
[Cloudinary](https://cloudinary.com) → Settings → API Keys

</details>

### 4. Run locally
```bash
# Terminal 1 — backend
cd api && yarn start

# Terminal 2 — frontend
cd client && yarn dev
```

Frontend → `http://localhost:5173`  
Backend → `http://localhost:4000`

---

## Deployment

The app is containerized with Docker and deployed on AWS EC2.

- **API** — `node:22-alpine` image, Express server
- **Client** — multi-stage build: `node:22-alpine` compiles the Vite app, `nginx:alpine` serves the static output
- **Nginx** — reverse proxies frontend and backend on a single server

GitHub Actions workflow (`.github/workflows/cicd.yml`) triggers on every push to `main`:
1. Builds and pushes both Docker images to Docker Hub
2. SSHs into EC2, pulls latest images, restarts containers

### Folder Structure
```
aerostay/
├── .github/
│   └── workflows/
│       └── cicd.yml        # CI/CD pipeline
├── api/                    # Express backend
│   ├── Dockerfile
│   ├── .dockerignore
│   └── .env                # not committed
├── client/                 # React frontend (Vite)
│   ├── Dockerfile
│   ├── .dockerignore
│   └── .env                # not committed
└── README.md
```
