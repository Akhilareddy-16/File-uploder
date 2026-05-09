Project Structure

```
fileuploader/
├── backend/
│   ├── controllers/
│   │   ├── authController.js     # Register & login logic
│   │   └── fileController.js     # Upload, list, delete, download
│   ├── middleware/
│   │   ├── authMiddleware.js     # JWT guard (protect routes)
│   │   └── uploadMiddleware.js   # Multer config (file validation)
│   ├── models/
│   │   ├── User.js               # MongoDB User schema
│   │   └── File.js               # MongoDB File schema
│   ├── routes/
│   │   ├── authRoutes.js         # /api/auth/*
│   │   └── fileRoutes.js         # /api/upload, /api/files/*
│   ├── uploads/                  # Uploaded files stored here (auto-created)
│   ├── .env.example              # Environment variable template
│   ├── package.json
│   └── server.js                 # Main entry point
│
└── frontend/
    ├── src/
    │   ├── components/
    │   │   ├── FileCard.jsx       # Individual file card
    │   │   ├── FileList.jsx       # Grid of file cards
    │   │   ├── Navbar.jsx         # Top navigation bar
    │   │   ├── SearchBar.jsx      # File search input
    │   │   └── UploadZone.jsx     # Drag & drop upload area
    │   ├── context/
    │   │   └── AuthContext.jsx    # Global auth state (React Context)
    │   ├── pages/
    │   │   ├── LoginPage.jsx
    │   │   ├── RegisterPage.jsx
    │   │   └── DashboardPage.jsx
    │   ├── services/
    │   │   └── api.js             # Axios client with JWT interceptor
    │   ├── styles/                # Per-component CSS files
    │   ├── App.jsx                # Router + protected routes
    │   └── main.jsx               # React entry point
    ├── index.html
    ├── package.json
    └── vite.config.js
```

---

## ⚙️ Prerequisites

Make sure you have these installed:

- **Node.js** v18 or higher → https://nodejs.org
- **MongoDB** (local) → https://www.mongodb.com/try/download/community
  - OR use **MongoDB Atlas** (free cloud) → https://cloud.mongodb.com
- **npm** (comes with Node.js)

---

## 🚀 Setup — Step by Step

### Step 1: Set up the Backend

```bash
# Navigate to backend folder
cd backend

# Install dependencies
npm install

# Create your .env file from the example
cp .env.example .env
```

Open `.env` and fill in your values:

```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/fileuploader
JWT_SECRET=replace_this_with_a_long_random_string_abc123xyz
JWT_EXPIRES_IN=7d
MAX_FILE_SIZE=10485760
FRONTEND_URL=http://localhost:5173
```

> 💡 For MongoDB Atlas, replace `MONGO_URI` with your Atlas connection string.

```bash
# Start the backend (development mode with auto-restart)
npm run dev

# You should see:
# ✅ Connected to MongoDB
# 🚀 Server running on http://localhost:5000
```

---

### Step 2: Set up the Frontend

Open a **new terminal** window:

```bash
# Navigate to frontend folder
cd frontend

# Install dependencies
npm install

# Start the frontend development server
npm run dev

# You should see:
# Local: http://localhost:5173
```

---

### Step 3: Open the App

1. Visit **http://localhost:5173**
2. Click **"Create one"** to register a new account
3. After registering, you'll be redirected to the dashboard
4. Upload files using drag & drop or the file picker
5. View, download, and delete your files!

---

## 📡 API Endpoints

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/api/auth/register` | ❌ | Create account |
| POST | `/api/auth/login` | ❌ | Login, get JWT |
| GET | `/api/auth/me` | ✅ | Get current user |
| POST | `/api/upload` | ✅ | Upload file |
| GET | `/api/files` | ✅ | List files (paginated) |
| GET | `/api/files/:id` | ✅ | Get file details |
| GET | `/api/files/:id/download` | ✅ | Download file |
| DELETE | `/api/files/:id` | ✅ | Delete file |

### Query params for GET /api/files
- `?page=1` — Page number
- `?limit=10` — Results per page
- `?search=resume` — Search by file name
- `?sort=newest` — Sort: `newest`, `oldest`, `name`, `size`

---

## 🔐 Authentication Flow

1. User registers or logs in → backend returns a **JWT token**
2. Frontend stores the token in **localStorage**
3. Every API request automatically includes: `Authorization: Bearer <token>`
4. If token expires (after 7 days), user is redirected to login

---

## 📋 Validation Rules

- **Allowed types:** JPG, JPEG, PNG, PDF, DOCX
- **Max file size:** 10 MB
- **Rate limiting:** 100 requests per 15 min (20 for auth routes)
- Files are **user-scoped** — users can only see/delete their own files

---

## 🛠️ Available npm Scripts

### Backend
```bash
npm run dev    # Development (auto-restart with nodemon)
npm start      # Production
```

### Frontend
```bash
npm run dev    # Development server (hot reload)
npm run build  # Build for production
npm run preview  # Preview production build
```

---

## 🌍 Deployment Tips

### Backend (e.g. Railway, Render, or VPS)
1. Set all `.env` variables in your hosting platform's environment settings
2. Use MongoDB Atlas instead of local MongoDB
3. Run `npm start` as the start command

### Frontend (e.g. Vercel, Netlify)
1. Build with `npm run build`
2. Update `vite.config.js` proxy to point to your deployed backend URL
3. Or set `VITE_API_BASE_URL` as an env variable and update `api.js`

---

## 🎯 Features Summary

- ✅ JWT Authentication (register, login, protected routes)
- ✅ Drag & drop file upload
- ✅ Upload progress bar
- ✅ Image preview in upload zone
- ✅ File type & size validation (frontend + backend)
- ✅ File list with search, sort, pagination
- ✅ File download with download count tracking
- ✅ File deletion (from disk + database)
- ✅ Helmet security headers
- ✅ CORS configuration
- ✅ Rate limiting
- ✅ MongoDB with Mongoose
- ✅ Dark mode UI with amber accent theme
- ✅ Responsive design
- ✅ Skeleton loading states
- ✅ Toast notifications
