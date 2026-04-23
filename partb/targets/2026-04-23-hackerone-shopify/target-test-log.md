# Target Test Log

## 1. Basic Information
- Date: 2026-04-23
- Platform: HackerOne
- Program: Shopify
- Program URL: `https://hackerone.com/shopify`
- Program overview section reviewed: Yes
- Key overview excerpt:
  - `Shopify - Bug Bounty Program | HackerOne`
  - `The Shopify Bug Bounty Program enlists the help of the hacker community at HackerOne to make Shopify more secure.`
- Scope proof:
  - The teacher-approved platform is HackerOne.
  - The direct HackerOne Shopify program page raw HTML was fetched and reviewed.
  - The recorded program page URL is `https://hackerone.com/shopify?type=team`.
- Scope page URL: `https://hackerone.com/shopify/policy_scopes?type=team`
- Official website URL: `https://www.shopify.com/`
- Target / asset tested: Direct HackerOne program page and Shopify official public website
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Likely yes for higher-value authenticated testing, but the public site already exposes many application entry points.
- Account type(s) used: None.
- Registration/login notes:
  - The Shopify homepage exposes public `Log in` and `Start for free` entry points.
  - The `Start for free` call-to-action points to `admin.shopify.com`.
  - The public homepage also exposes clear links to `Customer Accounts`, `Shopify Admin`, `Shopify App Store`, and `shopify.dev`.
- Role setup: Not started.
- Whether a second account or second role may be needed:
  - Likely yes for deeper merchant-to-merchant or collaborator-boundary testing later.

## 3. Testing Summary
- Main functionality tested:
  - Direct HackerOne Shopify program page
  - Shopify official homepage
  - Shopify account login entry
  - Shopify admin entry
  - Shopify developer documentation
  - Shopify App Store homepage
  - Shopify App Store app-detail page
- Vulnerability classes attempted:
  - Program intake and scope lock
  - Attack-surface mapping
  - Pre-auth UI review
  - Pre-auth DevTools deep dive
  - Access control / object exposure pre-check
  - Auth / session / invite / reset / verification flow pre-check
  - Token / redirect / parameter-shape pre-check
  - Misconfiguration / unsafe exposure pre-check
- Baseline checklist status:
  - 1. Access control / IDOR / object exposure: Pre-auth object and app-detail surface review completed; no direct unauthenticated exposure confirmed.
  - 2. Authentication / session / invite / reset / verification / OAuth flow flaws: Public auth-entry and login-initiation review completed.
  - 3. Token / nonce / randomness / binding quality: Initial `rid`, `verify`, `redirect_uri`, `return_to`, and signup parameter review completed.
  - 4. Input handling / XSS / unsafe reflection: Initial parameter and reflected-route review completed on App Store surfaces.
  - 5. Misconfiguration / unsafe exposure / business logic boundary: Initial public-surface and developer/app-store exposure review completed.
- Customized testing ideas:
  - Review whether `admin.shopify.com`, customer-account surfaces, or storefront/admin boundaries expose lower-friction public flows later.
  - Review whether app-store, developer, or admin-linked sub-surfaces offer better starting points than the main marketing site.
- Tools used: Direct HackerOne raw HTML fetch, Shopify homepage review, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Continued the Part B workflow using the independent-only approach.
2. Selected `Shopify` as the next HackerOne target candidate after excluding invalid or high-friction candidates.
3. Fetched and reviewed the raw HTML of the direct HackerOne program page at `https://hackerone.com/shopify?type=team`.
4. Extracted and recorded the program title and official description metadata from the direct HackerOne page.
5. Reviewed the public Shopify homepage at `https://www.shopify.com/`.
6. Confirmed that the homepage exposes multiple concrete public entry points, including `Log in`, `Start for free`, `Customer Accounts`, `Shopify Admin`, `Shopify App Store`, and `shopify.dev`.
7. Noted that the homepage appears significantly richer in publicly visible product surfaces than some earlier marketing-heavy targets.
8. Opened the Shopify account login flow and confirmed that public login resolves to `accounts.shopify.com/lookup` with request-specific `rid` and `verify` query parameters.
9. Opened `https://admin.shopify.com/` and confirmed that the public admin entry also redirects into the same Shopify account login flow rather than exposing admin content anonymously.
10. Opened `https://shopify.dev/docs` and confirmed that the developer documentation is publicly accessible and exposes links for `Apps`, `Storefronts`, `Agents`, `Help`, and `Log in`.
11. Verified the `shopify.dev` login entry by opening `http://dev.shopify.com/dashboard` directly and confirmed it redirects into the Shopify accounts login flow rather than exposing a developer dashboard anonymously.
12. Opened `https://apps.shopify.com/?locale=zh-CN` and confirmed that the App Store is publicly accessible, includes a search field, category pages, login/start links, and many app-detail pages.
13. Opened a specific App Store listing page and reviewed its public parameterized URL structure and visible content:
   - `https://apps.shopify.com/affliate-by-secomapp?...`
14. Confirmed that the app-detail page remains publicly viewable as expected, that login/install actions redirect into Shopify-controlled auth flows, and that no obvious open redirect, unauthenticated data leak, or abnormal parameter-driven behavior was observed in this round.
15. Re-opened Shopify using the enhanced workflow and reviewed the rendered HackerOne program page directly in Chrome rather than relying only on raw HTML.
16. Confirmed high-value program rules on the rendered HackerOne page:
   - create a Shopify account using the program's bug-bounty signup link
   - test only against stores created by the researcher
   - Shopify explicitly evaluates IDOR based on identifier predictability, data accessed, and overall impact
17. Reviewed the rendered Shopify homepage and mapped broader attack surfaces including customer accounts, discounts, analytics, Shopify admin, App Store, developer docs, and partner/developer pathways.
18. Reviewed the rendered Shopify developer docs and confirmed public access to `Apps`, `Storefronts`, `Agents`, CLI setup, App Store requirements, Theme Store requirements, and a developer `Log in` flow.
19. Opened the public developer login route `http://dev.shopify.com/dashboard` and confirmed it redirects into `accounts.shopify.com/lookup` with request-specific `rid` and `verify` parameters.
20. Reviewed the rendered Shopify account-login page and confirmed:
   - email continuation
   - passkey login
   - Apple, Facebook, and Google external login links
   - signup link bound to the same `rid`
   - active `hCaptcha` network traffic on the page
21. Revisited the public App Store homepage through DevTools and confirmed login and signup initiation routes with explicit `redirect_uri` and `return_to` parameters.
22. Reviewed the public App Store app-detail page in more depth and confirmed:
   - `login/initiate_shopify_auth_without_shop?...redirect_uri=...&return_to=...`
   - app-detail comments/reviews surfaces
   - partner/developer profile links
   - demo store links
   - public data-access descriptions for the app
23. Noted that the app-detail page exposes a public demo-store link containing a long encoded `_bt` parameter and references to `permanent_password_bypass`, but no exploitable unauthorized access was confirmed in this round.
24. Stopped after the strengthened pre-auth round because no directly supportable unauthenticated issue was confirmed and the next valuable step is likely authenticated testing with an owned Shopify bug-bounty store account.

## 4A. Attack Surface Map
- Public surfaces discovered:
  - `www.shopify.com/au`
  - `apps.shopify.com`
  - specific App Store listing pages
  - `shopify.dev/docs`
- Auth-related surfaces discovered:
  - `https://www.shopify.com/login?ui_locales=en-AU`
  - `https://accounts.shopify.com/lookup?...rid=...&verify=...`
  - App Store login-initiation flow
  - App Store signup/store-create flow
- Object-bearing surfaces discovered:
  - App listing slugs such as `/affliate-by-secomapp`
  - partner profile links
  - review-filter links
  - demo-store link on app pages
- Billing / subscription / trial surfaces discovered:
  - `Start free trial` / `store-create`
  - app pricing sections on listing pages
  - trial-related CTAs on homepage and App Store
- Sharing / invitation / collaboration surfaces discovered:
  - not yet confirmed in unauthenticated testing
- App / developer / API / docs surfaces discovered:
  - `shopify.dev/docs`
  - App Store
  - API documentation links
  - Theme Store and partner links

## 4B. DevTools Deep-Dive Notes
- Key pages inspected with DevTools:
  - HackerOne Shopify program page
  - Shopify homepage
  - Shopify developer docs
  - Shopify App Store homepage
  - Shopify App Store app-detail page
  - Shopify account login page
- Important XHR / fetch endpoints:
  - `https://apps.shopify.com/.well-known/dux`
  - `https://monorail-edge.shopifysvc.com/v1/produce`
  - `https://accounts.shopify.com/.well-known/dux`
  - `https://api2.hcaptcha.com/getcaptcha/...`
- Interesting object IDs / UUIDs / team or workspace markers:
  - login flow uses per-request `rid` UUID-like values
  - login flow uses `verify` values tied to the request
- Tokens / nonce / state / CSRF observations:
  - App Store login-initiation URLs carry `redirect_uri` and `return_to`
  - account login/signup flow carries `rid`
  - developer login redirects into account lookup with `rid` and `verify`
- Hidden parameters / embedded state / frontend routes:
  - `store-create` links include `_s`, `_y`, `signup_page`, and `signup_types[]`
  - App Store listing URLs include `surface_detail`, `surface_inter_position`, `surface_intra_position`, `surface_type`, and `surface_version`
  - demo-store link exposed a long encoded `_bt` parameter
- Bundle / source / feature-flag observations:
  - no directly actionable feature-flag or hidden-route leak was confirmed in this round.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/shopify?type=team`
  - `https://hackerone.com/shopify/policy_scopes?type=team`
  - `https://www.shopify.com/`
  - `https://accounts.shopify.com/store-login`
  - `https://admin.shopify.com/`
  - `https://apps.shopify.com/`
  - `https://shopify.dev/`
  - `https://shopify.dev/docs`
  - `http://dev.shopify.com/dashboard`
  - `https://apps.shopify.com/affliate-by-secomapp?...`
  - `https://accounts.shopify.com/lookup?rid=...&verify=...`
- Important requests/responses:
  - HackerOne Shopify program page raw HTML includes:
    - `Shopify - Bug Bounty Program | HackerOne`
    - `The Shopify Bug Bounty Program enlists the help of the hacker community at HackerOne to make Shopify more secure.`
  - Shopify homepage includes:
    - `Log in`
    - `Start for free`
    - `Customer Accounts`
    - `Shopify App Store`
    - `shopify.dev`
    - references to `Shopify Admin`
  - Shopify account-login observations:
    - public login resolves to `accounts.shopify.com/lookup`
    - login flow includes request-specific `rid` and `verify` query parameters
    - login options include email continuation and third-party identity providers
    - rendered login page loads `hCaptcha`
  - Shopify developer docs observations:
    - `shopify.dev/docs` is publicly readable
    - visible `Log in` entry routes into Shopify-controlled login flow
    - no anonymous dashboard content was exposed
  - Shopify App Store observations:
    - homepage and app-detail pages are publicly readable
    - login/install actions route into Shopify-controlled auth flows
    - login initiation includes `redirect_uri` and `return_to`
    - parameterized listing URLs did not show obvious unsafe behavior in this round
    - app-detail pages expose partner links, review filters, data-access summaries, and demo-store links
- Screenshots / recordings: None yet.
- Notes:
  - The direct HackerOne program page was readable through raw HTML and was reviewed first.
  - The rendered HackerOne program page is especially useful for Shopify because it explicitly tells researchers to create accounts through the bug-bounty signup link and only test on stores they created.
  - Shopify explicitly notes that IDOR eligibility depends on identifier predictability, accessed data, and overall impact.
  - The official homepage exposed a broader public product surface than Privy or Quora.
  - This makes Shopify more promising as a future independent-only candidate than some previously reviewed programs, even though no vulnerability was confirmed in this first round.
  - The developer docs and App Store are both useful public surfaces, but this pass still did not reveal a concrete auth flaw, data leak, or direct-object exposure.
  - Shopify login and signup appear guarded by `hCaptcha`, so a later authenticated round will probably require manual registration/login by the user.

## 6. Outcome
- Result: No finding
- What worked:
  - Confirmed the direct HackerOne Shopify program page and official program description.
  - Confirmed that Shopify's public homepage exposes several concrete product and admin-adjacent entry points.
  - Identified Shopify as a richer public-surface candidate than more marketing-only or immediately challenge-gated targets.
  - Confirmed that both public login and public admin entry points redirect into Shopify-controlled account-auth flows rather than exposing protected content.
  - Confirmed that `shopify.dev/docs` is a rich public developer surface and that its visible login entry also routes into controlled auth.
  - Confirmed that App Store listing pages are publicly readable but that install/login actions remain behind Shopify auth flows.
- What failed:
  - Did not begin authenticated testing.
  - Did not find an obvious open redirect, unauthenticated object exposure, or parameter-handling issue in the App Store surfaces reviewed.
- Why no finding was confirmed yet:
  - This round focused on public login/admin/dev/app-store entry points plus DevTools analysis only.
  - The more interesting Shopify bug classes likely require deeper follow-up inside authenticated merchant, developer, or partner contexts.
  - Shopify itself instructs researchers to create and test only against their own bug-bounty stores, which points directly toward an authenticated next step.

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
  - Direct HackerOne Shopify program page
  - Shopify official homepage
- Initial novelty judgment: Not applicable yet.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - Shopify is being assessed through the teacher-approved HackerOne platform.
  - The direct HackerOne Shopify program page was fetched and recorded before reviewing the target site.
- Safety constraints followed:
  - Only the public HackerOne program page and the public Shopify homepage were reviewed in this round.
  - No authenticated actions, brute force, or disruptive requests were used.
- Actions avoided:
  - No attempts against non-confirmed Shopify assets outside the clearly linked public surfaces.
  - No form submissions or account-creation attempts.

## 10. Next Actions
- Follow-up validation needed:
  - If the team wants to continue, open the official bug-bounty signup path for Shopify and let the user register/login a compliant owned store account.
  - After login, test merchant/admin/object surfaces with the enhanced workflow.
- Questions for teammates:
  - Should Shopify now become the next authenticated target?
- Whether to keep testing this target: Yes. The next valuable step is authenticated testing, not more public-page review.
- Next recommended testing state:
  - Request login / registration

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
