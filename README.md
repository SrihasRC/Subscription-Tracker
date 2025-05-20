# Subscription Tracker

A Node.js/Express application to help users manage and track their subscriptions, receive reminders, and keep their spending in check.

## Features
- User authentication (sign up, sign in, sign out)
- Full CRUD for users and subscriptions
- Track upcoming renewals
- Email reminders for renewals
- Categorize subscriptions
- Secure password storage

## Tech Stack
- **Backend:** Node.js, Express.js
- **Database:** MongoDB, Mongoose
- **Authentication:** JWT
- **Email:** Nodemailer
- **Other:** dotenv, bcrypt, Arcjet, Upstash Workflow

## Folder Structure
```
├── config/         # Configuration files (env, nodemailer, arcjet, upstash)
├── controllers/    # Route controllers for business logic
├── database/       # Database connection logic
├── middlewares/    # Express middlewares (auth, error, arcjet)
├── models/         # Mongoose models (User, Subscription)
├── routes/         # Express route definitions
├── utils/          # Utility functions (email templates, send email)
├── index.js        # Main entry point
├── package.json    # Project metadata and dependencies
```

## Environment Variables
Create a `.env.development.local` (or `.env.<NODE_ENV>.local`) file in the root directory with the following variables:

```
PORT=3000
SERVER_URL=http://localhost:3000
NODE_ENV=development
DB_URI=mongodb://localhost:27017/subscription-tracker
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=1d
ARCJET_KEY=your_arcjet_key
ARCJET_ENV=your_arcjet_env
QSTASH_URL=your_qstash_url
QSTASH_TOKEN=your_qstash_token
QSTASH_CURRENT_SIGNING_KEY=your_qstash_current_signing_key
QSTASH_NEXT_SIGNING_KEY=your_qstash_next_signing_key
EMAIL_PASSWORD=your_email_password
```

## Installation & Setup
1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd subscription-tracker
   ```
2. **Install dependencies:**
   ```bash
   npm install
   ```
3. **Set up environment variables:**
   - Copy `.env.example` to `.env.development.local` and fill in your values (create the file if it doesn't exist).
4. **Start the server:**
   ```bash
   npm run dev
   # or
   npm start
   ```

## API Endpoints

> **Note:** Endpoints marked with 🔒 require authentication (JWT in Authorization header).

### Auth
- `POST /api/v1/auth/sign-up` — Register a new user
- `POST /api/v1/auth/sign-in` — Login
- `POST /api/v1/auth/sign-out` — Logout

### Users
- `GET /api/v1/users/` 🔒 — List all users
- `GET /api/v1/users/:id` 🔒 — Get user by ID
- `POST /api/v1/users/` 🔒 — Create user
- `PUT /api/v1/users/:id` 🔒 — Update user
- `DELETE /api/v1/users/:id` 🔒 — Delete user

### Subscriptions
- `GET /api/v1/subscriptions/` — List all subscriptions
- `GET /api/v1/subscriptions/:id` — Get subscription details
- `POST /api/v1/subscriptions/` 🔒 — Create subscription
- `PUT /api/v1/subscriptions/:id` 🔒 — Update subscription
- `DELETE /api/v1/subscriptions/:id` 🔒 — Delete subscription
- `GET /api/v1/subscriptions/user/:id` 🔒 — Get subscriptions for a user
- `PUT /api/v1/subscriptions/:id/cancel` 🔒 — Cancel subscription
- `GET /api/v1/subscriptions/upcoming-renewals` — Get upcoming renewals

### Workflows
- `POST /api/v1/workflows/subscription/remainder` — Send renewal reminders

## Data Models

### User
- `name`: String (2-50 chars, required)
- `email`: String (unique, required, valid email)
- `password`: String (min 6 chars, required)

### Subscription
- `name`: String (2-100 chars, required)
- `price`: Number (min 0, required)
- `currency`: String (INR, USD, EUR; default: INR)
- `frequency`: String (daily, weekly, monthly, yearly)
- `category`: String (technology, sports, news, entertainment, other; required)
- `paymentMethod`: String (required)
- `status`: String (active, cancelled, expired; default: active)
- `startDate`: Date (required, before now)
- `renewalDate`: Date (auto-calculated if missing)
- `user`: ObjectId (User reference, required)

## Scripts
- `npm start` — Start the server
- `npm run dev` — Start with nodemon (auto-reload)

