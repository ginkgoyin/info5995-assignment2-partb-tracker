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

## 3. Testing Summary
- Main functionality tested:
  - Direct HackerOne Shopify program page
  - Shopify official homepage
- Vulnerability classes attempted:
  - Initial public-surface review
  - Initial auth-entry feasibility review
  - Initial misconfiguration / unsafe exposure review
  - Initial Shopify-specific customized review
- Baseline checklist status:
  - 1. IDOR / horizontal access control: Not started.
  - 2. Authentication / authorization flow mistakes: Initial public auth-entry review completed.
  - 3. Input handling issues: Not started in depth.
  - 4. Misconfiguration / unsafe exposure: Initial public-surface review completed.
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
8. Stopped after the first public-surface round because this pass was limited to independent screening and did not yet proceed into sub-surfaces such as admin or developer flows.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/shopify?type=team`
  - `https://hackerone.com/shopify/policy_scopes?type=team`
  - `https://www.shopify.com/`
  - `https://admin.shopify.com/`
  - `https://apps.shopify.com/`
  - `https://shopify.dev/`
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
- Screenshots / recordings: None yet.
- Notes:
  - The direct HackerOne program page was readable through raw HTML and was reviewed first.
  - The official homepage exposed a broader public product surface than Privy or Quora.
  - This makes Shopify more promising as a future independent-only candidate than some previously reviewed programs, even though no vulnerability was confirmed in this first round.

## 6. Outcome
- Result: No finding
- What worked:
  - Confirmed the direct HackerOne Shopify program page and official program description.
  - Confirmed that Shopify's public homepage exposes several concrete product and admin-adjacent entry points.
  - Identified Shopify as a richer public-surface candidate than more marketing-only or immediately challenge-gated targets.
- What failed:
  - Did not begin authenticated testing or sub-surface validation in this first round.
  - Did not yet verify whether admin-linked or developer-linked entry points remain low-friction when fetched directly.
- Why no finding was confirmed yet:
  - This round focused only on first-pass target qualification and public-surface review.
  - The more interesting Shopify bug classes likely require deeper follow-up on linked product surfaces.

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
  - Check whether `admin.shopify.com`, customer-account flows, or `shopify.dev` remain accessible enough for deeper independent review.
  - Compare Shopify against Semrush as a candidate for the strongest low-friction follow-up target.
- Questions for teammates:
  - Should Shopify be promoted into the higher-priority shortlist for deeper public-surface testing?
- Whether to keep testing this target: Yes, because it currently looks richer than several previously reviewed candidates.

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
