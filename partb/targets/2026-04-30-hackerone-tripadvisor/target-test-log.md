# Target Test Log

## 1. Basic Information
- Date: 2026-04-30
- Platform: HackerOne
- Program: TripAdvisor
- Program URL: `https://hackerone.com/tripadvisor`
- Program overview section reviewed: Partially, through the latest public HackerOne bug-bounty listing rather than the JS-rendered direct program page.
- Key overview excerpt:
  - `TripAdvisor Testing ... hackerone.com/tripadvisor`
- Scope proof:
  - The teacher-approved platform is HackerOne.
  - The latest publicly indexed HackerOne bug bounty programs page currently lists `TripAdvisor` with the direct handle `hackerone.com/tripadvisor`.
  - `TripAdvisor` does not already appear as a tested target in the local `partb/master-tracker.md`, so this is a new target for this workspace.
- Scope page URL: Not yet confirmed from current tooling because the direct HackerOne page requires JS rendering.
- Official website URL: `https://www.tripadvisor.com/`
- Target / asset tested: Current public program-existence evidence, TripAdvisor homepage, and public owner/management-center guidance pages
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Likely yes for meaningful access-control and owner-side testing.
- Account type(s) used: None yet.
- Registration/login notes:
  - The homepage exposes public user-facing entry points including `Write a review`, `Join`, and `Help Center`.
  - Public TripAdvisor Insights guidance references the owner path `https://www.tripadvisor.com/Owners`.
  - Public guidance also indicates owner verification paths such as phone call, SMS, email, and credit-card-based identity verification.
- Role setup: Not started.
- Whether a second account or second role may be needed:
  - Likely yes later if the target shows reviewer/owner/listing-management boundary behavior worth validating.

## 3. Testing Summary
- Main functionality tested:
  - Current HackerOne program existence evidence
  - TripAdvisor public homepage
  - TripAdvisor public management-center and owner-guidance pages
- Vulnerability classes attempted:
  - Program intake and scope lock
  - Attack-surface mapping
  - Pre-auth public-surface review
  - Initial auth-path and owner-surface mapping
- Baseline checklist status:
  - 1. Access control / IDOR / object exposure: Not started beyond public object-surface identification.
  - 2. Authentication / session / invite / reset / verification / OAuth flow flaws: Initial login/join and owner-verification surface mapping completed.
  - 3. Token / nonce / randomness / binding quality: Not meaningfully testable in this first pass.
  - 4. Input handling / XSS / unsafe reflection: Not meaningfully testable in this first pass.
  - 5. Misconfiguration / unsafe exposure / business logic boundary: Initial public owner/listing-management surface mapping completed.
- Customized testing ideas:
  - Revisit `Owners` and any listing-management or business-claim surfaces once an owned account exists.
  - Prioritize owner/listing verification, listing-claim, and multi-role reviewer-versus-owner boundaries if the program rules allow them.
- Tools used: Local tracker, public web-source review, PDF/rubric-aligned workflow.
- AI assistance used: Yes.

## 4. Test Steps
1. Reviewed the current local `partb/master-tracker.md` to identify targets that were already tested.
2. Confirmed that `TripAdvisor` was not already recorded as a tested target in the tracker.
3. Checked current public HackerOne program-listing evidence and confirmed that `TripAdvisor` is presently listed there with the direct handle `hackerone.com/tripadvisor`.
4. Reviewed the public TripAdvisor homepage and confirmed it is a large unauthenticated product surface with search, reviews, listings, and account-entry points.
5. Recorded visible public entry points from the homepage, including `Write a review`, `Join`, `Help Center`, and broad travel-search surfaces.
6. Reviewed public TripAdvisor Insights guidance and management-center material.
7. Confirmed that TripAdvisor publicly documents the owner path `https://www.tripadvisor.com/Owners` and describes verification methods such as phone, SMS, email, and credit-card-based verification.
8. Stopped after the initial intake round because the next meaningful step is direct testing of account and owner-side flows rather than more homepage scanning.

## 4A. Attack Surface Map
- Public surfaces discovered:
  - `https://www.tripadvisor.com/`
  - review and travel-search surfaces on the homepage
  - public help and support surfaces
  - public insights and management-center guidance pages
- Auth-related surfaces discovered:
  - `Join`
  - owner login path via `https://www.tripadvisor.com/Owners`
  - owner verification flows described through phone, SMS, email, and credit-card-based identity verification
- Object-bearing surfaces discovered:
  - travel listings
  - reviews
  - business/listing-management guidance
- Billing / subscription / trial surfaces discovered:
  - business-management and listing-ownership guidance suggests business-side management flows
- Sharing / invitation / collaboration surfaces discovered:
  - none directly validated yet in this first pass
- App / developer / API / docs surfaces discovered:
  - owner/management-center documentation
  - public help-center style support surfaces

## 4B. DevTools Deep-Dive Notes
- Key pages inspected with DevTools:
  - None in this initial source-validation round.
- Important XHR / fetch endpoints:
  - None recorded yet.
- Interesting object IDs / UUIDs / team or workspace markers:
  - None recorded yet.
- Tokens / nonce / state / CSRF observations:
  - None recorded yet.
- Hidden parameters / embedded state / frontend routes:
  - Public guidance references the owner route `/Owners`.
- Bundle / source / feature-flag observations:
  - Not started in this round.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/tripadvisor?type=team`
  - `https://www.hackerone.com/bug-bounty-programs`
  - `https://www.tripadvisor.com/`
  - `https://www.tripadvisor.com/Owners`
  - `https://www.tripadvisor.com/TripAdvisorInsights/GetStarted`
- Important requests/responses:
  - Latest public HackerOne bug-bounty listing includes `TripAdvisor Testing ... hackerone.com/tripadvisor`
  - TripAdvisor homepage exposes public search, listing, review, and account-entry surfaces
  - TripAdvisor Insights guidance states that listing-management users can access `www.tripadvisor.com/Owners`
  - Public owner guidance describes multiple identity-verification methods including phone, SMS, email, and credit card
- Screenshots / recordings: None yet.
- Notes:
  - This round is primarily a new-target intake and duplicate-avoidance record.
  - The most promising next step is not more marketing-page browsing; it is owner/account-side testing if scope and owned-account setup are confirmed.

## 6. Outcome
- Result: No finding yet
- What worked:
  - Confirmed that `TripAdvisor` is a current public HackerOne-listed target and a new target for this local workspace.
  - Confirmed meaningful public product and owner-management entry points.
  - Identified a potentially interesting owner/listing-management angle for later testing.
- What failed:
  - Did not reach direct JS-rendered HackerOne program-policy content from current tooling.
  - Did not yet enter authenticated account or owner-side flows.
- Why no finding was confirmed yet:
  - This was only an initial target-selection and attack-surface intake round.
  - High-value TripAdvisor bug classes are more likely to sit in owner, listing, review, or multi-role management flows.
- Whether this target should be revisited only with a richer account state or second account:
  - Very likely yes.

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
  - HackerOne public bug-bounty listing
  - TripAdvisor homepage
  - TripAdvisor Insights owner guidance
- Initial novelty judgment: Not applicable yet.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - It is currently listed on the latest public HackerOne bug-bounty programs page.
  - Testing started only after confirming it was not already covered in the local tracker.
- Safety constraints followed:
  - Only public source material and public TripAdvisor pages were reviewed in this round.
  - No login, verification, listing claim, or owner action was attempted.
- Actions avoided:
  - No form submission
  - No account creation
  - No verification-step interaction

## 10. Next Actions
- Follow-up validation needed:
  - Confirm the direct program-policy page through a JS-capable browser path when available.
  - If the team continues, prepare an owned TripAdvisor account and evaluate whether an owner/listing-management path is safely testable in scope.
- Questions for teammates:
  - Do we want to invest in owner/listing-management setup here, or keep TripAdvisor as a screened backup while we pick another fresh public target?
- Whether to keep testing this target:
  - Yes, but only if the next round moves into richer account or owner-side state.
- Next recommended testing state:
  - Request login / registration

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
