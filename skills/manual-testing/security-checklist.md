# Manual Security Testing Checklist (OWASP Top 10 — Basic Checks)

These are basic manual checks a tester can perform without specialized security tools.
For in-depth security testing, engage a dedicated security engineer.

---

## A01: Broken Access Control

- [ ] Change the user ID in URLs (`/users/123/profile` → `/users/124/profile`) — can you see another user's data?
- [ ] Remove the authentication token from API requests — do they still succeed?
- [ ] Try accessing admin pages as a regular user (`/admin`, `/dashboard/admin`)
- [ ] Test that role-based restrictions work (viewer can't edit, editor can't delete, etc.)
- [ ] After logging out, use the browser back button — can you still see protected pages?
- [ ] Check that API endpoints enforce the same permissions as the UI

## A02: Cryptographic Failures

- [ ] Sensitive pages (login, payment, profile) served over HTTPS only
- [ ] HTTP redirects to HTTPS (not accessible over plain HTTP)
- [ ] Passwords are not visible in URL parameters or server logs
- [ ] Sensitive data (SSN, credit card) masked in UI after entry
- [ ] "Forgot password" sends a reset link — not the actual password

## A03: Injection

- [ ] Enter `' OR '1'='1` in login fields — does it bypass authentication?
- [ ] Enter `<script>alert('XSS')</script>` in text fields — does it execute?
- [ ] Enter `../../../etc/passwd` in file path fields
- [ ] Enter `; DROP TABLE users;--` in search fields
- [ ] Check that user input displayed on screen is HTML-encoded (shows `&lt;` not `<`)

## A05: Security Misconfiguration

- [ ] Check response headers for security headers: `X-Content-Type-Options`, `X-Frame-Options`, `Content-Security-Policy`
- [ ] Error pages don't show stack traces, file paths, or version information
- [ ] Default admin credentials don't work (`admin/admin`, `admin/password`)
- [ ] Directory listing disabled (browsing to `/images/` shows 403, not a file list)

## A07: Authentication Failures

- [ ] Is there a limit on failed login attempts (account lockout or CAPTCHA)?
- [ ] Are passwords required to meet minimum complexity?
- [ ] Does "Remember me" work for a reasonable duration (not forever)?
- [ ] Session tokens change after login (session fixation prevention)
- [ ] Logout actually invalidates the session (token doesn't work after logout)

## A09: Security Logging & Monitoring

- [ ] Failed login attempts should be logged (verify with team)
- [ ] Sensitive operations (password change, payment, data deletion) should be logged

---

## What NOT to Test (Without Authorization)
- Do not attempt denial-of-service attacks
- Do not brute-force passwords against production systems
- Do not run automated scanners without explicit permission
- Do not access or exfiltrate real user data
