# Target Test Log

## 1. Basic Information
- Date: 2026-04-22
- Platform: HackerOne
- Program: Basecamp
- Program URL: `https://hackerone.com/basecamp`
- Program overview section reviewed: Yes
- Key overview excerpt:
  - `Our focus is on`
  - `Strong auth (sign-in, sessions, OAuth, account recovery, MFA)`
  - `Access control (bypasses, faults, CSRF, etc)`
  - `Injection prevention (SQL, XSS, method args, etc)`
  - `For HEY only: potential privacy leaks...`
- Scope proof:
  - HackerOne Bug Bounty Programs page lists Basecamp as a public bounty program.
  - HackerOne official documentation explains that testing scope is defined per program on the Security Page, not at the platform level.
  - The Basecamp HackerOne Overview page was rendered in headless Edge and reviewed before testing.
- Scope page URL: `https://hackerone.com/basecamp/policy_scopes?type=team`
- Official website URL: `https://basecamp.com/`
- Target / asset tested: Planning stage for Basecamp web assets. Exact in-scope assets still need to be confirmed from the program Security Page before active testing.
- Tester(s): YX / Codex

## 2. Target Access
- Account required: Likely yes for authenticated testing.
- Account type(s) used: None yet.
- Registration/login notes:
  - Basecamp official site shows free sign-up is available.
  - This makes it a good candidate for authenticated authorization testing later.
- Role setup: Not started.

## 3. Testing Summary
- Main functionality tested:
  - Public marketing pages
  - Pricing / sign-up entry pages
  - Help and support entry pages
  - Public app availability pages
- Vulnerability classes attempted:
  - Initial public-surface access control review
  - Initial reflected XSS surface review
  - Initial input-point discovery
- Tools used: Browser research, official program discovery pages, public page inspection, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Confirmed that the teacher-approved starting platform is HackerOne.
2. Confirmed that HackerOne is a directory of many programs, and that actual scope is defined per individual program Security Page.
3. Selected Basecamp as the first candidate program because it appears on HackerOne's bug bounty program listing and Basecamp offers free sign-up on its official site.
4. Rendered and reviewed the Basecamp HackerOne program Overview before testing.
5. Performed a first-pass public-surface review against the following publicly reachable Basecamp pages:
   - `https://basecamp.com/`
   - `https://basecamp.com/pricing/`
   - `https://basecamp.com/try-basecamp`
   - `https://basecamp.com/help`
   - `https://basecamp.com/support/`
   - `https://basecamp.com/apps`
6. Checked whether these pages exposed obvious query-parameter flows, object identifiers, unauthenticated sensitive content, or obvious reflected-input behavior from public GET endpoints.
7. Identified that the support page contains public input fields, but no submission testing was performed in this round.
8. Stopped after passive public-page review because authenticated flows and exact HackerOne scope details still need stronger confirmation before deeper testing.

## 5. Evidence
- Important URLs:
  - `https://hackerone.com/basecamp?type=team`
  - `https://www.hackerone.com/bug-bounty-programs`
  - `https://docs.hackerone.com/en/articles/8369900-opportunity-discovery`
  - `https://docs.hackerone.com/en/articles/8490833-security-page`
  - `https://hackerone.com/basecamp/policy_scopes?type=team`
  - `https://basecamp.com/`
  - `https://basecamp.com/pricing/`
  - `https://basecamp.com/try-basecamp`
  - `https://basecamp.com/help`
  - `https://basecamp.com/support/`
  - `https://basecamp.com/apps`
- Important requests/responses: None yet.
- Screenshots / recordings: None yet.
- Notes:
  - Verified the HackerOne program Overview before testing and extracted the focus areas from the rendered page.
  - Public pages reviewed in this round did not expose an obvious unauthenticated vulnerability.
  - The support page has public form inputs, but there is not enough evidence from passive review alone to claim reflected XSS or another finding.
  - Before deeper testing, the team should verify exact in-scope assets and any program-specific restrictions.

## 6. Outcome
- Result: No finding
- What worked:
  - Identified a concrete program to start with instead of treating the entire HackerOne platform as one target.
  - Chose a web-first product with visible free sign-up, which is suitable for later IDOR and access-control testing.
  - Completed an initial passive review of Basecamp public pages without creating risk for the target.
- What failed:
  - Could not directly extract the full dynamic Security Page content from HackerOne in this environment.
  - Could not reach authenticated workflows in this round, so IDOR and horizontal access-control testing could not start yet.
- Why no finding was confirmed yet:
  - No obvious unauthenticated issue was observed on the reviewed public pages.
  - Exact in-scope asset list still needs confirmation from the program page.
  - The highest-value bug classes for this target likely require authenticated workflows.

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
  - HackerOne official help pages for opportunities and security pages
- Initial novelty judgment: Not applicable yet.
- Finding type: Unknown

## 9. Scope and Safety Notes
- Why this target is in scope:
  - The teacher-approved platform is HackerOne.
  - Basecamp appears on HackerOne's official bug bounty program listing.
- Safety constraints followed:
  - No active testing yet.
  - No automation, brute force, or disruptive behavior used.
- Actions avoided:
  - No attempts against assets whose scope has not yet been confirmed.
  - No testing against the HackerOne platform itself.

## 10. Next Actions
- Follow-up validation needed:
  - Open the Basecamp program Security Page in a full browser session and confirm exact in-scope assets and restrictions.
  - Create test accounts only if the program rules allow the selected workflow.
  - If account creation is allowed, move to authenticated tests for IDOR, horizontal access control, and input handling.
- Questions for teammates:
  - Does the team want Basecamp as the first real target, or should we shortlist 2-3 more HackerOne programs first?
- Whether to keep testing this target: Yes, but the next useful step is authenticated testing rather than more passive browsing.

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
