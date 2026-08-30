# CampusLink

CampusLink is an anonymous campus chat platform built for students on the same college network. It lets students verify with an email OTP, create an account, and meet other students through text, audio, and video matching.

The goal of the project is simple: make it easier for students to talk to people on campus without the awkward first move.

## Live Project

- Frontend: https://campuslink-theta.vercel.app
- Backend API: https://campus-link-den.netlify.app

## What I Built

CampusLink is a full-stack web application with separate frontend and backend deployments.

- Anonymous student matching for text, audio, and video chats
- College email OTP authentication
- JWT-based login sessions
- Campus-only access checks through IP ranges
- User profiles with course, year, gender, and basic account data
- Subscription plan structure for premium features
- Admin/reporting tables for moderation workflows
- PostgreSQL schema and migrations
- Responsive frontend UI with light and dark mode support
- Backend deployment through Netlify Functions
- Frontend deployment through Vercel

## Tech Stack

Frontend:

- React
- Vite
- Tailwind CSS
- Framer Motion
- Lucide React icons

Backend:

- Node.js
- Express
- Socket.IO
- PostgreSQL
- JWT
- Nodemailer
- Razorpay integration structure
- Netlify Functions for HTTP API deployment

Database:

- PostgreSQL
- Neon for hosted production database

Deployment:

- Vercel for frontend
- Netlify for backend HTTP API
- GitHub for source control and continuous deployment

## Architecture

The project is organized as a monorepo with separate `frontend` and `backend` folders.

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

The frontend talks to the backend through REST API routes under `/api`. The backend uses Express for HTTP routes and Socket.IO for realtime chat matching when running as a normal Node server.

For deployment, the Express app is separated from the local server startup code. This allows the same backend routes to run inside Netlify Functions while keeping the local Socket.IO server entrypoint available for development and server-based hosting.

## Authentication Flow

1. The user enters their email address.
2. The backend validates whether the email domain is allowed.
3. A six-digit OTP is generated and stored as a hash in PostgreSQL.
4. The OTP is sent through SMTP using Nodemailer.
5. The user submits the OTP.
6. The backend verifies the OTP hash and creates a JWT session.
7. The frontend stores the session and allows the user into the app.

This flow avoids password storage and keeps login lightweight for students.

## Database Design

The first migration creates the main application tables:

- `users`
- `otp_verifications`
- `sessions`
- `subscription_plans`
- `subscriptions`
- `chat_sessions`
- `messages`
- `reports`
- `user_blocks`
- `audit_logs`
- `campus_ip_ranges`
- `rate_limit_log`

It also creates indexes for common lookup paths such as email, sessions, reports, chat sessions, and user roles.

## Development Process

I built CampusLink in stages:

1. Planned the main student flow: verify, enter, match, chat.
2. Built the backend schema and authentication routes.
3. Added OTP email delivery and login/register flows.
4. Created the frontend login, register, and main app screens.
5. Added responsive UI styling and a dark mode toggle.
6. Split the Express app from the local server entrypoint so the backend could run on Netlify Functions.
7. Connected GitHub to Netlify and Vercel for continuous deployment.
8. Migrated the production database to Neon.

## Deployment Notes

The frontend and backend are deployed separately:

- Vercel serves the React frontend.
- Netlify serves the Express HTTP API through a serverless function.
- Neon hosts the production PostgreSQL database.

Important environment variables are not committed to the repository. They must be configured in the deployment dashboards:

- `DATABASE_URL`
- `JWT_SECRET`
- `REFRESH_TOKEN_SECRET`
- `SMTP_HOST`
- `SMTP_PORT`
- `SMTP_USER`
- `SMTP_PASS`
- `EMAIL_FROM`
- `FRONTEND_URL`
- `COLLEGE_EMAIL_DOMAIN`

## What I Learned

This project helped me practice:

- Designing a full-stack app from idea to deployment
- Structuring an Express backend with controllers, middleware, services, and routes
- Building OTP authentication securely with hashed codes
- Working with PostgreSQL migrations and hosted databases
- Handling production CORS and environment variables
- Deploying a monorepo with separate frontend and backend platforms
- Designing responsive UI states for authentication and app flows
- Understanding the difference between serverless HTTP APIs and realtime WebSocket servers

## Current Limitations

- Netlify Functions are suitable for the HTTP API, but Socket.IO realtime chat needs a long-running Node server or another realtime service for production.
- Campus network access depends on correctly configured IP ranges.
- Production email delivery depends on valid SMTP credentials.

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

## Repository Status

This repository is public so the project can be showcased on a CV and portfolio. Secret values, local environment files, build outputs, and dependencies are excluded through `.gitignore`.

No license is included by default. That means the code is visible for review, but it is not explicitly open-sourced for reuse under a permissive license.
