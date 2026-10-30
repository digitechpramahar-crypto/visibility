# 🌍 TourBhook Admin Portal & Ecosystem

A fully integrated, production-ready, full-stack travel booking admin portal and mobile client ecosystem for **TourBhook**. The system bridges a responsive React web admin portal and a React Native mobile application with a centralized **Spring Boot REST API** backed by **Google Cloud SQL (MySQL)**. It delivers secure authentication, role-based access control (RBAC), live user activity tracking, content moderation, partner KYC verification queues, and system configuration management.

---

## ✨ Highlights

- **🔒 Secure Full-Stack Authentication:** Token-based JWT authentication featuring automatic 10-minute session inactivity tracking and secure password hashing using Spring Security and BCrypt.
- **👑 Isolated Super Admin Controls:** Dedicated `admins` table separating the super admin (`digitech.pramahar@gmail.com`) from regular application users, with dedicated account control panels for updating mobile numbers and secure password hashes.
- **🛡️ Role-Based Access Control (RBAC):** Live management of regular application user accounts with assignable roles (`SUPER_ADMIN`, `OPERATIONS`, `FINANCE_OPS`, `SUPPORT_TEAM`, `CONTENT_MODERATION`, `USER`).
- **📊 Live User Activity Dashboard:** Real-time metrics, dynamic reporting periods, and SVG activity charts driven by actual database account registrations.
- **🛡️ Content Moderation Workspace:** Live parameterized search, status/severity filters, report review drawers, persistent moderator notes, content removal flags, and user blocking linked directly to Cloud SQL tables.
- **📑 KYC Approvals Workspace:** Partner document verification queue, multi-status workflows, and real-time review states fully synchronized with the database.
- **📱 Mobile App Synchronization:** Cross-platform React Native mobile client completely wired to register and authenticate users directly through the Spring Boot backend.

---

## 🛠️ Tech Stack

### 🚀 Backend
- **Java 17 / Spring Boot 3.2**
- **Spring Security & BCrypt** Password Hashing
- **Spring Data JPA & JDBC Template**
- **Google Cloud SQL (MySQL)** Connector
- **JJWT** (JSON Web Token) Security

### 💻 Frontend (Admin Portal)
- **React 19 / Vite 8**
- **React Day Picker 10**
- Plain CSS & Responsive Flex/Grid Layouts

### 📱 Mobile App
- **React Native / TypeScript**
- Axios API Client with Token Interceptors

---

## 🚀 Getting Started

### Prerequisites

- Java Development Kit (JDK) 17 or later
- Node.js 18 or later
- Maven
- Configured Google Cloud SQL instance

---

### 1️⃣ Backend Setup & Run

1. Configure your database and environment properties in `src/main/resources/application.properties` and your root `.env` file:
   ```env
   DB_USER=
   DB_PASSWORD=YourPassword
   INSTANCE_CONNECTION_NAME=
   SUPER_ADMIN_EMAIL=
   SUPER_ADMIN_PASSWORD=
   ```

Build and run the Spring Boot application using Maven or directly from IntelliJ IDEA (runs on port 8080):
```bash
cd backend
bash ./mvnw spring-boot:run
```

### 2️⃣ Frontend Admin Portal Setup

Navigate to the frontend directory, install dependencies, and start the development server:
```bash
cd frontend
npm install
npm run dev
```
Open the local URL printed by Vite (http://localhost:5173) to access the secure login portal.

### 🌐 Deploying the Admin Portal on Hostinger

The frontend is a static Vite site. The Spring Boot backend is a separate Java service and must run on a Hostinger VPS or another Java-capable server; shared web hosting alone cannot run this backend.

1. Point `admin-tourbhook.com` and `www.admin-tourbhook.com` to the Hostinger website and enable its SSL certificate.
2. Run the frontend build:
   ```bash
   cd frontend
   npm ci
   npm run build
   ```
3. Upload the **contents** of `frontend/dist/` to the domain's `public_html` directory. The included `.htaccess` provides client-side route fallback.
4. Deploy the backend JAR on a Java 17 server with the required database, mail, Gemini, and administrator environment variables set. Never put backend secrets in the frontend or commit them.
5. Route `/api` on the website's HTTPS origin to the backend, or build the frontend with a separate HTTPS API origin:
   ```bash
   VITE_API_BASE_URL=https://api.admin-tourbhook.com npm run build
   ```
   For a separate API origin, set `APP_CORS_ALLOWED_ORIGINS` on the backend to `https://www.admin-tourbhook.com,https://admin-tourbhook.com`.

The default frontend API URL is same-origin (`/api`); Vite proxies it to `localhost:8080` during local development. `SUPER_ADMIN_PASSWORD` must be set in the backend environment before starting it.

### 3️⃣ Mobile App Setup
Navigate to your mobile project directory, install dependencies, and launch:

```bash
cd 
npm install
npx react-native run-android
```

🌟 Portal Areas & Live Architecture

📈 User Activity & RBAC
Live Metrics & Charts: Real-time database counts for active users, total users, and dynamic registration curves over custom date filters.

User Accounts Modal: Fetches all registered application users (excluding the super admin) live from the backend and allows dynamic role assignment.

Most Preferred Cities: Automatically aggregates live user account distributions.

🔍 Content Moderation
Live Database Search: Real-time parameterized queries filtering by report ID, content snippet, or user email.

Interactive Review Drawer: Persistent storage for moderator notes, content approval, content removal flags, and permanent user blocks stored in Cloud SQL tables (content_reports and blocked_users).

✅ KYC Approvals
Partner Verification Queue: Fetches partner document submissions directly from kyc_submissions.

Review Workflows: Approving, rejecting, or marking applications pending updates both the queue and the user's overall verification status instantly.

⚙️ Super Admin Security Settings
Accessible via the bottom-left profile menu. Allows the administrator to update their contact number and securely update their password hash in the admins table.

📁 Project Structure
Plaintext

TourBhook/
├── backend/                  # Spring Boot REST API & Cloud SQL Integration
│   └── src/main/java/com/psc/adminbackend/
│       ├── controller/       # AuthController, AdminController, DashboardController,
│       │                     # UserController, KycController, ModerationController
│       ├── service/          # AuditService, LocalizationService
│       └── security/         # SecurityConfig
├── frontend/                 # React Web Admin Portal
│   ├── src/
│   │   ├── Pages/            # Login, Dashboard, ContentModeration, KycApprovals, SystemConfiguration
│   │   ├── hooks/            # useModeration, useSystemConfiguration
│   │   ├── App.jsx           # Protected route routing & 10-minute session timeout
│   │   └── main.jsx
│   └── public/               # Brand assets & mock document previews
└── tourbhook-mobile/         # React Native Mobile Client Application
└── src/
└── api/              # Axios API client configured for backend synchronization


🛡️ Security & Session Management

Session Timeout: The admin portal enforces a strict 10-minute session inactivity timer. After 10 minutes of inactivity, the session is automatically cleared, returning the user safely to the login screen.

Credential Hashing: Super admin passwords are secured using enterprise-grade BCrypt hashing and managed via isolated database tables separate from regular application end-users.