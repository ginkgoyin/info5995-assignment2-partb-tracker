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
- Account required: Yes for higher-value authenticated testing.
- Account type(s) used: One owned Semrush account supplied by the user in Chrome.
- Registration/login notes:
  - The Semrush homepage exposes `Sign Up` and `Try for free` entry points.
  - The pricing page advertises `Try Semrush free for seven days. Cancel anytime.`
  - The public signup page is `https://www.semrush.com/signup/?src=header`.
  - The public login page is `https://www.semrush.com/login/?src=header`.
  - Raw HTML review showed SPA auth scaffolding, CSRF state, and auth metadata on both signup and login pages.
  - Rendered-page review through Chrome DevTools showed visible `reCAPTCHA` on both the login and signup pages.
- Role setup:
  - Single logged-in owned account only.
  - No second account or alternate role was available in this round.

## 3. Testing Summary
- Main functionality tested:
  - Semrush public homepage
  - Public pricing / trial page
  - Public login page (rendered UI)
  - Public signup page (rendered UI)
  - Semrush App Center public pages
  - Authenticated SEO toolkit landing page
  - Authenticated `/home/` landing page
  - Authenticated `My Reports`
  - Authenticated `My Apps`
  - Authenticated App Center store state
- Vulnerability classes attempted:
  - Initial public-surface access control review
  - Public authentication / authorization flow review
  - Initial input handling surface review
  - Public misconfiguration / unsafe exposure review
  - Initial Semrush-specific customized review
  - Initial authenticated session-state review
  - Initial authenticated object / subscription-boundary review
- Baseline checklist status:
  - 1. IDOR / horizontal access control: Partially explored in authenticated single-account surfaces; no second-account comparison yet.
  - 2. Authentication / authorization flow mistakes: Initial public-flow and first authenticated session review completed.
  - 3. Input handling issues: Initial public input-surface review completed.
  - 4. Misconfiguration / unsafe exposure: Initial public-surface and first authenticated exposure review completed.
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
8. Reviewed the Semrush pricing page and confirmed public seven-day trial messaging and public subscription links with `redirect_to` parameters such as `/analytics/overview/`.
9. Reviewed the rendered Semrush public login page and confirmed visible email/password auth UI, Google sign-in, SAML login, and a rendered `reCAPTCHA` widget.
10. Reviewed the rendered Semrush public signup page and confirmed visible email sign-up UI, Google sign-in, terms links, and a rendered `reCAPTCHA` widget.
11. Reviewed the Semrush App Center public pages and confirmed that public app listings, partner/developer links, and category links are exposed without immediate authentication.
12. Logged in with one owned Semrush account after the user completed the browser auth flow.
13. Reviewed the authenticated SEO toolkit landing page `https://www.semrush.com/seo/` and confirmed access to account-level navigation such as `Reports`, `App Center`, `My profile`, and `Create SEO project`.
14. Reviewed the authenticated landing page `https://www.semrush.com/home/` and confirmed that it exposed a project-start flow but no existing tenant objects or cross-account data.
15. Reviewed the authenticated reports area `https://www.semrush.com/my_reports` and confirmed that report-template flows are exposed through controlled URLs such as `/my_reports/constructor?...`, but no existing report objects from other users were exposed.
16. Reviewed the authenticated apps management page `https://www.semrush.com/apps/my-apps/` and confirmed that the account currently has `No active apps yet`, with no visible leakage of another user's subscriptions or trials.
17. Revisited the authenticated App Center store `https://www.semrush.com/apps/` and noted a UI inconsistency where top-level `Log In` and `Sign Up` labels remain visible even while the account can access `My Apps`; no actual authentication bypass or data exposure was confirmed from that inconsistency.
18. Reviewed authenticated network-request shape on `My Reports` and observed telemetry containing identifiers such as a `uid` and `semrush_team` label, but no directly exploitable authenticated API object exposure was confirmed in this round.
19. Stopped after this first authenticated round because the single-account surfaces did not produce a supportable finding and stronger access-control testing would likely require a second owned account or a more stateful project setup.

## 5. Evidence
- Important URLs:
  - `https://www.hackerone.com/bug-bounty-programs`
  - `https://hackerone.com/semrush`
  - `https://hackerone.com/semrush/policy_scopes?type=team`
  - `https://www.semrush.com/`
  - `https://www.semrush.com/pricing/`
  - `https://www.semrush.com/pricing/seo/`
  - `https://www.semrush.com/signup/?src=header`
  - `https://www.semrush.com/login/?src=header`
  - `https://www.semrush.com/apps/`
  - `https://www.semrush.com/seo/`
  - `https://www.semrush.com/home/`
  - `https://www.semrush.com/my_reports`
  - `https://www.semrush.com/apps/my-apps/`
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
  - Authenticated report/template links include controlled routes such as:
    - `/my_reports/constructor?action=openPopup&templateName=tc_ga4_atomic_widgets&type=template`
    - `/my_reports/constructor?accordionTab=integrations&action=openBlank&isPremium=false`
  - Authenticated `My Apps` state exposed:
    - `No active apps yet`
    - `Manage your subscriptions and trials`
- Screenshots / recordings: None yet.
- Notes:
  - The raw HTML of the direct HackerOne program page was fetched successfully and provided the official program title and description metadata.
  - The raw HTML of the direct HackerOne scope page was fetched successfully, but the asset details themselves still require client-side rendering.
  - The official HackerOne Bug Bounty Programs page was still useful as the direct source for the explicit owned-account testing rule.
  - The official Semrush public pages clearly expose account entry points and a seven-day trial path.
  - The rendered Semrush login and signup pages both visibly include `reCAPTCHA`, correcting the earlier raw-HTML-only assumption that captcha might not be present.
  - The pricing page exposes public plan and trial flows with `subscribe` URLs that include controlled `redirect_to` parameters, but no redirect issue was confirmed in this round.
  - The App Center exposes a larger public product surface, including app detail pages and partner/developer entry points, but no obvious unauthenticated issue was confirmed in this round.
  - The authenticated account gained access to `SEO`, `Home`, `Reports`, `App Center`, and `My Apps`, but the tested surfaces did not expose another user's data or shared objects in this round.
  - The authenticated App Center store still rendered top-level `Log In` / `Sign Up` labels while `My Apps` was accessible from the same session. This looked like a UI/session inconsistency, but no security impact was confirmed.
  - The authenticated `My Reports` network traffic included telemetry identifiers and product/team labels, but no directly actionable cross-account object exposure was confirmed.
  - No obvious unauthenticated vulnerability was observed on the reviewed public pages.

## 6. Outcome
- Result: No finding after pre-login and first authenticated round
- What worked:
  - Selected a second concrete HackerOne program and avoided retesting Basecamp.
  - Read the direct HackerOne Semrush program page through raw HTML instead of relying only on the general program listing.
  - Confirmed that the official HackerOne listing for Semrush includes a directly relevant rule: use only your own account(s).
  - Confirmed that Semrush exposes public signup and login entry points and a trial workflow.
  - Confirmed that the fetched signup and login pages expose concrete auth application scaffolding rather than only marketing redirects.
  - Confirmed that one owned account can access the SEO, Home, Reports, and App Center surfaces without obvious cross-account exposure in the tested paths.
- What failed:
  - Could not perform true horizontal access-control testing because only one owned account was available.
  - Could not confirm richer tenant, workspace, or shared-project behavior because the current account had no existing apps, reports, or collaborative objects.
- Why no finding was confirmed yet:
  - The first authenticated round still covered mostly empty or starter-state account surfaces.
  - The most promising Semrush bug classes likely require either more populated account objects or a second owned account for comparison.

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
  - If the team wants to continue later, prepare a second owned Semrush account or create richer in-account objects such as reports, projects, or app trials.
  - Revisit account/workspace boundary testing once there are comparable objects or multi-account surfaces.
- Questions for teammates:
  - Should Semrush be revisited only after a second account exists, or should the team move on now?
- Whether to keep testing this target: Pause for now unless the team wants to invest in a richer multi-account setup.

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
