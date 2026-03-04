# API Documentation

## Base URL
```
http://localhost:5000/api
```

## Authentication
All endpoints except `/auth/login` and `/auth/register` require a Bearer token in the Authorization header:

```
Authorization: Bearer <token>
```

---

## Authentication Endpoints

### Register User
- **URL**: `/auth/register`
- **Method**: `POST`
- **Body**:
```json
{
  "email": "user@example.com",
  "password": "password123",
  "firstName": "John",
  "lastName": "Doe",
  "role": "agency",
  "state": "Bihar",
  "district": "Patna",
  "block": "Patna City"
}
```
- **Response**: `201 Created`
```json
{
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": { ... }
}
```

### Login
- **URL**: `/auth/login`
- **Method**: `POST`
- **Body**:
```json
{
  "email": "central@samadhan.gov.in",
  "password": "password123"
}
```
- **Response**: `200 OK`

### Get Current User
- **URL**: `/auth/me`
- **Method**: `GET`
- **Response**: `200 OK` - User object

### Update Profile
- **URL**: `/auth/profile`
- **Method**: `PUT`
- **Body**:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "9876543210",
  "department": "Ministry",
  "designation": "Officer",
  "profilePicture": "base64_image_string"
}
```

### Change Password
- **URL**: `/auth/change-password`
- **Method**: `POST`
- **Body**:
```json
{
  "currentPassword": "password123",
  "newPassword": "newpassword123"
}
```

---

## Projects Endpoints

### Create Project
- **URL**: `/projects`
- **Method**: `POST`
- **Auth**: Central role required
- **Body**:
```json
{
  "title": "School Construction",
  "description": "Building a new school",
  "state": "Bihar",
  "district": "Patna",
  "block": "Patna City",
  "village": "Rajendra Nagar",
  "category": "Education",
  "budgetAmount": 5000000,
  "startDate": "2024-01-01",
  "endDate": "2024-12-31",
  "latitude": 25.5941,
  "longitude": 85.1376
}
```
- **Response**: `201 Created`

### Get All Projects
- **URL**: `/projects?state=Bihar&status=ongoing&category=Education`
- **Method**: `GET`
- **Query Parameters**: 
  - `state` - Filter by state
  - `status` - planning, ongoing, completed
  - `category` - Project category

### Get Single Project
- **URL**: `/projects/:id`
- **Method**: `GET`

### Update Project
- **URL**: `/projects/:id`
- **Method**: `PUT`
- **Auth**: Central role required
- **Body**: Same as create (partial update)

### Get Project Statistics
- **URL**: `/projects/:id/stats`
- **Method**: `GET`
- **Response**:
```json
{
  "project": { ... },
  "stats": {
    "inspections": 5,
    "tenders": 2,
    "totalPayments": 1500000,
    "completionPercentage": 45
  }
}
```

---

## Tenders Endpoints

### Create Tender
- **URL**: `/tenders`
- **Method**: `POST`
- **Auth**: Central, Block, or State role required
- **Body**:
```json
{
  "title": "Construction Tender",
  "description": "Tender for school construction",
  "category": "Construction",
  "estimatedBudget": 5000000,
  "publishDate": "2024-01-01",
  "closingDate": "2024-02-15",
  "projectId": "project-uuid",
  "state": "Bihar",
  "district": "Patna",
  "block": "Patna City"
}
```

### Get All Tenders
- **URL**: `/tenders?state=Bihar&status=published`
- **Method**: `GET`
- **Query Parameters**: 
  - `state` - Filter by state
  - `status` - published, closed, awarded
  - `category` - Tender category

### Get Tender Details
- **URL**: `/tenders/:id`
- **Method**: `GET`
- **Response**:
```json
{
  "tender": { ... },
  "applications": [ ... ],
  "applicationCount": 5
}
```

### Apply for Tender
- **URL**: `/tenders/:id/apply`
- **Method**: `POST`
- **Auth**: Agency role required
- **Body**:
```json
{
  "proposedBudget": 4500000,
  "documentUrl": "https://..."
}
```

### Update Tender Status
- **URL**: `/tenders/:id/status`
- **Method**: `PUT`
- **Auth**: Central, Block, or State role required
- **Body**:
```json
{
  "status": "awarded"
}
```

### Evaluate Application
- **URL**: `/tenders/application/:applicationId/evaluate`
- **Method**: `PUT`
- **Auth**: Central or State role required
- **Body**:
```json
{
  "score": 85,
  "status": "approved"
}
```

---

## Funds Endpoints

### Release Funds
- **URL**: `/funds/release`
- **Method**: `POST`
- **Auth**: Central, State, or Block role required
- **Body**:
```json
{
  "toLevel": "state",
  "amount": 5000000,
  "purposeType": "project-execution",
  "projectId": "project-uuid",
  "releaseDate": "2024-01-01"
}
```

### Get All Funds
- **URL**: `/funds?status=released&toLevel=state`
- **Method**: `GET`
- **Query Parameters**: 
  - `status` - pending, released, approved
  - `toLevel` - Recipient level
  - `projectId` - Filter by project

### Get Fund Summary by Level
- **URL**: `/funds/summary/level`
- **Method**: `GET`

### Get Project Fund Allocation
- **URL**: `/funds/project/:projectId`
- **Method**: `GET`
- **Response**:
```json
{
  "allocations": [ ... ],
  "summary": {
    "totalAllocated": 5000000,
    "totalReleased": 3000000,
    "pending": 2000000
  }
}
```

### Approve Fund Release
- **URL**: `/funds/:fundId/approve`
- **Method**: `PUT`
- **Auth**: Central or State role required

---

## Payments Endpoints

### Create Payment Record
- **URL**: `/payments`
- **Method**: `POST`
- **Auth**: Central or Block role required
- **Body**:
```json
{
  "projectId": "project-uuid",
  "agencyId": "user-uuid",
  "amount": 500000,
  "paymentType": "partial",
  "invoiceNumber": "INV-001",
  "paymentDate": "2024-01-15"
}
```

### Get All Payments
- **URL**: `/payments?status=pending&projectId=uuid`
- **Method**: `GET`
- **Query Parameters**:
  - `status` - pending, approved, rejected
  - `projectId` - Filter by project
  - `agencyId` - Filter by agency

### Get Payment Details
- **URL**: `/payments/:id`
- **Method**: `GET`
- **Response**:
```json
{
  "payment": { ... },
  "project": { ... },
  "agency": { ... }
}
```

### Approve Payment
- **URL**: `/payments/:id/approve`
- **Method**: `PUT`
- **Auth**: Central or State role required
- **Body**:
```json
{
  "transactionId": "TXN-12345"
}
```

### Reject Payment
- **URL**: `/payments/:id/reject`
- **Method**: `PUT`
- **Auth**: Central or State role required
- **Body**:
```json
{
  "reason": "Incomplete documentation"
}
```

### Get Payment Summary
- **URL**: `/payments/summary/stats`
- **Method**: `GET`

---

## Inspections Endpoints

### Schedule Inspection
- **URL**: `/inspections`
- **Method**: `POST`
- **Auth**: State or Central role required
- **Body**:
```json
{
  "projectId": "project-uuid",
  "inspectorId": "user-uuid",
  "scheduledDate": "2024-01-20",
  "location": "Project site address"
}
```

### Get All Inspections
- **URL**: `/inspections?status=scheduled&projectId=uuid`
- **Method**: `GET`
- **Query Parameters**:
  - `status` - scheduled, completed
  - `projectId` - Filter by project
  - `inspectorId` - Filter by inspector

### Get Inspection Details
- **URL**: `/inspections/:id`
- **Method**: `GET`

### Submit Inspection Report
- **URL**: `/inspections/:id/submit`
- **Method**: `PUT`
- **Auth**: Field Officer role required
- **Body**:
```json
{
  "completionPercentage": 50,
  "findings": "Construction progressing well",
  "recommendations": "Continue work as per schedule",
  "photoUrls": "[\"url1\", \"url2\"]",
  "gpsCoordinates": "25.5941,85.1376",
  "reportUrl": "https://..."
}
```

### Get Inspection Statistics
- **URL**: `/inspections/stats/summary`
- **Method**: `GET`

---

## Users Endpoints

### Get All Users
- **URL**: `/users?role=agency&state=Bihar&isActive=true`
- **Method**: `GET`
- **Auth**: Central or State role required
- **Query Parameters**:
  - `role` - User role
  - `state` - Filter by state
  - `isActive` - true/false

### Get User Details
- **URL**: `/users/:id`
- **Method**: `GET`

### Get Users by Role
- **URL**: `/users/role/agency`
- **Method**: `GET`

### Deactivate User
- **URL**: `/users/:id/deactivate`
- **Method**: `PUT`
- **Auth**: Central or State role required

### Activate User
- **URL**: `/users/:id/activate`
- **Method**: `PUT`
- **Auth**: Central or State role required

### Get User Statistics
- **URL**: `/users/stats/summary`
- **Method**: `GET`
- **Auth**: Central or State role required

---

## Notifications Endpoints

### Get Notifications
- **URL**: `/notifications?isRead=false`
- **Method**: `GET`
- **Query Parameters**:
  - `isRead` - true/false

### Get Unread Count
- **URL**: `/notifications/unread/count`
- **Method**: `GET`

### Mark as Read
- **URL**: `/notifications/:id/read`
- **Method**: `PUT`

### Mark All as Read
- **URL**: `/notifications/read/all`
- **Method**: `PUT`

### Delete Notification
- **URL**: `/notifications/:id`
- **Method**: `DELETE`

---

## Reports Endpoints

### Submit Report
- **URL**: `/reports`
- **Method**: `POST`
- **Body**:
```json
{
  "projectId": "project-uuid",
  "reportType": "progress",
  "period": "January 2024",
  "content": "Report content...",
  "fileUrl": "https://..."
}
```

### Get All Reports
- **URL**: `/reports?status=pending&reportType=progress`
- **Method**: `GET`
- **Query Parameters**:
  - `status` - pending, approved, rejected
  - `reportType` - progress, completion, financial
  - `projectId` - Filter by project
  - `submittedBy` - Filter by user

### Get Report Details
- **URL**: `/reports/:id`
- **Method**: `GET`

### Approve Report
- **URL**: `/reports/:id/approve`
- **Method**: `PUT`
- **Auth**: Central or State role required

### Reject Report
- **URL**: `/reports/:id/reject`
- **Method**: `PUT`
- **Auth**: Central or State role required
- **Body**:
```json
{
  "reason": "Incomplete details"
}
```

### Get Report Statistics
- **URL**: `/reports/stats/summary`
- **Method**: `GET`

---

## Status Codes

- `200 OK` - Successful GET request
- `201 Created` - Successful resource creation
- `400 Bad Request` - Invalid input
- `401 Unauthorized` - Missing or invalid authentication
- `403 Forbidden` - Insufficient permissions
- `404 Not Found` - Resource not found
- `409 Conflict` - Duplicate entry or conflict
- `500 Internal Server Error` - Server error

---

## Error Response Format

```json
{
  "success": false,
  "message": "Error description"
}
```

---

## Rate Limiting

Currently not implemented. Consider adding for production.

---

## Webhook Support

Not implemented. Plans for future versions.

---

## Changelog

### Version 1.0.0
- Initial release with all core features
- Authentication and role-based access
- Project, Tender, Fund, Payment management
- Inspection and Report tracking
- Real-time notifications with Socket.io
