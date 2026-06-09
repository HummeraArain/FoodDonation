# Food Donation System

A full-stack MERN-based web application built to minimize food waste and ensure food reaches those in need. The system connects **donors** with **recipients**, facilitating smooth campaign creation, meal applications, real-time communication, and tracking — all within a secure and intuitive environment.

---

## Features

### Authentication & Session Management
- JWT-based login system with access and refresh tokens
- Secure token refresh flow via HTTP-only cookies
- Automatic session handling with toast notifications
- Public and protected route distinction

### Donor Panel
- Create and manage food donation campaigns
- Accept or reject recipient requests
- Real-time chat with recipients for **associated campaigns only**
- View donation history
- Edit personal donor profile

### Recipient Panel
- Browse all available food donation campaigns
- Apply for meals easily
- Real-time chat with associated donors **after applying**
- Track application status (active, granted, etc.)
- View and update recipient profile

### Real-Time Chat (Socket.IO)
- One-to-one messaging between donor and recipient
- Chat enabled only for:
  - Donor’s **own campaigns**
  - Recipients who applied/granted
- Auto-deletion of chats after **7 days** via cron job

### Smart Access Logic
- Donors can't chat or accept requests for others' campaigns
- Chat and actions only available for relevant users
- Unmatched routes handled by a `NotFound` page without unnecessary errors


##  Tech Stack

### Frontend
- **React.js** (Hooks + Router v6)
- **React Toastify** for notifications
- **Tailwind CSS + Custom CSS**
- **Socket.IO client**
- **Font Awesome & Lucide Icons**
- **Formik + Yup**
- **Recharts**

### Backend
- **Node.js + Express.js**
- **MongoDB + Mongoose**
- **Socket.IO** for real-time messaging
- **JWT Auth with Refresh Token Logic**
- **Nodemailer (for emails)**
- **Cron jobs** for chat cleanup

---

## Installation

### 1. Clone the repository
  ```bash
  - git clone https://github.com/salwaaliakbar/FoodDonation.git
  - cd food-donation-system

### 2. Backend setup
  - cd server
  - npm install
  - add .env file
    - MONGO_URI=ADD_YOUR_MONGOURL
    - JWT_SECRET=ADD_YOUR_JWT_SECRET
    - REFRESH_SECRET=ADD_YOUR_REFRESH_TOKEN_SECRET
    - EMAIL_USER=ADD_YOUR_EMAIL
    - EMAIL_PASS=ADD_YOUR_EMAIL_PASSKEY
  - npm start

### 3. Frontnd setup
  - cd client
  - npm install
  - npm run dev


## 📋 Table of Contents

- [Features](#features)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Database Setup](#database-setup)
- [Environment Variables](#environment-variables)
- [Frontend Setup](#frontend-setup)
- [Backend Setup](#backend-setup)
- [Running the Project](#running-the-project)
- [API Overview](#api-overview)
- [Screenshots](#screenshots)

---

## ✨ Features

### 🔐 Authentication & Session Management
- JWT-based login with **access token** (1h) and **refresh token** (30 days)
- Secure HTTP-only cookies for token storage
- Automatic token refresh with silent re-authentication
- Forgot password & reset password via email link (Nodemailer)
- Role-based routing: **Donor** and **Recipient** panels

### 🧑‍🍳 Donor Panel
- Create and manage food donation campaigns (title, food type, quantity, expiry, location, etc.)
- Accept recipient applications and **award meals** to specific applicants
- Real-time notifications when a recipient applies
- View **donation history** (Active / Awarded / Expired campaigns)
- Edit personal profile (name, email, phone, organization)
- Delete campaigns
- Stats dashboard (total active, granted, expired campaigns)

### 🙋 Recipient Panel
- Browse the **general campaign feed** with all active donations
- Apply for meals with number of persons
- Track application status: Active → Granted
- View **active applications** and **granted meals** separately
- Real-time notification when a meal is awarded
- Edit personal profile

### 💬 Real-Time Chat (Socket.IO)
- One-to-one messaging between donor and matched recipient
- Chat rooms are scoped per campaign — donors only chat with their own applicants
- Previous messages loaded on room join
- **Auto-deletion of messages after 7 days** via cron job

### 🔒 Security & Performance
- Rate limiting middleware (global + per-route limiters)
- Protected routes on frontend (role-based access)
- Campaigns auto-expire via scheduled cron job
- Input validation with Yup on frontend

---

## 🛠 Technologies Used

### Frontend
| Technology | Purpose |
|---|---|
| React.js 19 (Hooks + Router v7) | UI framework |
| Tailwind CSS v4 | Styling |
| Formik + Yup | Form handling & validation |
| Socket.IO Client | Real-time chat |
| Recharts | Statistics charts |
| React Toastify | Notifications |
| Lucide React + Font Awesome | Icons |
| Framer Motion | Animations |
| Vite | Build tool |

### Backend
| Technology | Purpose |
|---|---|
| Node.js + Express.js v5 | Server framework |
| MongoDB + Mongoose | Database & ODM |
| Socket.IO | Real-time bidirectional events |
| JSON Web Token (JWT) | Authentication |
| bcrypt | Password hashing |
| Nodemailer | Email service (password reset) |
| node-cron | Scheduled tasks |
| express-rate-limit | Rate limiting |
| cookie-parser | Cookie management |

### Database
- **MongoDB Atlas** (cloud) — recommended for production
- **MongoDB local** (`mongodb://localhost:27017/FoodDonation`) — for development

---

## 📁 Project Structure

```
FoodDonation/
├── client/                        # React frontend (Vite)
│   ├── public/
│   ├── src/
│   │   ├── assets/images/         # Static images
│   │   ├── components/            # Shared UI components
│   │   │   ├── Common/            # ConfirmationDialog, StatusDialog, BtnLoader
│   │   │   ├── Footer/
│   │   │   └── Navbar/
│   │   ├── constants/             # App-level constants
│   │   ├── context/               # React Context (UserContext, SocketProvider, ChangeContext)
│   │   ├── customHooks/           # useSecureFetch, useJoinMealSocket, useHandleDelete
│   │   ├── pages/
│   │   │   ├── Auth/              # Login, Signup, ForgetPassword, ResetPassword
│   │   │   ├── Donor/             # DonorDashboard, CreateCampaign, MealCards, Chat
│   │   │   ├── Recipent/          # RecipientDashboard, Feed, GrantedMeals, ActiveMeals
│   │   │   ├── LandingPage/       # HeroSection, WhyDonate, WorkMethod, etc.
│   │   │   ├── About/
│   │   │   ├── Services/
│   │   │   ├── FAQs/
│   │   │   ├── ContactUs/
│   │   │   └── notFound/
│   │   ├── yupschemas/            # Validation schemas
│   │   ├── App.jsx                # Route definitions
│   │   └── main.jsx
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
│
├── server/                        # Express backend
│   ├── config/
│   │   └── db.js                  # MongoDB connection
│   ├── Controllers/
│   │   ├── authController.js      # login, signup, logout, refresh, forgotPassword, resetPassword
│   │   ├── donorController.js     # createCampaign, updateProfile, getHistory, updateStatus, deleteCampaign
│   │   ├── applyCampaign.js       # Recipient apply to campaign
│   │   ├── getCampaignsFeed.js    # General feed for recipients
│   │   ├── activeFeed.js          # Recipient active applications
│   │   ├── grantedMeals.js        # Recipient granted meals
│   │   ├── mealStatistics.js      # Recipient stats
│   │   └── awardCampaign.js
│   ├── cronJobs/
│   │   ├── expireMeal.js          # Auto-expire campaigns past their date
│   │   └── messagesCron.js        # Delete messages older than 7 days
│   ├── Middlewares/
│   │   ├── authMiddleware.js      # JWT verification
│   │   ├── rateLimiterMiddleware.js
│   │   └── socketStore.js
│   ├── Models/
│   │   ├── userModel.js           # User schema (fullname, email, phone, role, organization, username, password)
│   │   ├── campaignModel.js       # Campaign schema with applied[] and awarded[] subdocuments
│   │   ├── recipentModel.js       # Recipient actions (applied[], awarded[])
│   │   └── messageModel.js        # Chat message schema
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── donorRoutes.js
│   │   └── recipentRoutes.js
│   ├── utils/
│   │   └── constantVariables.js   # ACTIVE, GRANTED, EXPIRED status constants
│   ├── index.js                   # Server entry point + Socket.IO setup
│   ├── .env.example
│   └── package.json
│
└── README.md
```

---

## 🗄 Database Setup

This project uses **MongoDB** as its database. You can use either MongoDB Atlas (cloud) or a local MongoDB installation.

### Option 1: MongoDB Atlas (Recommended)

1. Go to [https://www.mongodb.com/atlas](https://www.mongodb.com/atlas) and create a free account.
2. Create a new **cluster** (free tier is fine).
3. Under **Database Access**, create a new database user with a username and password.
4. Under **Network Access**, add your IP address (or `0.0.0.0/0` for all IPs during development).
5. Click **Connect → Connect your application** and copy the connection string. It looks like:
   ```
   mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/FoodDonation?retryWrites=true&w=majority
   ```
6. Paste this string as the value of `MONGO_URI` in your `server/.env` file.

### Option 2: Local MongoDB

1. Install MongoDB Community Edition from [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community).
2. Start the MongoDB service:
   ```bash
   # Windows
   net start MongoDB

   # macOS/Linux
   sudo systemctl start mongod
   ```
3. Use the default local connection string in your `.env`:
   ```
   MONGO_URI=mongodb://localhost:27017/FoodDonation
   ```

> **Note:** No manual schema creation is needed. Mongoose will automatically create the `users`, `campaign`, `recipent`, and `messages` collections on first use.

---

## 🔐 Environment Variables

The backend requires a `.env` file inside the `server/` directory. Copy the example file and fill in your values:

```bash
cp server/.env.example server/.env
```

### `server/.env.example`

```env
# MongoDB connection string
# Atlas example: mongodb+srv://<user>:<pass>@cluster0.xxxxx.mongodb.net/FoodDonation
# Local example: mongodb://localhost:27017/FoodDonation
MONGO_URI=your_mongodb_connection_string_here

# JWT secret for access tokens (use a long random string)
JWT_SECRET=your_jwt_secret_key_here

# JWT secret for refresh tokens (use a different long random string)
REFRESH_SECRET=your_refresh_token_secret_here

# Gmail address used to send password reset emails
EMAIL_USER=your_gmail_address@gmail.com

# Gmail App Password (not your normal Gmail password)
# Generate at: https://myaccount.google.com/apppasswords
EMAIL_PASS=your_gmail_app_password_here

# Server port (optional, defaults to 5000)
PORT=5000

# Set to "production" only when deploying
NODE_ENV=development
```

> **How to get Gmail App Password:** Go to your Google Account → Security → 2-Step Verification → App Passwords. Generate a password for "Mail".

---

## 💻 Frontend Setup

```bash
# Navigate to the client directory
cd client

# Install dependencies
npm install

# Start the development server
npm run dev
```

The frontend will run at **http://localhost:5173** by default.

> **Note:** The frontend is configured to proxy API requests to `http://localhost:5000`. Make sure the backend server is running before using the app.

---

## ⚙️ Backend Setup

```bash
# Navigate to the server directory
cd server

# Install dependencies
npm install

# Create your .env file from the example
cp .env.example .env
# Then open .env and fill in your actual values

# Start the development server (with nodemon auto-restart)
npm start
```

The backend will run at **http://localhost:5000** by default.

---

## 🚀 Running the Project

Follow these steps in order to run the full application:

### Step 1 — Clone the repository
```bash
git clone https://github.com/salwaaliakbar/FoodDonation.git
cd FoodDonation
```

### Step 2 — Setup and run the Backend
```bash
cd server
npm install
cp .env.example .env
# Edit .env with your MongoDB URI, JWT secrets, and email credentials
npm start
```

### Step 3 — Setup and run the Frontend
Open a **new terminal window/tab**, then:
```bash
cd client
npm install
npm run dev
```

### Step 4 — Open the app
Visit **http://localhost:5173** in your browser.

> Make sure both the backend (port 5000) and frontend (port 5173) are running at the same time.

---

## 🔌 API Overview

### Auth Routes (`/api/`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| POST | `/api/login` | Login user | No |
| POST | `/api/signup` | Register new user | No |
| GET | `/api/logout` | Logout user (clears cookies) | No |
| GET | `/api/me` | Verify session & get user data | Yes |
| GET | `/api/refresh` | Refresh access token via cookie | No |
| POST | `/api/forgotPassword` | Send password reset email | No |
| PUT | `/api/resetPassword/:id/:token` | Reset password with token | No |

### Donor Routes (`/api/`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| POST | `/api/createCampaign` | Create a new donation campaign | Yes |
| PUT | `/api/updateProfile` | Update donor profile | Yes |
| GET | `/api/getHistoy` | Get campaigns by status | Yes |
| GET | `/api/getUserData/:id` | Get a specific user's data | Yes |
| PUT | `/api/updateStatus/:id/:p_id/:p_name/:awardfor` | Award meals to a recipient | Yes |
| GET | `/api/statSummary` | Get campaign count stats | Yes |
| DELETE | `/api/deleteCampaign/:id` | Delete a campaign | Yes |

### Recipient Routes (`/api/`)
| Method | Endpoint | Description | Auth Required |
|---|---|---|---|
| GET | `/api/generalFeed` | Browse all active campaigns | Yes |
| POST | `/api/applyCampaign` | Apply for a campaign | Yes |
| GET | `/api/activeFeed` | Get recipient's active applications | Yes |
| GET | `/api/grantedMeals` | Get recipient's granted meals | Yes |
| GET | `/api/mealStatistics` | Get recipient's meal stats | Yes |

### Socket.IO Events
| Event | Direction | Description |
|---|---|---|
| `joinNotificationRoom` | Client → Server | Join personal notification room |
| `joinRoom` | Client → Server | Join a chat room (loads previous messages) |
| `sendMessage` | Client → Server | Send a chat message |
| `receiveMessage` | Server → Client | Receive a chat message |
| `loadPreviousMessages` | Server → Client | Load chat history on room join |
| `mealAwarded` | Server → Client | Notify recipient when meal is awarded |

---




