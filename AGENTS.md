You are helping me build SRAC (Student Result Analysis & Collection), a web app for teachers at a VTU-affiliated engineering college (BIT, Mangalore).

PROBLEM: After VTU semester results, teachers manually type every student's name, USN, subject marks, grades and SGPA into a class report. It takes ~7 days. SRAC lets a teacher create a Batch with a USN range (e.g. 4BP25CS001 to 4BP25CS051), fetches each student's result, and generates an Excel + PDF report with analytics.

STACK: Monorepo. Frontend: React + TypeScript + Vite + Tailwind + shadcn/ui + TanStack Query + Recharts. Backend: Node.js + Express + TypeScript, Zod validation, Prisma + SQLite (hackathon), in-memory job queue, Playwright for fetching, Cheerio for parsing, ExcelJS for xlsx, Socket.io for realtime. Tests with Vitest.

RULES:
- TypeScript strict mode. No `any` unless justified in a comment.
- Never hardcode the VTU results URL, grading tables, or credits. They come from .env or the database.
- Never silently accept bad data: validate every parsed result and flag failures.
- Do not implement automated CAPTCHA bypass. Use the CaptchaProvider interface with a ManualWebSocketProvider as default.
- Keep concurrency low (2-3) and add delays when contacting the VTU portal.
- Every manual edit to a result writes an AuditLog row.
- Write unit tests for USN expansion, SGPA calculation, and the parser (using saved HTML fixtures).
- After each task: run lint, typecheck and tests, then summarize what changed and what is left.
- Ask me before adding any dependency not listed above.
