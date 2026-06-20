# Feature 1 — Authentication (Register / Login / Profile / Avatar)

## What's included

**Backend** (`backend/`)
- `server.js` — Express app with only auth routes mounted
- `db.js` — MongoDB connection
- `routes/authRoutes.js` — `/api/auth/register`, `/api/auth/login`, `/api/auth/profile`, `/api/auth/avatar`
- `controllers/authController.js`
- `middleware/authMiddleware.js` — JWT `protect` + `adminOnly`
- `middleware/errorHandler.js`
- `middleware/upload.js` — Multer + Cloudinary avatar upload
- `models/User.js`
- `schemas/authSchemas.js` — Zod validation
- `utils/generateToken.js`, `utils/asyncWrapper.js`, `utils/AppError.js`

**Frontend** (`frontend/`)
- `src/pages/LoginPage.jsx`
- `src/pages/RegisterPage.jsx`
- `src/pages/AccountPage.jsx` (profile + avatar upload + logout)
- `src/pages/HomePage.jsx` (placeholder)
- `src/components/Navbar.jsx`
- `src/components/ProtectedRoute.jsx`
- `src/store/authStore.js` — Zustand auth state (persisted to localStorage)
- `src/api/axiosClient.js` — Axios instance with JWT interceptor

## Setup

### Backend
```bash
cd backend
npm install
cp .env.secrets .env    # contains real keys — see warning below
npm run dev             # runs on http://localhost:5000
```

### Frontend
```bash
cd frontend
npm install
npm run dev             # runs on http://localhost:5173
```

The Vite dev server proxies `/api/*` to `http://localhost:5000`.

## API Endpoints (Feature 1)

| Method | Endpoint            | Auth        | Description                |
|--------|---------------------|-------------|----------------------------|
| POST   | `/api/auth/register`| Public      | Create account, returns JWT|
| POST   | `/api/auth/login`   | Public      | Login, returns JWT         |
| GET    | `/api/auth/profile` | Bearer JWT  | Get logged-in user         |
| PATCH  | `/api/auth/avatar`  | Bearer JWT  | Upload avatar (multipart)  |

## ⚠️ Security Warning

`.env.secrets` contains real MongoDB, JWT, and Cloudinary credentials that were
shared during chat. **Rotate these before deploying to production:**
- Change the MongoDB Atlas password for user `sssshyam702`
- Generate a new `JWT_SECRET`
- Regenerate the Cloudinary API secret

Never commit `.env` or `.env.secrets` to git (already in `.gitignore`).

## Integration tracking notes

- Frontend talks to backend via `axiosClient` (`baseURL: /api`)
- JWT stored in `authStore` (Zustand, persisted), auto-attached to requests
- On 401, user is auto-logged out and redirected to `/login`
- Navbar links to `/todos`, `/chat`, `/admin` are commented out — they'll be
  uncommented as Feature 2+ packages (Todos, AI Chat, Admin) are added
- `App.jsx` routes only include Home / Login / Register / Account for now

Say "next" when ready for Feature 2.
