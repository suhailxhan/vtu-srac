# SRAC — Student Result Analysis & Collection

SRAC is a web application designed for VTU-affiliated engineering colleges (built for BIT, Mangalore) to automate semester result collection, verification, SGPA computation, and automated report generation (Excel + PDF) in under 15 minutes instead of 7 days of manual typing.

---

## 🚀 Quick Start

### Prerequisites
- **Node.js**: v18+ (tested on v22 & v26)
- **npm**: v9+

### 1. Installation
From the repository root:
```bash
npm install
```

### 2. Database Setup
The hackathon demo uses **SQLite** (zero external database configuration required).
```bash
npm run db:migrate
npm run db:seed
```

This seeds default accounts:
- **Admin**: `admin@srac.dev` / `admin123`
- **Teacher**: `teacher@srac.dev` / `teacher123`
- **Scheme**: VTU 2022 Scheme with official grading table and sample CSE 3rd semester course credits.

### 3. Start Development Server
```bash
npm run dev
```
This runs both:
- **API Server** on `http://localhost:3001`
- **Vite Web App** on `http://localhost:5173`

---

## 🧑‍🏫 User Roles & Features

### 1. Teacher Workflow
1. **New Batch Wizard**:
   - **Step 1**: Department (CSE), Semester (3), Scheme (2022), Exam Name (`Dec 2025 / Jan 2026`).
   - **Step 2**: USN Prefix (`4BP25CS`), Start (`001`), End (`051`), lateral entries (extras), and detained (skips).
   - **Live Preview**: See every generated USN validated against `/^[1-4][A-Z]{2}\d{2}[A-Z]{2,3}\d{3}$/`.
   - **Step 3**: Confirm batch.
2. **Fetch Pipeline with Human-in-the-Loop CAPTCHA**:
   - Concurrency capped at 2–3 with randomized 1–3s delays to respect VTU servers.
   - When a CAPTCHA is required, a real-time modal pops up over WebSocket with the CAPTCHA image and auto-focused text field.
   - Live progress indicator tracks done, failed, needs captcha, and remaining.
3. **Review & Manual Correction**:
   - Sortable table of student results with subject-wise marks (internal/external/total/grade) and computed SGPA.
   - Inline manual edits automatically compute updated grades and write to `AuditLog`.
4. **Report Generation**:
   - **Excel Report (`.xlsx`)**: 5 formatted sheets (Class Result, Backlogs, Subject Analysis, Toppers Top 10, Batch Summary) with frozen headers and color-coded fails.
   - **PDF Report**: College-formatted landscape report with class table and sign-off blocks.
   - **Analytics Tab**: Interactive charts powered by Recharts (Grade distribution, subject pass rates, SGPA histogram, rankers).
5. **Exam Cell Excel/CSV Import**:
   - Import official result files directly to bypass portal fetching entirely.

### 2. Admin Capabilities
- User management (Create, update, delete teachers and HODs).
- Scheme & Course Credit management (Create new schemes, grading rules, and course codes).
- View audit logs and system activity.

---

## 🛠️ Tech Stack

- **Monorepo**: npm workspaces
- **Frontend**: React 19, TypeScript, Vite, Tailwind CSS, shadcn/ui, TanStack Query, Recharts, Socket.io client, Lucide icons
- **Backend**: Node.js, Express 5, TypeScript, Zod, Prisma ORM, SQLite (local demo), Socket.io, ExcelJS, Puppeteer, Cheerio, Argon2, JWT (httpOnly cookies)
- **Testing**: Vitest with unit tests for USN expansion, SGPA calculation, and Cheerio parser fixtures

---

## 🧪 Testing & Verification

Run backend unit tests:
```bash
npm run test
```

Run TypeScript verification across all workspaces:
```bash
npm run typecheck
```

Build production bundles:
```bash
npm run build
```

---

## 🔄 Adapting for Production

For production deployment:
1. Update `apps/api/prisma/schema.prisma` datasource provider from `"sqlite"` to `"postgresql"`.
2. Configure `DATABASE_URL` and `REDIS_URL` in `apps/api/.env`.
3. Start Redis and PostgreSQL using `docker-compose up -d`.
4. Deploy using Dockerfiles or your chosen cloud provider.
