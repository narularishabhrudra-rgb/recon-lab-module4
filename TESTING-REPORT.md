# Testing Report: Log Analyzer

**Tester:** [Your Name]  **Date:** [Date]
**Environment:** Local machine and GitHub Pages deployment. Synthetic data only (reserved documentation IPs).

## 1. Test Method
1. Engine test: ran the `analyze()` function against the built-in sample (33 lines) and edge cases in Node.js.
2. UI test: repeated the key cases in the browser. **Fill in the last column after you test on your own device.**

## 2. Test Cases
| ID | Test | Input | Expected | Engine result | Browser result |
|----|------|-------|----------|---------------|----------------|
| T1 | Parse sample data | Built-in sample | 33 lines parsed, 3 unique IPs | Pass (33/33, 3 IPs) | [ ] |
| T2 | Brute-force detection | 12 failed SSH logins from one IP, threshold 5 | High finding | Pass (12 failures, 2 accounts) | [ ] |
| T3 | Compromise correlation | Successful login after the failures | Critical finding | Pass | [ ] |
| T4 | SQL injection | `1' OR 1=1--` and `UNION SELECT` in URL | High finding, 2 requests | Pass | [ ] |
| T5 | Path traversal | `../../etc/passwd` in URL | High finding | Pass | [ ] |
| T6 | XSS attempt | `<script>` in URL | High finding | Pass | [ ] |
| T7 | Sensitive file probe | `/.env`, `/phpmyadmin/` | Medium finding | Pass (3 requests) | [ ] |
| T8 | Scanning behaviour | 14 4xx responses from one IP | Medium finding | Pass | [ ] |
| T9 | Clean traffic | Normal 200 request plus junk line | 0 findings | Pass | [ ] |
| T10 | Threshold change | Sample with threshold 20 | No brute-force finding | Pass | [ ] |
| T11 | Output safety | Log line containing `<img src=x onerror=alert(1)>` | Shown as plain text, no script runs | [ ] | [ ] |
| T12 | Empty input | Click Analyze with empty box | No findings, no crash | [ ] | [ ] |

## 3. Bugs Found and Fixed
| Bug | Cause | Fix |
|-----|-------|-----|
| Sample data broke the page script | A literal `</script>` inside the sample string ended the script block early | Escaped it as `<\/script>` |

## 4. Limitations Observed
- Signature detection can be bypassed by heavy obfuscation.
- Only SSH auth and combined access log formats are parsed.

## 5. Conclusion
[Your summary: what works, what you would improve.]
