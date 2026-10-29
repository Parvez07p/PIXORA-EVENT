# ANIMATIXXXX - AI-Powered CSS Animation Recreation Challenge

Department of IT & AIDS

## Overview

ANIMATIXXXX is a full-stack web application for a college technical event where participants observe animations for a limited time and recreate them using HTML, CSS, JavaScript, and Generative AI tools.

## Technology Stack

### Frontend
- React 19
- Vite
- React Router
- Framer Motion
- Axios

### Backend
- Node.js
- Express.js
- MongoDB
- Mongoose ODM
- JWT Authentication
- bcrypt for password hashing

## Features

### Participant Portal
- Team registration (2-4 members)
- Secure login with JWT
- Personalized dashboard
- Challenge cards with status tracking
- 25-second observation system (configurable)
- HTML/CSS/JS code editors
- Live preview in sandboxed iframe
- ZIP upload for submissions
- Submission history
- Activity tracking

### Administrator Portal
- Separate admin authentication
- Dashboard with real-time statistics
- Team management with detailed activity logs
- Animation import from ZIP files
- Challenge management
- Event settings configuration
- Event status control (start/pause/resume)
- Judging and evaluation system
- Leaderboard management

### Security Features
- Secure password hashing (bcrypt)
- HTTP-only cookies
- Role-based access control
- Rate limiting
- SQL injection protection
- XSS protection
- CSRF protection
- Secure file uploads
- Authenticated video streaming

## Project Structure

```
css/
├── backend/
│   ├── animations/          # Extracted animation files
│   ├── controllers/         # Business logic
│   ├── middleware/          # Express middleware
│   ├── prisma/              # Database schema
│   ├── routes/              # API routes
│   ├── scripts/             # Utility scripts
│   ├── uploads/             # File uploads
│   ├── utils/               # Helper utilities
│   ├── .env                 # Environment variables
│   ├── .env.example         # Environment template
│   ├── package.json
│   ├── prisma.schema        # Prisma schema
│   └── server.js            # Express server
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── api/             # API clients
│   │   ├── context/         # React contexts
│   │   ├── pages/           # Page components
│   │   ├── App.jsx
│   │   ├── index.css
│   │   └── main.jsx
│   ├── .env
│   ├── package.json
│   └── vite.config.js
└── README.md
```

## Installation

### Prerequisites
- Node.js (v18 or higher)
- MongoDB (v6 or higher) - Can be installed locally or use MongoDB Atlas
- npm or yarn

### Database Setup

1. Ensure MongoDB is running locally or use MongoDB Atlas connection string

2. Configure environment variables in `backend/.env`:
```env
MONGODB_URI="mongodb://localhost:27017/animatixxx"
JWT_SECRET="your-super-secret-jwt-key-change-this-in-production"
JWT_EXPIRES_IN="7d"
PORT=5000
NODE_ENV="development"
UPLOAD_MAX_SIZE=10485760
FRONTEND_URL="http://localhost:5174"
```

3. Seed the database (create admin and import animations):
```bash
cd backend
npm run db:seed
```

### Backend Setup

```bash
cd backend
npm install
npm run dev
```

The backend will run on `http://localhost:5000`

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend will run on `http://localhost:5174` (or 5173 if available)

## Creating Administrator Account

The admin account is created automatically when running the seed script. Default credentials:

- Email: `admin@animatixxx.com`
- Password: `Admin@123`

**IMPORTANT:** Change the password after first login!

To create a custom admin account, set environment variables before seeding:

```bash
ADMIN_EMAIL="your-email@example.com"
ADMIN_PASSWORD="YourSecurePassword123"
npm run db:seed
```

## Importing Animations

### Using the Admin Portal

1. Login as administrator
2. Navigate to `/admin/import`
3. Upload a ZIP file containing animation videos
4. Review the inspection results
5. Confirm import to create challenges

### Using Command Line

Animations have already been extracted to `backend/animations/`. To import them:

```bash
cd backend
node scripts/importAnimations.js
```

Supported formats: MP4, WebM, GIF

## Configuring Event Settings

1. Login as administrator
2. Navigate to `/admin/settings`
3. Configure:
   - Event title and department
   - Event start/end times
   - Observation duration (default: 25 seconds)
   - Registration window
   - Submission deadline
   - Maximum team size
   - Submission limits
   - Challenge access order (any/sequential)
   - Revisit policy
   - Leaderboard visibility

## Starting the Event

1. Configure all event settings
2. Click "Start Event" in the admin settings page
3. The event status will change to "ACTIVE"
4. Participants can now access challenges

## Testing

### Backend Tests
```bash
cd backend
npm test
```

### Frontend Tests
```bash
cd frontend
npm test
```

## Deployment

### Environment Variables

For production, set the following environment variables:

**Backend (.env):**
```env
DATABASE_URL="postgresql://user:password@host:5432/database?schema=public"
JWT_SECRET="your-production-secret-key"
JWT_EXPIRES_IN="7d"
PORT=5000
NODE_ENV="production"
UPLOAD_MAX_SIZE=10485760
FRONTEND_URL="https://your-frontend-domain.com"
```

**Frontend (.env):**
```env
VITE_API_URL=https://your-backend-domain.com/api
```

### Deployment Steps

1. **Build the frontend:**
```bash
cd frontend
npm run build
```

2. **Deploy backend:**
   - Upload backend files to server
   - Install dependencies: `npm install --production`
   - Run migrations: `npm run db:push`
   - Start server: `npm start` (or use PM2)

3. **Deploy frontend:**
   - Upload the `dist/` folder to your web server
   - Configure nginx or Apache to serve the static files
   - Set up reverse proxy for API requests

### Using PM2 for Process Management

```bash
# Install PM2
npm install -g pm2

# Start backend
cd backend
pm2 start server.js --name animatixxx-backend

# Start frontend (if using node server)
cd frontend
pm2 start preview.js --name animatixxx-frontend

# Save PM2 configuration
pm2 save
pm2 startup
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Team registration
- `POST /api/auth/login` - Participant login
- `POST /api/auth/admin/login` - Admin login
- `POST /api/auth/logout` - Logout
- `GET /api/auth/me` - Get current user

### Participants
- `GET /api/participants/dashboard` - Get dashboard data
- `GET /api/participants/challenges` - Get available challenges

### Challenges
- `POST /api/challenges/:id/observe` - Start observation session
- `GET /api/challenges/:id/observe` - Get observation status
- `GET /api/challenges/:id/video` - Get challenge video (authenticated)
- `POST /api/challenges/:id/complete` - Mark observation complete

### Submissions
- `POST /api/submissions/:id` - Submit solution via code
- `POST /api/submissions/:id/upload` - Submit solution via ZIP
- `GET /api/submissions/:id/history` - Get submission history
- `GET /api/submissions/submission/:id` - Get specific submission

### Admin
- `GET /api/admin/dashboard` - Admin dashboard stats
- `GET /api/admin/teams` - Get all teams
- `GET /api/admin/teams/:id` - Get team details
- `PATCH /api/admin/teams/:id/status` - Update team status
- `GET /api/admin/challenges/analytics` - Challenge analytics
- `GET /api/admin/settings` - Get event settings
- `PATCH /api/admin/settings` - Update event settings
- `GET /api/admin/announcements` - Get announcements
- `POST /api/admin/announcements` - Create announcement
- `DELETE /api/admin/announcements/:id` - Delete announcement

### Animation Import
- `POST /api/admin/animation/upload` - Upload and inspect ZIP
- `POST /api/admin/animation/confirm` - Confirm import
- `GET /api/admin/animation/challenges` - Get all challenges
- `DELETE /api/admin/animation/challenges/:id` - Delete challenge

### Judging
- `GET /api/judging/pending` - Get pending evaluations
- `GET /api/judging/submission/:id` - Get submission for evaluation
- `POST /api/judging/submission/:id/evaluate` - Submit evaluation
- `GET /api/judging/leaderboard` - Get leaderboard

## Animation Files

The original animation ZIP file contained **46 MP4 video files** (83 MB total).

These have been extracted to `backend/animations/` and can be imported as challenges using the admin portal or the import script.

Each video will create a challenge with:
- Unique challenge number
- Original filename preserved
- File metadata (format, size)
- Sequential ordering

## Scoring System

Evaluation criteria (configurable):
- Visual Similarity: 40 points
- Originality & Creativity: 25 points
- Animation Smoothness: 15 points
- Code Quality: 10 points
- Responsive Design: 10 points
- **Total: 100 points**

## Troubleshooting

### Database Connection Issues
- Verify PostgreSQL is running
- Check DATABASE_URL in .env
- Ensure database exists

### Port Already in Use
- Change PORT in backend/.env
- Kill process using the port

### CORS Errors
- Verify FRONTEND_URL in backend/.env
- Check API_URL in frontend/.env

### Animation Import Issues
- Verify ZIP file format
- Check file permissions
- Ensure animations directory exists

## Support

For issues or questions, contact the development team.

## License

Department of IT & AIDS - ANIMATIXXXX Event
