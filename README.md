# Samadhan Bandhu - Government Project Management Portal

[![License](https://img.shields.io/badge/license-ISC-blue.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/node-%3E%3D16.0.0-brightgreen.svg)](https://nodejs.org/)
[![React](https://img.shields.io/badge/react-%3E%3D19.0.0-61dafb.svg)](https://reactjs.org/)
[![Status](https://img.shields.io/badge/status-Production%20Ready-brightgreen.svg)]()

**Samadhan Bandhu** is a comprehensive government project management portal designed to streamline project execution, tender management, fund allocation, and inspections across central, state, and block levels in India.

## 🎯 Overview

Samadhan Bandhu provides an integrated platform for managing government-funded development projects with features for multi-level governance, tender management, payment tracking, and quality inspections through an intuitive, role-based interface.

### Key Highlights
- 🔐 **Multi-Level Governance** - Central, State, Block, and Agency management
- 📋 **Project Management** - Complete project lifecycle management
- 🏆 **Tender System** - Create, publish, and evaluate tenders
- 💰 **Fund Allocation** - Track fund release and allocation across levels
- 💳 **Payment Processing** - Secure payment approval workflow
- 🔍 **Inspections** - Field inspections with photo/GPS verification
- 📊 **Real-time Reporting** - Comprehensive dashboards and analytics
- 👥 **User Management** - Role-based access control
- 🔔 **Notifications** - Real-time updates via Socket.IO
- 📱 **Responsive Design** - Mobile-friendly interface

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Authentication](#authentication)
- [User Roles](#user-roles)
- [Database Schema](#database-schema)
- [Development](#development)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

### 🏛️ Central Government
- Create and manage projects
- Create and publish tenders
- Approve/reject payments and fund releases
- Manage user accounts across all levels
- Send system-wide broadcasts
- View comprehensive reports and statistics

### 🏘️ State Level
- View and manage state projects
- Create tenders for state projects
- Release funds to block level
- Approve/reject tender applications
- Monitor inspections
- Generate state-level reports

### 🏪 Block Level
- Manage block-level projects
- Create tenders
- Release funds
- Track payments
- Submit progress reports
- View field inspections

### 🏢 Agency (Implementation Partner)
- Apply for tenders
- Track tender applications
- Submit progress reports
- Access project information
- View payment status

### 👮 Field Officer
- Schedule and conduct field inspections
- Submit inspection reports with photos
- Record GPS coordinates
- Update project completion percentage
- Document findings and recommendations

## 🛠 Tech Stack

### Backend
- **Runtime**: Node.js (v16+)
- **Framework**: Express.js 4.18
- **Database**: SQLite3
- **Authentication**: JWT (JSON Web Tokens)
- **Real-time**: Socket.IO 4.7
- **Security**: bcryptjs, Helmet
- **File Upload**: Multer
- **Image Processing**: Sharp

### Frontend
- **Library**: React 19
- **Build Tool**: Vite 7.2
- **Routing**: React Router v7
- **HTTP Client**: Axios
- **Styling**: Tailwind CSS 4.1
- **UI Components**: Lucide React
- **Charts**: Recharts
- **Maps**: Leaflet & React-Leaflet
- **Real-time**: Socket.IO Client
- **PDF Export**: jsPDF & jsPDF AutoTable

## 📦 Prerequisites

- **Node.js** v16.0.0 or higher
- **npm** v7.0.0 or higher (or yarn)
- **Git** for version control
- Modern web browser (Chrome, Firefox, Safari, Edge)

### System Requirements
- **RAM**: Minimum 2GB
- **Storage**: 500MB for installation
- **Disk Space**: 1GB for database and uploads

## 🚀 Installation

### 1. Clone Repository

```bash
git clone <repository-url>
cd SamadhanBandhu_PS-25153
```

### 2. Backend Setup

```bash
cd SamadhanBandhu_Backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Initialize database and seed test data
node scripts/seed.js

# Start development server
npm run dev
```

**Backend runs on**: `http://localhost:5000`

### 3. Frontend Setup

```bash
cd ../SamadhanBandhu_Frontend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Start development server
npm run dev
```

**Frontend runs on**: `http://localhost:5173`

## ⚡ Quick Start

### Prerequisites Met? Start Here:

```bash
# Terminal 1 - Backend
cd SamadhanBandhu_Backend
npm install
node scripts/seed.js  # First time only
npm run dev

# Terminal 2 - Frontend (new terminal)
cd SamadhanBandhu_Frontend
npm install
npm run dev
```

### Access Application

Open your browser and navigate to: **http://localhost:5173**

### Test Login Credentials

| Role | Email | Password |
|------|-------|----------|
| Central Admin | central@samadhan.gov.in | password123 |
| State Officer | state@samadhan.gov.in | password123 |
| Block Manager | block@samadhan.gov.in | password123 |
| Agency | agency@samadhan.gov.in | password123 |
| Field Officer | fieldofficer@samadhan.gov.in | password123 |

## 📁 Project Structure

```
SamadhanBandhu_PS-25153/
├── SamadhanBandhu_Backend/
│   ├── src/
│   │   ├── database/
│   │   │   └── init.js           # Database initialization & schema
│   │   ├── middleware/
│   │   │   ├── auth.js           # JWT authentication & authorization
│   │   │   └── errorHandler.js   # Error handling middleware
│   │   ├── routes/
│   │   │   ├── auth.js           # Authentication endpoints
│   │   │   ├── projects.js       # Project CRUD operations
│   │   │   ├── tenders.js        # Tender management
│   │   │   ├── payments.js       # Payment processing
│   │   │   ├── funds.js          # Fund allocation
│   │   │   ├── inspections.js    # Inspection management
│   │   │   ├── notifications.js  # Notification system
│   │   │   ├── reports.js        # Report submission
│   │   │   ├── broadcasts.js     # System broadcasts
│   │   │   └── users.js          # User management
│   │   └── utils/
│   │       └── helpers.js        # Utility functions
│   ├── scripts/
│   │   └── seed.js               # Database seeding script
│   ├── server.js                 # Express app entry point
│   ├── package.json
│   └── .env                      # Environment variables
│
├── SamadhanBandhu_Frontend/
│   ├── src/
│   │   ├── features/
│   │   │   ├── auth/             # Authentication components
│   │   │   ├── central/          # Central admin dashboards
│   │   │   ├── state/            # State level dashboards
│   │   │   ├── block/            # Block level dashboards
│   │   │   ├── agency/           # Agency dashboards
│   │   │   └── field-officer/    # Field officer dashboards
│   │   ├── shared/
│   │   │   ├── components/       # Reusable UI components
│   │   │   ├── layouts/          # Layout components
│   │   │   ├── hooks/            # Custom React hooks
│   │   │   ├── services/         # API services
│   │   │   └── utils/            # Shared utilities
│   │   ├── routes/
│   │   │   └── RoleBasedRoute.jsx # Role-based routing
│   │   ├── App.jsx               # Main App component
│   │   └── main.jsx              # React entry point
│   ├── package.json
│   └── .env                      # Environment variables
│
└── Documentation/
    └── README.md                 # This file
```

## 🔌 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Authentication Endpoints

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "firstName": "John",
  "lastName": "Doe",
  "role": "agency"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword"
}

Response:
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "role": "agency"
  }
}
```

#### Verify Credentials (2FA)
```http
POST /api/auth/verify-credentials
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword",
  "role": "agency"
}
```

### Project Endpoints

```http
GET    /api/projects              # Get all projects
POST   /api/projects              # Create project (Central only)
GET    /api/projects/:id          # Get project details
PUT    /api/projects/:id          # Update project (Central only)
GET    /api/projects/:id/stats    # Get project statistics
```

### Tender Endpoints

```http
GET    /api/tenders               # Get all tenders
POST   /api/tenders               # Create tender
GET    /api/tenders/:id           # Get tender details
POST   /api/tenders/:id/apply     # Apply for tender (Agency)
PUT    /api/tenders/:id/status    # Update tender status
PUT    /api/tenders/application/:appId/evaluate  # Evaluate application
```

### Payment Endpoints

```http
GET    /api/payments              # Get all payments
POST   /api/payments              # Create payment record
GET    /api/payments/:id          # Get payment details
PUT    /api/payments/:id/approve  # Approve payment
PUT    /api/payments/:id/reject   # Reject payment
GET    /api/payments/summary/stats # Payment statistics
```

### Full API Documentation
See [API_DOCS.md](./SamadhanBandhu_Backend/API_DOCS.md) for complete API reference.

## 🔐 Authentication

The application uses JWT (JSON Web Tokens) for authentication.

### Flow

1. **User Login** → Credentials verified
2. **Token Generation** → JWT token created
3. **Token Storage** → Stored in localStorage
4. **Token Submission** → Sent in Authorization header
5. **Token Verification** → Backend validates token
6. **Access Control** → Role-based permissions enforced

### Token Structure

```javascript
{
  "id": "user-uuid",
  "email": "user@example.com",
  "role": "agency",
  "iat": 1704657000,
  "exp": 1705262000
}
```

### Token Expiry
- **Default**: 7 days
- **Configurable**: In `.env` file via `JWT_EXPIRY`

### Adding Token to Requests

All requests automatically include the token via Axios interceptor:

```javascript
Authorization: Bearer <token>
```

## 👥 User Roles

### Central Government
**Permissions**: Full system access
- Create and manage projects
- Create tenders
- Approve payments
- Manage all users
- Send broadcasts

### State Officer
**Permissions**: State-level management
- View projects
- Create state tenders
- Release funds to blocks
- Approve/reject applications
- View state reports

### Block Manager
**Permissions**: Block-level management
- Manage block projects
- Create block tenders
- Release funds
- Create payments
- Submit reports

### Agency
**Permissions**: Limited operational access
- View tenders
- Apply for tenders
- Submit reports
- View project details

### Field Officer
**Permissions**: Field operations
- Schedule inspections
- Submit inspection reports
- Upload photos and GPS data
- Update completion status


## 🗄️ Database Schema

### Key Tables

**users**
- id, email, password (hashed)
- firstName, lastName
- role (central, state, block, agency, field-officer)
- state, district, block
- department, designation
- isActive, createdAt, updatedAt

**projects**
- id, title, description
- state, district, block, village
- category, budgetAmount
- allocatedFunds, releasedFunds
- status, completionPercentage
- latitude, longitude (for GIS)
- createdBy (userId)

**tenders**
- id, title, description
- category, estimatedBudget
- publishDate, closingDate
- status, projectId
- createdBy (userId)

**payments**
- id, projectId, agencyId
- amount, paymentType
- status (pending, approved, rejected)
- invoiceNumber, paymentDate
- approvedBy, approvedDate

**inspections**
- id, projectId, inspectorId
- status, scheduledDate, actualDate
- completionPercentage
- findings, recommendations
- photoUrls, gpsCoordinates

See [ARCHITECTURE_DIAGRAM.md](./ARCHITECTURE_DIAGRAM.md) for complete schema visualization.

## 💻 Development

### Running in Development Mode

```bash
# Backend with auto-reload (nodemon)
cd SamadhanBandhu_Backend
npm run dev

# Frontend with hot reload (Vite)
cd SamadhanBandhu_Frontend
npm run dev
```

### Building for Production

```bash
# Frontend build
cd SamadhanBandhu_Frontend
npm run build
# Output: dist/ directory

# Backend production start
cd SamadhanBandhu_Backend
npm start
```

### Code Quality

```bash
# Linting (Frontend)
cd SamadhanBandhu_Frontend
npm run lint
```

### Database Management

```bash
# Reset database
cd SamadhanBandhu_Backend
rm -f data/samadhan.db
node scripts/seed.js

# Add new user via API
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "newuser@example.com",
    "password": "password123",
    "firstName": "First",
    "lastName": "Last",
    "role": "agency"
  }'
```

### Environment Variables

#### Backend (.env)
```env
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173

JWT_SECRET=your_jwt_secret_key
JWT_EXPIRY=7d

DB_PATH=./data/samadhan.db
MAX_FILE_SIZE=50000000
UPLOAD_DIR=./uploads

SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

GOOGLE_MAPS_API_KEY=your_api_key
```

#### Frontend (.env)
```env
VITE_API_BASE_URL=http://localhost:5000/api
```

## 🚀 Deployment

### Deployment Checklist

- [ ] Change `JWT_SECRET` to strong random value
- [ ] Set `NODE_ENV=production`
- [ ] Update `FRONTEND_URL` to production domain
- [ ] Update `VITE_API_BASE_URL` to production API URL
- [ ] Enable HTTPS/SSL
- [ ] Set up database backups
- [ ] Configure rate limiting
- [ ] Set up monitoring and logging
- [ ] Review CORS origins
- [ ] Test all user roles
- [ ] Set up CDN for static assets

### Docker Deployment (Optional)

```dockerfile
# Backend Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 5000
CMD ["npm", "start"]

# Frontend - Build stage
FROM node:18-alpine as builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Frontend - Serve stage
FROM node:18-alpine
WORKDIR /app
RUN npm install -g serve
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["serve", "-s", "dist", "-l", "3000"]
```

## 🐛 Troubleshooting

### Backend Won't Start

**Port Already in Use**
```bash
# Find process using port 5000
lsof -i :5000
# Kill the process
kill -9 <PID>
```

**Database Connection Error**
```bash
# Ensure data directory exists
mkdir -p data

# Reset database
rm -f data/samadhan.db
node scripts/seed.js
```

**Module Not Found**
```bash
# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### Frontend Won't Start

**Port 5173 in Use**
```bash
# Try different port
npm run dev -- --port 3000
```

**CORS Errors**

Check that:
1. Backend is running on port 5000
2. Frontend `.env` has `VITE_API_BASE_URL=http://localhost:5000/api`
3. Backend CORS is configured for `http://localhost:5173`

### Login Issues

1. **Verify database is seeded**
   ```bash
   cd SamadhanBandhu_Backend
   node scripts/seed.js
   ```

2. **Check browser console** for error messages (F12)

3. **Verify API connectivity**
   ```bash
   curl http://localhost:5000/api/health
   ```

4. **Clear browser cache** and localStorage

### API Requests Fail

1. **Check network tab** in browser DevTools
2. **Verify token in localStorage** (F12 → Application)
3. **Check server logs** in backend terminal
4. **Verify authorization headers** are present

## 📚 Documentation

- [QUICK_START.md](./QUICK_START.md) - Step-by-step setup guide
- [COMPATIBILITY_REPORT.md](./COMPATIBILITY_REPORT.md) - Frontend-backend compatibility details
- [ARCHITECTURE_DIAGRAM.md](./ARCHITECTURE_DIAGRAM.md) - System architecture and diagrams
- [API_DOCS.md](./SamadhanBandhu_Backend/API_DOCS.md) - Complete API reference
- [SETUP.md](./SamadhanBandhu_Backend/SETUP.md) - Backend setup guide
- [DEPLOYMENT.md](./SamadhanBandhu_Backend/DEPLOYMENT.md) - Production deployment guide

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes** with clear commit messages
4. **Test thoroughly** before submitting
5. **Submit a pull request** with detailed description

### Coding Standards
- Use consistent naming conventions
- Add comments for complex logic
- Follow existing code style
- Test all changes locally

## 📄 License

This project is licensed under the ISC License - see [LICENSE](LICENSE) file for details.

## 👥 Support

For issues, questions, or suggestions:

1. **Check existing documentation** in the docs folder
2. **Search GitHub issues** for similar problems
3. **Create a new issue** with detailed description
4. **Contact project maintainers** via email

## 🎓 Learning Resources

### Official Documentation
- [Node.js Docs](https://nodejs.org/docs/)
- [Express.js Guide](https://expressjs.com/)
- [React Documentation](https://react.dev/)
- [Vite Guide](https://vitejs.dev/)
- [SQLite Docs](https://www.sqlite.org/docs.html)

### Related Tutorials
- JWT Authentication in Node.js
- Building RESTful APIs with Express
- React Hooks and State Management
- Tailwind CSS Styling

## 📞 Contact

- **Project Lead**: Anubhab Mishra
- **Email**: support@samadhan.gov.in
- **GitHub Issues**: [Project Repository]

## 🙏 Acknowledgments

- Government of India for project requirements
- Open-source community for excellent tools and libraries
- Contributors who helped improve the project

---

**Version**: 1.0.0  
**Last Updated**: January 7, 2026  
**Status**: Production Ready ✅

For the latest updates, visit the [project repository](https://github.com/your-org/samadhan-bandhu).
