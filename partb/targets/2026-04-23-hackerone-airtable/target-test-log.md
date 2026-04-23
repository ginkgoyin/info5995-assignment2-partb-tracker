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
  - Authenticated Airtable home / onboarding page
  - Authenticated account settings page
  - Authenticated Builder Hub API-key page
  - Authenticated personal access tokens page
  - Authenticated personal access token creation form
  - Authenticated OAuth integrations page
  - Authenticated OAuth integration registration form
  - Public developer API reference page
- Vulnerability classes attempted:
  - Program intake and scope lock
  - Attack-surface mapping
  - Pre-auth UI review
  - Pre-auth DevTools deep dive
  - Access control / object exposure pre-check
  - Auth / session / invite / reset / verification flow pre-check
  - Token / parameter / binding pre-check
  - Misconfiguration / exposure pre-check
  - Authenticated account-state confirmation
  - Authenticated object and builder-surface review
  - Authenticated token / OAuth / integration surface review
- Baseline checklist status:
  - 1. Access control / IDOR / object exposure: No direct unauthenticated exposure confirmed pre-auth; authenticated round exposed stable user and workspace identifiers but no cross-account access issue yet.
  - 2. Authentication / session / invite / reset / verification / OAuth flow flaws: Public login/signup review completed; authenticated OIDC/session flows and OAuth builder surfaces reviewed.
  - 3. Token / nonce / randomness / binding quality: Initial login/signup parameter review completed; authenticated round exposed PKCE/OIDC parameters and developer-token surfaces.
  - 4. Input handling / XSS / unsafe reflection: No obvious public reflected input issue confirmed in this round.
  - 5. Misconfiguration / unsafe exposure / business logic boundary: Public-surface and developer/API exposure review completed; authenticated builder/account pages reviewed.
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
16. After the user logged in, confirmed access to an authenticated Airtable home screen with a first-run onboarding / app-building experience.
17. Confirmed that the logged-in home flow exposed multiple authenticated user requests containing a stable Airtable user identifier:
   - `usrKlp7S1Dg3HN8Eq`
18. Confirmed authenticated home requests that enumerate user-scoped state:
   - `getFavorites`
   - `getMostRecentlyViewed`
   - `getMostRecentlyOpenedWorkspaces`
   - `listApplicationsAndPageBundlesForDisplay`
19. Observed a full silent-auth OIDC flow in the authenticated session including:
   - `app.airtable.com/auth/oidc/silentAuth`
   - `airtable.com/oidc/auth?...code_challenge=...&nonce=...&state=...`
   - `app.airtable.com/auth/oidc/callback?code=...&state=...`
20. Opened the authenticated account settings page and confirmed:
   - account identity `Erin YIN`
   - email `erin1xiaohao@gmail.com`
   - workspace link `My First Workspace`
   - workspace identifier `wsps10QrDKzva8Ijq`
   - account-level API, referrals, recent activity, privacy, and delete-account surfaces
21. Reviewed the authenticated account page network activity and confirmed additional account-scoped endpoints:
   - `getGoogleDriveIntegrationState`
   - `getLastSyncedToGoogleDriveTime`
22. Confirmed that the account page exposes an API/developer hub handoff instead of directly rendering secrets in place.
23. Opened the authenticated Builder Hub API-key page at `https://airtable.com/create/apikey` and confirmed:
   - API keys are deprecated
   - the page exposes links to `Personal access tokens`, `Custom extensions`, `Secrets`, and `OAuth integrations`
24. Reviewed the Builder Hub API-key page network activity and confirmed:
   - `getApiKey`
   - no active API key value was exposed in the UI during this round
25. Opened the authenticated personal access tokens page and confirmed:
   - `You have no personal access tokens`
   - `Create token`
26. Reviewed the personal access tokens page network activity and confirmed:
   - `getPersonalAccessTokensForDevelopersHub`
   - `getEnterpriseAccountsUserIsAdminOf`
27. Opened the authenticated OAuth integrations page and confirmed:
   - `You have no OAuth integrations`
   - `Register an OAuth integration`
28. Reviewed the OAuth page network activity and confirmed:
   - `getOauthApplicationsForDevelopersHub`
29. Opened the personal access token creation form directly and confirmed it requires explicit user-provided input before creation:
   - token `Name`
   - `Add a scope`
   - explicit `Access`
   - disabled `Create token` button until required input is provided
30. Confirmed the token creation page explicitly states:
   - the token grants access only to selected workspaces and bases
   - the token also grants access to some non-workspace/base endpoints
   - the user can only grant access to bases and workspaces they already have access to
31. Opened the OAuth integration registration form directly and confirmed it requires explicit user-provided registration fields:
   - integration `Name`
   - `OAuth redirect URLs`
   - `Add redirect URL`
   - `Register integration`
32. Confirmed the OAuth registration form does not pre-populate redirect URIs or silently register an integration in its initial state.
33. Opened the public developer API reference and confirmed the visible existence of developer documentation for:
   - authentication
   - scopes
   - OAuth reference
   - rate limits
   - records
   - collaborators / invites / shares
   - workspaces / users / audit logs
34. Stopped after the deeper authenticated follow-up because no supportable access-control bypass, exposed secret, token leak, redirect weakness, or account-boundary failure was confirmed without actually creating a token or registering an OAuth integration.

### Later deeper validation (concise)
- Tried abnormal OAuth registration input on `https://airtable.com/create/oauth/new`
  - Input used: `javascript:alert(1)` as the redirect URL
  - Result: server rejected it with HTTP `422`
  - Server message: `Invalid redirect URL: must use https; http is allowed for localhost only`
- Confirmed the rejection came from the Airtable backend endpoint:
  - `POST /v0.3/user/usrKlp7S1Dg3HN8Eq/createOauthApplication`
- Re-checked authenticated home-session behavior after reload
  - Observed a repeated `silentAuth` path
  - Did not observe an obvious reusable or leaked `state/nonce/code_challenge` issue in this same-session reload check

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
  - `app.airtable.com/auth/oidc/silentAuth`
  - `app.airtable.com/auth/oidc/callback`
  - `https://airtable.com/oidc/auth`
- Object-bearing surfaces discovered:
  - account settings
  - `My First Workspace`
  - Builder Hub developer surfaces
  - token and OAuth management pages
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
  - `https://airtable.com/create/apikey`
  - `https://airtable.com/create/tokens`
  - `https://airtable.com/create/tokens/new`
  - `https://airtable.com/create/oauth`
  - `https://airtable.com/create/oauth/new`
  - `https://airtable.com/create/secrets`
  - `https://airtable.com/create/extensions`

## 4B. DevTools Deep-Dive Notes
- Key pages inspected with DevTools:
  - `https://www.airtable.com/`
  - `https://airtable.com/login`
  - `https://airtable.com/signup`
  - `https://airtable.com/`
  - `https://airtable.com/account`
  - `https://airtable.com/create/apikey`
  - `https://airtable.com/create/tokens`
  - `https://airtable.com/create/tokens/new`
  - `https://airtable.com/create/oauth`
  - `https://airtable.com/create/oauth/new`
  - `https://airtable.com/developers/web/api/introduction`
- Important XHR / fetch endpoints:
  - `https://airtable.com/internal/stats-batch`
  - `https://collector-px0ozadu9k.px-cloud.net/api/v2/collector`
  - multiple Airtable-hosted static bundle endpoints under `https://static.airtable.com/`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getFavorites`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getMostRecentlyViewed`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getMostRecentlyOpenedWorkspaces`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/listApplicationsAndPageBundlesForDisplay`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getAiPermissionsStateData`
  - `https://airtable.com/account/getGoogleDriveIntegrationState`
  - `https://airtable.com/account/getLastSyncedToGoogleDriveTime`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getApiKey`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getPersonalAccessTokensForDevelopersHub`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getEnterpriseAccountsUserIsAdminOf`
  - `https://airtable.com/v0.3/user/usrKlp7S1Dg3HN8Eq/getOauthApplicationsForDevelopersHub`
- Interesting object IDs / UUIDs / team or workspace markers:
  - user ID: `usrKlp7S1Dg3HN8Eq`
  - workspace ID: `wsps10QrDKzva8Ijq`
- Tokens / nonce / state / CSRF observations:
  - Authenticated session used a silent OIDC flow with visible `code_challenge`, `nonce`, and `state` parameters.
  - No immediately actionable token-binding failure or leak was confirmed in this round.
- Hidden parameters / embedded state / frontend routes:
  - Login route loaded `run_signin.js`.
  - Signup route loaded `run_multistep_signup.js`.
  - Authenticated builder pages are routed under `/create/...`.
- Bundle / source / feature-flag observations:
  - Both login and signup flows are substantial Airtable-hosted frontend applications, not trivial redirect stubs.
  - `PerimeterX` is present in both login and signup flows, implying anti-automation or bot-detection logic.
  - Authenticated account and builder pages expose a stable internal object model around user and workspace IDs, which is useful for later access-control testing.
  - The deeper builder forms did not expose unsafe defaults in their initial state; token creation and OAuth registration both required explicit user-provided inputs.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/airtable?type=team`
  - `https://hackerone.com/airtable/policy_scopes?type=team`
  - `https://www.airtable.com/`
  - `https://airtable.com/login`
  - `https://airtable.com/signup`
  - `https://airtable.com/account`
  - `https://airtable.com/create/apikey`
  - `https://airtable.com/create/tokens`
  - `https://airtable.com/create/tokens/new`
  - `https://airtable.com/create/oauth`
  - `https://airtable.com/create/oauth/new`
  - `https://airtable.com/developers`
  - `https://airtable.com/developers/web/api/introduction`
- Important requests/responses:
  - HackerOne page title: `Airtable | Bug Bounty Program Policy | HackerOne`
  - Program page visible stat: `Assets In Scope: 5`
  - Login page bundle: `run_signin.js`
  - Signup page bundle: `run_multistep_signup.js`
  - Anti-automation traffic: `PerimeterX` init and collector requests
  - Airtable internal telemetry endpoint: `airtable.com/internal/stats-batch`
  - Authenticated home/user-scoped requests included user ID `usrKlp7S1Dg3HN8Eq`
  - Account page exposed workspace link `My First Workspace` with workspace ID `wsps10QrDKzva8Ijq`
  - Builder Hub API-key page called `getApiKey`
  - Token page called `getPersonalAccessTokensForDevelopersHub`
  - OAuth page called `getOauthApplicationsForDevelopersHub`
  - Authenticated silent-auth flow exposed OIDC `code_challenge`, `nonce`, and `state` parameters in the browser requests
  - Token creation form required explicit scopes and access selection before creation
  - OAuth integration registration form required explicit redirect URLs before registration
- Screenshots / recordings: None yet.
- Notes:
  - This is a valid next-step target under the current workflow.
  - The current state is ideal for handing off to user registration/login before deeper authenticated testing.
  - Later testing moved beyond UI observation and included an actual malformed OAuth redirect-URL submission.

## 6. Outcome
- Result: No finding yet after pre-auth round
  and first authenticated round
- What worked:
  - Confirmed a valid Airtable HackerOne program page.
  - Confirmed real signup and login flows.
  - Confirmed useful developer/API surfaces for later authenticated follow-up.
  - Confirmed authenticated access to Airtable home, account settings, and Builder Hub.
  - Confirmed stable user and workspace identifiers in authenticated requests and account settings.
  - Confirmed developer/token/OAuth management surfaces exist and are reachable from the logged-in account.
  - Confirmed that the deeper token and OAuth creation forms do not expose secrets or silently pre-authorized objects in their initial state.
  - Confirmed by direct submission that Airtable's OAuth registration endpoint rejects a clearly unsafe redirect URL scheme.
- What failed:
  - No unauthenticated object exposure, redirect issue, or obvious reflected-input issue was confirmed in this round.
  - No exposed secret, reusable token, cross-account object access, or privilege-boundary flaw was confirmed in the authenticated round.
  - Same-session reload did not reproduce a new full OIDC auth chain that exposed reusable `state/nonce/code_challenge` values for comparison.
- Why no finding was confirmed yet:
  - The higher-value Airtable attack surface likely sits behind authenticated workspace/base/account contexts.
  - The current account is still in a very early state with only one visible workspace and no richer collaboration/base-sharing objects to compare.
  - The deeper token and OAuth checks reached form-validation state, but not creation state, because creating credentials or integrations would materially alter the account.
- Whether this target should be revisited only with a richer account state or second account:
  - Very likely yes if the team wants to push deeper into workspace sharing, invites, and API-boundary testing later.

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
  - Public HackerOne pages and authenticated Airtable pages already opened by the user were reviewed.
  - No token was created and no OAuth integration was registered.
- Actions avoided:
  - No brute force.
  - No token creation.
  - No OAuth integration registration.

## 10. Next Actions
- Follow-up validation needed:
  - If the team wants to continue later, create richer test state inside the workspace and possibly prepare a second Airtable account for sharing/collaboration checks.
- Questions for teammates:
  - Should Airtable be revisited after creating a second account or shared base/workspace?
- Whether to keep testing this target:
  - Pause for now unless the team wants to invest in richer Airtable account state.
- Next recommended testing state:
  - Move to next program

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
