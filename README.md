# Personal Trainer Management Platform

> Eine vollwertige **B2B SaaS-Lösung** für Personal Trainer zur Verwaltung von Kunden, Trainingsplänen, Ernährungsplänen und Geschäftsdokumenten.

**Live Demo:** [karsten-gerth.de](https://karsten-gerth.de/)

---

## 📋 Überblick

Diese Anwendung ist eine **End-to-End Managementsoftware** für Personal Trainer. Sie umfasst:

### **Kunde-Perspektive**

- 📱 Persönliches Dashboard mit Trainingsfortschritt
- 🏋️ Zugriff auf individualisierte Trainingspläne
- 🥗 Ernährungspläne und Mahlzeiten-Tracking
- 🎯 Smart Goals mit Verfolgung und Feedback
- 📊 Fortschrittsvisualisierung
- 💬 Kommunikation mit Trainer

### **Trainer/Admin-Perspektive**

- 👥 Vollständiges Kundenmanagement
- 📝 Intelligente Plan-Erstellung:
  - Trainingspläne (aus Vorlagen oder individuell)
  - Ernährungspläne (personalisiert)
  - Smart Goals Definition
- 📚 **Vorlagen-Bibliothek**: Wiederverwendbare Pläne & Ziele
- 🧠 Plan-Verwaltung: 1-Klick Zuweisen aus der Sammlung
- 📄 Geschäftsdokumentengenerierung:
  - Rechnungen (PDF-Export)
  - Angebote (PDF-Export)
- 📅 Terminkalender & Planung
- 📊 Echtzeit-Dashboards & Analytics

---

## 🎯 Key Features

### 1. **Kundenmanagement**

- Kundenprofile mit Kontaktinfos
- Trainingsstatus & Fortschritt
- Kommunikationsverlauf

### 2. **Intelligente Plan-Erstellung**

- **Template-System**: Vordefinierte Übungen/Ernährung
- **Personalisierung**: Anpassung an Kundenziele
- **Dokumentation**: Alle Pläne mit Details & Notizen
- **Wiederverwendung**: Gespeicherte Pläne als Vorlagen

### 3. **Smart Goals**

- SMART-Ziele-Framework Implementierung
- Goal-Tracking & Fortschritt
- Regelmäßige Überprüfungen

### 4. **Geschäftsverwaltung**

- **PDF-Generierung**: Rechnungen & Angebote
- **Buchhaltung**: Übersicht Einnahmen/Ausgaben
- **Vertragsmanagement**: Digitale Signaturen

### 5. **Kunden-Portal**

- Responsive Mobile-optimiert
- Sicherer Zugriff zu eigenen Dokumenten
- Fortschritts-Tracking

---

## 🏗️ Architektur

```
┌─────────────────────────────────────────────────────────┐
│                  FRONTEND (Next.js)                       │
├────────────────────────────────────────────────────────┤
│  Client App (responsive)  │  Trainer Admin Panel         │
│  - Dashboard              │  - Kundenmanagement         │
│  - Pläne Ansicht         │  - Plan-Editor              │
│  - Fortschritt Tracking   │  - Dokumentgenerator        │
│  - Profil                 │  - Dashboard/Analytics       │
└──────────────┬──────────────────────────────────────────┘
               │
        API Routes (Next.js)
        ├─ /api/auth (JWT)
        ├─ /api/clients
        ├─ /api/plans
        ├─ /api/documents
        └─ /api/goals
               │
┌──────────────┴──────────────────────────────────────────┐
│              BACKEND / DATABASE                          │
├────────────────────────────────────────────────────────┤
│  Supabase (PostgreSQL)                                  │
│  - Authentifizierung                                   │
│  - Datenspeicherung                                    │
│  - Real-time Sync                                      │
│                                                        │
│  Services                                              │
│  - Nodemailer (Email)                                 │
│  - Resend (Email API)                                 │
│  - PDF-Rendering (@react-pdf/renderer)               │
└────────────────────────────────────────────────────────┘
```

### Authentifizierung & Sicherheit

- **JWT-basierte Authentication** (Jose)
- **Role-based Access Control** (Trainer vs. Client)
- **Password Hashing** (@node-rs/argon2)
- **Secure Cookies** mit HTTPOnly Flag
- **Environment-based Secrets**

---

## 💻 Tech Stack

### **Frontend**

| Kategorie               | Technologie                      | Zweck                                     |
| ----------------------- | -------------------------------- | ----------------------------------------- |
| **Framework**           | Next.js 15 (App Router)          | Full-Stack React Framework mit API Routes |
| **UI Components**       | React 19                         | Component-basierte UI                     |
| **Styling**             | Tailwind CSS + Headless UI       | Modern, responsive Design                 |
| **Formulare**           | React Hook Form + Zod            | Validierung & Formularmanagement          |
| **Icons**               | Lucide React                     | UI Icons                                  |
| **PDF**                 | @react-pdf/renderer, html2pdf.js | PDF-Export & Dokumentengenerierung        |
| **Datums-Verarbeitung** | date-fns, moment                 | Datum- & Zeit-Formatting                  |
| **Rich Text**           | Quill                            | WYSIWYG Editor                            |
| **Kalender**            | react-big-calendar               | Event-Planning & Terminverwaltung         |
| **Diagramme**           | Recharts                         | Datenvisualisierung                       |
| **Animationen**         | Framer Motion                    | Smooth UI Animations                      |
| **Benachrichtigungen**  | react-toastify                   | Toast Notifications                       |
| **HTTP Client**         | SWR                              | Data Fetching & Caching                   |

### **Backend**

| Kategorie             | Technologie           | Zweck                        |
| --------------------- | --------------------- | ---------------------------- |
| **API**               | Next.js API Routes    | RESTful Endpoints            |
| **Authentifizierung** | Jose, @node-rs/argon2 | JWT & Password Hashing       |
| **Datenbank**         | Supabase (PostgreSQL) | Cloud Database mit Real-time |
| **Email**             | Nodemailer, Resend    | Email-Versand                |
| **Datenvalidierung**  | Zod                   | Schema Validation            |
| **Security**          | isomorphic-dompurify  | XSS Protection               |

### **DevOps & Build**

- **Package Manager** | npm
- **Build Tool** | Next.js (mit Turbopack)
- **Linting** | ESLint
- **Dependency Check** | depcheck

---

## 🚀 Hauptmerkmale (Detailliert)

### **1. Kunden-Dashboard**

- Überblick der aktuellen Trainingspläne
- Ernährungsplan-Info
- Progress Tracking (Gewicht, Leistung, etc.)
- Kommende Trainingstage
- Nachrichten vom Trainer

### **2. Admin-Dashboard (Trainer)**

- **KPI-Überblick**: Aktive Kunden, Pläne zugewiesen, etc.
- **Kundenübersicht**: Sortierbar, filterbar
- **Quick Actions**: Neuer Plan, neue Rechnung, etc.
- **Terminkalender**: Alle Kundentrainingstermine

### **3. Plan-Editor**

- **Drag-and-Drop** Übungen hinzufügen
- **Sets/Reps** Konfiguration
- **Notizen & Anweisungen** pro Übung
- **Speichern als Vorlage** für Wiederverwendung
- **Client Assignment**: Plan direkt zuweisen

### **4. Template-Bibliothek**

- **Trainings-Vorlagen**: Nach Ziel kategorisiert (Muskelaufbau, Ausdauer, etc.)
- **Ernährungs-Vorlagen**: Nach Ernährungstyp (Massephase, Diät, etc.)
- **Smart Goals Vorlagen**: Vordefinierte Ziele
- **Schnelle Zuordnung**: Vorlagen in 1 Klick zuweisen

### **5. Dokumentengenerierung**

- **Rechnungen**:
  - Automatische Nummerierung
  - Kundendetails
  - Leistungsbeschreibung
  - Gesamtsumme & Zahlungsbedingungen
  - PDF-Export & Download
- **Angebote**:
  - Ähnlich Rechnung
  - Status-Tracking (angeboten, akzeptiert, abgelehnt)
  - Gültigkeit-Datum

### **6. Kundenzugang**

- Responsive Mobile App
- Sicherer Login
- Zugriff nur auf eigene Pläne/Ziele
- Fortschritt-Loggen

---

## 📊 Screenshots & Demo-Flow

> [Screenshots/GIFs würden hier eingefügt]

### **Trainer Workflow Beispiel:**

1. Neuer Kunde kommt rein → Profil erstellen
2. Trainer wählt aus Template-Bibliothek → Trainingsplan
3. Plan wird personalisiert (Übungen angepasst)
4. Plan wird dem Kunden zugewiesen
5. Kunde sieht Plan auf seinem Dashboard
6. Nach 4 Wochen: Rechnung erstellen & senden
7. Plan-Feedback: Neuer Plan aus Template (mit Erfahrung)

---

## 🔐 Sicherheit & Best Practices

✅ **Authentifizierung**

- JWT Token-basiert
- Secure HTTP-only Cookies
- Token Refresh Mechanism

✅ **Autorisierung**

- Role-based Access Control (Trainer/Client)
- Row-level Security in Supabase
- Middleware für Protected Routes

✅ **Datenschutz**

- HTTPS enforced
- XSS Protection (DOMPurify)
- Password Hashing (Argon2)
- Environment Variables für Secrets

✅ **Validierung**

- Server-side Zod Validation
- Client-side Fehlerbehandlung
- Input Sanitization

---

## 📈 Metriken & Performance

- **Lighthouse Score**: 90+ (Performance, Accessibility)
- **Page Load**: < 2s (optimiert mit Next.js)
- **API Response**: < 500ms (Supabase optimiert)
- **Mobile Responsive**: 100% responsive Design

---

## 🛠️ Setup & Entwicklung

### Voraussetzungen

- Node.js 18+
- npm oder yarn
- Supabase Account (kostenlos)

### Installation

```bash
# Repo klonen
git clone <repo-url>
cd <project-folder>

# Dependencies installieren
npm install

# Environment Variablen setzen (.env.local)
# NEXT_PUBLIC_SUPABASE_URL=...
# NEXT_PUBLIC_SUPABASE_ANON_KEY=...
# etc.

# Development Server starten
npm run dev
# Öffne http://localhost:3000
```

### Build & Deployment

```bash
npm run build
npm run start
```

---

## 📚 Wichtige Code-Struktur

```
app/
├── (admin)/              # Trainer Admin Routes
│   ├── dashboard/
│   ├── clients/
│   ├── plans/
│   ├── documents/
│   └── templates/
├── (customers)/          # Kunde Routes
│   ├── dashboard/
│   ├── plans/
│   └── goals/
├── (public)/             # Public Pages (Login, etc.)
├── api/                  # Next.js API Routes
│   ├── auth/
│   ├── clients/
│   ├── plans/
│   ├── documents/
│   └── goals/
└── _components/          # Shared Components
```

---

## 🎓 Lessons Learned & Highlights

### Was diese App zeigt:

✅ **Full-Stack Development**: Frontend, Backend, Database, Authentication
✅ **SaaS-Pattern**: Multi-user System mit Role-based Access
✅ **Complex State Management**: Pläne, Templates, Dokumentation
✅ **Real-world Features**: PDF-Export, Email, File Management
✅ **Best Practices**: Security, Performance, User Experience
✅ **Modern Stack**: Next.js 15, React 19, TypeScript (strukturiert)

### Technische Highlights:

- Server-side Rendering für Performance
- API Routes für Backend-Logic
- Supabase für Real-time Datenbank
- Middleware für Authentication
- PDF-Generation Client & Server-side
- Email-Integration (Nodemailer + Resend)
- Responsive Design (Mobile-first)

---

## 📞 Kontakt & Portfolio

**GitHub**: [(https://github.com/MiriamS284/MiriamS284)]
**LinkedIn**: [(https://www.linkedin.com/in/miriam-sparbrod/)]

---

## 📄 Lizenz

Dieses Projekt ist proprietär und wird nicht open-sourced. Der Code wird nur zu Demo-Zwecken mit potenziellen Arbeitgebern/Clients geteilt.

---

**Erstellt von**: Miriam | Full-Stack MERN Developer
**Technologie**: Next.js, React, Supabase, Tailwind CSS
**Typ**: SaaS / Business Management Software
