# Target Test Log

## 1. Basic Information
- Date: 2026-04-23
- Platform: HackerOne
- Program: Semrush
- Program URL: `https://hackerone.com/semrush`
- Program overview section reviewed: Yes
- Key overview excerpt:
  - `Semrush - Bug Bounty Program | HackerOne`
  - `The Semrush Bug Bounty Program enlists the help of the hacker community at HackerOne to make Semrush more secure.`
  - `Test only with your own account(s) when investigating bugs.`
- Scope proof:
  - The teacher-approved starting platform is HackerOne.
  - The official HackerOne Bug Bounty Programs page currently lists `Semrush` as a public bug bounty program.
  - The raw HTML of the direct program page `https://hackerone.com/semrush?type=team` was fetched and reviewed.
  - The same official HackerOne listing snippet includes an explicit policy excerpt requiring testing only with the researcher's own account(s).
- Scope page URL: `https://hackerone.com/semrush/policy_scopes?type=team`
- Official website URL: `https://www.semrush.com/`
- Target / asset tested: Public Semrush web pages and public auth entry points for `www.semrush.com`
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Likely yes for higher-value authenticated testing.
- Account type(s) used: None yet.
- Registration/login notes:
  - The Semrush homepage exposes `Sign Up` and `Try for free` entry points.
  - The pricing page advertises `Try Semrush free for seven days. Cancel anytime.`
  - The public signup page is `https://www.semrush.com/signup/?src=header`.
  - The public login page is `https://www.semrush.com/login/`.
  - The fetched signup page source exposes a SPA auth shell, a CSRF token, and auth state metadata, but no obvious captcha requirement was observed in the fetched HTML.
  - The fetched login page source exposes a similar SPA auth shell, a CSRF token, and auth state metadata.
- Role setup: Not started.

## 3. Testing Summary
- Main functionality tested:
  - Semrush public homepage
  - Public pricing / trial page
  - Public signup page source
  - Public login page source
- Vulnerability classes attempted:
  - Initial public-surface access control review
  - Public authentication / authorization flow review
  - Initial input handling surface review
  - Public misconfiguration / unsafe exposure review
  - Initial Semrush-specific customized review
- Baseline checklist status:
  - 1. IDOR / horizontal access control: Not started in authenticated flows.
  - 2. Authentication / authorization flow mistakes: Initial public-flow review completed.
  - 3. Input handling issues: Initial public input-surface review completed.
  - 4. Misconfiguration / unsafe exposure: Initial public-surface review completed.
- Customized testing ideas:
  - Review whether trial activation, toolkit selection, and plan-specific onboarding expose role or tenant-boundary issues once test accounts exist.
  - Review whether account provisioning, trial state, and subscription-related endpoints leak plan or organization data between users.
- Tools used: HackerOne official listing page, Semrush official website pages, public page source inspection, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Confirmed that the teacher-approved starting platform remains HackerOne.
2. Chose `Semrush` as the next target because the official HackerOne Bug Bounty Programs page lists it publicly and includes a useful policy excerpt: test only with your own account(s).
3. Recorded the direct HackerOne program URL as `https://hackerone.com/semrush`.
4. Fetched and reviewed the raw HTML of the direct HackerOne Semrush program page and extracted the program title and description metadata before testing.
5. Reviewed the official HackerOne Bug Bounty Programs page snippet for the Semrush program and extracted the policy-relevant rule to test only with owned accounts.
6. Fetched the raw HTML of the direct HackerOne Semrush scope page URL and confirmed that the scope page itself is reachable, even though its asset details are rendered client-side.
7. Reviewed the Semrush homepage `https://www.semrush.com/` and confirmed public `Log In`, `Sign Up`, and `Try for free` entry points.
8. Reviewed the Semrush pricing page and confirmed the public seven-day trial messaging.
9. Fetched the Semrush public signup page source and confirmed that it loads an SPA auth shell with CSRF state and signup-related auth metadata.
10. Fetched the Semrush public login page source and confirmed that it loads an SPA auth shell with CSRF state and login-related auth metadata.
11. Reviewed the fetched public auth-page source for obvious blockers or constraints, including visible captcha references, and did not observe an obvious captcha script in the fetched HTML.
12. Stopped after the first public/auth-entry review because higher-value bug classes for this target still appear to depend on real account creation and authenticated workflows.

## 5. Evidence
- Important URLs:
  - `https://www.hackerone.com/bug-bounty-programs`
  - `https://hackerone.com/semrush`
  - `https://hackerone.com/semrush/policy_scopes?type=team`
  - `https://www.semrush.com/`
  - `https://www.semrush.com/pricing/`
  - `https://www.semrush.com/pricing/seo/`
  - `https://www.semrush.com/signup/?src=header`
  - `https://www.semrush.com/login/`
- Important requests/responses:
  - Public signup page source includes:
    - canonical `https://www.semrush.com/signup/`
    - hidden `csrfmiddlewaretoken`
    - `window.__AUTH_STATE__`
    - `form id="mktoForm_1025"`
  - Public login page source includes:
    - canonical `https://www.semrush.com/login/`
    - hidden `csrfmiddlewaretoken`
    - `window.__AUTH_STATE__`
    - `formAction` set to `/login` in embedded initial state
- Screenshots / recordings: None yet.
- Notes:
  - The raw HTML of the direct HackerOne program page was fetched successfully and provided the official program title and description metadata.
  - The raw HTML of the direct HackerOne scope page was fetched successfully, but the asset details themselves still require client-side rendering.
  - The official HackerOne Bug Bounty Programs page was still useful as the direct source for the explicit owned-account testing rule.
  - The official Semrush public pages clearly expose account entry points and a seven-day trial path.
  - No obvious unauthenticated vulnerability was observed on the reviewed public pages.
  - No obvious captcha blocker was visible in the fetched Semrush signup HTML, so authenticated testing may be easier to advance here than it was for Basecamp.

## 6. Outcome
- Result: No finding
- What worked:
  - Selected a second concrete HackerOne program and avoided retesting Basecamp.
  - Read the direct HackerOne Semrush program page through raw HTML instead of relying only on the general program listing.
  - Confirmed that the official HackerOne listing for Semrush includes a directly relevant rule: use only your own account(s).
  - Confirmed that Semrush exposes public signup and login entry points and a trial workflow.
  - Confirmed that the fetched signup and login pages expose concrete auth application scaffolding rather than only marketing redirects.
- What failed:
  - Could not begin IDOR or horizontal access-control testing because no test account has been created yet.
  - Could not confirm role structure or organization / workspace concepts from unauthenticated public review alone.
- Why no finding was confirmed yet:
  - The first round was limited to public pages and auth entry-point analysis.
  - The most promising Semrush bug classes likely require authenticated workflows and at least one owned account.

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
  - HackerOne Bug Bounty Programs page
  - Semrush official homepage
  - Semrush official pricing page
- Initial novelty judgment: Not applicable yet.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - The teacher-approved platform is HackerOne.
  - Semrush is currently listed on the official HackerOne Bug Bounty Programs page.
- Safety constraints followed:
  - Only public pages and public auth entry points were reviewed.
  - No account creation, brute force, automation, or disruptive behavior was used.
- Actions avoided:
  - No attempts against non-confirmed assets outside the official Semrush public site.
  - No attempts to access another user's account or data.
  - No active submission of signup or login forms in this round.

## 10. Next Actions
- Follow-up validation needed:
  - Confirm whether the team wants to create a Semrush test account using an owned email, consistent with the HackerOne listing rule to use only owned accounts.
  - If account creation succeeds, move into authenticated testing for IDOR, horizontal access control, and onboarding / trial-boundary checks.
- Questions for teammates:
  - Does the team want Semrush to become the first target where we create a real test account, or should we still shortlist more programs first?
- Whether to keep testing this target: Yes, because the auth flow appears more operationally feasible than Basecamp from the current environment.

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
