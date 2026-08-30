<div align="center">

# CampusLink

**Anonymous campus matching for students on the same college network.**

CampusLink verifies students with college email OTP, then gives them a lightweight way to meet people on campus through text, audio, and video chat flows.

<br />

[![Frontend](https://img.shields.io/badge/Frontend-Vercel-171717?style=for-the-badge&logo=vercel&logoColor=white)](https://campuslink-theta.vercel.app)
[![Backend](https://img.shields.io/badge/API-Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white)](https://campus-link-den.netlify.app)
![Stack](https://img.shields.io/badge/Stack-React%20%2B%20Express%20%2B%20PostgreSQL-0070f3?style=for-the-badge)

</div>

<br />

## Overview

CampusLink is a full-stack student social platform designed around one simple campus habit: sometimes it is easier to start a conversation when the first move is anonymous.

The app uses email OTP verification, JWT sessions, campus access checks, profile metadata, and a PostgreSQL-backed chat data model. The frontend is deployed on Vercel, while the backend HTTP API runs through Netlify Functions.

| Surface | Link |
|---|---|
| Frontend | https://campuslink-theta.vercel.app |
| Backend API | https://campus-link-den.netlify.app |

## Product Flow

```text
Student email
    -> domain validation
    -> OTP delivery
    -> JWT session
    -> profile setup
    -> anonymous match
    -> text, audio, or video chat
```

## Features

| Area | What CampusLink supports |
|---|---|
| Authentication | College email OTP login with hashed verification codes |
| Sessions | JWT-based login sessions and refresh-token structure |
| Matching | Anonymous student matching for text, audio, and video modes |
| Access | Campus-only access checks through configured IP ranges |
| Profiles | Course, year, gender, and basic student account metadata |
| Moderation | Reports, user blocks, audit logs, and admin-ready tables |
| Monetization | Subscription plan and subscription table structure |
| Deployment | Split frontend/backend production deployment |

## Tech Stack

| Layer | Tools |
|---|---|
| Frontend | React, Vite, Tailwind CSS, Framer Motion, Lucide React |
| Backend | Node.js, Express, Socket.IO, JWT, Nodemailer |
| Database | PostgreSQL, Neon, SQL migrations |
| Payments | Razorpay integration structure |
| Hosting | Vercel frontend, Netlify Functions backend |
| Source Control | GitHub |

## Architecture

The project is structured as a monorepo with separate frontend and backend applications.

```text
campuslink/
  backend/
    migrations/
    netlify/functions/
    public/
    src/
      config/
      controllers/
      middleware/
      routes/
      services/
      socket/
      utils/
  frontend/
    src/
      components/
      pages/
      styles/
```

The frontend talks to the backend through REST routes under `/api`. The backend keeps the Express app separate from the local server entrypoint, which allows the same HTTP routes to run as a Netlify Function while still leaving a local Socket.IO server path available for development.

## Authentication

CampusLink avoids password storage and keeps login short for students.

1. The student enters a college email address.
2. The backend checks whether the email domain is allowed.
3. A six-digit OTP is generated and stored as a hash in PostgreSQL.
4. Nodemailer sends the OTP through SMTP.
5. The student submits the OTP.
6. The backend verifies the hash and creates a JWT session.
7. The frontend stores the session and opens the app.

## Database Model

The first migration creates the core platform tables:

| Domain | Tables |
|---|---|
| Identity | `users`, `otp_verifications`, `sessions` |
| Chat | `chat_sessions`, `messages` |
| Safety | `reports`, `user_blocks`, `audit_logs` |
| Access | `campus_ip_ranges`, `rate_limit_log` |
| Plans | `subscription_plans`, `subscriptions` |

Indexes are included for common lookup paths such as email, sessions, reports, chat sessions, and user roles.

## Deployment

| Service | Responsibility |
|---|---|
| Vercel | Hosts the React frontend |
| Netlify Functions | Hosts the Express HTTP API |
| Neon | Hosts the production PostgreSQL database |

Production secrets are configured in the hosting dashboards, not committed to the repository.

```env
DATABASE_URL=
JWT_SECRET=
REFRESH_TOKEN_SECRET=
SMTP_HOST=
SMTP_PORT=
SMTP_USER=
SMTP_PASS=
EMAIL_FROM=
FRONTEND_URL=
COLLEGE_EMAIL_DOMAIN=
```

## Running Locally

Backend:

```bash
cd backend
npm install
npm run migrate
npm run dev
```

Frontend:

```bash
cd frontend
npm install
npm run dev
```

Create local `.env` files for the frontend and backend before running the app.

## Build Notes

- Netlify Functions are suitable for the HTTP API, but Socket.IO realtime chat needs a long-running Node server or a dedicated realtime service for production.
- Campus network access depends on correctly configured IP ranges.
- Production email delivery depends on valid SMTP credentials.
- The public showcase repository excludes secrets, local environment files, dependencies, and build outputs.

## What This Project Demonstrates

- Full-stack planning from product flow to deployment
- Secure OTP authentication with hashed codes
- Express route organization with controllers, middleware, services, and utilities
- PostgreSQL schema design and migration workflow
- Production CORS and environment-variable handling
- Separate frontend and backend deployment pipelines
- Responsive UI work with light and dark mode support

## Repository Status

This repository is public for portfolio and CV review. No license is included by default, so the code is visible for review but is not explicitly open-sourced for reuse under a permissive license.
