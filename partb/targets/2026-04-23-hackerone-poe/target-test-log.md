# Target Test Log

## 1. Basic Information
- Date: 2026-04-23
- Platform: HackerOne
- Program: Poe
- Program URL: `https://hackerone.com/poe?type=team`
- Program overview section reviewed: Attempted, but the URL resolved to a HackerOne user profile instead of a program security page.
- Key overview excerpt:
  - `Poe | Profile | HackerOne`
  - `Poe (poe)`
- Scope proof:
  - The teacher-approved platform is HackerOne.
  - The direct URL pattern that would normally represent a program page was opened and reviewed in Chrome.
  - The page content did not expose program overview, scope, policy, or report-submission guidance for a bug bounty program.
- Scope page URL: Not confirmed because a valid Poe program security page was not identified.
- Official website URL: `https://poe.com/`
- Target / asset tested: Direct HackerOne Poe URL and Poe public login surface
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Likely yes for meaningful product testing.
- Account type(s) used: None.
- Registration/login notes:
  - `https://poe.com/` redirected to `https://poe.com/login?redirect_url=%2F`.
  - The Poe login page exposed Google, Apple, email, and phone login paths.
  - The Poe login page visibly loaded `reCAPTCHA`.
- Role setup: None.
- Whether a second account or second role may be needed: Unknown.

## 3. Testing Summary
- Main functionality tested:
  - Direct HackerOne Poe URL
  - Poe public login page
- Vulnerability classes attempted:
  - Program intake and scope lock
  - Attack-surface mapping
  - Pre-auth UI review
  - Pre-auth DevTools deep dive
- Baseline checklist status:
  - 1. Access control / IDOR / object exposure: Not meaningfully testable because no valid program page or in-scope asset list was confirmed.
  - 2. Authentication / session / invite / reset / verification / OAuth flow flaws: Only the public login entry was observed.
  - 3. Token / nonce / randomness / binding quality: Not meaningfully testable in this round.
  - 4. Input handling / XSS / unsafe reflection: Not meaningfully testable in this round.
  - 5. Misconfiguration / unsafe exposure / business logic boundary: No clear public signal beyond the login page.
- Customized testing ideas:
  - Revisit only if a valid HackerOne security page or other teacher-approved scope proof for Poe is identified later.
- Tools used: Chrome page review, Chrome snapshot review, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Opened the direct HackerOne URL `https://hackerone.com/poe?type=team` as the candidate Poe program page.
2. Confirmed that the rendered HackerOne page title was `Poe | Profile | HackerOne`, not a program security page.
3. Reviewed the rendered page structure and confirmed it contained user-profile content such as `Profile`, `Badges`, `Hacktivity`, `Joined April 2016`, and no bug bounty scope or policy content.
4. Opened `https://poe.com/` and confirmed it redirected into the Poe login page.
5. Reviewed the public Poe login page and confirmed visible login options for Google, Apple, email, and phone.
6. Confirmed that the Poe login page visibly loaded `reCAPTCHA`.
7. Stopped the round because the HackerOne URL did not resolve to a valid program security page and therefore did not provide sufficient scope confidence for further target testing.

## 4A. Attack Surface Map
- Public surfaces discovered:
  - `https://poe.com/login?redirect_url=%2F`
- Auth-related surfaces discovered:
  - Google login
  - Apple login
  - email login
  - phone login
- Object-bearing surfaces discovered:
  - None in this round.
- Billing / subscription / trial surfaces discovered:
  - None in this round.
- Sharing / invitation / collaboration surfaces discovered:
  - None in this round.
- App / developer / API / docs surfaces discovered:
  - None in this round.

## 4B. DevTools Deep-Dive Notes
- Key pages inspected with DevTools:
  - `https://hackerone.com/poe?type=user`
  - `https://poe.com/login?redirect_url=%2F`
- Important XHR / fetch endpoints:
  - None recorded as actionable in this round.
- Interesting object IDs / UUIDs / team or workspace markers:
  - None recorded in this round.
- Tokens / nonce / state / CSRF observations:
  - The visible login page presented a standard login redirect parameter `redirect_url=%2F`.
- Hidden parameters / embedded state / frontend routes:
  - None recorded as actionable in this round.
- Bundle / source / feature-flag observations:
  - The Poe login page visibly presented `reCAPTCHA`, implying manual-auth friction if this target is revisited later.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/poe?type=team`
  - `https://hackerone.com/poe?type=user`
  - `https://poe.com/`
  - `https://poe.com/login?redirect_url=%2F`
- Important requests/responses:
  - HackerOne rendered title: `Poe | Profile | HackerOne`
  - Visible Poe login options: Google, Apple, email, phone
  - Visible login protection: `reCAPTCHA`
- Screenshots / recordings: None yet.
- Notes:
  - This round is valuable mainly as duplicate-avoidance evidence.
  - The key blocker is not a website challenge first; it is the absence of a confirmed valid HackerOne bug bounty program page for Poe.

## 6. Outcome
- Result: Out of scope
- What worked:
  - Quickly validated that the apparent HackerOne Poe URL did not expose a real program security page.
  - Confirmed that the public Poe site currently funnels directly into a login flow with `reCAPTCHA`.
- What failed:
  - Could not confirm a valid HackerOne Poe program page, scope page, or policy content.
- Why no finding was confirmed yet:
  - Without a valid program security page, scope confidence is insufficient and the target should not be treated as a normal next-step bounty program under the current workflow.
- Whether this target should be revisited only with a richer account state or second account:
  - No. Revisit only if a valid teacher-allowed Poe bounty program page is identified first.

## 7. Candidate Finding Details
- Vulnerability title: None.
- Vulnerability class: None.
- Root cause: None.
- Reproduction summary: None.
- Security impact: None.
- Affected assets/data: None.
- Severity estimate: None.
- Platform severity mapping: None.
- CVSS fallback (if needed): None.

## 8. Novelty Check
- Similar public reports searched: Not started.
- Public references checked:
  - HackerOne rendered page for `poe`
  - Poe login page
- Initial novelty judgment: Not applicable.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - It was evaluated only because it appeared to be a candidate under the teacher-approved HackerOne platform.
  - The round ended once the page proved not to be a valid program security page.
- Safety constraints followed:
  - Only a public HackerOne page and Poe login page were reviewed.
  - No login attempt, brute force, or authenticated interaction was performed.
- Actions avoided:
  - No testing beyond public login-surface review.
  - No attempt to continue into product functionality without confirmed scope.

## 10. Next Actions
- Follow-up validation needed:
  - None unless a valid HackerOne Poe program page is found later.
- Questions for teammates:
  - None.
- Whether to keep testing this target:
  - No.
- Next recommended testing state:
  - Move to next program

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
