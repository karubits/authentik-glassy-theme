# Security Review - 2026-05-26

**Reviewer:** Automated (Hermes + claude-opus-4-6)
**Scope:** All files in karubits/authentik-glassy-theme @ v1.2
**Result:** PASS - No security issues found

---

## Files Reviewed

| File | Size | Status |
|------|------|--------|
| theme.css | 88,650 bytes (2,336 lines) | Clean |
| README.md | 4,357 bytes | Clean |
| selfhosted-post.md | 2,023 bytes | Clean |
| LICENSE | 1,265 bytes | Clean |
| example1-4.png | Screenshots | N/A (binary) |

---

## CSS Security Analysis (theme.css)

### 1. External URLs
**CLEAN** - No external resource loading.
- Lines 4, 6: GitHub URLs in comments only (not executed)
- Line 18: `url(/static/dist/media/wallpaper9.webp)` is a local relative path
- Lines 771, 1072: `http://www.w3.org/2000/svg` inside data URIs are XML namespace declarations, not network requests

### 2. Data URIs
**CLEAN** - Two data URIs found, both safe.
- Line 771: `data:image/svg+xml` containing an SVG noise filter (`feTurbulence`). No scripts, no event handlers, no external references. Used for glassmorphism texture.
- Line 1072: `data:image/svg+xml` containing a Font Awesome key icon SVG path. Pure vector path data only.
- Both SVGs verified: no `<script>`, `onclick`, `onload`, or any executable content.

### 3. CSS Injection Vectors
**CLEAN** - No dangerous CSS features used.
- `@import`: None
- `expression()`: None
- `-moz-binding`: None
- `behavior:`: None
- `javascript:`: None
- `eval()`: None
- `@font-face`: None
- All `url()` references (3 total) point to local paths or inline data URIs.

### 4. Information Disclosure
**CLEAN** - No sensitive data exposed.
- No API keys, tokens, or passwords
- No IP addresses or internal hostnames
- No email addresses
- No hardcoded filesystem paths beyond standard web paths (`/static/dist/media/`)
- References to "password" and "token" in selectors are Authentik component names (e.g. `ak-stage-password`, `ak-user-token-list`), not credentials

### 5. UI Manipulation / Clickjacking
**CLEAN** - All z-index and pointer-events usage is legitimate.
- `z-index` values range from -1 to 100000, all for standard UI layering (backgrounds, buttons, accessibility skip link)
- `pointer-events: none` used only on decorative pseudo-elements (shine effects, background overlay)
- `pointer-events: auto` used only on the password toggle button
- No `opacity: 0` invisible overlays
- No elements positioned to intercept clicks on security-sensitive UI

### 6. Content Injection
**CLEAN** - No misleading injected text.
- `content: '✓'` for checkbox styling
- `content: var(--ak-social-separator-text)` resolves to "Continue with" (configurable)
- `content: ""` or `content: none` for decorative pseudo-elements
- No fake "Verified", "Secure", or phishing text injected

### 7. Branding Modification
**INFORMATIONAL** - "Powered by authentik" branding is hidden (lines 698-724). This is a common cosmetic customization and does not hide security warnings, error messages, or authentication UI.

### 8. Base64 Content
**CLEAN** - No Base64 encoded content. Data URIs use URL-encoding (percent-encoding).

---

## Documentation Review (README.md, selfhosted-post.md)

### Links
**CLEAN** - All links point to expected destinations:
- `https://goauthentik.io/` (official Authentik site)
- `https://github.com/karubits` (fork author)
- `https://github.com/VULGA01/Authentik-Login-theme-Glassmorphism` (original repo)
- `https://github.com/nousresearch/hermes` (AI tool used)
- `sso.example.com` references are placeholders

### Credentials
**CLEAN** - API examples use proper placeholders (`<your-api-token>`, `<your-brand-uuid>`). No real credentials exposed.

### Installation Instructions
**CLEAN** - Both installation methods (UI paste and API PATCH) are standard Authentik procedures. The volume mount example uses `:ro` (read-only), which is security-conscious.

---

## License Compliance

**COMPLIANT** - MIT license with dual copyright:
- `Copyright (c) 2025 VULGA01 (original theme)`
- `Copyright (c) 2026 karubits (Shadow DOM fork)`
- Original repo linked in LICENSE, README, CSS header, and selfhosted post

---

## Recommendations

None. The repository is clean and suitable for public distribution.
