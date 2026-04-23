# Target Test Log

## 1. Basic Information
- Date: 2026-04-23
- Platform: HackerOne
- Program: Airtable
- Program URL: `https://hackerone.com/airtable?type=team`
- Program overview section reviewed: Yes
- Key overview excerpt:
  - `Airtable | Bug Bounty Program Policy | HackerOne`
  - `Assets In Scope: 5`
  - `Managed by HackerOne`
- Scope proof:
  - The teacher-approved platform is HackerOne.
  - The direct Airtable HackerOne program page rendered correctly in Chrome.
  - The page exposed `Security page`, `Program guidelines`, `Scope`, `Hacktivity`, `Thanks`, and `Updates`, confirming this is a valid program page rather than a user profile.
- Scope page URL: `https://hackerone.com/airtable/policy_scopes?type=team`
- Official website URL: `https://www.airtable.com/`
- Target / asset tested: Direct HackerOne program page, Airtable homepage, Airtable login page, Airtable signup page
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Yes for higher-value authenticated testing.
- Account type(s) used: None yet.
- Registration/login notes:
  - The homepage exposes clear `Sign up for free` and `Log in` entry points.
  - `https://airtable.com/login` is a real login page with email, SSO, Google, and Apple login.
  - `https://airtable.com/signup` is a real signup page with work-email signup, SSO, Google, and Apple options.
- Role setup: None yet.
- Whether a second account or second role may be needed:
  - Possibly yes later for deeper collaboration, sharing, or workspace-boundary testing.

## 3. Testing Summary
- Main functionality tested:
  - Direct HackerOne Airtable program page
  - Airtable homepage
  - Airtable login page
  - Airtable signup page
- Vulnerability classes attempted:
  - Program intake and scope lock
  - Attack-surface mapping
  - Pre-auth UI review
  - Pre-auth DevTools deep dive
  - Access control / object exposure pre-check
  - Auth / session / invite / reset / verification flow pre-check
  - Token / parameter / binding pre-check
  - Misconfiguration / exposure pre-check
- Baseline checklist status:
  - 1. Access control / IDOR / object exposure: No direct unauthenticated object exposure confirmed in this round.
  - 2. Authentication / session / invite / reset / verification / OAuth flow flaws: Public login and signup entry review completed.
  - 3. Token / nonce / randomness / binding quality: Initial login/signup parameter and anti-automation review completed.
  - 4. Input handling / XSS / unsafe reflection: No obvious public reflected input issue confirmed in this round.
  - 5. Misconfiguration / unsafe exposure / business logic boundary: Initial public-surface and developer/API exposure review completed.
- Customized testing ideas:
  - After login, prioritize workspace/base sharing, API/developer surfaces, and account/workspace boundary checks.
  - Revisit developer docs and API objects once an owned account exists.
- Tools used: Chrome page review, Chrome network inspection, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Opened the direct HackerOne Airtable program page at `https://hackerone.com/airtable?type=team`.
2. Confirmed that the page rendered as a real bug bounty policy page rather than a user profile.
3. Reviewed the visible HackerOne program structure and confirmed:
   - `Program guidelines`
   - `Scope`
   - `Hacktivity`
   - `Thanks`
   - `Updates`
4. Confirmed useful high-level program signals from the rendered page:
   - `Airtable | Bug Bounty Program Policy | HackerOne`
   - `Assets In Scope: 5`
   - `Managed by HackerOne`
5. Opened the Airtable homepage at `https://www.airtable.com/`.
6. Mapped public attack surfaces from the homepage, including:
   - pricing
   - enterprise
   - signup
   - login
   - developer docs
   - API
   - help center
   - security/trust pages
7. Confirmed the homepage exposes real auth entry points:
   - `Sign up for free`
   - `Log in`
8. Reviewed the homepage network activity and confirmed it is a real application/marketing surface with many third-party analytics and support integrations.
9. Clicked into the Airtable login page and confirmed it is a real login flow, not a simple redirect shell.
10. Confirmed the Airtable login page exposes:
   - email input
   - `Continue`
   - `Sign in with Single Sign On`
   - `Continue with Google`
   - `Continue with Apple ID`
11. Inspected the login page network activity and confirmed:
   - `run_signin.js`
   - Airtable-hosted static bundles
   - `PerimeterX` initialization
   - no immediate `reCAPTCHA` style blocker in the visible UI
12. Clicked through to the Airtable signup page and confirmed it is a real signup flow.
13. Confirmed the Airtable signup page exposes:
   - work-email input
   - `Continue with email`
   - `Continue with Single Sign On`
   - `Continue with Google`
   - `Continue with Apple`
14. Inspected the signup page network activity and confirmed:
   - `run_multistep_signup.js`
   - `airtable.com/internal/stats-batch`
   - `PerimeterX` collector traffic
   - no direct unauthenticated vulnerability confirmed in this round
15. Stopped after the pre-auth round because the next valuable step is authenticated testing with a user-created account.

## 4A. Attack Surface Map
- Public surfaces discovered:
  - `https://www.airtable.com/`
  - `https://airtable.com/pricing`
  - `https://www.airtable.com/solutions/enterprise`
  - `https://www.airtable.com/company/trust-and-security`
  - `https://support.airtable.com/`
- Auth-related surfaces discovered:
  - `https://airtable.com/login`
  - `https://airtable.com/signup`
  - SSO sign-in
  - Google sign-in
  - Apple sign-in
- Object-bearing surfaces discovered:
  - Not meaningfully exposed in this round before login.
- Billing / subscription / trial surfaces discovered:
  - pricing page
  - signup flow
  - enterprise and contact-sales paths
- Sharing / invitation / collaboration surfaces discovered:
  - Workspace-oriented product language is visible on the homepage, but no concrete invite object was inspected pre-auth.
- App / developer / API / docs surfaces discovered:
  - `https://airtable.com/developers`
  - `https://airtable.com/developers/web/api/introduction`
  - `https://academy.airtable.com/`
  - `https://community.airtable.com/`

## 4B. DevTools Deep-Dive Notes
- Key pages inspected with DevTools:
  - `https://www.airtable.com/`
  - `https://airtable.com/login`
  - `https://airtable.com/signup`
- Important XHR / fetch endpoints:
  - `https://airtable.com/internal/stats-batch`
  - `https://collector-px0ozadu9k.px-cloud.net/api/v2/collector`
  - multiple Airtable-hosted static bundle endpoints under `https://static.airtable.com/`
- Interesting object IDs / UUIDs / team or workspace markers:
  - No concrete user/team/workspace object IDs were exposed pre-auth.
- Tokens / nonce / state / CSRF observations:
  - No immediately actionable public token/nonce issue was confirmed in this round.
- Hidden parameters / embedded state / frontend routes:
  - Login route loaded `run_signin.js`.
  - Signup route loaded `run_multistep_signup.js`.
- Bundle / source / feature-flag observations:
  - Both login and signup flows are substantial Airtable-hosted frontend applications, not trivial redirect stubs.
  - `PerimeterX` is present in both login and signup flows, implying anti-automation or bot-detection logic.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/airtable?type=team`
  - `https://hackerone.com/airtable/policy_scopes?type=team`
  - `https://www.airtable.com/`
  - `https://airtable.com/login`
  - `https://airtable.com/signup`
  - `https://airtable.com/developers`
  - `https://airtable.com/developers/web/api/introduction`
- Important requests/responses:
  - HackerOne page title: `Airtable | Bug Bounty Program Policy | HackerOne`
  - Program page visible stat: `Assets In Scope: 5`
  - Login page bundle: `run_signin.js`
  - Signup page bundle: `run_multistep_signup.js`
  - Anti-automation traffic: `PerimeterX` init and collector requests
  - Airtable internal telemetry endpoint: `airtable.com/internal/stats-batch`
- Screenshots / recordings: None yet.
- Notes:
  - This is a valid next-step target under the current workflow.
  - The current state is ideal for handing off to user registration/login before deeper authenticated testing.

## 6. Outcome
- Result: No finding yet after pre-auth round
- What worked:
  - Confirmed a valid Airtable HackerOne program page.
  - Confirmed real signup and login flows.
  - Confirmed useful developer/API surfaces for later authenticated follow-up.
- What failed:
  - No unauthenticated object exposure, redirect issue, or obvious reflected-input issue was confirmed in this round.
- Why no finding was confirmed yet:
  - The higher-value Airtable attack surface likely sits behind authenticated workspace/base/account contexts.
- Whether this target should be revisited only with a richer account state or second account:
  - Continue immediately after user registration/login.

## 7. Candidate Finding Details
- Vulnerability title: None yet.
- Vulnerability class: None yet.
- Root cause: None yet.
- Reproduction summary: None yet.
- Security impact: None yet.
- Affected assets/data: None yet.
- Severity estimate: None yet.
- Platform severity mapping: None yet.
- CVSS fallback (if needed): None yet.

## 8. Novelty Check
- Similar public reports searched: Not started.
- Public references checked:
  - Direct HackerOne Airtable program page
  - Airtable homepage
  - Airtable login/signup pages
- Initial novelty judgment: Not applicable yet.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - Airtable is being assessed through the teacher-approved HackerOne platform.
  - The direct HackerOne Airtable program page rendered correctly and exposed policy/scope structure.
- Safety constraints followed:
  - Only public HackerOne and public Airtable pages were reviewed in this round.
  - No login attempt or account submission was performed by the agent.
- Actions avoided:
  - No brute force, no automated account creation, no authenticated testing yet.

## 10. Next Actions
- Follow-up validation needed:
  - User should complete registration/login on the currently opened Airtable signup or login page.
- Questions for teammates:
  - None.
- Whether to keep testing this target:
  - Yes.
- Next recommended testing state:
  - Request login / registration

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
