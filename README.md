# AeroStay 

A full-stack Hospitality Management App built with the MERN stack — browse accommodations, view listings, make bookings, and manage your own properties.

---

## Tech Stack

- **Frontend** — React.js (Vite), Tailwind CSS
- **Backend** — Node.js, Express.js
- **Database** — MongoDB (Atlas)
- **Auth** — JWT, Google OAuth 2.0
- **Media** — Cloudinary

---

## Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/) (v18+)
- [Yarn](https://yarnpkg.com/)
- [Git](https://git-scm.com/)

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Rishav256/aerostay.git
cd aerostay
```

### 2. Install dependencies

```bash
# Frontend
cd client
yarn install

# Backend
cd ../api
yarn install
```

### 3. Set up environment variables

#### `client/.env`

```env
VITE_BASE_URL=http://localhost:4000
VITE_GOOGLE_CLIENT_ID=your_google_client_id
```

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

> See the section below for how to obtain each value.

### 4. Run the project

```bash
# In one terminal — start the backend
cd api
yarn start

# In another terminal — start the frontend
cd client
yarn run dev
```

Frontend runs at `http://localhost:5173`  
Backend runs at `http://localhost:4000`

---

## Obtaining Environment Variables

### `VITE_GOOGLE_CLIENT_ID`
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a project → **APIs & Services** → **OAuth consent screen** → configure it
3. Go to **Credentials** → **Create Credentials** → **OAuth 2.0 Client ID**
4. Set Application type to **Web application**
5. Add `http://localhost:5173` to both Authorized JavaScript Origins and Redirect URIs
6. Copy the generated Client ID

### `DB_URL`
1. Sign up at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Go to **Connect** → **Drivers** → copy the connection string
4. Replace `<username>` and `<password>` with your Atlas credentials
5. Append your database name: `.../aerostay`

### `JWT_SECRET` & `SESSION_SECRET`
Generate secure random strings using Node.js:
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```
Run twice — use one value for each secret.

### Cloudinary Keys
1. Sign up at [Cloudinary](https://cloudinary.com)
2. Go to **Settings** → **API Keys**
3. Your Cloud name, API Key, and API Secret are all listed there

---

## Folder Structure

.env files should be properly located

```
aerostay/
├── client/       # React frontend (Vite)
│   └── .env
├── api/          # Express backend
│   └── .env
└── README.md
```

---


## A personal project by Rishav256
## :)
