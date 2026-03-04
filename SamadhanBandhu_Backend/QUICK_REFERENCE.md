# Quick Reference Card

## 🚀 Getting Started (Copy & Paste)

```bash
# 1. Navigate to backend
cd SamadhanBandhu_Backend

# 2. Install packages
npm install

# 3. Setup environment
cp .env.example .env

# 4. Initialize database with test data
node scripts/seed.js

# 5. Start server
npm run dev
```

✅ Backend running on `http://localhost:5000`

---

## 📝 Test Login Commands

```bash
# Login as Central Admin
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "central@samadhan.gov.in",
    "password": "password123"
  }'
```

---

## 🔑 Test Accounts

```
Central:      central@samadhan.gov.in / password123
State:        state@samadhan.gov.in / password123
Block:        block@samadhan.gov.in / password123
Agency:       agency@samadhan.gov.in / password123
Field Officer: fieldofficer@samadhan.gov.in / password123
```

---

## 📂 Key Files

| File | Purpose |
|------|---------|
| `server.js` | Main Express server |
| `src/database/init.js` | Database setup |
| `src/routes/*.js` | API endpoints |
| `src/middleware/auth.js` | Authentication |
| `.env` | Configuration |
| `scripts/seed.js` | Test data |

---

## 🔗 Main API Routes

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/auth/login` | Login |
| POST | `/api/projects` | Create project |
| GET | `/api/projects` | List projects |
| POST | `/api/tenders` | Create tender |
| GET | `/api/tenders` | List tenders |
| POST | `/api/funds/release` | Release funds |
| GET | `/api/funds` | List funds |
| POST | `/api/payments` | Create payment |
| GET | `/api/payments` | List payments |
| POST | `/api/inspections` | Schedule inspection |
| GET | `/api/inspections` | List inspections |

**Full API Docs**: See `API_DOCS.md`

---

## 🛠️ Common Commands

```bash
# Development mode (with auto-reload)
npm run dev

# Production mode
npm start

# Seed database
node scripts/seed.js

# View logs
npm run dev 2>&1 | tail -f
```

---

## 📊 Database

- **Type**: SQLite3
- **Location**: `./data/samadhan.db`
- **Tables**: 11
- **Relationships**: Multi-level hierarchy

---

## 🔒 Authentication

1. User sends email + password to `/api/auth/login`
2. Backend returns JWT token
3. Client stores token in localStorage
4. All requests include: `Authorization: Bearer <token>`

---

## ✅ Frontend Integration

Frontend is already configured:
- API URL: `http://localhost:5000/api`
- File: `src/shared/services/api.js`

Just run: `npm run dev` in frontend directory

---

## 🚨 Troubleshooting

| Problem | Solution |
|---------|----------|
| Port 5000 in use | Change PORT in .env |
| Database not found | Run `node scripts/seed.js` |
| Auth fails | Check token in header |
| CORS error | Update FRONTEND_URL in .env |

---

## 📚 Documentation Files

```
SamadhanBandhu_Backend/
├── README.md              ← Main documentation
├── SETUP.md              ← Installation guide
├── API_DOCS.md           ← Full API reference
├── DEPLOYMENT.md         ← Production setup
└── BACKEND_SUMMARY.md    ← This summary
```

---

## 💡 Tips

1. **Use Postman** or **Insomnia** to test APIs
2. **Check console** for detailed error messages
3. **Always include token** in Authorization header
4. **Test with sample data** first
5. **Check API_DOCS.md** for endpoint details

---

## 🎯 Your Next Steps

1. ✅ Backend created
2. ✅ Database initialized
3. ✅ Test data seeded
4. **→ Start backend**: `npm run dev`
5. **→ Test endpoints** with Postman
6. **→ Start frontend**: `npm run dev`
7. **→ Login and explore**

---

## 📞 Quick Help

**Backend not starting?**
```bash
# Check for errors
npm install
node scripts/seed.js
npm run dev
```

**Frontend can't connect?**
- Verify backend is running on port 5000
- Check FRONTEND_URL in .env
- Verify API base URL in frontend

**Database issues?**
```bash
# Reinitialize everything
rm -rf data/
node scripts/seed.js
```

---

**Version**: 1.0.0 | **Status**: Production Ready ✅
