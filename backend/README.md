# Academix Backend

## Overview

This repository contains the backend microservices for the Academix platform. It is built as an npm workspace with multiple services managed together in `backend/`.

## Architecture

The backend is composed of the following services:

- **api-gateway** (`backend/api-gateway`)
  - Main gateway for frontend requests
  - Proxies requests to downstream services
  - Provides service-level routing for auth, profile, course, payment, AI, contact, and admin endpoints
  - Default port: `4000`

- **user-service** (`backend/user-service`)
  - Handles authentication, user profiles, password reset, contact messages, and admin user management
  - Default port: `4001`

- **course-service** (`backend/course-service`)
  - Handles course creation, sections, subsections, reviews, discussions, and real-time socket events
  - Default port: `4002`

- **payment-service** (`backend/payment-service`)
  - Handles payment capture, verification, refund requests, and payment-related admin flows
  - Default port: `4003`

- **ai-service** (`backend/ai-service`)
  - Handles AI-powered study features such as summary generation, document chat, and doubt answering
  - Default port: `4004`

- **shared-utils** (`backend/shared-utils`)
  - Shared utilities used by all services
  - Includes environment validation, mailing, queue workers, file upload helpers, and shared middleware

## Service Ports and URLs

The default service ports are:

- API Gateway: `http://localhost:4000`
- User Service: `http://localhost:4001`
- Course Service: `http://localhost:4002`
- Payment Service: `http://localhost:4003`
- AI Service: `http://localhost:4004`

The gateway forwards frontend traffic to service endpoints using the `/api/v1` namespace.

## Getting Started

### Install dependencies

From `backend/`:

```bash
npm install
```

This installs the workspace dependencies and links local packages such as `shared-utils`.

### Configure environment variables

Each service has its own `.env.example` file. Copy those examples into a `.env` file inside each service folder:

```bash
cp api-gateway/.env.example api-gateway/.env
cp user-service/.env.example user-service/.env
cp course-service/.env.example course-service/.env
cp payment-service/.env.example payment-service/.env
cp ai-service/.env.example ai-service/.env
```

Update the variables with your MongoDB connection, JWT secret, mail credentials, Razorpay keys, Gemini API key, and CORS origins.

### Run all services locally

From `backend/`:

```bash
npm run dev
```

This command uses `concurrently` to start all services in development mode.

### Run production-style mode

From `backend/`:

```bash
npm start
```

### Clean dependencies

```bash
npm run clean
```

## Individual Service Commands

Inside a service folder, you can also run:

```bash
npm install
npm run dev
npm start
```

## Important Environment Variables

### `backend/api-gateway/.env.example`

- `PORT` — gateway listen port
- `USER_SERVICE_URL` — URL for user service
- `COURSE_SERVICE_URL` — URL for course service
- `PAYMENT_SERVICE_URL` — URL for payment service
- `AI_SERVICE_URL` — URL for AI service
- `CORS_ORIGINS` — allowed origins for CORS
- `NODE_ENV`

### `backend/user-service/.env.example`

- `MONGODB_URL`
- `JWT_SECRET`
- `MAIL_HOST`, `MAIL_USER`, `MAIL_PASS`
- `ADMIN_MAIL`
- `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET` (optional)
- `CORS_ORIGINS`
- `NODE_ENV`

### `backend/course-service/.env.example`

- `MONGODB_URL`
- `JWT_SECRET`
- `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_API_KEY`, `CLOUDINARY_API_SECRET`
- `CORS_ORIGINS`
- `NODE_ENV`

### `backend/payment-service/.env.example`

- `MONGODB_URL`
- `JWT_SECRET`
- `RAZORPAY_KEY`, `RAZORPAY_SECRET`
- `COURSE_SERVICE_URL`, `USER_SERVICE_URL`
- `CORS_ORIGINS`
- `NODE_ENV`

### `backend/ai-service/.env.example`

- `MONGODB_URL`
- `JWT_SECRET`
- `GEMINI_API_KEY`
- `CORS_ORIGINS`
- `NODE_ENV`

## API Gateway Routes

The gateway exposes the following main route groups:

- `/api/v1/auth` → User Service auth routes
- `/api/v1/profile` → User Service profile routes
- `/api/v1/course` → Course Service routes
- `/api/v1/payment` → Payment Service routes
- `/api/v1/smart-study` → AI Service routes
- `/api/v1/contact` → User Service contact route
- `/api/v1/admin/payment` → Payment Service admin routes
- `/api/v1/admin/user` → User Service admin routes
- `/api/v1/admin/course` → Course Service admin routes
- `/api/v1/admin/ai` → AI Service admin routes

The gateway also proxies Socket.IO traffic to the Course Service at `/socket.io`.

## Health Checks

Each service provides a root health check endpoint, for example:

- `GET http://localhost:4000/`
- `GET http://localhost:4001/`
- `GET http://localhost:4002/`
- `GET http://localhost:4003/`
- `GET http://localhost:4004/`

## Notes

- The backend uses ES modules (`type: module`).
- Shared utilities are stored in `backend/shared-utils` and linked into each service as a local package.
- The API Gateway is the single entry point for frontend traffic.
- CORS is configured on each service plus the gateway, allowing `http://localhost:3000` by default.
- `npm run kill-ports` in `backend/` will terminate processes on ports `4000` through `4004`.
