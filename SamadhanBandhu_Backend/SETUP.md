# Installation and Setup Guide

## Quick Start (5 minutes)

### 1. Navigate to backend directory
```bash
cd SamadhanBandhu_Backend
```

### 2. Install dependencies
```bash
npm install
```

### 3. Create and configure .env file
```bash
cp .env.example .env
```

### 4. Seed test database
```bash
node scripts/seed.js
```

### 5. Start the server
```bash
npm run dev
```

Your backend is now running on **http://localhost:5000**

---

## Frontend Configuration

Make sure your frontend is configured to use the backend:

The frontend API endpoint should be: `http://localhost:5000/api`

This is already configured in:
- `src/shared/services/api.js` - baseURL: 'http://localhost:5000/api'

---

## Testing the Backend

### Using cURL

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "central@samadhan.gov.in",
    "password": "password123"
  }'
```

**Get Projects:**
```bash
curl -X GET http://localhost:5000/api/projects \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Using Postman

1. Import these endpoints into Postman
2. Copy the token from login response
3. Add to Authorization header: `Bearer {token}`

---

## Database Details

- **File Location**: `./data/samadhan.db`
- **Type**: SQLite 3
- **Tables**: 11 main tables
- **Sample Data**: Yes (run seed.js)

---

## Common Issues & Solutions

### Issue: "Cannot find module 'sqlite3'"
**Solution**: Run `npm install` again

### Issue: "Port 5000 already in use"
**Solution**: Change PORT in .env file or:
```bash
# Windows
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# macOS/Linux
lsof -ti:5000 | xargs kill -9
```

### Issue: "ENOENT: no such file or directory, open './data/samadhan.db'"
**Solution**: Run `node scripts/seed.js` to initialize database

---

## Frontend Integration

After backend is running, start your frontend:

```bash
cd SamadhanBandhu_Frontend
npm run dev
```

Frontend will be available at: **http://localhost:5173**

---

## Available Test Users

| Role | Email | Password |
|------|-------|----------|
| Central | central@samadhan.gov.in | password123 |
| State | state@samadhan.gov.in | password123 |
| Block | block@samadhan.gov.in | password123 |
| Agency | agency@samadhan.gov.in | password123 |
| Field Officer | fieldofficer@samadhan.gov.in | password123 |

---

## Production Deployment

1. **Update .env for production**
   ```
   NODE_ENV=production
   JWT_SECRET=generate_strong_random_key
   FRONTEND_URL=your_production_url
   ```

2. **Use process manager (PM2)**
   ```bash
   npm install -g pm2
   pm2 start server.js --name "samadhan-backend"
   ```

3. **Set up SSL/HTTPS** with nginx/Apache

---

## Next Steps

1. ✅ Backend is running
2. ✅ Database initialized
3. ✅ Test data seeded
4. **Now**: Start frontend `npm run dev`
5. **Test**: Login with test credentials

---

## Need Help?

Check logs in console for detailed error messages. Most common issues are resolved by:
1. Running `npm install` 
2. Running `node scripts/seed.js`
3. Restarting the server
