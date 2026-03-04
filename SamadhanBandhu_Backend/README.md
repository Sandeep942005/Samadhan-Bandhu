# Samadhan Bandhu Backend

A comprehensive government project management portal backend built with Node.js, Express, and SQLite.

## Features

- **Authentication**: JWT-based authentication with role-based access control
- **Project Management**: Create, track, and manage government projects
- **Tender System**: Publish tenders and manage applications
- **Fund Management**: Track fund allocation and release at different administrative levels
- **Payment Processing**: Manage and approve payments with transaction tracking
- **Inspections**: Schedule and track field inspections with photo/GPS support
- **Reporting**: Submit and approve various types of reports
- **Notifications**: Real-time notifications via Socket.io
- **User Management**: Multi-role user system (Central, State, Block, Agency, Field Officer)

## Tech Stack

- **Node.js** - JavaScript runtime
- **Express.js** - Web framework
- **SQLite3** - Database
- **JWT** - Authentication
- **bcryptjs** - Password hashing
- **Socket.io** - Real-time notifications
- **multer** - File uploads

## Installation

### Prerequisites
- Node.js 16+ installed
- npm or yarn package manager

### Setup Steps

1. **Clone and navigate to backend directory**
   ```bash
   cd SamadhanBandhu_Backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Create environment file**
   ```bash
   cp .env.example .env
   ```

4. **Configure environment variables** (edit `.env`)
   ```
   PORT=5000
   NODE_ENV=development
   FRONTEND_URL=http://localhost:5173
   JWT_SECRET=your_jwt_secret_key_change_this_in_production
   JWT_EXPIRY=7d
   DB_PATH=./data/samadhan.db
   ```

5. **Initialize database and seed test data**
   ```bash
   node scripts/seed.js
   ```

6. **Start the server**
   ```bash
   # Development with auto-reload
   npm run dev

   # Production
   npm start
   ```

The server will start on `http://localhost:5000`

## API Endpoints

### Authentication (`/api/auth`)
- `POST /register` - Register new user
- `POST /login` - User login
- `GET /me` - Get current user profile
- `POST /logout` - Logout
- `PUT /profile` - Update profile
- `POST /change-password` - Change password

### Projects (`/api/projects`)
- `POST /` - Create project
- `GET /` - Get all projects
- `GET /:id` - Get single project
- `PUT /:id` - Update project
- `GET /:id/stats` - Get project statistics

### Tenders (`/api/tenders`)
- `POST /` - Create tender
- `GET /` - Get all tenders
- `GET /:id` - Get tender details
- `POST /:id/apply` - Apply for tender
- `PUT /:id/status` - Update tender status
- `PUT /application/:id/evaluate` - Evaluate application

### Funds (`/api/funds`)
- `POST /release` - Release funds
- `GET /` - Get all funds
- `GET /summary/level` - Fund summary by level
- `GET /project/:projectId` - Get project allocations
- `PUT /:fundId/approve` - Approve fund release

### Payments (`/api/payments`)
- `POST /` - Create payment record
- `GET /` - Get all payments
- `GET /:id` - Get payment details
- `PUT /:id/approve` - Approve payment
- `PUT /:id/reject` - Reject payment
- `GET /summary/stats` - Payment statistics

### Inspections (`/api/inspections`)
- `POST /` - Schedule inspection
- `GET /` - Get all inspections
- `GET /:id` - Get inspection details
- `PUT /:id/submit` - Submit inspection report
- `GET /stats/summary` - Inspection statistics

### Users (`/api/users`)
- `GET /` - Get all users
- `GET /:id` - Get user details
- `GET /role/:role` - Get users by role
- `PUT /:id/deactivate` - Deactivate user
- `PUT /:id/activate` - Activate user
- `GET /stats/summary` - User statistics

### Notifications (`/api/notifications`)
- `GET /` - Get user notifications
- `GET /unread/count` - Get unread count
- `PUT /:id/read` - Mark as read
- `PUT /read/all` - Mark all as read
- `DELETE /:id` - Delete notification

### Reports (`/api/reports`)
- `POST /` - Submit report
- `GET /` - Get all reports
- `GET /:id` - Get report details
- `PUT /:id/approve` - Approve report
- `PUT /:id/reject` - Reject report
- `GET /stats/summary` - Report statistics

## User Roles

1. **Central** - National-level administrator
2. **State** - State-level coordinator
3. **Block** - Block-level manager
4. **Agency** - Partner agency/contractor
5. **Field Officer** - On-ground inspector/officer

## Test Credentials

Use the following credentials to test the application (after running seed script):

| Role | Email | Password |
|------|-------|----------|
| Central Admin | central@samadhan.gov.in | password123 |
| State Officer | state@samadhan.gov.in | password123 |
| Block Manager | block@samadhan.gov.in | password123 |
| Agency | agency@samadhan.gov.in | password123 |
| Field Officer | fieldofficer@samadhan.gov.in | password123 |

## Database Schema

The application uses SQLite with the following main tables:
- `users` - User accounts and profiles
- `projects` - Government projects
- `tenders` - Tender announcements
- `tender_applications` - Agency applications for tenders
- `funds` - Fund allocations and releases
- `payments` - Payment records
- `inspections` - Field inspections
- `notifications` - User notifications
- `reports` - Submitted reports
- `broadcasts` - System broadcasts
- `verification_assignments` - Verification tasks
- `audit_log` - Activity audit trail

## Error Handling

The API returns standardized error responses:

```json
{
  "success": false,
  "message": "Error description"
}
```

## Socket.io Events

Real-time notification support:
- `join-notifications` - Subscribe to user notifications
- `leave-notifications` - Unsubscribe from notifications

## Development Notes

1. **Database**: SQLite database file is created at `./data/samadhan.db`
2. **Uploads**: File uploads are stored in `./uploads` directory
3. **Security**: Always change `JWT_SECRET` in production
4. **CORS**: Frontend URL must match `FRONTEND_URL` in `.env`

## Project Structure

```
src/
├── database/
│   └── init.js          # Database initialization
├── middleware/
│   ├── auth.js          # Authentication middleware
│   └── errorHandler.js  # Error handling
├── routes/
│   ├── auth.js          # Authentication routes
│   ├── projects.js      # Project routes
│   ├── tenders.js       # Tender routes
│   ├── funds.js         # Fund routes
│   ├── payments.js      # Payment routes
│   ├── inspections.js   # Inspection routes
│   ├── users.js         # User routes
│   ├── notifications.js # Notification routes
│   └── reports.js       # Report routes
└── utils/
    └── helpers.js       # Utility functions
scripts/
└── seed.js             # Database seeding script
```

## Performance Tips

1. Add database indexes for frequently queried fields
2. Implement pagination for large datasets
3. Cache frequently accessed data
4. Use connection pooling for database
5. Enable compression middleware in production

## Security Checklist

- [ ] Change JWT_SECRET in production
- [ ] Use HTTPS in production
- [ ] Enable rate limiting
- [ ] Validate all inputs
- [ ] Use environment variables for secrets
- [ ] Implement CORS properly
- [ ] Add SQL injection protection (using parameterized queries)
- [ ] Set secure session cookies

## Troubleshooting

**Database not found error**
- Ensure `./data` directory exists or create it manually
- Run `node scripts/seed.js` to initialize

**Port already in use**
- Change PORT in .env file
- Or kill process using port 5000: `lsof -ti:5000 | xargs kill -9`

**Authentication failures**
- Check JWT_SECRET matches in .env
- Verify token is being sent in Authorization header
- Check token expiry time

## Contributing

Follow the existing code structure and naming conventions. Always:
1. Add proper error handling
2. Validate input data
3. Document API endpoints
4. Test with different user roles

## License

ISC

## Support

For issues and questions, please create an issue in the repository.
