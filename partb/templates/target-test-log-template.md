# Target Test Log Template

## 1. Basic Information
- Date:
- Platform:
- Program:
- Program URL:
- Program overview section reviewed:
- Key overview excerpt:
- Scope proof:
- Scope page URL:
- Official website URL:
- Target / asset tested:
- Tester(s):

## 2. Target Access
- Account required: Yes / No
- Account type(s) used:
- Registration/login notes:
- Role setup:
- Whether a second account or second role may be needed:

## 3. Testing Summary
- Main functionality tested:
- Vulnerability classes attempted:
- Baseline checklist status:
  - 1. Access control / IDOR / object exposure:
  - 2. Authentication / session / invite / reset / verification / OAuth flow flaws:
  - 3. Token / nonce / randomness / binding quality:
  - 4. Input handling / XSS / unsafe reflection:
  - 5. Misconfiguration / unsafe exposure / business logic boundary:
- Customized testing ideas:
- Tools used:
- AI assistance used:

## 4. Test Steps
1.
2.
3.

## 4A. Attack Surface Map
- Public surfaces discovered:
- Auth-related surfaces discovered:
- Object-bearing surfaces discovered:
- Billing / subscription / trial surfaces discovered:
- Sharing / invitation / collaboration surfaces discovered:
- App / developer / API / docs surfaces discovered:

## 4B. DevTools Deep-Dive Notes
- Key pages inspected with DevTools:
- Important XHR / fetch endpoints:
- Interesting object IDs / UUIDs / team or workspace markers:
- Tokens / nonce / state / CSRF observations:
- Hidden parameters / embedded state / frontend routes:
- Bundle / source / feature-flag observations:

## 5. Evidence
- Important URLs:
- Important requests/responses:
- Screenshots / recordings:
- Notes:

## 6. Outcome
- Result: No finding / Candidate finding / Out of scope / Blocked
- What worked:
- What failed:
- Why no finding was confirmed yet:
- Whether this target should be revisited only with a richer account state or second account:

## 7. Candidate Finding Details
- Vulnerability title:
- Vulnerability class:
- Root cause:
- Reproduction summary:
- Security impact:
- Affected assets/data:
- Severity estimate:
- Platform severity mapping:
- CVSS fallback (if needed):

## 8. Novelty Check
- Similar public reports searched:
- Public references checked:
- Initial novelty judgment:
- Finding type: Type 1 confirmed non-zero-day / Type 2 zero-day candidate / Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
- Safety constraints followed:
- Actions avoided:

## 10. Next Actions
- Follow-up validation needed:
- Questions for teammates:
- Whether to keep testing this target:
- Next recommended testing state:
  - Continue pre-auth
  - Request login / registration
  - Continue post-auth
  - Prepare second account / second role
  - Move to next program

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
