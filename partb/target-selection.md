# Part B Target Selection

Date: 2026-04-30

## Scope of this document

This is a curated target-selection document, not an exhaustive catalog of every program on every approved platform.

It uses only sources approved by the assignment spec:
- approved bug bounty platforms named in the spec
- explicitly allowed company-run programs named in the spec
- official program pages, platform rules, and official product/help pages

This shortlist is prioritized for:
- safe, low-risk testing
- normal user-account workflows where possible
- passive/manual checking before any deeper authenticated work
- clear evidence collection for Part B presentation and Q&A

## Platform severity references

- `HackerOne`: `None / Low / Medium / High / Critical`, with manual severity and CVSS support
  - Source: [HackerOne Severity](https://docs.hackerone.com/en/articles/8475343-severity)
- `GitHub Bug Bounty`: GitHub reward bands and severity guidelines
  - Source: [GitHub Rewards](https://bounty.github.com/rewards.html)
- `OpenAI Security Bug Bounty`: OpenAI security issues are managed through Bugcrowd; OpenAI also runs a separate Safety Bug Bounty
  - Sources: [OpenAI Bug Bounty](https://openai.com/index/bug-bounty-program/), [OpenAI Safety Bug Bounty](https://openai.com/index/safety-bug-bounty/), [Bugcrowd VRT](https://www.bugcrowd.com/products/vulnerability-rating-taxonomy/)
- `Anthropic Model Safety Bug Bounty`: sliding-scale bounty up to `$35,000` for novel universal jailbreaks
  - Source: [Anthropic Model Safety Bug Bounty](https://support.anthropic.com/en/articles/12119250-model-safety-bug-bounty-program)

## Priority bands

### Priority 1: Best fit for beginner-safe Part B work

These are the strongest current options if the goal is safe, reproducible testing with ordinary product workflows and relatively good evidence collection.

### 1. Airtable

- Program name: `Airtable`
- Platform: `HackerOne`
- In-scope assets:
  - confirmed program page and scope page on HackerOne
  - Airtable web app, login/signup, account settings, builder/developer surfaces already reached in prior official-page review
  - visible developer/API surfaces such as tokens, OAuth, account, and workspace-related pages
- Out-of-scope assets:
  - anything outside the listed Airtable program assets on the HackerOne security page
  - third-party systems not owned by Airtable
  - any non-owned user/workspace/base data
- Forbidden testing:
  - brute force
  - destructive testing
  - creating credentials or integrations just to force risky behavior unless clearly necessary and still safe
  - cross-account testing on assets you do not own
- Severity model: `HackerOne`
- Beginner suitability: `High`
  - Why: clean login/signup flow, strong product structure, and many interesting but low-risk pages that can be manually reviewed first
- Likely safe vulnerability classes to check:
  - share/invite/workspace boundary checks
  - account/workspace/base object-boundary checks
  - OAuth redirect validation
  - session/OIDC parameter handling
  - passive review of PAT/OAuth/token creation forms without completing dangerous actions
- Notes on evidence required:
  - capture exact workspace/base/object IDs
  - explain why the target object should not be reachable
  - if testing redirects or OIDC, record exact parameters and server response
  - if claiming access-control impact, show both authorized context and unauthorized context

### 2. Shopify

- Program name: `Shopify`
- Platform: `HackerOne`
- In-scope assets:
  - official Shopify HackerOne program
  - public Shopify surfaces already identified from official review: `shopify.com`, `apps.shopify.com`, `shopify.dev`, partner dashboard flows
  - self-created bug-bounty stores and partner account flows
- Out-of-scope assets:
  - stores or data not created/owned by the researcher
  - assets outside the listed Shopify program scope
  - third-party apps or merchants not covered by the program
- Forbidden testing:
  - testing against other merchants' stores or data
  - disruptive actions on shared or production-like objects
  - brute force or automation against guarded auth flows
- Severity model: `HackerOne`
- Beginner suitability: `Medium`
  - Why: rich target surface, but stronger findings usually need a self-created store and sometimes a second account
- Likely safe vulnerability classes to check:
  - partner/store/collaborator boundary behavior
  - app-store redirect and login-initiation validation
  - invite and role-boundary checks on owned stores
  - developer/admin object references
  - low-risk parameter review on app-store and login flows
- Notes on evidence required:
  - prove the store/account was self-created and in scope
  - document exact partner/store/member IDs
  - if IDOR is claimed, show how the identifier was obtained and why access should fail
  - record the full redirect chain for auth-related findings

### 3. Basecamp

- Program name: `Basecamp`
- Platform: `HackerOne`
- In-scope assets:
  - confirmed from prior official review: `3.basecamp.com`, `launchpad.37signals.com`
  - Basecamp account, project, people-management, and invite-role flows
- Out-of-scope assets:
  - assets not listed on the Basecamp program security page
  - non-owned accounts, projects, and data
  - unrelated 37signals services unless explicitly listed
- Forbidden testing:
  - bypassing CAPTCHA
  - social engineering
  - testing outside owned accounts/projects
  - destructive actions on real project content
- Severity model: `HackerOne`
- Beginner suitability: `Medium`
  - Why: strong access-control potential, but best results likely need multi-role testing
- Likely safe vulnerability classes to check:
  - client/collaborator/internal-staff role separation
  - project object direct access
  - invite flow logic
  - account/admin CSRF or authorization boundary checks
  - session and login redirect handling
- Notes on evidence required:
  - show the exact role of each test account
  - preserve URLs for project/message/file objects
  - capture before/after authorization expectations
  - if using isolated-browser checks, show the redirect or denial behavior clearly

### 4. TripAdvisor

- Program name: `TripAdvisor`
- Platform: `HackerOne`
- In-scope assets:
  - currently listed on HackerOne public bug bounty listing
  - public consumer site
  - public owner/management-center path references such as `tripadvisor.com/Owners`
- Out-of-scope assets:
  - anything not covered by the TripAdvisor program security page once live scope is confirmed
  - listings, owner accounts, or business data not owned by the researcher
  - third-party services linked from TripAdvisor
- Forbidden testing:
  - claiming, verifying, or modifying business listings you do not own
  - phone/email/credit-card verification abuse
  - high-volume review or listing manipulation
- Severity model: `HackerOne`
- Beginner suitability: `Medium`
  - Why: public surface is easy to enter, but stronger findings likely depend on owner/listing-management workflows
- Likely safe vulnerability classes to check:
  - listing-management object-boundary checks
  - reviewer/owner role boundary issues
  - owner/login/verification flow logic
  - session or redirect handling on owner-side portals
- Notes on evidence required:
  - confirm live scope page before deeper testing
  - if testing owner-side flows, prove the account/listing is owned by the researcher
  - preserve exact listing or owner object references
  - any verification-flow claim needs precise step-by-step evidence

## Priority 2: Good targets, but with more setup or narrower safe surface

### 5. GitHub

- Program name: `GitHub Bug Bounty`
- Platform: `GitHub company-run program`
- In-scope assets:
  - `github.com` and listed subdomains except the explicit exclusions
  - `githubassets.com`, `githubusercontent.com`, `githubapp.com`, `githubwebhooks.net`, `github.net`, `npmjs.com`, `npmjs.org`
  - GitHub CLI, Desktop, Mobile, Enterprise Server, and Enterprise Cloud as listed
- Out-of-scope assets:
  - excluded `github.com` and `githubapp.com` subdomains from the scope page
  - many open-source repositories
  - GitHub Pages hosted user content
  - vulnerabilities requiring local access or obvious social engineering
- Forbidden testing:
  - social engineering, phishing, and physical attacks
  - DDoS/network flooding
  - issues that only affect user-controlled GitHub Pages content
  - most open-source upstream dependency issues
- Severity model: `GitHub Bug Bounty` reward structure
- Beginner suitability: `Medium`
  - Why: the rules are very clear and official docs are strong, but the product is heavily researched and many trivial ideas are already ineligible
- Likely safe vulnerability classes to check:
  - web auth/session/account-boundary issues on owned repos/accounts
  - CSRF on sensitive owned-account actions
  - private-repo or private-package access control using owned resources
  - clearly scoped mobile/Desktop/CLI flows only if you can reproduce them safely
- Notes on evidence required:
  - must tie the issue to a listed in-scope asset
  - if private data is involved, stop once enough proof exists
  - GitHub wants high-quality reproduction and clear impact on user or platform security

### 6. WordPress

- Program name: `WordPress`
- Platform: `HackerOne`
- In-scope assets:
  - WordPress Core and the related projects/infrastructure listed by the official program
  - owned WordPress.org account workflows where applicable
- Out-of-scope assets:
  - random third-party WordPress sites
  - plugins/themes/infrastructure not listed in the official scope
  - content or accounts you do not own
- Forbidden testing:
  - attacking unrelated WordPress-powered sites
  - account abuse outside owned accounts
  - destructive testing on shared community infrastructure
- Severity model: `HackerOne`
- Beginner suitability: `Medium`
  - Why: big attack surface and familiar web flows, but the ecosystem creates scope traps
- Likely safe vulnerability classes to check:
  - account settings and session logic on owned accounts
  - object-boundary issues in clearly in-scope APIs
  - OAuth or linked-account flow validation
  - CSRF on owned-account changes
- Notes on evidence required:
  - prove the exact asset is in the WordPress program scope
  - if API behavior is involved, capture request, auth context, and expected authorization rule

### 7. Dropbox

- Program name: `Dropbox`
- Platform: `HackerOne`
- In-scope assets:
  - official Dropbox program assets and owned-account web flows
  - file request and sharing-related surfaces if listed in scope
- Out-of-scope assets:
  - other users' files and accounts
  - assets not listed on the Dropbox HackerOne scope page
- Forbidden testing:
  - high-volume token guessing
  - modifying or accessing non-owned files
  - risky upload content or destructive sharing changes
- Severity model: `HackerOne`
- Beginner suitability: `Medium`
  - Why: owned-account testing is practical, but safe access-control work needs discipline
- Likely safe vulnerability classes to check:
  - file-request token behavior
  - sharing boundary checks on self-created objects
  - auth/session behavior on owned resources
  - low-risk input handling on request titles or similar owned metadata
- Notes on evidence required:
  - prove the object was self-created
  - if testing token mutation, keep it low frequency and record exact behavior
  - if an access-control issue appears, stop once you have enough proof

## Priority 3: Approved, but poor fit for beginner-safe normal-account testing

These are allowed by the spec, but they are not currently the best targets if the team wants low-risk, ordinary-account, evidence-friendly testing.

### 8. OpenAI

- Program name: `OpenAI Bug Bounty Program` and `OpenAI Safety Bug Bounty`
- Platform: `OpenAI company-run program`
- In-scope assets:
  - OpenAI systems covered by the official security bug bounty
  - for the separate safety program: AI abuse/safety scenarios listed by OpenAI, especially agentic misuse, sensitive information exposure, and account/platform integrity issues
- Out-of-scope assets:
  - generic low-impact jailbreaks
  - content-policy bypasses without meaningful safety or abuse impact
  - issues outside the listed safety categories for the Safety Bug Bounty
- Forbidden testing:
  - anything that creates material harm
  - testing third parties outside their own terms
  - broad unsafe-model experimentation without a scoped, reproducible safety claim
- Severity model:
  - Security issues: OpenAI bug bounty through Bugcrowd
  - Safety issues: OpenAI Safety Bug Bounty criteria
- Beginner suitability: `Low`
  - Why: the current public Safety Bug Bounty is specialized, and many interesting claims require stronger safety methodology than standard web testing
- Likely safe vulnerability classes to check:
  - ordinary account/platform integrity issues
  - clear authorization issues on owned resources
  - only well-scoped safety scenarios that OpenAI explicitly lists
- Notes on evidence required:
  - reproducibility matters a lot
  - for safety reports, show tangible abuse or harm pathway, not just a quirky output
  - keep security and safety findings clearly separated

### 9. Anthropic

- Program name: `Anthropic Model Safety Bug Bounty Program`
- Platform: `HackerOne` invite-based model safety program
- In-scope assets:
  - universal jailbreaks against Anthropic's protected models under the program terms
  - the specific question set and model alias provided to accepted participants
- Out-of-scope assets:
  - ordinary web vulnerabilities for Anthropic systems under this program
  - narrow one-off jailbreaks that do not meet the program's universal standard
  - public disclosure of the testing materials
- Forbidden testing:
  - disclosing program details
  - testing outside the provided model-access channel
  - treating this as a normal web-app bug bounty
- Severity model:
  - Anthropic internal grading within the program, up to `$35,000`
- Beginner suitability: `Very low`
  - Why: this is specialized safety red-teaming, not ordinary web/application bug hunting
- Likely safe vulnerability classes to check:
  - only the program's intended universal jailbreak category
- Notes on evidence required:
  - extremely strong reproducibility is required
  - reports must align with the provided harmful-query set
  - not a good choice for a normal-user-account beginner Part B strategy

### 10. GitLab

- Program name: `GitLab`
- Platform: `HackerOne`
- In-scope assets:
  - official GitLab HackerOne program assets
  - owned namespaces, projects, and account workflows once access is established
- Out-of-scope assets:
  - assets outside the GitLab program scope
  - other users' projects and private resources unless a valid in-scope authorization flaw is safely demonstrated
- Forbidden testing:
  - challenge bypass attempts
  - automation against protected auth flows
  - destructive project activity
- Severity model: `HackerOne`
- Beginner suitability: `Low to medium`
  - Why: good product for access-control research, but current signup/login friction is relatively high
- Likely safe vulnerability classes to check:
  - owned namespace/project/member boundary checks
  - invite/visibility logic
  - auth/session/account recovery only if setup is already complete
- Notes on evidence required:
  - first prove the flow is actually reachable with owned accounts
  - keep testing on self-created namespaces and projects
  - if a boundary failure appears, preserve exact object path and role context

## Recommended order right now

If the goal is the safest and most practical Part B workflow for this repository, the current recommended order is:

1. `Airtable`
2. `Shopify`
3. `Basecamp`
4. `TripAdvisor`
5. `GitHub`

## Evidence standards to apply to every target

For any target that moves beyond screening, the evidence package should include:

- exact program page URL
- exact target URL
- proof the asset is in scope
- proof the account/object is owned by the researcher where relevant
- exact reproduction steps
- screenshots or request/response evidence
- clear statement of expected vs actual authorization behavior
- platform severity mapping
- novelty check notes for Part B `Type 1` vs `Type 2`

## Source notes

Primary official sources used for this document:

- Assignment-approved HackerOne target pages and official HackerOne docs
  - [HackerOne Bug Bounty Programs](https://www.hackerone.com/bug-bounty-programs)
  - [HackerOne Severity](https://docs.hackerone.com/en/articles/8475343-severity)
- GitHub official program pages
  - [GitHub Bug Bounty](https://bounty.github.com/)
  - [GitHub Scope](https://bounty.github.com/scope)
  - [GitHub Rules of Engagement](https://bounty.github.com/rules.html)
  - [GitHub Ineligible Submissions](https://bounty.github.com/ineligible.html)
  - [GitHub Rewards](https://bounty.github.com/rewards.html)
- OpenAI official pages
  - [OpenAI Bug Bounty](https://openai.com/index/bug-bounty-program/)
  - [OpenAI Safety Bug Bounty](https://openai.com/index/safety-bug-bounty/)
  - [OpenAI Coordinated Vulnerability Disclosure](https://openai.com/security/disclosure/)
- Anthropic official pages
  - [Anthropic Model Safety Bug Bounty Program](https://support.anthropic.com/en/articles/12119250-model-safety-bug-bounty-program)
- Local repo notes derived from prior official-program reviews
  - `partb/master-tracker.md`
  - detailed target logs under `partb/targets/`
