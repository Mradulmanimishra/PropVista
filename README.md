<div align="center">

# 🏠 PropVista

### Full-stack real estate platform for Vrindavan & Mathura, India

**Buy, sell, and discover properties — powered by Django REST, React 19, JWT auth, Cloudinary, and Gemini AI.**

[![Django](https://img.shields.io/badge/Django-5.2-092E20?style=for-the-badge&logo=django&logoColor=white)](https://djangoproject.com)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-7.0-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![JWT](https://img.shields.io/badge/JWT-SimpleJWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io)
[![Cloudinary](https://img.shields.io/badge/Cloudinary-Media-3448C5?style=for-the-badge&logo=cloudinary&logoColor=white)](https://cloudinary.com)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Production-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

</div>

---

## 🚀 Live URLs (local dev)

| Service | URL |
|---|---|
| Frontend | http://localhost:5173 |
| Backend API | http://localhost:8000 |
| Admin Panel | http://localhost:8000/admin |

---

## ✨ What's Built

### 🔐 Authentication
- Register with email + password
- Email verification (24-hour token, console backend in dev)
- JWT login/logout with refresh token blacklist
- Password reset via email link
- Dev-verify endpoint for local testing (disabled in production)

### 🏡 Property Listings
- Multi-step sell form — 5 steps: Info → Location → Features → Photos → Seller
- Multiple image upload per listing (`PropertyImage` model)
- Search & filter — city, type, price range, bedrooms, verified-only
- Pagination — 12 listings per page
- Property status badges — Active / Sold / Under Review
- Verified listing badge — admin-controlled
- View count — increments on every property detail visit
- Property edit & delete — owner only

### 🔍 Discovery
- Property comparison tool — select up to 3, side-by-side spec table
- Favourites / Wishlist — synced to backend (logged in) or localStorage (guest)
- Recently viewed — last 5 properties tracked in localStorage
- Similar properties — same city + type, shown on detail page
- Price trend chart — monthly average price by city (recharts)

### 📋 Property Detail
- Image lightbox — click to fullscreen
- 5 tabs: Details / Inquiry / Reviews / EMI Calculator / Map
- EMI calculator — pre-fills property price, calculates monthly payment
- Map — Leaflet + OpenStreetMap, no API key needed
- WhatsApp contact button — pre-filled message to seller
- Share button — copies URL to clipboard
- Seller contact reveal — phone + email toggle

### 💬 Inquiry & Reviews
- Send inquiry form — stored in DB + email notification to seller
- 5-star reviews + text comments — one per user per property
- Average rating shown on listing cards and detail page

### 🤖 AI Chat
- Floating chat widget powered by Gemini 2.0 Flash (free tier)
- Fed with live property listings as context
- Ask: "Show 3BHK under ₹50L in Vrindavan" → returns matching properties

### 👤 User Dashboard
- My Listings — view, edit, delete own properties
- Saved Properties — favourites list with remove option

---

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, React Router v7, Axios, Vite 7 |
| Backend | Django 5.2, Django REST Framework 3.16 |
| Auth | JWT (SimpleJWT) + Email Verification |
| Database | SQLite (dev) → PostgreSQL (production via `dj-database-url`) |
| Media Storage | Cloudinary |
| Static Files | WhiteNoise |
| Email | Gmail SMTP (console backend in dev) |
| Maps | Leaflet + OpenStreetMap (free, no API key) |
| Charts | Recharts |
| AI | Google Gemini 2.0 Flash API (free tier) |
| Deployment | Railway (backend) + Vercel (frontend) |
| Package Mgr | `uv` (Python), `npm` (Node) |

---

## 📁 Project Structure

```
PropVista/
├── .github/
│   ├── ISSUE_TEMPLATE/        # Bug & feature request templates
│   ├── PULL_REQUEST_TEMPLATE.md
│   ├── CONTRIBUTING.md
│   └── SECURITY.md
├── Backend/
│   ├── backend/
│   │   ├── accounts/          # Auth: register, login, verify, reset, JWT
│   │   ├── PostProperty/      # Listings, images, favourites, reviews, inquiry, AI chat
│   │   └── backend/           # Django settings, URLs, WSGI
│   ├── Procfile               # Railway deployment
│   ├── nixpacks.toml          # Railway build config
│   ├── requirements.txt       # Production dependencies
│   └── pyproject.toml
├── frontend/
│   └── src/
│       ├── Components/
│       │   ├── Auth/          # Login + Signup
│       │   ├── Buy/           # BuyPage + PropertyDetail
│       │   ├── Chat/          # AI chat widget
│       │   ├── Dashboard/     # User dashboard
│       │   ├── Home/          # Home, Project, AboutGroup
│       │   └── Sell/          # Multi-step SellPage
│       ├── utils/
│       │   ├── api.js         # Centralised API base URL
│       │   └── RequireAuth.jsx
│       └── App.jsx
├── LICENSE
└── README.md
```

---

## ⚙️ Local Setup

### Prerequisites
- [uv](https://docs.astral.sh/uv/) — Python package manager
- Node.js 18+

### 1. Clone
```bash
git clone https://github.com/Mradulmanimishra/PropVista.git
cd PropVista
```

### 2. Backend
```bash
cd Backend

# Install dependencies
uv sync

# Create environment file
cp .env.example backend/.env
# Edit backend/.env with your values

# Run migrations
uv run python backend/manage.py migrate

# Start server
uv run python backend/manage.py runserver
```

### 3. Frontend
```bash
cd frontend

# Install dependencies
npm install

# Create env file
echo "VITE_API_URL=http://localhost:8000" > .env

# Start dev server
npm run dev
```

### 4. Create admin user (optional)
```bash
cd Backend
uv run python backend/manage.py createsuperuser
```

---

## 🔌 API Reference

### Auth — `/api/accounts/`

| Method | Endpoint | Description |
|---|---|---|
| POST | `auth/register/` | Register new user |
| GET | `auth/verify-email/<token>/` | Verify email |
| POST | `auth/login/` | Login → JWT tokens |
| POST | `auth/logout/` | Logout (blacklist refresh token) |
| POST | `auth/password-reset/request/` | Request password reset email |
| PATCH | `auth/password-reset/setnewpassword/` | Set new password |
| POST | `auth/dev-verify/` | Dev only — verify user without email |

### Property — `/api/property/`

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| GET | `buy/` | List properties (paginated, filterable) | No |
| POST | `sell/` | Create listing + upload images | Required |
| GET | `property/<id>/` | Property detail + increments view count | No |
| PATCH | `my-listings/<id>/update/` | Edit own listing | Required |
| DELETE | `my-listings/<id>/delete/` | Delete own listing | Required |
| GET | `my-listings/` | Current user's listings | Required |
| POST | `favourites/<id>/toggle/` | Add/remove favourite | Required |
| GET | `favourites/` | List saved properties | Required |
| POST | `property/<id>/inquiry/` | Send inquiry to seller | Required |
| GET/POST | `property/<id>/reviews/` | List or submit reviews | Required |
| GET | `property/<id>/similar/` | Similar properties | No |
| POST | `chat/` | AI chat (Gemini) | Required |
| GET | `price-trend/` | Monthly avg price by city | No |

---

## 🌍 Deployment

### Frontend → Vercel
1. Import repo at [vercel.com](https://vercel.com)
2. Root directory: `frontend`
3. Add env variable: `VITE_API_URL=https://your-railway-app.up.railway.app`
4. Deploy

### Backend → Railway
1. New project at [railway.app](https://railway.app) → Deploy from GitHub
2. Root directory: `Backend`
3. Add PostgreSQL plugin (auto-sets `DATABASE_URL`)
4. Add environment variables (see `.env.example`)
5. Railway auto-runs `migrate` on every deploy via `Procfile`

### Environment Variables

| Variable | Description |
|---|---|
| `SECRET_KEY` | Django secret key |
| `DEBUG` | `True` for dev, `False` for prod |
| `ALLOWED_HOSTS` | Comma-separated allowed hosts |
| `CORS_ALLOWED_ORIGINS` | Frontend URL (e.g. `https://propvista.vercel.app`) |
| `DATABASE_URL` | PostgreSQL URL (auto-set by Railway) |
| `CLOUD_NAME` | Cloudinary cloud name |
| `CLOUD_API_KEY` | Cloudinary API key |
| `CLOUD_API_SECRET` | Cloudinary API secret |
| `EMAIL_HOST_USER` | Gmail address |
| `EMAIL_HOST_PASSWORD` | Gmail App Password |
| `GEMINI_API_KEY` | Google Gemini API key (free at aistudio.google.com) |

---

## 🗺️ Roadmap

### ✅ Phase 1 — Core
- [x] JWT auth + email verification + password reset
- [x] Multi-step property listing form
- [x] Property browse + detail view
- [x] Cloudinary image uploads
- [x] Mobile-responsive design

### ✅ Phase 2 — Enhancements
- [x] Search & filter (city, type, price, bedrooms)
- [x] Favourites / wishlist
- [x] Property comparison tool
- [x] User dashboard (my listings + saved)
- [x] Property status badges (Active / Sold / Under Review)
- [x] Verified listing badge
- [x] Pagination
- [x] Property edit & delete
- [x] Multiple images per listing
- [x] View count
- [x] Map (Leaflet/OpenStreetMap)
- [x] EMI calculator
- [x] WhatsApp contact button
- [x] Property inquiry form
- [x] Reviews & ratings
- [x] Price trend chart
- [x] Recently viewed
- [x] Image lightbox
- [x] Share button
- [x] AI chat widget (Gemini)
- [x] Production config (Railway + Vercel + PostgreSQL)

### 🔄 Phase 3 — AI & ML
- [ ] ML price prediction (Random Forest / XGBoost)
- [ ] Smart property recommendations
- [ ] Image quality auto-check
- [ ] Neighborhood insights (temples, schools, hospitals via Overpass API)

### 📱 Phase 4 — Mobile
- [ ] React Native app (Expo)
- [ ] Push notifications
- [ ] In-app messaging

---

## 🤝 Contributing

Read [CONTRIBUTING.md](.github/CONTRIBUTING.md) before opening a PR.

1. Fork → `git checkout -b feature/your-feature`
2. Commit → `git commit -m 'feat: your feature'`
3. Push → `git push origin feature/your-feature`
4. Open a Pull Request

---

## 🔒 Security

Report vulnerabilities via [SECURITY.md](.github/SECURITY.md) — do not open public issues.

---

## 📄 License

[MIT License](LICENSE)

---

<div align="center">

Built by [Mradul Mani Mishra](https://github.com/Mradulmanimishra)  
Vrindavan, Uttar Pradesh, India

*"Our Client Satisfaction is Our Priority"*

⭐ Star this repo if you find it useful

</div>
