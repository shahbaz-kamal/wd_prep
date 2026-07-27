# 9. Security

- **XSS** — injecting script into pages. Fix: output encoding, CSP, sanitize input. Stored / reflected / DOM-based.
- **CSRF** — forcing an authenticated user's browser to make a request. Fix: anti-CSRF tokens, SameSite cookies.
- **SQL injection** — untrusted input in a query. Fix: parameterized queries / prepared statements. (ORMs like Prisma parameterize by default; raw queries do not.)
- **Hashing vs encryption** — hashing is one-way (bcrypt, argon2, SHA-256); encryption is reversible with a key.
- **Symmetric** (AES, one shared key, fast) vs **asymmetric** (RSA, public/private key pair).
- **Salt** — random per-user value added before hashing, defeats rainbow tables.
- **TLS handshake** — asymmetric crypto to exchange a symmetric session key, then symmetric for the session. Certificates signed by a CA.
- **Authentication** = who you are. **Authorization** = what you may do.
- **JWT** — header.payload.signature, base64url encoded. Stateless, signed not encrypted (never put secrets in the payload).
- **Sessions** vs JWT — server-side state and easy revocation vs stateless scaling and hard revocation.
- **CORS** — browser same-origin policy relaxation via response headers. Preflight OPTIONS for non-simple requests.
- **OWASP Top 10** themes: broken access control, cryptographic failures, injection, insecure design, misconfiguration, vulnerable components, auth failures, integrity failures, logging failures, SSRF.
- **Principle of least privilege**, **defense in depth**, **rate limiting**, **MITM attack**, **DDoS**.
