# Visual Architecture & Flow Diagrams

## 1. System Architecture

```
┌────────────────────────────────────────────────────────────────┐
│                     CLIENT LAYER                               │
│                  React Frontend (Vite)                         │
│              http://localhost:5173                             │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌──────────────────┐  ┌──────────────────┐  ┌────────────┐   │
│  │   Dashboard      │  │   Projects       │  │  Tenders   │   │
│  └──────────────────┘  └──────────────────┘  └────────────┘   │
│                                                                │
│  ┌──────────────────┐  ┌──────────────────┐  ┌────────────┐   │
│  │   Inspections    │  │   Payments       │  │  Reports   │   │
│  └──────────────────┘  └──────────────────┘  └────────────┘   │
│                                                                │
└────────────┬──────────────────────────────────┬────────────────┘
             │                                  │
        HTTP Requests                    WebSocket (Socket.io)
        JSON with JWT Token              Real-time Updates
             │                                  │
             ↓                                  ↓
┌────────────────────────────────────────────────────────────────┐
│                     API LAYER                                  │
│                Express.js Server                               │
│              http://localhost:5000                             │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐      │
│  │   Auth   │  │ Projects │  │ Tenders  │  │  Funds   │      │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘      │
│                                                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐      │
│  │ Payments │  │Inspections│ │ Reports  │  │  Users   │      │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘      │
│                                                                │
│  Middleware Layer:                                            │
│  ├─ Authentication (JWT)                                      │
│  ├─ Authorization (Role-based)                                │
│  ├─ Error Handling                                            │
│  ├─ CORS & Security (Helmet)                                  │
│  └─ Request Validation                                        │
│                                                                │
└────────────┬──────────────────────────────────────────────────┘
             │
        SQL Queries
             │
             ↓
┌────────────────────────────────────────────────────────────────┐
│                   DATABASE LAYER                               │
│                    SQLite3                                     │
│               ./data/samadhan.db                               │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  Users  │ Projects │ Tenders │ Applications │ Funds │ Payments│
│  ────────────────────────────────────────────────────         │
│  Inspections │ Notifications │ Reports │ Broadcasts           │
│                                                                │
│  Relationships:                                               │
│  ├─ Users → Projects (creator)                                │
│  ├─ Projects → Tenders                                        │
│  ├─ Tenders → Applications                                    │
│  ├─ Projects → Funds                                          │
│  ├─ Projects → Payments                                       │
│  ├─ Projects → Inspections                                    │
│  └─ Users → Notifications                                     │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

## 2. User Authentication Flow

```
                    USER LOGIN
                        │
                        ↓
        ┌───────────────────────────────┐
        │ Email & Password              │
        └───────────┬───────────────────┘
                    ↓
        ┌───────────────────────────────┐
        │ Backend Validates             │
        │ ├─ Check if user exists      │
        │ ├─ Compare passwords         │
        │ └─ Check if active           │
        └───────────┬───────────────────┘
                    │
        ┌──────────┴──────────┐
        │ Valid? │            │ Invalid
        ↓                     ↓
    ┌────────────┐        ┌──────────┐
    │ Generate   │        │ Error    │
    │ JWT Token  │        │ 401      │
    └────┬───────┘        └──────────┘
         │
         ↓
    ┌─────────────────────┐
    │ Send Token to       │
    │ Frontend            │
    └────┬────────────────┘
         │
         ↓
    ┌─────────────────────┐
    │ Store in            │
    │ localStorage        │
    └────┬────────────────┘
         │
         ↓
    ┌─────────────────────────────┐
    │ Send Token in Every          │
    │ Request Header:              │
    │ Authorization: Bearer {token}│
    └─────────────────────────────┘
```

---

## 3. Role-Based Access Control

```
┌─────────────────────────────────────────────────────┐
│                    USER ROLES                       │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌────────────────┐  Level: National               │
│  │ Central Admin  │  ├─ Create projects            │
│  │ (central)      │  ├─ Approve funds              │
│  │                │  ├─ Manage tenders             │
│  └────────────────┘  └─ View all data              │
│                                                     │
│  ┌────────────────┐  Level: State                  │
│  │ State Officer  │  ├─ Approve allocations        │
│  │ (state)        │  ├─ Evaluate tenders           │
│  │                │  ├─ Manage state projects      │
│  └────────────────┘  └─ Verify applications        │
│                                                     │
│  ┌────────────────┐  Level: Block                  │
│  │ Block Manager  │  ├─ Release tenders            │
│  │ (block)        │  ├─ Track projects             │
│  │                │  ├─ Manage payments            │
│  └────────────────┘  └─ Monitor activities         │
│                                                     │
│  ┌────────────────┐  Level: Agency                 │
│  │ Agency         │  ├─ Apply for tenders          │
│  │ (agency)       │  ├─ Submit reports             │
│  │                │  ├─ Track projects             │
│  └────────────────┘  └─ Manage own projects        │
│                                                     │
│  ┌────────────────┐  Level: Ground                 │
│  │ Field Officer  │  ├─ Schedule inspections       │
│  │ (field-officer)│  ├─ Submit reports             │
│  │                │  ├─ Upload photos              │
│  └────────────────┘  └─ Track completion           │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 4. Project Lifecycle

```
                    START
                      │
                      ↓
        ┌─────────────────────────┐
        │ PLANNING Phase          │
        ├─────────────────────────┤
        │ ✓ Create project        │
        │ ✓ Define scope          │
        │ ✓ Set budget            │
        │ ✓ Allocate funds        │
        └─────────┬───────────────┘
                  │ Approved
                  ↓
        ┌─────────────────────────┐
        │ EXECUTION Phase         │
        ├─────────────────────────┤
        │ ✓ Release funds         │
        │ ✓ Publish tenders       │
        │ ✓ Process applications  │
        │ ✓ Make payments         │
        │ ✓ Start work            │
        └─────────┬───────────────┘
                  │ In Progress
                  ↓
        ┌─────────────────────────┐
        │ MONITORING Phase        │
        ├─────────────────────────┤
        │ ✓ Schedule inspections  │
        │ ✓ Verify work quality   │
        │ ✓ Track progress        │
        │ ✓ Submit reports        │
        │ ✓ Update completion %   │
        └─────────┬───────────────┘
                  │ Completed
                  ↓
        ┌─────────────────────────┐
        │ CLOSURE Phase           │
        ├─────────────────────────┤
        │ ✓ Final inspection      │
        │ ✓ Approve final report  │
        │ ✓ Release final payment │
        │ ✓ Archive records       │
        │ ✓ Close project         │
        └─────────┬───────────────┘
                  │
                  ↓
                FINISH
```

---

## 5. Tender Application Flow

```
┌──────────────────────────┐
│ Central Authority        │
│ ├─ Create Tender         │
│ └─ Set Closing Date      │
└──────────┬───────────────┘
           │
           ↓
┌──────────────────────────┐
│ TENDER PUBLISHED         │
│ └─ Visible to all        │
└──────────┬───────────────┘
           │
           ↓
┌──────────────────────────┐
│ Agencies View Tenders    │
│ └─ Select & Apply        │
└──────────┬───────────────┘
           │
           ↓
┌──────────────────────────┐
│ APPLICATIONS SUBMITTED   │
│ ├─ Budget proposal       │
│ ├─ Documents             │
│ └─ Timeline              │
└──────────┬───────────────┘
           │
           ↓
┌──────────────────────────┐
│ State Reviews            │
│ ├─ Evaluate bids         │
│ ├─ Score applications    │
│ └─ Recommend best        │
└──────────┬───────────────┘
           │
    ┌──────┴──────┬─────────┐
    │ Approved    │ Rejected│
    ↓             ↓         ↓
┌──────────┐  ┌────────┐  ┌──────┐
│ AWARDED  │  │ PENDING│  │ REJECTED
│ to Agency│  │ Review │  │
└──────────┘  └────────┘  └──────┘
    │
    ↓
┌──────────────────────────┐
│ Project Execution        │
│ ├─ Payments              │
│ ├─ Inspections           │
│ └─ Reports               │
└──────────────────────────┘
```

---

## 6. Fund Flow Hierarchy

```
                 ┌──────────────────┐
                 │  CENTRAL LEVEL   │
                 │   Total Budget   │
                 └────────┬─────────┘
                          │ Release
                          ↓
              ┌────────────────────────┐
              │   STATE LEVEL          │
              │   State Allocation     │
              │  (Multiple States)     │
              └────┬──────────┬────────┘
                   │ Release  │ Release
                   ↓          ↓
         ┌──────────────┐  ┌──────────────┐
         │  DISTRICT    │  │  DISTRICT    │
         │  Allocation  │  │  Allocation  │
         └─────┬────────┘  └─────┬────────┘
               │ Release         │ Release
               ↓                 ↓
         ┌──────────┐      ┌──────────┐
         │  BLOCK   │      │  BLOCK   │
         │  Funds   │      │  Funds   │
         └────┬─────┘      └────┬─────┘
              │ Execute        │ Execute
              ↓                ↓
         ┌──────────┐      ┌──────────┐
         │ PROJECT  │      │ PROJECT  │
         │ WORK     │      │ WORK     │
         └──────────┘      └──────────┘
```

---

## 7. Data Relationships

```
                    ┌──────────────┐
                    │    USERS     │
                    └──────┬───────┘
                           │
          ┌────────────────┼────────────────┐
          ↓                ↓                ↓
     ┌────────┐    ┌────────────┐    ┌────────────┐
     │PROJECTS│    │  TENDERS   │    │NOTIFICATIONS
     └────┬───┘    └──────┬─────┘    └────────────┘
          │               │
          ├─→ ┌────────────────────┐
          │   │ TENDER APPLICATION │
          │   └────────────────────┘
          │
          ├─→ ┌───────────────┐
          │   │  FUNDS        │
          │   └───────────────┘
          │
          ├─→ ┌───────────────┐
          │   │  PAYMENTS     │
          │   └───────────────┘
          │
          ├─→ ┌────────────────┐
          │   │  INSPECTIONS  │
          │   └────────────────┘
          │
          └─→ ┌───────────────┐
              │  REPORTS      │
              └───────────────┘
```

---

## 8. Request-Response Cycle

```
┌──────────────┐
│ Frontend App │
└──────┬───────┘
       │
       │ 1. HTTP Request
       │    Headers: Authorization Bearer {token}
       │    Body: JSON data
       │
       ↓
┌──────────────────────────────┐
│ Express Server               │
├──────────────────────────────┤
│ 2. Parse Request             │
│ 3. Verify JWT Token          │
│ 4. Check Authorization       │
│ 5. Validate Input Data       │
└──────┬───────────────────────┘
       │
       │ Valid?
       │
       ├─→ YES ──→ 6. Execute Business Logic
       │           7. Query Database
       │           8. Process Data
       │
       ├─→ NO ──→ 9. Generate Error Response
       │
       ↓
┌──────────────────────────┐
│ Generate Response        │
├──────────────────────────┤
│ 10. JSON Data            │
│ 11. Status Code          │
│ 12. Headers              │
└──────┬───────────────────┘
       │
       │ 13. HTTP Response
       │
       ↓
┌──────────────────────┐
│ Frontend App         │
├──────────────────────┤
│ 14. Parse Response   │
│ 15. Handle Errors    │
│ 16. Update UI        │
│ 17. Show Results     │
└──────────────────────┘
```

---

## 9. Real-time Notification Flow

```
┌──────────────────────┐
│ Action Triggered     │
│ (Project Created,    │
│  Payment Approved,   │
│  etc.)               │
└──────┬───────────────┘
       │
       ↓
┌──────────────────────┐
│ Backend Processing   │
├──────────────────────┤
│ 1. Process Action    │
│ 2. Update Database   │
│ 3. Create Notification
└──────┬───────────────┘
       │
       ↓
┌──────────────────────────────┐
│ Socket.io Emission           │
├──────────────────────────────┤
│ Emit to: user-{userId}       │
│ Event: notification          │
│ Data: Notification object    │
└──────┬───────────────────────┘
       │
       ↓ Real-time
┌──────────────────────┐
│ Frontend Connection  │
├──────────────────────┤
│ Listen on Socket.io  │
│ Receive Notification │
│ Update UI in Real    │
│ Time                 │
└──────────────────────┘
```

---

## 10. Error Handling Flow

```
                   ┌─────────────┐
                   │ API Request │
                   └──────┬──────┘
                          │
                   ┌──────→ Error?
                   │
        ┌──────────┴──────────┐
        │                     │
       YES                   NO
        │                     │
        ↓                     ↓
   ┌────────────┐      ┌─────────┐
   │ Error Type │      │ Success │
   └─────┬──────┘      └────┬────┘
         │                  │
   ┌─────┴─────┬──────┬─────┘
   │           │      │
   ↓           ↓      ↓
 ┌─────┐  ┌────────┐  ┌──────────┐
 │Auth │  │Validation Process & Return
 │     │  │Request  │  Data
 └────┬┘  └────┬───┘  └────┬─────┘
      │        │           │
      ↓        ↓           ↓
   ┌─────────────────────────────┐
   │ Generate Response           │
   ├─────────────────────────────┤
   │ 401 Unauthorized            │
   │ 400 Bad Request             │
   │ 404 Not Found               │
   │ 500 Server Error            │
   │ 200 OK (Success)            │
   │ 201 Created                 │
   └─────┬───────────────────────┘
         │
         ↓
   ┌──────────────────┐
   │ Send to Frontend │
   │ With Error Code  │
   │ & Message        │
   └──────────────────┘
```

---

**All diagrams show the complete backend architecture and flow!**
