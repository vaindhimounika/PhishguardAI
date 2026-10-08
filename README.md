# PhishGuard AI

A responsive, multilingual phishing-link analysis application built with React, TypeScript, Vite, Express, and PostgreSQL/Prisma. Scan history deliberately stores the hostname, bounded risk score, warning summary, privacy preference, redirect indication, and date—not the submitted URL or its query parameters.

## Requirements

- Node.js 22.12 or newer
- PostgreSQL 14 or newer

## Local development

1. Install packages: `npm install`.
2. Create `.env` from `.env.example` and set `DATABASE_URL`, `CLIENT_ORIGIN`, and `PORT`.
3. Prepare the database: `npm run db:generate` and `npm run db:migrate`.
4. Run both services: `npm run dev`.
5. Open the Vite URL printed in the terminal (usually `http://localhost:5173`).

The Vite development server proxies `/api` requests to Express. The API listens on port `3001` by default. Generate and apply the Prisma migration before scanning; scan history requires the configured PostgreSQL database.

## Production build

Run `npm run build`, apply database migrations with `npx prisma migrate deploy`, then run the API with `npm start`. Serve the built `dist/` frontend with a static web server configured to send `/api` requests to the API. Set `CLIENT_ORIGIN` to that exact frontend origin.

## What analysis does—and does not do

The server examines URL structure, HTTPS use, domains and subdomains, IP-address hosts, suspicious terms, uncommon TLDs, known shorteners, IDN/punycode hostnames, possible brand impersonation, lookalike spellings, and embedded redirect parameters. Individual signals have bounded weights, and no single weak signal can produce a dangerous score. Results are explainable heuristics, not a guarantee of safety or a substitute for independent verification.

Scanned links are treated as text. The application does **not** visit submitted websites, execute site code, resolve short links, follow redirects, or call an external reputation service. A recognized redirect is marked unresolved; no destination is inferred. External reputation lookups are intentionally disabled, so scan URLs are not sent to third parties.

## Privacy and language

English, Telugu, and Hindi are stored in separate JSON translation files. The selected language is kept in browser local storage. The database retains a hostname rather than the full address, including no path, query string, passwords, cookies, or tokens.

## Validation

- `npm test` — URL-analysis unit tests
- `npm run build` — frontend and backend TypeScript checks and production frontend build
