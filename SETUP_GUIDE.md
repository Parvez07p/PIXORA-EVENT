# ANIMATIXXXX - Quick Setup Guide

## Project Summary

**Animation Files Discovered:** 46 MP4 videos (83 MB total)
**Challenges to Import:** 46 challenges
**Files Location:** `backend/animations/`

## Quick Start (5 Minutes)

### 1. Database Setup

Ensure MongoDB is running:
- **Local MongoDB:** Start MongoDB service or run `mongod`
- **MongoDB Atlas:** Get connection string from MongoDB Atlas dashboard

### 2. Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your MongoDB connection string

# Seed database (creates admin + imports animations)
npm run db:seed
```

**Default Admin Credentials:**
- Email: `admin@animatixxx.com`
- Password: `Admin@123`

### 3. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

### 4. Start Backend (New Terminal)

```bash
cd backend
npm run dev
```

### 5. Access the Application

- **Frontend:** http://localhost:5174 (or 5173 if available)
- **Backend API:** http://localhost:5000
- **Admin Login:** http://localhost:5174/admin/login

## First-Time Configuration

1. Login as administrator at `/admin/login`
2. Navigate to `/admin/settings`
3. Configure event parameters:
   - Event start/end times
   - Registration window
   - Observation duration (default: 25 seconds)
   - Maximum team size (default: 4)
   - Submission limits (default: 3)
4. Click "Start Event" to activate the competition

## Importing Animations

Animations are already extracted to `backend/animations/`. They can be imported via:

**Option 1: Admin Portal**
1. Go to `/admin/import`
2. The system will detect files in the animations directory
3. Click "Import" to create challenges

**Option 2: Command Line**
```bash
cd backend
node scripts/importAnimations.js
```

## Common Issues

### Database Connection Error
```
Error: MongoServerError: Connection failed
```
**Solution:** Ensure MongoDB is running and MONGODB_URI is correct in .env

### Port Already in Use
```
Error: Port 5000 is already in use
```
**Solution:** Change PORT in backend/.env or kill the process using the port

### Admin Creation Fails
```
Error: Admin user already exists
```
**Solution:** Delete existing admin from MongoDB or set new ADMIN_EMAIL/ADMIN_PASSWORD

## Production Deployment

### Backend

```bash
cd backend
npm install --production
npm run db:push
npm start
```

### Frontend

```bash
cd frontend
npm run build
# Deploy the 'dist' folder to your web server
```

### Environment Variables

**Production .env:**
```env
MONGODB_URI="mongodb+srv://user:pass@cluster.mongodb.net/animatixxx"
JWT_SECRET="your-secure-production-secret"
NODE_ENV="production"
PORT=5000
FRONTEND_URL="https://your-domain.com"
```

**Frontend .env:**
```env
VITE_API_URL=https://your-api-domain.com/api
```

Note: For MongoDB Atlas, use a connection string like:
```
mongodb+srv://username:password@cluster.mongodb.net/animatixxx?retryWrites=true&w=majority
```

## Support

For detailed documentation, see README.md
