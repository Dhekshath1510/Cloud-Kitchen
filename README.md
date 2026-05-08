# 🍽️ Cloud Kitchen

A full-stack **cloud-native food ordering platform** built with **React + Vite** on the frontend and **FastAPI + PostgreSQL** on the backend. The system supports customer-facing menu browsing, cart management, checkout with payment and discount processing, and a real-time analytics dashboard for admins — all containerised with Docker and deployable on Azure.

---

## 📦 Repositories

| Layer | Repository |
|-------|-----------|
| 🎨 **Frontend** | [https://github.com/Dhekshath1510/Cloud-kitchen-frontend.git](https://github.com/Dhekshath1510/Cloud-kitchen-frontend.git) |
| ⚙️ **Backend** | [https://github.com/Dhekshath1510/cloud-kitchen-backend.git](https://github.com/Dhekshath1510/cloud-kitchen-backend.git) |

---
## Azure App Link

** App ** : [https://brave-plant-05d91e300.7.azurestaticapps.net/](https://brave-plant-05d91e300.7.azurestaticapps.net/)

---
## 🏗️ Project Architecture

```
Cloud Kitchen
├── Frontend  (React 19 + Vite + Tailwind CSS)
│   ├── Landing / Login / Register Pages
│   ├── Customer: Home → Menu → Cart → Checkout
│   └── Admin: Analytics Dashboard (Weekly & Monthly charts)
│
└── Backend  (FastAPI + SQLAlchemy + PostgreSQL)
    ├── Auth API  (Register / Login / JWT + Refresh Tokens)
    ├── Menu API  (CRUD for food items by category)
    ├── Orders API (Place, track, and manage orders)
    └── Analytics API (Weekly & Monthly order reports)
```

---

## 🎨 Frontend

**Repository:** [Cloud-kitchen-frontend](https://github.com/Dhekshath1510/Cloud-kitchen-frontend.git)

The frontend is a modern single-page application (SPA) built with **React 19** and **Vite**, styled with **Tailwind CSS** and enriched with smooth animations via **Motion** and chart visualisations via **Recharts**.

### ✨ Key Features

- **Authentication** — Login & Registration with role-based access (`ROLE_USER` / `ROLE_ADMIN`). JWT tokens are stored in `localStorage` with automatic refresh handling.
- **Protected Routes** — Unauthenticated users are automatically redirected to the login page.
- **Customer Flow:**
  - 🏠 **Home / Landing Page** — Browse the available menu fetched from the backend.
  - 🛒 **Cart** — Add/remove items, view totals with tax calculation.
  - 💳 **Checkout** — Apply discount codes, select payment method (UPI, Card, Net Banking, Wallet), and place orders.
  - 📦 **Delivery Info** — Track order delivery details.
- **Admin Dashboard:**
  - 📊 **Weekly Orders Bar Chart** — Fetches order counts per day for the current week.
  - 📈 **Monthly Orders Line Chart** — Displays order trends across all months of the current year.
  - 🔗 **Quick-action shortcuts** — Manage Menu, Orders, Users, Payments, and Discounts.
- **UX Enhancements** — Loading states, empty states, error handling, animated buttons, and message components.

### 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| React 19 | UI framework |
| Vite 7 | Build tool & dev server |
| React Router v7 | Client-side routing |
| TanStack React Query v5 | Server state & data fetching |
| Axios | HTTP client |
| Recharts | Data visualisation charts |
| Motion (Framer Motion) | Micro-animations & transitions |
| Tailwind CSS v4 | Utility-first styling |

### 📁 Project Structure

```
src/
├── components/
│   ├── LandingPage.jsx       # Entry landing screen
│   ├── Login.jsx             # Login & Register form
│   ├── HomePage.jsx          # Customer home with menu
│   ├── Cart.jsx              # Shopping cart
│   ├── Checkout.jsx          # Order checkout & payment
│   ├── AdminPage.jsx         # Admin dashboard shell
│   ├── OrderAnalytics.jsx    # Weekly bar chart analytics
│   ├── MonthlyOrdersLine.jsx # Monthly line chart analytics
│   ├── Header.jsx            # Top navigation bar
│   ├── Menu.jsx              # Menu item cards
│   ├── DiscountCard.jsx      # Discount code display
│   ├── DeliveryInfo.jsx      # Delivery details component
│   ├── AnimatedButton.jsx    # Reusable animated button
│   ├── Loading.jsx           # Loading spinner
│   └── Message.jsx           # Toast/notification messages
├── services/                 # Axios API service layer
├── assets/                   # Static assets & images
├── icons/                    # SVG icon components
├── App.jsx                   # Root app with routing
└── main.jsx                  # Application entry point
```

### 🚀 Getting Started (Frontend)

**Prerequisites:** Node.js 20+

```bash
# Clone the repository
git clone https://github.com/Dhekshath1510/Cloud-kitchen-frontend.git
cd Cloud-kitchen-frontend

# Install dependencies
npm install

# Set environment variable (optional, defaults to http://localhost:8001)
# Create a .env file:
VITE_API_URL=http://localhost:8001

# Start development server
npm run dev
```

The app will be available at `http://localhost:5173`.

### 🐳 Docker (Frontend)

```bash
# Build the Docker image (multi-stage: Node build → Nginx serve)
docker build -t cloud-kitchen-frontend:latest .

# Run the container
docker run -d --name cloud-kitchen-frontend -p 4173:80 cloud-kitchen-frontend:latest

# Access at http://localhost:4173
```

### 🔗 API Endpoints Consumed

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/login` | Login with `{ userName, password, role }` |
| `POST` | `/register` | Register a new user |
| `GET` | `/menu` | Fetch all menu items (Bearer token required) |
| `GET` | `/analytics/orders/weekly` | Weekly order counts (`startDate`, `endDate`) |
| `GET` | `/analytics/orders/monthly` | Monthly order counts for current year |

---

## ⚙️ Backend

**Repository:** [cloud-kitchen-backend](https://github.com/Dhekshath1510/cloud-kitchen-backend.git)

The backend is a production-grade REST API built with **FastAPI**, using **SQLAlchemy** for ORM, **Alembic** for database migrations, and **PostgreSQL** as the database. Authentication is handled via **JWT access tokens** and **refresh tokens** stored in the database, with passwords secured using **bcrypt / Argon2** hashing.

### ✨ Key Features

- **JWT Authentication** — Secure login and registration with role-based access control (`ROLE_USER` / `ROLE_ADMIN`).
- **Refresh Token System** — Long-lived refresh tokens stored in the database with expiry tracking.
- **Menu Management** — Full CRUD API for food items with categories (Starter, Main Course, Dessert, Beverage, Pizza, Burger, etc.).
- **Order Management** — Place orders with multiple items, track status transitions (`PENDING → CONFIRMED → PROCESSING → SHIPPED → DELIVERED`).
- **Payment Processing** — Record payments with multiple methods (UPI, Card, Net Banking, Wallet) and status tracking.
- **Discount System** — Apply flat or percentage discounts with usage limits, minimum order values, and expiry handling.
- **Analytics API** — Aggregate weekly and monthly order data for admin reporting.
- **CORS Support** — Configured for cross-origin requests from the frontend.
- **Azure-ready** — Reads `PORT` from environment variables, suitable for Azure App Service or Azure Container Apps deployment.

### 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| FastAPI 0.110 | API framework |
| Uvicorn / Gunicorn | ASGI server |
| SQLAlchemy 2.0 | ORM & database abstraction |
| Alembic 1.13 | Database migrations |
| PostgreSQL | Relational database |
| Pydantic v2 | Data validation & settings |
| python-jose | JWT token creation & validation |
| passlib + bcrypt | Password hashing |
| argon2-cffi | Argon2 password hashing |
| pydantic-settings | Environment config management |
| python-dotenv | `.env` file support |

### 📁 Project Structure

```
app/
├── main.py                   # FastAPI app entry point, CORS, router registration
├── api/
│   ├── routes/
│   │   ├── auth.py           # Login, Register, Token refresh endpoints
│   │   ├── menu.py           # Menu item CRUD endpoints
│   │   └── orders.py         # Order placement & management endpoints
│   └── dependencies/         # Auth dependencies (get_current_user, etc.)
├── core/
│   └── config.py             # App settings via pydantic-settings
├── db/
│   ├── base.py               # SQLAlchemy declarative base
│   └── session.py            # Database engine & session factory
├── models/
│   └── domain.py             # All SQLAlchemy ORM models (see below)
├── schemas/                  # Pydantic request/response schemas
├── repository/               # Data access layer (CRUD operations)
└── services/                 # Business logic layer
```

### 🗄️ Database Models

| Model | Table | Description |
|-------|-------|-------------|
| `User` | `users` | Registered users with role (`ROLE_USER` / `ROLE_ADMIN`) |
| `Item` | `items` | Menu items with category, price, availability, and veg flag |
| `Order` | `orders` | Customer orders with status, tax, total cost, and discount code |
| `OrderItem` | `order_items` | Line items linking orders to menu items with quantities |
| `Payment` | `payments` | Payment records linked to orders with method and status |
| `Discount` | `discounts` | Discount codes with type (flat/percentage), usage limits, and expiry |
| `RefreshToken` | `refresh_tokens` | Stored refresh tokens for secure session management |

#### Enumerations

- **ItemCategory:** `STARTER`, `MAIN_COURSE`, `DESSERT`, `BEVERAGE`, `SNACKS`, `SALAD`, `SOUP`, `ADD_ON`, `PIZZA`, `BURGER`, `SANDWICH`, `PASTA`
- **OrderStatus:** `PENDING` → `CONFIRMED` → `PROCESSING` → `SHIPPED` → `DELIVERED` / `FAILED` / `CANCELLED`
- **PaymentStatus:** `PENDING`, `SUCCESS`, `FAILED`, `CANCELLED`, `REFUNDED`
- **PaymentMethod:** `UPI`, `CARD`, `NET_BANKING`, `WALLET`
- **DiscountType:** `FLAT`, `PERCENTAGE`

### 🚀 Getting Started (Backend)

**Prerequisites:** Python 3.11+, PostgreSQL

```bash
# Clone the repository
git clone https://github.com/Dhekshath1510/cloud-kitchen-backend.git
cd cloud-kitchen-backend

# Install dependencies
pip install -r requirements.txt

# Configure environment — create a .env file:
DATABASE_URL=postgresql://user:password@localhost:5432/cloud_kitchen
SECRET_KEY=your-super-secret-key

# Run database migrations
alembic upgrade head

# Start the development server
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The API will be available at `http://localhost:8000`.  
Interactive API docs (Swagger UI): `http://localhost:8000/docs`

### 🐳 Docker (Backend)

```bash
# Build the Docker image (multi-stage: builder → slim runtime)
docker build -t cloud-kitchen-backend:latest .

# Run with environment variables
docker run -d --name cloud-kitchen-backend \
  -p 8001:8000 \
  -e DATABASE_URL="postgresql://user:password@host:5432/cloud_kitchen" \
  -e SECRET_KEY="your-secret-key" \
  cloud-kitchen-backend:latest

# Access the API at http://localhost:8001
# Swagger docs at http://localhost:8001/docs
```

---

## 🔄 How Frontend & Backend Connect

```
Browser (React SPA)
       │
       │  HTTP requests with Bearer JWT
       ▼
FastAPI Backend (Port 8001 / Azure)
       │
       │  SQLAlchemy ORM queries
       ▼
PostgreSQL Database
```

1. User registers or logs in via the React frontend → backend returns a **JWT access token**.
2. All subsequent API calls include `Authorization: Bearer <token>` in the header.
3. The frontend fetches the **menu**, places **orders**, and applies **discount codes** through dedicated REST endpoints.
4. Admin users access the **analytics dashboard** which queries aggregated order data from the backend.
5. **Refresh tokens** are used to silently renew expired access tokens without requiring re-login.

---

## 🌐 Deployment (Azure)

Both services are containerised and deployable to **Azure Container Apps** or **Azure App Service**:

- The backend reads the `PORT` environment variable dynamically, compatible with Azure's dynamic port assignment.
- The frontend is served via **Nginx** inside its Docker container, built as a production-optimised static bundle.
- Set `VITE_API_URL` in the frontend build environment to point to the deployed backend URL.

---

## 📝 Environment Variables

### Frontend (`.env`)
```env
VITE_API_URL=https://your-backend-url.azurewebsites.net
```

### Backend (`.env`)
```env
DATABASE_URL=postgresql://user:password@host:5432/cloud_kitchen
SECRET_KEY=your-super-secret-jwt-signing-key
```

---

## 📄 License

This project is intended for educational and demonstration purposes as part of a cloud computing study on Microsoft Azure.
