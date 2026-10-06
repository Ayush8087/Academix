# Academix Frontend

## Overview

The frontend is a React application built with Create React App and Tailwind CSS. It provides the user-facing interface for the Academix platform, including public pages, student and instructor dashboards, admin management, and AI-powered study tools.

## Key Features

- Public landing pages, catalog browsing, and course details
- Login/signup flows with Auth0 and custom auth endpoints
- Student dashboard for enrolled courses, cart, profile, settings, and AI study assistant
- Instructor dashboard for course creation, management, and content updates
- Admin dashboard for user, instructor, course, and refund management
- AI features such as summary generation, document chat, doubt answering, and text-to-video
- Socket.IO integration for real-time course interactions and updates

## Project Structure

- `src/App.js` — application routing and Auth0 provider configuration
- `src/services/apiconnector.js` — axios instance and request interceptor with auth management
- `src/services/apis.js` — centralized API endpoint declarations through the API Gateway
- `src/components/` — reusable UI components and feature-specific screens
- `src/pages/` — top-level route pages for the app
- `src/contexts/SocketContext.jsx` — socket connection provider
- `src/utils/` — constants and helper utilities
- `public/` — static HTML and metadata

## Environment Variables

Create a `.env` file in `frontend/` with the values required for your environment. The key variables are:

```env
REACT_APP_API_GATEWAY_URL=http://localhost:4000/api/v1
REACT_APP_AUTH0_DOMAIN=your-auth0-domain
REACT_APP_AUTH0_CLIENT_ID=your-auth0-client-id
REACT_APP_SOCKET_URL=http://localhost:4000
```

### Notes

- `REACT_APP_API_GATEWAY_URL` is the base URL used for all backend API calls
- `REACT_APP_AUTH0_DOMAIN` and `REACT_APP_AUTH0_CLIENT_ID` are required for Auth0 authentication
- `REACT_APP_SOCKET_URL` should point to the API Gateway socket proxy or directly to the Course Service if configured

## Available Scripts

In the `frontend/` directory, run:

- `npm install` — install frontend dependencies
- `npm start` — start the development server
- `npm run build` — build the app for production
- `npm test` — run the test suite
- `npm run eject` — eject Create React App configuration

## Running Locally

Install dependencies and start the frontend:

```bash
cd frontend
npm install
npm start
```

The app runs by default at `http://localhost:3000`.

## Backend Integration

The frontend communicates with the backend through the API Gateway at `http://localhost:4000/api/v1` by default. All service calls are routed through the gateway.

### Main API Areas

- `/auth` — authentication and user signup/login
- `/profile` — profile management and enrolled courses
- `/course` — course browsing, creation, editing, and progress
- `/payment` — payments, verification, and refunds
- `/smart-study` — AI study companion endpoints
- `/contact` — contact form submissions
- `/admin` — admin routes for management dashboards

## Important Frontend Behavior

- Auth tokens are stored in `localStorage` and sent automatically in request headers
- File upload requests use `FormData` and do not set `Content-Type` manually
- 401 responses clear auth state and redirect users to `/login`
- Protected routes use React Router and custom role-based access checks

## Additional Notes

- The project uses React Router for routing and nested dashboard/admin routes
- The frontend uses `@auth0/auth0-react` for Auth0 flows
- AI features and smart study helpers are integrated into the dashboard experience
- Socket.IO is enabled by default for real-time updates and interactive course states
