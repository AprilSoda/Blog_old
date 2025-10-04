# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a full-stack blog application with a React frontend and Express/MongoDB backend. The app uses JWT authentication, AWS S3 for file uploads, CKEditor for rich text editing, and Greenlock Express for HTTPS/SSL.

## Development Commands

### Server Development
```bash
npm run dev               # Start server with nodemon (auto-reload) on port 8080
```

### Client Development
```bash
cd client
npm start                # Start React dev server with proxy to backend
```

### Building for Production
```bash
npm run prebuild         # Builds client first
npm run build            # Transpiles server code with Babel to /build
npm run build:server     # Transpiles only server code
```

### Testing
```bash
cd client
npm test                # Run Jest tests
```

## Architecture

### Backend (Express + MongoDB)
- **Entry Point**: `server/server.js` - Uses Greenlock Express for HTTPS
- **App Configuration**: `server/app.js` - Express setup with security middleware (hpp, helmet), MongoDB connection, HTTPS redirect middleware
- **API Routes**: Located in `server/routes/api/`
  - `/api/post` - Post CRUD operations
  - `/api/user` - User management
  - `/api/auth` - Authentication (JWT)
  - `/api/search` - Search functionality
- **Models**: `server/models/` - Mongoose schemas (post, user, comment, category)
- **Middleware**: `server/middleware/auth.js` - JWT authentication
- **Config**: `server/config/index.js` - Loads environment variables (MONGO_URI, JWT_SECRET, PORT)

### Frontend (React + Redux)
- **Entry Point**: `client/src/index.js` -> `App.js`
- **State Management**: Redux with Redux Saga for side effects
  - Store: `client/src/store.js`
  - Reducers: `client/src/redux/reducers/`
  - Sagas: `client/src/redux/sagas/`
  - Types: `client/src/redux/types.js`
- **Routing**: React Router with Connected React Router (history integration)
  - Router: `client/src/routes/Router.js`
  - Normal Routes: `client/src/routes/normalRoute/` (PostCardList, PostDetail, PostWrite, PostEdit, Profile, Search, CategoryResult)
  - Protected Routes: `client/src/routes/protectedRoute/ProtectedRoute.js`
- **Components**: `client/src/components/`
  - auth - Authentication forms
  - post - Post display components
  - comments - Comment functionality
  - editor - CKEditor integration
  - search - Search UI
  - AppNavbar.js, Header.js, Footer.js - Layout components

### Key Technologies
- **Rich Text Editor**: CKEditor 5 (Classic build with custom configuration)
- **File Upload**: AWS S3 via multer-s3
- **HTTPS/SSL**: Greenlock Express for automatic SSL certificate management
- **Security**: helmet (security headers), hpp (parameter pollution), cors
- **Styling**: Bootstrap 4 + Reactstrap + SASS

## Important Notes

### Environment Variables
Create a `.env` file in the root with:
- `MONGO_URI` - MongoDB connection string
- `JWT_SECRET` - Secret for JWT token signing
- `PORT` - Server port (default 8080)

### HTTPS Redirect
All HTTP requests are automatically redirected to HTTPS via middleware in `server/app.js:41-49`

### Production Build
In production, the Express server serves the static React build from `client/build/` and handles all routes with `index.html` for client-side routing

### Proxy Configuration
Client development server proxies API requests to `http://localhost:8080` (configured in `client/package.json`)
