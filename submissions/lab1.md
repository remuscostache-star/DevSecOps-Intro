# Lab 1 — Foundations & SDLC

## Triage report

### Asset
- **Image tag:** `bkimminich/juice-shop:v20.0.0`
- **Image digest:** `sha256:fd58bdc9745416afce8184ee0666278a436574633ea7880365153a63bfd418b0`
- **Host OS:** `Linux 6.8.0-1070-azure` (GitHub Codespaces)
- **Docker version:** `Docker version 29.9.2-2, build a82e528901260c8df6073311379eac65f92db204`

### Deployment
- **Run command:** `docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop:v20.0.0`
- **Access URL:** `http://127.0.0.1:3000`
- **Port binding & why it matters:** Yes, the port is bound to `127.0.0.1` (localhost only). This matters because OWASP Juice Shop is an intentionally vulnerable application; binding it only to localhost prevents external attackers on the network or internet from accessing and exploiting it.
- **Restart policy:** `no` (default policy; the container does not automatically restart if it crashes or the host reboots).

### Health
- **HTTP code on `/`:** `200 OK`
- **Application version:** `v20.0.0`
- **Product count (`/api/Products`):** `46`
- **`docker ps` output:**
  ```text
  5db5d1385d95   bkimminich/juice-shop:v20.0.0   "/nodejs/bin/node /j…"   Up   127.0.0.1:3000->3000/tcp   juice-shop
  ```

### Surface
- **Login and registration:** Accessible via the top-right Account menu; anyone can register a new user account without email verification or strict password complexity enforcement.
- **Products:** All 46 products, their descriptions, and customer reviews (`/api/Products/<id>/reviews`) are publicly visible and accessible via API without authentication.
- **Admin or account area:** No direct admin link is exposed in the public navigation bar for unauthenticated users, though hidden administrative and challenge routes exist in the application.
- **Console errors:** No blocking runtime errors on initial page load, though browser developer tools expose client-side routing and API calls clearly.
- **Local storage and cookies:** Initial session cookies (such as language preference `language` and `welcomebanner_status`) and local storage entries are pre-populated upon visiting the storefront.

### Headers
- **`curl -sI` output:**
  ```text
  HTTP/1.1 200 OK
  Content-Type: text/html; charset=UTF-8
  Content-Length: 9903
  Vary: Accept-Encoding
  Date: Tue, 17 Nov 2026 20:26:15 GMT
  Connection: keep-alive
  Keep-Alive: timeout=5
  ```
- **Missing security headers:** All four checked security headers are missing (`Content-Security-Policy`, `Strict-Transport-Security`, `X-Content-Type-Options`, and `X-Frame-Options`).

### Top 3 risks
1. **Missing HTTP Security Headers (OWASP Top 10:2025 — A02: Security Misconfiguration)**
   The server omits `Content-Security-Policy`, `Strict-Transport-Security`, `X-Content-Type-Options`, and `X-Frame-Options`. Without these browser defenses, users are exposed to clickjacking, MIME-sniffing, and cross-site scripting (XSS) attacks, and traffic is not forced over encrypted HTTPS.
2. **Unrestricted Account Registration & Weak Authentication Controls (OWASP Top 10:2025 — A07: Authentication Failures)**
   Anyone can register an account immediately without email verification or strong password policies. This allows attackers to automate fake account creation, enumerate users, or attempt credential stuffing attacks against customer accounts.
3. **Unauthenticated API Exposure & Hidden Administrative Routes (OWASP Top 10:2025 — A01: Broken Access Control)**
   Product and review API endpoints return detailed data without requiring authentication, and sensitive routes rely on obscurity rather than strict server-side access checks. If administrative endpoints lack proper authorization enforcement, attackers can discover and access privileged functions
## PR template
- **File path:** `.github/PULL_REQUEST_TEMPLATE.md`
- **Required sections:** `Goal`, `Changes`, `Testing`, `Artifacts & Screenshots`
- **Checklist items:**
  - `Title follows feat(labN): <topic>`
  - `No secrets or large temp files committed`
  - `submissions/labN.md exists`
- **Draft PR URL:** https://github.com

## GitHub community
Stars act as a public trust signal and feedback metric that help open-source maintainers measure adoption, attract contributors, and justify continued development. Following teammates and maintainers keeps everyone informed of repository activity, code reviews, and upstream updates in collaborative team projects.
