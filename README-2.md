# Log Analyzer: Cybersecurity Capstone

A browser-based **defensive security tool** that analyzes SSH authentication logs and web server access logs (Apache/Nginx combined format) and flags suspicious activity. It runs 100% client-side: logs never leave the user's browser.

**Live site:** [add your GitHub Pages link]
**Demo video:** [add link]

> Use only on logs from systems you own or are authorized to analyze. The built-in sample data uses reserved documentation IP ranges (RFC 5737).

## Features
| Detection | Severity | How it works |
|-----------|----------|--------------|
| Brute-force logins | High | Failed SSH logins per source IP at or above a configurable threshold (default 5) |
| Login success after failures | Critical | Same IP has a successful login after repeated failures (possible compromise) |
| SQL injection attempts | High | Signature match on URL (raw and URL-decoded) |
| Cross-site scripting attempts | High | Signature match on URL |
| Path traversal attempts | High | `../` and encoded variants |
| Sensitive file / admin probes | Medium | `.env`, `.git/`, `phpmyadmin`, `wp-login.php`, `/etc/passwd` |
| Scanning behaviour | Medium | 10 or more 4xx responses from one IP |

Also shows: top source IPs, HTTP status distribution, and a downloadable JSON report.

## Security Architecture
```
 [User's log file / pasted text]
            |
            v
 +---------------------------+      Browser only (no backend, no network calls)
 |  Input layer              |  - size limited to 5 MB
 |  (file picker / textarea) |
 +-------------+-------------+
               v
 +---------------------------+
 |  Parser                   |  - regex for SSH auth and combined access logs
 |  (analyze())              |  - unparseable lines are skipped safely
 +-------------+-------------+
               v
 +---------------------------+
 |  Detection engine         |  - threshold rules (brute force, scanning)
 |                           |  - signature rules (SQLi, XSS, traversal, probes)
 |                           |  - correlation (success after failures)
 +-------------+-------------+
               v
 +---------------------------+
 |  Renderer                 |  - DOM built with textContent only (no innerHTML)
 |  (render())               |  - findings table, charts, JSON export
 +---------------------------+
```

### Security design decisions
| Risk | Control |
|------|---------|
| Logs are attacker-controlled (log injection / XSS via log lines) | Output uses `textContent` only, never `innerHTML` |
| Data leakage | No server, no network requests; Content-Security-Policy `default-src 'none'` blocks external connections |
| Third-party supply chain | Zero external libraries or CDNs |
| Denial of service from huge files | Input capped at 5 MB |
| Regex DoS | Simple, non-nested patterns |

### Limitations
- Signature rules are heuristic and can produce false positives or miss obfuscated attacks.
- Only two log formats are supported.
- Not a replacement for a SIEM or WAF.

## Usage
1. Open the live site.
2. Click **Load sample data** for an instant demo, or upload/paste your own log.
3. Adjust the brute-force threshold if needed and click **Analyze**.
4. Review findings, then **Download report (JSON)**.

## Deploy on GitHub Pages
1. Upload `index.html` to the repo root.
2. Repo **Settings** → **Pages** → Source: **Deploy from a branch** → Branch: `main` / `(root)` → **Save**.
3. After a minute, your site is at `https://<username>.github.io/<repo-name>/`.

## Repository Files
| File | Purpose |
|------|---------|
| `index.html` | The complete application (HTML, CSS, JS) |
| `README.md` | Documentation and architecture |
| `TESTING-REPORT.md` | Test cases and results |

## Future Improvements
IP geolocation, more log formats (Windows event logs, firewall logs), time-based charts, MITRE ATT&CK mapping.

## Author
[Your Name] | [Company Name] Cybersecurity Intern
