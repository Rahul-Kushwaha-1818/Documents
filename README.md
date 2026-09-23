# 📋 FitPlus Tracker — Complete Project Documentation

> **Project Name:** FitPlus Tracker  
> **Developer:** Rahul Kushwaha  
> **Type:** Full-Stack Web Application (Gym Management & Workout Tracking)  
> **License:** MIT (Open Source)

---

## 📌 Table of Contents

1. [What Is This Project?](#1--what-is-this-project)
2. [Who Is This For? (Three Types of Users)](#2--who-is-this-for-three-types-of-users)
3. [Technology Stack (What Tools Are Used)](#3--technology-stack-what-tools-are-used)
4. [Project Folder Structure](#4--project-folder-structure)
5. [Database Models (How Data Is Stored)](#5--database-models-how-data-is-stored)
6. [Backend API Endpoints (Server Features)](#6--backend-api-endpoints-server-features)
7. [Security Features](#7--security-features)
8. [Frontend Pages & Components (What Users See)](#8--frontend-pages--components-what-users-see)
9. [Feature-by-Feature Breakdown](#9--feature-by-feature-breakdown)
10. [Gamification System (Points, Streaks, Badges)](#10--gamification-system-points-streaks-badges)
11. [Multi-Gym / Multi-Tenant System](#11--multi-gym--multi-tenant-system)
12. [QR Code System (How Check-In Works)](#12--qr-code-system-how-check-in-works)
13. [Billing & Receipt System](#13--billing--receipt-system)
14. [Announcement / Notice Board System](#14--announcement--notice-board-system)
15. [Leaderboard & Competition System](#15--leaderboard--competition-system)
16. [Profile & Shareable Performance Card](#16--profile--shareable-performance-card)
17. [Deployment & Hosting](#17--deployment--hosting)
18. [Complete File-by-File Summary](#18--complete-file-by-file-summary)

---

## 1. 🏋️ What Is This Project?

**FitPlus Tracker** is a **complete gym management website** where:

- **Gym members** can check in using a QR code, track their workouts, see their streaks, and compete on a leaderboard.
- **Gym owners (Admins)** can manage members, handle billing, post announcements, and view gym analytics.
- **The platform owner (SuperAdmin)** can rent out the software to multiple gyms, manage all gyms from one dashboard, and track rental payments.

**Think of it like this:**
- The **SuperAdmin** is like a landlord who owns the building (the platform).
- Each **Admin** is like a shop owner who rents a shop (a gym instance) in that building.
- Each **Member** is like a customer who visits a specific shop (their gym).

> [!IMPORTANT]
> Each gym's data is completely **isolated** — members of Gym A cannot see Gym B's data, and vice versa. This is called **multi-tenancy**.

---

## 2. 👥 Who Is This For? (Three Types of Users)

| Role | Who They Are | What They Can Do |
|------|-------------|------------------|
| **Member (User)** | A person who goes to the gym | Check in via QR code, log workouts, track streaks, view heatmap, see leaderboard, view receipts, manage profile |
| **Admin** | The gym owner or manager | Add/edit/delete members, view dashboard analytics, handle billing & receipts, post announcements, manage gym desk QR display |
| **SuperAdmin** | The platform owner (you, the developer) | Create & manage multiple gyms, set rental periods & fees, create admin accounts, suspend/delete gyms, view global stats |

---

## 3. 🛠 Technology Stack (What Tools Are Used)

### Frontend (What Users See in the Browser)

| Tool | What It Does |
|------|-------------|
| **React 18** | JavaScript library for building the user interface (pages, buttons, forms) |
| **Vite** | Super-fast development server and build tool — makes the app load quickly |
| **Tailwind CSS** | Utility-based CSS framework for styling (colors, spacing, layout) |
| **React Router v6** | Handles page navigation (going from login to dashboard, etc.) |
| **Axios** | Makes HTTP requests to the backend API (fetching data, submitting forms) |
| **Recharts** | Creates beautiful bar charts and area charts for analytics |
| **html5-qrcode** | Opens the phone camera and scans QR codes |
| **qrcode** | Generates QR code images on the client side |
| **html-to-image** | Converts the performance report card into a downloadable PNG image |
| **Lucide React** | Provides beautiful, modern icons throughout the app |
| **Plus Jakarta Sans** | The custom Google Font used throughout the app |

### Backend (The Server That Handles Data)

| Tool | What It Does |
|------|-------------|
| **Node.js (v20+)** | JavaScript runtime that runs the server |
| **Express.js** | Web framework that creates the API (handles requests and responses) |
| **MongoDB** | NoSQL database where all data is stored (members, workouts, gyms) |
| **Mongoose** | Library that makes it easy to work with MongoDB in Node.js |
| **JWT (JSON Web Tokens)** | Secure login tokens that keep users authenticated |
| **bcryptjs** | Encrypts (hashes) passwords so they can't be read even if the database is hacked |
| **mongodb-memory-server** | A fallback in-memory database — if MongoDB isn't installed, the app still works! |
| **dotenv** | Reads secret configuration values from a `.env` file |
| **cors** | Allows the frontend and backend to communicate across different domains |
| **qrcode** | Generates QR code images on the server side |

---

## 4. 📁 Project Folder Structure

```
FitPlus-Tracker/
│
├── backend/                        ← Server-side code
│   ├── server.js                   ← Main entry point (starts the server)
│   ├── seed.js                     ← Script to create demo data
│   ├── package.json                ← Backend dependencies list
│   ├── .env                        ← Secret settings (database URL, JWT key)
│   │
│   ├── config/
│   │   └── db.js                   ← Connects to MongoDB (with auto-fallback)
│   │
│   ├── models/                     ← Database table definitions
│   │   ├── User.js                 ← Member/Admin/SuperAdmin accounts
│   │   ├── Gym.js                  ← Gym tenant data (name, logo, rental)
│   │   ├── GymQRToken.js           ← Daily rotating QR check-in tokens
│   │   ├── WorkoutLog.js           ← Individual workout sessions
│   │   ├── PersonalRecord.js       ← Member personal best lifts (PRs)
│   │   ├── PaymentReceipt.js       ← Fee payment receipts
│   │   └── Announcement.js         ← Gym notice board posts
│   │
│   ├── controllers/                ← Business logic (what happens when API is called)
│   │   ├── authController.js       ← Login, activate, forgot password, profile
│   │   ├── adminController.js      ← Member management, dashboard stats
│   │   ├── superAdminController.js ← Multi-gym management, rentals
│   │   ├── workoutController.js    ← QR scan, workout logs, streaks, leaderboard, badges
│   │   ├── qrController.js         ← Generate & regenerate daily QR codes
│   │   ├── receiptController.js    ← Create & view payment receipts
│   │   └── announcementController.js ← Create & manage gym announcements
│   │
│   ├── routes/                     ← URL path definitions (maps URLs to controllers)
│   │   ├── authRoutes.js
│   │   ├── adminRoutes.js
│   │   ├── superAdminRoutes.js
│   │   ├── workoutRoutes.js
│   │   ├── qrRoutes.js
│   │   ├── receiptRoutes.js
│   │   └── announcementRoutes.js
│   │
│   ├── middleware/
│   │   └── authMiddleware.js       ← Checks login tokens & user permissions
│   │
│   └── utils/
│       └── imageUrl.js             ← Converts Google Drive links to displayable image URLs
│
├── frontend/                       ← Client-side code (browser)
│   ├── index.html                  ← HTML shell (loads the React app)
│   ├── package.json                ← Frontend dependencies list
│   ├── vite.config.js              ← Build tool configuration
│   ├── tailwind.config.js          ← Design system colors & theme
│   ├── postcss.config.js           ← CSS processing config
│   │
│   └── src/
│       ├── main.jsx                ← App entry point (mounts React)
│       ├── App.jsx                 ← Router & layout shell (defines all routes)
│       ├── index.css               ← Global styles & glow effects
│       │
│       ├── context/
│       │   └── AuthContext.jsx     ← Global login state manager
│       │
│       ├── services/
│       │   └── api.js              ← Axios HTTP client with JWT auto-injection
│       │
│       ├── utils/
│       │   └── imageUrl.js         ← Google Drive URL formatter (frontend)
│       │
│       ├── components/             ← Reusable UI building blocks
│       │   ├── Navbar.jsx          ← Top navigation bar (role-aware)
│       │   ├── Footer.jsx          ← Bottom footer with features & QR
│       │   ├── ProtectedRoute.jsx  ← Route guard (blocks unauthorized access)
│       │   ├── QRScanner.jsx       ← Camera-based QR code scanner
│       │   ├── StatCard.jsx        ← Metric display card
│       │   ├── WorkoutHeatmap.jsx  ← GitHub-style attendance dot grid
│       │   └── ReceiptModal.jsx    ← Printable fee receipt popup
│       │
│       └── pages/                  ← Full-page views
│           ├── Home.jsx            ← Public landing page
│           ├── Login.jsx           ← Sign-in page
│           ├── ActivateAccount.jsx ← First-time password setup
│           ├── ForgotPassword.jsx  ← Password reset page
│           ├── UserDashboard.jsx   ← Member workout portal (MAIN PAGE)
│           ├── AdminDashboard.jsx  ← Gym owner control panel
│           ├── SuperAdminDashboard.jsx ← Multi-gym master console
│           ├── MembersList.jsx     ← Member directory & management
│           ├── GymDeskQR.jsx       ← Full-screen kiosk QR display
│           ├── Leaderboard.jsx     ← Competition rankings page
│           ├── WorkoutLogs.jsx     ← Admin workout audit logs
│           └── Profile.jsx         ← Profile settings & performance card
│
├── vercel.json                     ← Vercel deployment config
├── .gitignore                      ← Files excluded from Git
└── README.md                       ← Project documentation
```

---

## 5. 💾 Database Models (How Data Is Stored)

The app uses **MongoDB** (a NoSQL database). Here are the **7 data collections** (like tables):

---

### 5.1 User Model — `User.js`

> Stores every person who uses the app (members, admins, superadmins).

| Field | Type | What It Stores |
|-------|------|---------------|
| `memberId` | String | Unique ID like `GYM-1001`, `ADMIN-001`, `SUPER-001` |
| `name` | String | Full name of the person |
| `email` | String | Email address (unique, lowercase) |
| `phone` | String | Phone number |
| `password` | String | Encrypted password (hashed with bcrypt) |
| `role` | String | One of: `superadmin`, `admin`, `user` |
| `gym` | Reference | Links to which Gym this person belongs to |
| `membership.joinDate` | Date | When the membership started |
| `membership.durationMonths` | Number | Plan length (1, 3, 6, or 12 months) |
| `membership.expiryDate` | Date | When the membership expires |
| `membership.status` | String | `active`, `expired`, or `pending_activation` |
| `streak` | Number | Consecutive days of gym attendance |
| `lastCheckInDate` | String | Last date the member checked in (YYYY-MM-DD) |
| `points` | Number | Gamification points earned |
| `activationToken` | String | 6-character code for first-time password setup |
| `isActivated` | Boolean | Whether the member has set their password |

**Special behaviors:**
- Passwords are **automatically encrypted** before saving (using bcrypt with salt round 10).
- The `matchPassword()` method securely compares entered passwords against the stored hash.

---

### 5.2 Gym Model — `Gym.js`

> Stores each gym's identity, branding, and rental information.

| Field | Type | What It Stores |
|-------|------|---------------|
| `name` | String | Gym name (e.g., "FitPulse Elite Gym") |
| `logo` | String | Logo image URL (supports Google Drive links) |
| `tagline` | String | Gym slogan |
| `code` | String | Unique gym code (e.g., `FIT-001`) |
| `adminUser` | Reference | Links to the Admin user managing this gym |
| `contactEmail` | String | Gym's contact email |
| `contactPhone` | String | Gym's contact phone |
| `address` | String | Physical gym address |
| `rentalStartDate` | Date | When the gym started renting the platform |
| `rentalDurationMonths` | Number | How many months of rental purchased |
| `rentalExpiryDate` | Date | When the rental expires |
| `rentAmount` | Number | Monthly rent fee in ₹ |
| `status` | String | `active`, `expiring_soon`, `expired`, or `suspended` |
| `notes` | String | Admin notes (also stores rental payment history) |

**Special behaviors:**
- Google Drive logo links are **automatically converted** to direct image URLs.
- Expiry date is **auto-calculated** from start date + duration if not manually set.

---

### 5.3 GymQRToken Model — `GymQRToken.js`

> Stores the daily rotating QR code tokens used for check-in.

| Field | Type | What It Stores |
|-------|------|---------------|
| `token` | String | Unique daily token (e.g., `GYM_ENTRY_FIT_2026-09-23_A1B2C3D4`) |
| `date` | String | The date this token is valid for (YYYY-MM-DD) |
| `gym` | Reference | Which gym this token belongs to |
| `qrDataUrl` | String | Base64-encoded QR code image |
| `isActive` | Boolean | Whether this token is still valid |
| `createdAt` | Date | Auto-deletes from database after **24 hours** (TTL index) |

**Special behaviors:**
- Tokens **automatically expire and self-delete** from the database after 24 hours.
- Multiple tokens per day are supported (old ones get deactivated when regenerated).

---

### 5.4 WorkoutLog Model — `WorkoutLog.js`

> Stores each individual workout session (one record per gym visit).

| Field | Type | What It Stores |
|-------|------|---------------|
| `user` | Reference | Which member did this workout |
| `gym` | Reference | Which gym this happened at |
| `date` | String | Date of the workout (YYYY-MM-DD) |
| `checkInTime` | Date | Exact time the member scanned in |
| `checkOutTime` | Date | Exact time the member scanned out |
| `durationMinutes` | Number | Total workout duration in minutes |
| `status` | String | `in_progress` (still in gym) or `completed` (checked out) |
| `bodyParts` | Array | Muscle groups trained (e.g., `["Chest", "Triceps", "Shoulders"]`) |

---

### 5.5 PersonalRecord Model — `PersonalRecord.js`

> Stores members' personal best lifts (1-rep max records).

| Field | Type | What It Stores |
|-------|------|---------------|
| `user` | Reference | Which member this PR belongs to |
| `gym` | Reference | Which gym |
| `exerciseName` | String | Name of the exercise (e.g., "Bench Press") |
| `weight` | Number | Weight lifted |
| `unit` | String | `kg` or `lbs` |
| `reps` | Number | Number of reps (default: 1 for 1RM) |
| `date` | String | Date the record was set |
| `notes` | String | Optional notes |

---

### 5.6 PaymentReceipt Model — `PaymentReceipt.js`

> Stores membership fee payment records and printable invoices.

| Field | Type | What It Stores |
|-------|------|---------------|
| `receiptNumber` | String | Auto-generated receipt ID (e.g., `REC-FIT-2026-0001`) |
| `member` | Reference | Which member paid |
| `gym` | Reference | Which gym |
| `memberName` | String | Member's name |
| `memberId` | String | Member's ID |
| `email`, `phone` | String | Contact info |
| `planName` | String | Name of the membership plan |
| `durationMonths` | Number | Plan duration |
| `amount` | Number | Fee amount paid (₹) |
| `paymentMethod` | String | `Cash`, `UPI`, `Card`, or `Bank Transfer` |
| `paymentDate` | Date | When the payment was made |
| `startDate` | Date | Plan start date |
| `expiryDate` | Date | Plan end date |
| `status` | String | `paid` or `refunded` |
| `notes` | String | Optional notes |
| `createdBy` | Reference | Admin who issued this receipt |

---

### 5.7 Announcement Model — `Announcement.js`

> Stores gym notice board posts and announcements.

| Field | Type | What It Stores |
|-------|------|---------------|
| `gym` | Reference | Which gym this notice is for |
| `title` | String | Notice title |
| `content` | String | Notice body text |
| `priority` | String | `normal`, `high`, or `urgent` |
| `isActive` | Boolean | Whether to show or hide this notice |
| `author` | Reference | Admin who posted it |
| `authorName` | String | Name of the posting admin |

---

## 6. 🔌 Backend API Endpoints (Server Features)

### 6.1 Authentication APIs — `/api/auth`

| Method | URL | Who Can Use | What It Does |
|--------|-----|-------------|--------------|
| `GET` | `/branding` | 🌐 Everyone | Gets the gym's name, logo, and tagline for the login screen |
| `POST` | `/login` | 🌐 Everyone | Signs in with Member ID/email + password; returns JWT token |
| `POST` | `/activate` | 🌐 Everyone | First-time password setup using activation code |
| `POST` | `/forgot-password` | 🌐 Everyone | Resets password using Member ID + activation code |
| `PUT` | `/change-password` | 🔒 Logged-in users | Changes password (needs current password) |
| `PUT` | `/profile` | 🔒 Logged-in users | Updates name and phone number |
| `GET` | `/me` | 🔒 Logged-in users | Gets current user's profile and gym info |

---

### 6.2 Admin APIs — `/api/admin`

> All these require **Admin or SuperAdmin** login.

| Method | URL | What It Does |
|--------|-----|--------------|
| `POST` | `/members` | Adds a new gym member (generates activation code) |
| `GET` | `/members` | Lists all members in the admin's gym |
| `PUT` | `/members/:id` | Edits a member's info or extends their plan |
| `DELETE` | `/members/:id` | Deletes a member and all their workout logs |
| `GET` | `/stats` | Gets dashboard stats (total members, check-ins, trends) |

---

### 6.3 SuperAdmin APIs — `/api/superadmin`

> All these require **SuperAdmin** login only.

| Method | URL | What It Does |
|--------|-----|--------------|
| `GET` | `/gyms` | Lists all gym tenants with their details |
| `POST` | `/gyms` | Creates a new gym + its admin account |
| `GET` | `/stats` | Gets platform-wide stats (total gyms, revenue, members) |
| `PUT` | `/gyms/:id` | Updates gym branding, contact info, and rent |
| `POST` | `/gyms/:id/renew` | Extends a gym's rental period |
| `PATCH` | `/gyms/:id/status` | Suspends or reactivates a gym |
| `DELETE` | `/gyms/:id` | Deletes a gym and all its associated data |

---

### 6.4 Workout & Check-In APIs — `/api/workout`

| Method | URL | Who Can Use | What It Does |
|--------|-----|-------------|--------------|
| `POST` | `/scan` | 🔒 Members | Processes QR scan (checks in or checks out) |
| `POST` | `/toggle` | ❌ Disabled | Manual check-in is intentionally blocked (anti-cheat) |
| `GET` | `/my-logs` | 🔒 Members | Gets all workout logs, stats, heatmap, muscle analysis, badges |
| `GET` | `/leaderboard` | 🔒 All logged-in | Gets gym competition rankings |
| `GET` | `/all-logs` | 🔒 Admin only | Gets all members' workout logs (admin audit view) |
| `GET` | `/prs` | 🔒 Members | Gets personal best records |
| `POST` | `/prs` | 🔒 Members | Saves or updates a personal best record |
| `DELETE` | `/prs/:id` | 🔒 Members | Deletes a personal best record |

---

### 6.5 QR Code APIs — `/api/qr`

| Method | URL | What It Does |
|--------|-----|--------------|
| `GET` | `/today` | Gets today's active QR code for the gym desk display |
| `POST` | `/regenerate` | Creates a brand new QR code (invalidates the old one) |

---

### 6.6 Receipt APIs — `/api/receipts`

| Method | URL | Who Can Use | What It Does |
|--------|-----|-------------|--------------|
| `GET` | `/` | 🔒 All logged-in | Members see their own receipts; Admins see all gym receipts |
| `POST` | `/` | 🔒 Admin only | Creates a new payment receipt |
| `GET` | `/:id` | 🔒 All logged-in | Views a specific receipt (with access control) |

---

### 6.7 Announcement APIs — `/api/announcements`

| Method | URL | Who Can Use | What It Does |
|--------|-----|-------------|--------------|
| `GET` | `/` | 🔒 All logged-in | Members see active notices only; Admins see all |
| `POST` | `/` | 🔒 Admin only | Creates a new gym announcement |
| `DELETE` | `/:id` | 🔒 Admin only | Deletes an announcement |
| `PATCH` | `/:id/toggle` | 🔒 Admin only | Shows/hides an announcement |

---

## 7. 🔐 Security Features

### 7.1 Password Security
- All passwords are **encrypted using bcrypt** (salted hash with 10 rounds) — even if the database is stolen, passwords cannot be read.
- Passwords are **never returned** in API responses (stripped using `select('-password')`).
- Minimum password length: **6 characters**.

### 7.2 Authentication (JWT Tokens)
- After login, the server issues a **JWT (JSON Web Token)** valid for **30 days**.
- Every API request includes this token in the `Authorization: Bearer <token>` header.
- The server verifies the token on every request — if it's expired or fake, access is denied.

### 7.3 Role-Based Access Control (RBAC)
- **Three middleware guards** protect different routes:
  - `protect` — Only logged-in users can access.
  - `adminOnly` — Only Admins and SuperAdmins can access.
  - `superAdminOnly` — Only the SuperAdmin can access.
- Frontend uses `<ProtectedRoute>` component to redirect unauthorized users.

### 7.4 Multi-Tenant Data Isolation
- Every query is **scoped to the user's gym** — an admin of Gym A cannot see or modify Gym B's members.
- Members can only scan QR codes belonging to **their own gym**.

### 7.5 Anti-Cheat QR System
- **Manual check-in is disabled** (returns 403 error) — members MUST physically scan the QR code.
- QR tokens **rotate daily** and contain the date — old screenshots/photos don't work.
- Tokens **auto-expire after 24 hours** (MongoDB TTL index).
- A **60-second minimum cooldown** prevents accidental instant check-out.
- The gym code inside the QR token is verified against the member's assigned gym.

### 7.6 Account Activation Security
- New members created by admin get a **6-character cryptographic activation code** (generated via `crypto.randomBytes`).
- Members must enter this code to set their password — no default passwords exist.
- The activation code also doubles as a password reset code.

### 7.7 Database Fallback
- If MongoDB is not installed or unreachable, the app automatically starts an **in-memory database** (`mongodb-memory-server`) so development works without any setup.

---

## 8. 🖥 Frontend Pages & Components (What Users See)

### 8.1 Public Pages (Anyone Can Access)

---

#### 🏠 Home Page (`Home.jsx`) — Route: `/`
**What it is:** A beautiful marketing landing page that showcases all the features of FitPlus Tracker.

**What it shows:**
- ✅ Hero section with gym branding, catchy headline, and call-to-action buttons
- ✅ An interactive mockup preview showing a live workout session card
- ✅ 6 feature cards (QR Check-In, Muscle Logging, Consistency Heatmap, Daily Streaks, Leaderboard, Routine Balance)
- ✅ "How It Works" — 3-step visual walkthrough (Scan → Train → Level Up)
- ✅ Consistency & Muscle Monitoring deep-dive section
- ✅ FAQ accordion answering 4 common member questions
- ✅ Bottom call-to-action to sign in or activate

**Smart behavior:**
- If the user is already logged in, the CTA buttons change to "Go to Your Dashboard" instead of "Sign In".

---

#### 🔑 Login Page (`Login.jsx`) — Route: `/login`
**What it is:** The sign-in page for all users (members, admins, superadmins).

**What it shows:**
- ✅ Gym logo and name (dynamically loaded)
- ✅ Login form with Member ID / Email + Password fields
- ✅ "Forgot Password?" link
- ✅ Link for new members to set their password
- ✅ Quick demo credentials box (for testing)
- ✅ Error messages and activation prompts

**Smart behavior:**
- If login returns "account not activated," it shows a special banner with a direct link to the activation page.
- After successful login, redirects to the correct dashboard based on role:
  - SuperAdmin → `/superadmin`
  - Admin → `/admin`
  - Member → `/dashboard`

---

#### 🔓 Activate Account Page (`ActivateAccount.jsx`) — Route: `/activate`
**What it is:** First-time password setup page for new gym members.

**What it shows:**
- ✅ Member ID / Email input (auto-filled if coming from login page)
- ✅ Activation Code input (auto-converts to uppercase)
- ✅ New Password + Confirm Password fields
- ✅ Success message with auto-redirect to login

**How it works:**
1. Admin creates a new member and gives them a Member ID + 6-character Activation Code.
2. The member visits this page, enters their ID and code, and creates their own password.
3. The account becomes active and the member can log in normally.

---

#### 🔄 Forgot Password Page (`ForgotPassword.jsx`) — Route: `/forgot-password`
**What it is:** Self-service password reset using the activation code.

**What it shows:**
- ✅ Member ID + Activation Code + New Password + Confirm Password form
- ✅ Success message with auto-redirect

**How it works:**
- The member uses the same activation code their admin gave them to reset their password — no email verification needed.

---

#### 📺 Gym Desk QR Display (`GymDeskQR.jsx`) — Route: `/gym-desk`
**What it is:** A full-screen kiosk page designed to be shown on a tablet or TV at the gym reception desk.

**What it shows:**
- ✅ Gym logo, name, and tagline
- ✅ **Live digital clock** (updates every second) showing day, date, and time
- ✅ **Large QR code** that members scan to check in and out
- ✅ "Regenerate QR Code" button for staff
- ✅ Anti-cheat notice ("Old photos won't work — code changes daily")
- ✅ 4-step instructions for members
- ✅ Website QR code for members to download the app

**Smart behavior:**
- The **Footer is automatically hidden** on this page for a clean kiosk experience.
- At **midnight**, the page **auto-refreshes** to load a new daily QR code.
- Staff can **regenerate** the QR code at any time if it gets leaked.

---

### 8.2 Member Pages (Logged-in Members)

---

#### 📊 User Dashboard (`UserDashboard.jsx`) — Route: `/dashboard`
**What it is:** The **main page** for gym members — their personal workout command center.

> [!NOTE]
> This is the **largest and most feature-rich page** in the entire application (80KB of code!).

**What it shows — Section by Section:**

1. **Welcome Banner**
   - Personalized greeting with member name
   - Current streak counter (🔥 days)
   - Total points earned (⚡ points)
   - Big "Scan QR to Check In" or "Scan QR to Check Out" button

2. **Membership Alerts**
   - 🔴 Red alert if membership has expired
   - 🟡 Amber warning if membership expires within 7 days

3. **Gym Announcements Banner**
   - Shows active notices from the admin (urgent ones highlighted)

4. **Active Workout Session Card** (when checked in)
   - Live stopwatch timer counting up (HH:MM:SS)
   - Check-in time display
   - "Checkout via QR" button

5. **Body Part & Muscle Training Monitor**
   - **Routine Balance Score** (percentage showing how balanced workouts are)
   - Top trained muscle group highlight
   - Neglected/skipped muscle group warnings
   - 11 muscle group cards: Chest, Back, Biceps, Triceps, Shoulders, Legs, Abs, Cardio, Glutes, Forearms, Full Body
   - Each card shows: volume progress bar, session count, "last trained X days ago"

6. **Workout Consistency Heatmap**
   - GitHub-style dot grid calendar (like GitHub's contribution graph)
   - Filter by: Current Month, 3 Months, 6 Months, 1 Year
   - Color-coded dots: rest day (dark), quick workout (teal), medium (emerald), intense (bright green with glow)
   - Click any dot to see that day's details (duration, body parts trained)

7. **Weekly Workout Duration Chart**
   - Beautiful gradient area chart (Recharts) showing workout hours over the past 7 days

8. **Winner of the Month Widget**
   - Shows the current #1 ranked member with podium standings

9. **Membership Plan Info**
   - Plan name, start date, expiry date, days remaining
   - "View Official Fee Receipts" button

10. **Milestone Badges & Achievements** (9 badges)
    - Progress bars and unlocked/locked status (see Section 10)

11. **Personal Records (PR) Tracker**
    - Cards showing exercise name, weight, reps, date
    - "Log New PR" button with a form modal

12. **My Workout Logs History**
    - Filterable by muscle group (All, Chest, Back, etc.)
    - Shows date, check-in/out times, duration, body parts trained

13. **QR Scanner Modal**
    - Opens phone camera to scan the gym desk QR code
    - On check-out: shows muscle selection screen with quick presets (Push Day, Pull Day, Legs & Abs, etc.)

---

#### 🏆 Leaderboard Page (`Leaderboard.jsx`) — Route: `/leaderboard`
**What it is:** Competition rankings page showing who's the "Winner of the Month."

**What it shows:**
- ✅ Current #1 Winner highlight card with crown badge
- ✅ **Top 3 Podium** with gold, silver, bronze styling
- ✅ Full member standings table with rank, name, score, streak, points, hours
- ✅ Competition score formula: `(Points × 10) + Total Workout Minutes`

---

#### 👤 Profile Page (`Profile.jsx`) — Route: `/profile`
**What it is:** Personal profile settings + **shareable 4:5 performance report card**.

**Two tabs:**

**Tab 1 — Performance Report:**
- A beautiful **4:5 ratio card** (designed for Instagram/WhatsApp stories) showing:
  - For Members: Gym logo, name, streak, rank, gym time, points, and current month attendance heatmap
  - For Admins: Active members, today's check-ins, expiring plans, weekly bar chart, top 2 podium
- **"Download 4:5 PNG"** button to save as image
- **"Share Picture"** button (uses Web Share API on mobile)

**Tab 2 — Account Settings:**
- Update name and phone number
- Change password (current + new + confirm)
- Read-only membership info summary

---

### 8.3 Admin Pages (Gym Owners)

---

#### 📈 Admin Dashboard (`AdminDashboard.jsx`) — Route: `/admin`
**What it is:** The gym owner's control panel with real-time analytics.

**What it shows:**
- ✅ **4 Stat Cards**: Total Members, Currently in Gym (live), Today's Check-Ins, Pending Activations
- ✅ **Weekly Attendance Bar Chart** (7-day trend with Recharts)
- ✅ **Recent Members** list with status badges
- ✅ **Winner of the Month** card
- ✅ **Notice Board** with create/delete announcement functionality
- ✅ **"How Password Setup Works"** 4-step instruction card
- ✅ **Website QR Code** for sharing with new members
- ✅ **"Launch Gym Desk QR"** button to open the kiosk display
- ✅ **"Add New Member"** button

---

#### 👥 Members List (`MembersList.jsx`) — Route: `/admin/members`
**What it is:** Full member directory with add/edit/delete/billing management.

**What it shows:**
- ✅ Search bar (by name, member ID, email)
- ✅ Status filter tabs: All, Active, Expiring Soon, Expired, Pending Setup (with counts)
- ✅ Member list (card view on mobile, table on desktop)
- ✅ Each member shows: ID, name, email, phone, plan dates, activation code (with copy button), status

**Actions available:**
- ➕ **Add New Member**: Form with name, email, phone, plan duration (1/3/6/12 months), fee amount, payment method. Auto-generates member ID and activation code. Can auto-generate fee receipt.
- ✏️ **Edit Member**: Update info, extend plan, optionally generate renewal receipt.
- 🗑 **Delete Member**: Removes member and all their workout data.
- 📋 **Copy Activation Code**: One-click copy to clipboard.
- 🧾 **View Receipts**: Opens receipt modal for printing.

---

#### 📝 Workout Logs (`WorkoutLogs.jsx`) — Route: `/admin/logs`
**What it is:** Audit view of all member workout sessions in the gym.

**What it shows:**
- ✅ Search by member name, ID, date, or muscle group
- ✅ Each log shows: date, member name/ID, check-in/out times, duration, muscles trained, status

---

### 8.4 SuperAdmin Pages (Platform Owner)

---

#### 🏢 SuperAdmin Dashboard (`SuperAdminDashboard.jsx`) — Route: `/superadmin`
**What it is:** The master SaaS control console for managing all gyms on the platform.

**What it shows:**
- ✅ **6 Global Stat Cards**: Total Gyms, Active Rentals, Expiring Soon, Expired, Total Members, Total Rent Value (₹)
- ✅ **Search bar** and **status filter** (all, active, expiring_soon, expired, suspended)
- ✅ **Gym tenant cards** — each showing:
  - Gym logo, name, code, status badge
  - Rental start/expiry dates, days remaining, monthly rent
  - Admin contact info and member count
  - Action buttons: Extend Rent, Launch Desk QR, Edit Branding, Suspend/Reactivate, Delete

**Actions available:**
- ➕ **Rent Out New Gym**: Create a new gym with name, logo (Google Drive link), tagline, rental duration/fee, and admin credentials (name + email + password).
- 🔄 **Extend Rental**: Quick buttons for +1, +2, +6, +12 months with payment amount.
- ✏️ **Edit Gym Branding**: Update name, logo, tagline, phone, email, rent amount.
- ⏸ **Suspend/Reactivate**: Toggle gym status.
- 🗑 **Delete Gym**: Permanently removes gym and ALL its members, logs, and data.

**Smart behavior:**
- Google Drive logo links show a **live preview** with error detection.
- Rental status is **auto-calculated** (active, expiring soon, expired) based on dates.

---

### 8.5 Shared Components (Used Across Pages)

| Component | What It Does |
|-----------|-------------|
| **Navbar** | Sticky top bar with gym logo, role-specific navigation links, streak badge, profile link, and mobile hamburger menu |
| **Footer** | 4-column footer with feature badges, quick links, contact info, website QR code. Auto-hides on gym desk page |
| **ProtectedRoute** | Guards pages — redirects to login if not authenticated, or to the correct dashboard if wrong role |
| **QRScanner** | Opens phone camera, scans QR codes, passes the decoded token to the parent component |
| **StatCard** | Reusable metric card with icon, title, value, and color variants (indigo, green, amber, rose, cyan) |
| **WorkoutHeatmap** | GitHub-style dot grid showing workout consistency. Filterable by 1/3/6/12 months. Click dots for day details |
| **ReceiptModal** | Professional printable fee receipt with gym branding, member info, payment details, terms & conditions, and signature line |

---

## 9. ⭐ Feature-by-Feature Breakdown

### 9.1 QR Code Check-In / Check-Out

```
┌──────────────────────────────────────────────────────────────┐
│                    HOW QR CHECK-IN WORKS                      │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  1. Admin opens Gym Desk Display (/gym-desk) on tablet/TV   │
│     → Shows a LARGE QR code at the reception desk           │
│                                                              │
│  2. Member opens their Dashboard on their phone              │
│     → Taps "Scan QR to Check In"                            │
│     → Phone camera opens and scans the QR code              │
│                                                              │
│  3. Server receives the scan:                                │
│     → Validates the QR token exists & matches today's date  │
│     → Checks the member belongs to this gym                 │
│     → Creates a new WorkoutLog with status "in_progress"    │
│     → Updates member's streak & awards +10 points           │
│     → Live stopwatch timer starts on the dashboard          │
│                                                              │
│  4. When leaving, member scans QR again:                     │
│     → Must wait at least 60 seconds (anti-cheat cooldown)   │
│     → A muscle selection popup appears                      │
│     → Member tags body parts trained (or skips)             │
│     → WorkoutLog is marked "completed" with duration        │
│     → Stopwatch stops                                       │
│                                                              │
│  ⚠️ ANTI-CHEAT PROTECTIONS:                                 │
│     • Manual check-in is DISABLED (code returns 403)        │
│     • QR codes change DAILY (old photos don't work)         │
│     • 60-second cooldown prevents instant check-out         │
│     • Gym ID verification prevents cross-gym scanning       │
│     • QR tokens auto-delete from database after 24 hours    │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

### 9.2 Workout & Muscle Tracking

After checking out, members select which muscle groups they trained. The app tracks **11 muscle groups**:

| # | Muscle Group | Tracking |
|---|-------------|----------|
| 1 | Chest | Sessions, volume, last trained |
| 2 | Back | Sessions, volume, last trained |
| 3 | Biceps | Sessions, volume, last trained |
| 4 | Triceps | Sessions, volume, last trained |
| 5 | Shoulders | Sessions, volume, last trained |
| 6 | Legs | Sessions, volume, last trained |
| 7 | Abs | Sessions, volume, last trained |
| 8 | Cardio | Sessions, volume, last trained |
| 9 | Glutes | Sessions, volume, last trained |
| 10 | Forearms | Sessions, volume, last trained |
| 11 | Full Body | Sessions, volume, last trained |

**Quick workout presets** for easy tagging:
- 🔵 **Push Day** → Chest, Shoulders, Triceps
- 🟢 **Pull Day** → Back, Biceps, Forearms
- 🟠 **Legs & Abs** → Legs, Glutes, Abs
- 🟣 **Upper Body** → Chest, Back, Shoulders, Biceps, Triceps
- ⚫ **Full Body** → All muscle groups

Members can also type **custom muscle group names**.

**Routine Balance Analysis:**
- The app calculates a **Routine Balance Score** (0-100%) based on how evenly muscles are trained.
- Highlights the **most trained** muscle group.
- Warns about **neglected/skipped** muscle groups.
- Each muscle is rated: `trained_well`, `moderate`, or `needs_attention`.

---

### 9.3 Streak System

| Rule | Description |
|------|-------------|
| **Streak Increment** | If the member checked in yesterday AND checks in today → streak increases by 1 |
| **Streak Reset** | If the member missed yesterday → streak resets to 1 |
| **Midnight Boundary** | Streaks are based on calendar dates, not hours. Missing a full day breaks the streak |
| **Points** | Each daily check-in earns **+10 points** |
| **Display** | Streak shown as 🔥 fire icon in navbar, dashboard, and profile |

---

### 9.4 Attendance Heatmap

A visual dot grid calendar (inspired by GitHub's contribution graph) that shows:

| Dot Color | Meaning |
|-----------|---------|
| ⚫ Dark slate | Rest day (no workout) |
| 🟢 Teal | Quick workout (< 30 minutes) |
| 🟢 Emerald | Medium workout (30-59 minutes) |
| 🟢 Bright Green (glowing) | Intense workout (≥ 60 minutes) |

**Filters:** View current month, last 3 months, last 6 months, or full year.

**Day Inspector:** Click any dot to see:
- Date
- "Completed Workout" or "Rest Day"
- Duration in minutes/hours
- Body parts trained that day

---

## 10. 🎮 Gamification System (Points, Streaks, Badges)

### 10.1 Points System
- **+10 points** per daily check-in
- Points accumulate over time and never decrease
- Used in leaderboard ranking formula

### 10.2 Streak System
- Consecutive days of gym attendance
- Resets to 1 if a day is missed
- Displayed with 🔥 fire icon throughout the app

### 10.3 Milestone Badges (9 Total)

| Badge Name | Requirement | Description |
|-----------|-------------|-------------|
| 🏋️ **First Workout** | 1 workout completed | "Congratulations on your first gym session!" |
| 🔥 **3-Day Streak** | 3 consecutive days | "Three days strong! Building the habit." |
| ⚡ **7-Day Streak** | 7 consecutive days | "A full week of consistency!" |
| 💪 **14-Day Streak** | 14 consecutive days | "Two weeks of dedication!" |
| 👑 **30-Day Streak** | 30 consecutive days | "A full month! Iron discipline." |
| ⏱ **10 Hours Club** | 10 total workout hours | "10 hours of total gym time!" |
| 🏆 **50 Hours Club** | 50 total workout hours | "50 hours of grinding!" |
| 💯 **Century Club** | 100 total workout hours | "100 hours! Legendary commitment." |
| 🎯 **Full Body Master** | Trained all muscle groups | "Trained every major muscle group." |

Each badge shows:
- ✅ Unlocked status (green glow) or 🔒 Locked status (grayed out)
- Progress bar showing percentage toward unlocking
- Current progress vs. required target

### 10.4 Leaderboard Scoring
```
Competition Score = (Points × 10) + Total Workout Minutes
```

Members are ranked within their gym. The top member is crowned **"Winner of the Month"** and appears on:
- The Leaderboard page with a gold crown
- The Admin Dashboard
- The User Dashboard widget
- The Profile performance card

---

## 11. 🏢 Multi-Gym / Multi-Tenant System

This is one of the most powerful features of FitPlus Tracker. The platform supports **multiple gyms**, each completely isolated from the others.

```
┌─────────────────────────────────────────────────┐
│              SUPERADMIN (Platform Owner)         │
│                                                  │
│  Can create, manage, suspend, delete gyms        │
│  Tracks rental payments and expiry dates         │
│  Views global stats across all gyms              │
├─────────────────────────────────────────────────┤
│                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌────────┐ │
│  │   GYM A      │  │   GYM B      │  │ GYM C  │ │
│  │              │  │              │  │        │ │
│  │ Admin: John  │  │ Admin: Sara  │  │Admin:  │ │
│  │ Members: 50  │  │ Members: 30  │  │Raj     │ │
│  │ Logo: ⭐     │  │ Logo: 💪     │  │Mem: 80 │ │
│  │              │  │              │  │        │ │
│  │ ↕ Isolated   │  │ ↕ Isolated   │  │↕ Isol. │ │
│  │ data & users │  │ data & users │  │data    │ │
│  └──────────────┘  └──────────────┘  └────────┘ │
│                                                  │
└─────────────────────────────────────────────────┘
```

### How Gym Rental Works:
1. SuperAdmin creates a new gym → enters gym name, logo, rental duration (1/2/6/12 months), monthly rent (₹)
2. The system auto-generates a unique gym code (e.g., `FIT-001`)
3. An admin account is created for the gym owner
4. The gym status auto-updates based on rental expiry:
   - **Active** → More than 14 days remaining
   - **Expiring Soon** → 14 days or less remaining
   - **Expired** → Past the expiry date
   - **Suspended** → Manually suspended by SuperAdmin

---

## 12. 📱 QR Code System (How Check-In Works)

### Daily Token Generation
- Each day, a **unique token** is generated for each gym in the format:
  ```
  GYM_ENTRY_FIT_2026-09-23_A1B2C3D4
  ```
- This token is encoded into a QR code image (PNG, 300px, high error correction)
- The QR image is displayed on the gym's kiosk screen (`/gym-desk` page)

### Token Security
- Tokens are stored in the database with a **24-hour TTL** (Time To Live) — MongoDB automatically deletes them after 24 hours
- Each token is linked to a **specific gym** and **specific date**
- The token contains a **random hex component** (6 bytes) that changes with each regeneration
- Staff can **regenerate** the QR code at any time to invalidate leaked codes

### Scan Validation Flow
1. Member scans QR code → sends token to `/api/workout/scan`
2. Server checks: Does this token exist? ✅
3. Server checks: Is the token's date today? ✅
4. Server checks: Is the token active (not revoked)? ✅
5. Server checks: Does the member belong to this gym? ✅
6. Server checks: Is the membership expired? ❌ (blocks if expired)
7. If no active session exists → **Check In** (creates WorkoutLog, updates streak)
8. If active session exists → **Check Out** (but only if 60+ seconds have passed)

---

## 13. 💳 Billing & Receipt System

### Receipt Generation
- When an admin adds a new member or renews a plan, they can **auto-generate a payment receipt**.
- Receipt numbers follow the format: `REC-FIT-2026-0001` (auto-incrementing).

### Receipt Contents
The printable receipt includes:
- Gym header (logo, name, tagline, address, phone)
- "✓ Payment Received" status badge
- Receipt number and issue date
- Member details (name, ID, phone, email)
- Payment details (method, duration, valid date range)
- Fee breakdown table with amount in ₹
- Total settlement amount with green checkmark
- Terms & Conditions section
- Authorized Signatory line

### Receipt Access
- **Members** can only view their own receipts
- **Admins** can view all receipts for their gym
- **SuperAdmins** can filter receipts by gym

### Print Feature
- The receipt modal has a **"Print / Save PDF"** button that triggers the browser's native print dialog
- The receipt is styled with special **print CSS** that converts to a clean white paper format

---

## 14. 📢 Announcement / Notice Board System

### How It Works
- **Admins** can create gym-wide announcements with:
  - Title
  - Content
  - Priority level: `normal`, `high`, or `urgent`
- Announcements appear on the **Admin Dashboard** notice board and on the **Member Dashboard** as banners.
- Admins can **toggle** announcements between visible and hidden.
- Admins can **delete** announcements.
- Each announcement is **scoped to its gym** — members of other gyms don't see it.

### Priority Levels
| Priority | Meaning | Visual Style |
|----------|---------|-------------|
| Normal | Regular update | Standard styling |
| High | Important notice | Highlighted |
| Urgent | Critical alert | Red/urgent banner |

---

## 15. 🏆 Leaderboard & Competition System

### Ranking Formula
```
Competition Score = (Streak Points × 10) + Total Workout Minutes
```

### Podium Display
| Position | Style | Features |
|----------|-------|----------|
| 🥇 1st Place | Gold gradient, crown badge, bouncing animation | Largest card, centered on desktop |
| 🥈 2nd Place | Silver/slate theme | Standard size |
| 🥉 3rd Place | Bronze/amber theme | Standard size |

### Full Standings
Below the podium, a complete table shows ALL members ranked by score with:
- Rank number
- Member name
- Competition score
- Daily streak (🔥)
- Streak points (⚡)
- Total workout hours

---

## 16. 👤 Profile & Shareable Performance Card

### Performance Report Card (4:5 Ratio)
A visually stunning card designed for **social media sharing** (Instagram stories, WhatsApp status):

**For Members:**
- Gym logo and brand header with date
- Member name and role badge
- 4 highlight metrics: Total Streak, Gym Rank, Gym Time, Total Points
- Current month attendance heatmap with day numbers

**For Admins (Gym Owners):**
- Gym logo and brand header with date
- 4 admin metrics: Active Members, Today's Check-Ins, Expiring Soon, Ranked Athletes
- 7-Day Weekly Attendance Bar Chart
- Top 2 Podium Rankings

### Export Options
- 📥 **"Download 4:5 PNG"** — Saves a high-resolution image to the device
- 📤 **"Share Picture"** — Uses the Web Share API to share directly to WhatsApp, Instagram, etc. (on mobile)

### Account Settings
- Update full name and phone number
- Change password (requires current password)
- View membership info summary

---

## 17. 🚀 Deployment & Hosting

### Current Deployment
| Component | Hosted On | URL |
|-----------|-----------|-----|
| Frontend | Vercel | `www.fitplus-tracker.in` |
| Backend API | Render | `fitplus-tracker.onrender.com` |
| Database | MongoDB Atlas | Cloud-hosted MongoDB |

### How the Frontend Connects to Backend
- The Axios client (`api.js`) sends all requests to:
  ```
  VITE_API_URL (from .env) OR https://fitplus-tracker.onrender.com/api
  ```
- In development, Vite proxies `/api` requests to the backend.

### Vercel Configuration
- `vercel.json` rewrites all routes to `/index.html` (required for single-page app routing).

### Auto-Seeding on First Run
When the backend starts for the first time:
1. Creates a default gym: **"FitPulse Elite Gym"** (code: `FIT-001`)
2. Creates a **SuperAdmin** account: `superadmin@fitpulse.com` / `superadmin123`
3. Creates an **Admin** account: `admin@gym.com` / `admin123`
4. Creates sample members: `Rahul Sharma` (active) and `Alex Johnson` (pending activation)

---

## 18. 📂 Complete File-by-File Summary

### Backend Files (27 files)

| # | File | Purpose |
|---|------|---------|
| 1 | `server.js` | Main entry point — starts Express, mounts routes, auto-seeds database |
| 2 | `seed.js` | Standalone script to create demo data (`npm run seed`) |
| 3 | `package.json` | Lists all backend dependencies and npm scripts |
| 4 | `config/db.js` | Connects to MongoDB with auto-fallback to in-memory database |
| 5 | `middleware/authMiddleware.js` | JWT token verification + role-based access guards |
| 6 | `utils/imageUrl.js` | Converts Google Drive URLs to direct image CDN links |
| 7 | `models/User.js` | User schema — members, admins, superadmins with encrypted passwords |
| 8 | `models/Gym.js` | Gym schema — name, logo, rental info, branding |
| 9 | `models/GymQRToken.js` | Daily rotating QR tokens with 24-hour auto-expiry |
| 10 | `models/WorkoutLog.js` | Individual workout sessions with timing and muscle groups |
| 11 | `models/PersonalRecord.js` | Member personal best lifting records |
| 12 | `models/PaymentReceipt.js` | Membership fee payment receipts |
| 13 | `models/Announcement.js` | Gym notice board posts |
| 14 | `controllers/authController.js` | Login, activation, password reset, profile management |
| 15 | `controllers/adminController.js` | Member CRUD, dashboard stats, analytics |
| 16 | `controllers/superAdminController.js` | Multi-gym management, rental tracking |
| 17 | `controllers/workoutController.js` | QR scan, streaks, points, leaderboard, badges, muscle analysis |
| 18 | `controllers/qrController.js` | Generate and regenerate daily QR codes |
| 19 | `controllers/receiptController.js` | Create and view payment receipts |
| 20 | `controllers/announcementController.js` | Create, delete, toggle gym announcements |
| 21 | `routes/authRoutes.js` | Maps auth URLs to controller functions |
| 22 | `routes/adminRoutes.js` | Maps admin URLs (protected by adminOnly middleware) |
| 23 | `routes/superAdminRoutes.js` | Maps superadmin URLs (protected by superAdminOnly) |
| 24 | `routes/workoutRoutes.js` | Maps workout/check-in URLs |
| 25 | `routes/qrRoutes.js` | Maps QR code generation URLs |
| 26 | `routes/receiptRoutes.js` | Maps receipt URLs |
| 27 | `routes/announcementRoutes.js` | Maps announcement URLs |

### Frontend Files (29 files)

| # | File | Purpose |
|---|------|---------|
| 1 | `App.jsx` | Root component — defines all 13 routes and page layout |
| 2 | `main.jsx` | Entry point — mounts React app into HTML |
| 3 | `index.css` | Global styles — Tailwind setup, glow effects |
| 4 | `context/AuthContext.jsx` | Global auth state — login, logout, branding, session validation |
| 5 | `services/api.js` | Axios HTTP client with JWT auto-injection |
| 6 | `utils/imageUrl.js` | Google Drive URL converter (frontend version) |
| 7 | `components/Navbar.jsx` | Top navigation bar — role-aware links, streak badge, mobile menu |
| 8 | `components/Footer.jsx` | Bottom footer — feature badges, links, contact, website QR |
| 9 | `components/ProtectedRoute.jsx` | Route guard — blocks unauthorized access |
| 10 | `components/QRScanner.jsx` | Camera-based QR code reader using html5-qrcode |
| 11 | `components/StatCard.jsx` | Reusable metric card with icon and color variants |
| 12 | `components/WorkoutHeatmap.jsx` | GitHub-style attendance dot grid with day inspector |
| 13 | `components/ReceiptModal.jsx` | Printable fee receipt modal with terms & signature |
| 14 | `pages/Home.jsx` | Public landing page — hero, features, FAQ, CTAs |
| 15 | `pages/Login.jsx` | Sign-in page with role-based redirect |
| 16 | `pages/ActivateAccount.jsx` | First-time password setup with activation code |
| 17 | `pages/ForgotPassword.jsx` | Password reset using activation code |
| 18 | `pages/UserDashboard.jsx` | Member portal — QR check-in, stopwatch, muscles, heatmap, badges, PRs |
| 19 | `pages/AdminDashboard.jsx` | Gym owner dashboard — stats, charts, notices, winner |
| 20 | `pages/SuperAdminDashboard.jsx` | Platform console — multi-gym management, rentals |
| 21 | `pages/MembersList.jsx` | Member directory — add/edit/delete/billing |
| 22 | `pages/GymDeskQR.jsx` | Full-screen kiosk QR display for reception desk |
| 23 | `pages/Leaderboard.jsx` | Competition rankings with gold/silver/bronze podium |
| 24 | `pages/WorkoutLogs.jsx` | Admin audit view of all workout sessions |
| 25 | `pages/Profile.jsx` | Profile settings + shareable 4:5 performance card |
| 26 | `package.json` | Frontend dependencies and build scripts |
| 27 | `vite.config.js` | Vite bundler config — port 3000, API proxy |
| 28 | `tailwind.config.js` | Custom color palette (gym.dark, gym.accent, gym.primary, etc.) |
| 29 | `index.html` | HTML shell — Plus Jakarta Sans font, dark theme |

---

## 🎨 Visual Design System

| Element | Value | Usage |
|---------|-------|-------|
| **Background** | `slate-950` (#020617) | App background |
| **Card Background** | `slate-900` (#0f172a) | Cards, modals |
| **Primary Color** | `indigo-500` (#6366f1) | Buttons, links, active states |
| **Accent Color** | `cyan-500` (#06b6d4) | Highlights, badges |
| **Success Color** | `emerald-500` (#10b981) | Active status, heatmap, check marks |
| **Warning Color** | `amber-500` (#f59e0b) | Expiring plans, streaks, gold podium |
| **Danger Color** | `rose-500` (#ef4444) | Expired status, errors, delete |
| **Font** | Plus Jakarta Sans | 400, 500, 600, 700, 800 weights |
| **Glow Effect** | `box-shadow: 0 0 25px -5px rgba(99, 102, 241, 0.4)` | Active cards |
| **Text Selection** | `selection:bg-indigo-500 selection:text-white` | Selected text |
| **Glass Effect** | `backdrop-blur-md bg-slate-900/90` | Navbar, modals |

---

> [!TIP]
> **Quick Summary for Non-Technical Readers:**  
> FitPlus Tracker is a gym website where members scan a QR code to check in, track their workouts and streaks, compete on a leaderboard, and earn badges. Gym owners can manage members, handle billing, and post announcements. The platform owner can rent out the system to multiple gyms, each with its own separate data and branding. Everything is secured with encrypted passwords, login tokens, and anti-cheat measures.

---

<p align="center">
  📄 Documentation generated for <strong>FitPlus Tracker</strong> by Rahul Kushwaha<br>
  🕐 Last updated: September 2026
</p>
