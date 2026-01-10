---
name: security-standards
description: Security validation patterns, secrets management, and OWASP considerations. Use when handling user input, authentication, secrets, or any security-sensitive code.
---

# Security Standards

Security is not optional. Every change must consider security implications.

---

## Data Sensitivity Levels

| Level | Description | Examples | Handling |
|-------|-------------|----------|----------|
| **Critical** | Severe harm if exposed | Passwords, API keys, payment data | Encrypt, never log |
| **Sensitive** | Significant harm | PII, emails, phone numbers | Encrypt at rest, mask in logs |
| **Internal** | Business-sensitive | Analytics, metrics | Access controls |
| **Public** | No harm | Marketing, public docs | Standard handling |

---

## Secrets Management

### Never Commit Secrets

Absolutely forbidden in code:
- API keys
- Passwords
- Database credentials
- Encryption keys
- OAuth tokens
- Private certificates
- Connection strings with credentials

### How to Handle Secrets

```
DO:
- Use environment variables
- Use secrets management (Vault, AWS Secrets Manager)
- Use .env files (gitignored)
- Use configuration management

DON'T:
- Hardcode in source files
- Commit to git (even "test" branches)
- Log secrets
- Include in error messages
- Send in URLs (query parameters)
```

### If Secrets Are Accidentally Committed

1. **Immediately rotate** the exposed secret
2. **Remove from git history**:
   ```bash
   git filter-branch --force --index-filter \
     "git rm --cached --ignore-unmatch PATH_TO_FILE" \
     --prune-empty --tag-name-filter cat -- --all
   ```
3. **Force push** (coordinate with team)
4. **Audit** for unauthorized access
5. **Document** the incident

---

## Input Validation

### Validate All External Input

External input includes:
- User form submissions
- API request parameters
- File uploads
- URL parameters
- Headers and cookies
- Database content (if externally populated)

### Validation Rules

```python
# GOOD: Whitelist validation
ALLOWED_TYPES = ["image/png", "image/jpeg"]
if file.content_type not in ALLOWED_TYPES:
    raise ValidationError("Invalid file type")

# BAD: Blacklist validation (incomplete!)
FORBIDDEN_TYPES = ["application/x-executable"]
if file.content_type in FORBIDDEN_TYPES:
    raise ValidationError("Invalid file type")
```

### Common Validations

| Input Type | Validations |
|------------|-------------|
| Strings | Length limits, allowed characters, format |
| Numbers | Range, type, sign |
| Emails | Format, domain allowlist if applicable |
| URLs | Protocol (https only), domain allowlist |
| Files | Type, size, content validation |
| IDs | Format, existence, authorization |

---

## Injection Prevention

### SQL Injection

```python
# NEVER: String concatenation
query = f"SELECT * FROM users WHERE id = {user_id}"

# ALWAYS: Parameterized queries
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))

# ALWAYS: ORM methods
User.objects.filter(id=user_id)
```

### Command Injection

```python
# NEVER: Shell with user input
os.system(f"convert {user_filename} output.png")

# BETTER: Avoid shell entirely
subprocess.run(["convert", user_filename, "output.png"], shell=False)

# BEST: Validate and sanitize
if not re.match(r'^[\w\-\.]+$', user_filename):
    raise ValidationError("Invalid filename")
```

### XSS Prevention

```html
<!-- NEVER: Raw user content -->
<div>{{ user_content | safe }}</div>

<!-- ALWAYS: Escaped by default -->
<div>{{ user_content }}</div>

<!-- For HTML: Sanitize first -->
<div>{{ user_content | sanitize_html }}</div>
```

### Path Traversal

```python
# NEVER: Direct path concatenation
file_path = os.path.join(BASE_DIR, user_input)

# ALWAYS: Validate and resolve
safe_path = os.path.realpath(os.path.join(BASE_DIR, user_input))
if not safe_path.startswith(os.path.realpath(BASE_DIR)):
    raise SecurityError("Path traversal attempt")
```

---

## Authentication & Authorization

### Authentication Requirements

- [ ] Passwords hashed with bcrypt/argon2
- [ ] Password requirements enforced
- [ ] Rate limiting on login attempts
- [ ] Session tokens random and unpredictable
- [ ] Sessions expire appropriately
- [ ] Secure cookie flags (HttpOnly, Secure, SameSite)

### Authorization Requirements

- [ ] Every endpoint checks authorization
- [ ] Authorization checked server-side (not just client)
- [ ] Principle of least privilege
- [ ] Resource access verified (user can access THIS resource)
- [ ] Admin functions protected

### Authorization Pattern

```python
# BAD: Only checks if logged in
@login_required
def view_document(request, doc_id):
    doc = Document.objects.get(id=doc_id)
    return render(request, 'doc.html', {'doc': doc})

# GOOD: Checks ownership/permission
@login_required
def view_document(request, doc_id):
    doc = Document.objects.get(id=doc_id)
    if not doc.can_view(request.user):
        raise PermissionDenied()
    return render(request, 'doc.html', {'doc': doc})
```

---

## Cryptography

### Use Standard Libraries

```
DO: Use well-tested libraries
- Python: cryptography, bcrypt
- Node: crypto (built-in), bcrypt
- Go: crypto (standard library)

DON'T:
- Implement your own crypto
- Use deprecated algorithms (MD5, SHA1)
- Use ECB mode
- Hardcode IVs or salts
```

### Algorithm Recommendations

| Purpose | Recommended | Avoid |
|---------|-------------|-------|
| Password hashing | bcrypt, argon2 | MD5, SHA1, plain SHA256 |
| Symmetric encryption | AES-256-GCM | DES, 3DES, AES-ECB |
| Asymmetric | RSA-2048+, ECDSA | RSA-1024 |
| Hashing (non-password) | SHA-256, SHA-3 | MD5, SHA1 |
| Random generation | Cryptographic RNG | Math.random, rand() |

---

## Logging Security

### What to Log
- Authentication events (success and failure)
- Authorization failures
- Input validation failures
- System errors
- Admin actions

### What NOT to Log
- Passwords (even hashed)
- API keys or tokens
- Credit card numbers
- Full PII (mask it)
- Session tokens

```python
# BAD
logger.info(f"Login for {email} with password {password}")

# GOOD
logger.info(f"Login attempt for user_id={user.id} from ip={request.ip}")

# GOOD: Masked
logger.info(f"Processing card ending in {card_number[-4:]}")
```

---

## Security Checklist

### For Every Change
- [ ] No secrets in code
- [ ] Input validation present
- [ ] Output encoding for user content
- [ ] Authorization checks in place
- [ ] Errors don't leak sensitive info
- [ ] Logging doesn't include secrets

### For Authentication Changes
- [ ] Password hashing is secure
- [ ] Session management is secure
- [ ] Rate limiting implemented

### For API Changes
- [ ] Authentication required
- [ ] Authorization verified
- [ ] Input validated
- [ ] Rate limiting in place

---

## Incident Response

### If Security Issue Found

1. **Assess severity** — What data/systems affected?
2. **Contain** — Prevent further damage
3. **Document** — What happened, when, impact
4. **Fix** — Resolve the vulnerability
5. **Review** — How to prevent similar issues
6. **Communicate** — Inform affected parties if needed

### Severity Levels

| Severity | Description | Response Time |
|----------|-------------|---------------|
| Critical | Active exploitation | Immediate |
| High | Exploitable vulnerability | 24 hours |
| Medium | Potential vulnerability | 1 week |
| Low | Minor improvement | Next release |
