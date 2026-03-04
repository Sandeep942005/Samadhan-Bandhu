# Samadhan Bandhu - Government Project Management Portal

[![License](https://img.shields.io/badge/license-ISC-blue.svg)](LICENSE)        
[![Node.js](https://img.shields.io/badge/node-%3E%3D16.0.0-brightgreen.svg)](https://nodejs.org/)                                                               [![React](https://img.shields.io/badge/react-%3E%3D19.0.0-61dafb.svg)](https://reactjs.org/)                                                                    [![Status](https://img.shields.io/badge/status-Production%20Ready-brightgreen.svg)]()
**Samadhan Bandhu** is a comprehensive government project management portal designed to streamline project execution, tender management, fund allocation, and inspections across central, state, and block levels in India.                     
## 🎯 Overview

Samadhan Bandhu provides an integrated platform for managing government-funded development projects with features for multi-level governance, tender management, payment tracking, and quality inspections through an intuitive, role-based interface.                                                                          
### Key Highlights
- 🗂 **Multi-Level Governance** - Central, State, Block, and Agency management 
- 📋 **Project Management** - Complete project lifecycle management
- ⚙ **Tender System** - Create, publish, and evaluate tenders
- 💰 **Fund Allocation** - Track fund release and allocation across levels    
- 🧾 **Payment Processing** - Secure payment approval workflow
- 🔍 **Inspections** - Field inspections with photo/GPS verification
- 📊 **Real-time Reporting** - Comprehensive dashboards and analytics
- 👥 **User Management** - Role-based access control
- 🔔 **Notifications** - Real-time updates via Socket.IO
- 📱 **Responsive Design** - Mobile-friendly interface

## 🔍 Table of Contents

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

## ⭐ Features

### 🌐 Central Government
- Create and manage projects
- Create and publish tenders
- Approve/reject payments and fund releases
- Manage user accounts across all levels
- Send system-wide broadcasts
- View comprehensive reports and statistics

### 👥 State Level
- View and manage state projects
- Create tenders for state projects
- Release funds to block level
- Approve/reject tender applications
- Monitor inspections
- Generate state-level reports

### 🏘 Block Level
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

## 🧱 Tech Stack

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
```
