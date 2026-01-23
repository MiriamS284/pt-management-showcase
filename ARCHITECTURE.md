# Technische Architektur

## System-Übersicht

┌─────────────────────────────────────────────────────────────┐
│ VERCEL (Frontend Deploy) │
├─────────────────────────────────────────────────────────────┤
│ Next.js 15 App Router │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ React 19 Components + Hooks │ │
│ │ - (admin) Trainer Dashboard & Management │ │
│ │ - (customers) Client Portal │ │
│ │ - (public) Auth & Static Pages │ │
│ └─────────────────────────────────────────────────────────┘ │
│ ↓ │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ Next.js API Routes (/api) │ │
│ │ - Authentication (JWT, Argon2) │ │
│ │ - Data CRUD Operations │ │
│ │ - Document Generation (PDF) │ │
│ │ - Email Services (Nodemailer, Resend) │ │
│ └─────────────────────────────────────────────────────────┘ │
└──────────────────────────┬───────────────────────────────────┘
│
HTTPS / REST API
│
┌──────────────────────────┴───────────────────────────────────┐
│ Supabase (Backend) │
├───────────────────────────────────────────────────────────────┤
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ PostgreSQL Database + PostgREST API │ │
│ │ ┌───────────────────────────────────────────────────┐ │ │
│ │ │ Tables: │ │ │
│ │ │ - users (Authentifizierung) │ │ │
│ │ │ - profiles (Trainer/Client Details) │ │ │
│ │ │ - clients (Kundendatenbank) │ │ │
│ │ │ - training_plans (Trainingspläne) │ │ │
│ │ │ - nutrition_plans (Ernährungspläne) │ │ │
│ │ │ - exercises (Übungs-Bibliothek) │ │ │
│ │ │ - smart_goals (Ziele) │ │ │
│ │ │ - documents (Rechnungen, Angebote) │ │ │
│ │ │ - plan_templates (Vorlagen) │ │ │
│ │ └───────────────────────────────────────────────────┘ │ │
│ └─────────────────────────────────────────────────────────┘ │
│ │ │
│ ┌─────────────────────────┴──────────────────────────────┐ │
│ │ Authentication & Authorization (Row-Level) │ │
│ │ - Policies für Trainer vs. Client Access │ │
│ │ - Role-based Column Masking │ │
│ └────────────────────────────────────────────────────────┘ │
│ │ │
│ ┌─────────────────────────┴──────────────────────────────┐ │
│ │ Real-time Features (optional, future use) │ │
│ │ - Live Collaboration on Plans │ │
│ └────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────┘
│
┌──────┴──────┐
│ │
┌──────────▼──┐ ┌─────▼─────────┐
│ Nodemailer │ │ Resend API │
│ (SMTP Email) │ │ (Email API) │
└──────────────┘ └───────────────┘

## 📊 Datenbank-Schema (Vereinfacht)

### **Core Tabellen**

sql
-- Authentifizierung & User Management
users {
id: UUID PRIMARY KEY
email: VARCHAR UNIQUE
password_hash: VARCHAR (Argon2)
role: ENUM('trainer', 'client')
created_at: TIMESTAMP
}

-- Trainer Profil
trainer_profiles {
id: UUID PRIMARY KEY
user_id: UUID FK → users
name: VARCHAR
specialization: TEXT
phone: VARCHAR
subscription_tier: ENUM('basic', 'pro', 'premium')
}

-- Client Daten
clients {
id: UUID PRIMARY KEY
trainer_id: UUID FK → trainer_profiles
name: VARCHAR
email: VARCHAR
phone: VARCHAR
age: INT
weight: DECIMAL
height: DECIMAL
goals: TEXT
created_at: TIMESTAMP
}

-- Trainingspläne
training_plans {
id: UUID PRIMARY KEY
client_id: UUID FK → clients
trainer_id: UUID FK → trainer_profiles
title: VARCHAR
description: TEXT
start_date: DATE
end_date: DATE
plan_template_id: UUID FK → plan_templates (nullable)
is_template: BOOLEAN
created_at: TIMESTAMP
}

-- Plan Items (Übungen)
plan_exercises {
id: UUID PRIMARY KEY
plan_id: UUID FK → training_plans
exercise_id: UUID FK → exercises
order: INT
sets: INT
reps: INT
weight: DECIMAL (nullable)
notes: TEXT
}

-- Übungs-Bibliothek
exercises {
id: UUID PRIMARY KEY
name: VARCHAR
description: TEXT
category: ENUM('chest', 'back', 'legs', etc.)
muscle_groups: TEXT[]
difficulty: ENUM('beginner', 'intermediate', 'advanced')
image_url: VARCHAR (nullable)
}

## 🔐 Sicherheits-Architektur

### **Authentication Flow**

Client sendet email + password
↓
/api/auth/login (POST)
↓
Server-side: Validation, Email Lookup, Password Verify
↓
JWT Token generiert
↓
Response: Token in HttpOnly Cookie
↓
Middleware bei jedem Request: Token Verification
↓
Token Valid? → Request erlaubt

### **Authorization (Role-based)**

Trainer Routes (/admin/_): role === 'trainer'
Client Routes (/customers/_): role === 'client'
Row-level Security: Trainer sieht nur eigene Clients

---

## 🚀 Deployment-Architektur

Local Development (npm run dev)
↓
Git Push → GitHub
↓
Vercel Auto-Deploy
↓
Vercel Edge Network (karsten-gerth.de)
↓
Supabase (Backend)

---

## 💡 Performance Optimierungen

- **SWR Client-side**: Automatic Deduplication & Revalidation
- **Next.js Caching**: ISR für Templates
- **Code Splitting**: Dynamic Imports
- **Indexes**: Auf häufig gefilterten Spalten

---

**Technische Highlights:**

- Server-side Rendering für Performance
- API Routes für Backend-Logic
- JWT Authentication mit Argon2 Password Hashing
- Row-level Security in Supabase
- PDF-Generation Client & Server-side
- Email-Integration (Nodemailer + Resend)
- Responsive Design (Mobile-first)
