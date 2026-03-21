# SkillShare — Learning Management System

 A full-stack Learning Management System (LMS) built with Spring Boot, React, and PostgreSQL featuring role-based access control, JWT authentication, and a responsive Material UI interface.

## Table of Contents

- [About the Project](#-about-the-project)
- [Tech Stack](#-tech-stack)
- [Features](#-features)
- [Architecture & Design](#-architecture--design)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)

## About the Project

**SkillShare** is a full-stack LMS platform that allows organizations to manage online courses and learners efficiently. The platform supports three user roles — **Super Admin**, **Admin (Instructor)**, and **Student** — each with dedicated dashboards and access-controlled APIs.

Key highlights:
- Stateless JWT-based authentication with **access token + refresh token** flow
- Role-based authorization using Spring Security's `@PreAuthorize`
- Email notification support via Spring Mail (SMTP)
- Clean layered architecture: Controller → Service → Repository → Entity
- React + Vite SPA frontend with React Router v6 and Material UI v6

## Tech Stack

### Backend
| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 21 | Core language |
| Spring Boot | 3.3.2 | Application framework |
| Spring Security | 6.x | Authentication & Authorization |
| Spring Data JPA | 3.3.2 | ORM & Database access |
| Spring Mail | 3.3.2 | Email notifications |
| JJWT | 0.12.5 | JWT token generation & validation |
| PostgreSQL | Latest | Relational database |
| Lombok | Latest | Boilerplate reduction |
| Maven | 3.x | Build tool |

### Frontend
| Technology | Version | Purpose |
|------------|---------|---------|
| React | 18.3.1 | UI library |
| Vite | 5.4.8 | Build tool & dev server |
| React Router DOM | 6.27 | Client-side routing |
| Material UI (MUI) | 6.1.3 | UI component library |
| Axios | 1.7.7 | HTTP client |
| React Hook Form | 7.53 | Form management |

## Features

### Authentication & Authorization
- User **Registration & Login** with JWT tokens
- **Access token** (short-lived) + **Refresh token** (long-lived) strategy
- **Automatic token refresh** on expiry — seamless UX
- Username uniqueness check before registration
- Secure **logout** that invalidates refresh tokens
- Custom `JwtAuthenticationEntryPoint` returning structured JSON `401` errors

### Super Admin
- Register and manage **Admin accounts**
- View, activate/deactivate any user
- Delete users from the system
- Full user listing with role info

### Admin (Instructor)
- Create, update, and delete **courses**
- Add and manage **lessons** within courses
- Toggle course **active/inactive** status
- View courses they personally manage

### Student
- Browse all available courses
- Enroll in / view assigned courses
- Access course lessons
- Personalized student dashboard

### System
- Auditing:— all entities track `createdAt`, `updatedAt`, `createdBy`, `modifiedBy` automatically
- Global exception handling:- for clean API error responses
- Dev/Prod profile:- switching via Maven profiles

## Architecture & Design
```
skill-share/
├── backend/    ← Spring Boot 3 REST API
└── frontend/   ← React 18 + Vite SPA
```
### Backend Layered Architecture
```
HTTP Request
     │
     ▼
┌─────────────┐
│  Controller │  ← REST endpoints, request/response mapping
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Service   │  ← Business logic, transaction management
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Repository  │  ← Spring Data JPA interfaces
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Entity    │  ← JPA-mapped domain objects
└─────────────┘
```

## Getting Started

### Prerequisites

- Java 21+
- Maven 3.8+
- Node.js 18+ & npm
- PostgreSQL (running locally on port `5432`)

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/skill-share.git
cd skill-share
```
### 2. Backend Setup
```bash
cd backend

# Create the PostgreSQL database (default: testing2)
psql -U postgres -c "CREATE DATABASE testing2;"

# Run with the dev profile (default)
mvn spring-boot:run -Pdev
```
The backend starts at `http://localhost:8080`

### 3. Frontend Setup
```bash
cd frontend
npm install
npm run dev
```
The frontend starts at `http://localhost:5173` and proxies API calls to `http://localhost:8080`.

## Project Structure
```
skill-share/
│
├── backend/
│   └── src/main/java/com/Final/Project/
│       ├── Auditor/          # JPA auditing configuration
│       ├── Filter/           # JWT request filter
│       ├── bootstrap/        # Data seeding (roles, super admin)
│       ├── config/           # Security config, entry point
│       ├── controller/       # REST controllers (Auth, Course, Lesson, Admin, Student)
│       ├── dto/              # Data Transfer Objects
│       ├── entity/           # JPA entities (Users, Course, Lesson, Roles, Tokens)
│       ├── exception/        # Custom exceptions & global handler
│       ├── mapper/           # Entity ↔ DTO mappers
│       ├── repository/       # Spring Data JPA repositories
│       ├── service/          # Business logic services
│       └── utils/            # JWT utility class
│
└── frontend/
    └── src/
        ├── components/       # Reusable components (Dashboard, CoursesList, UserList)
        ├── pages/
        │   ├── login/        # Login page
        │   ├── register/     # Registration page
        │   ├── dashboards/   # Admin & Super Admin dashboards
        │   ├── courseDashboard/  # Course management views
        │   └── studentDashboard/ # Student view
        ├── App.jsx           # Root component with routes
        └── main.jsx          # React entry point`
```
