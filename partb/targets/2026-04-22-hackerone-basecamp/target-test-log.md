# Target Test Log

## 1. Basic Information
- Date: 2026-04-22
- Platform: HackerOne
- Program: Basecamp
- Program URL: `https://hackerone.com/basecamp`
- Scope proof:
  - HackerOne Bug Bounty Programs page lists Basecamp as a public bounty program.
  - HackerOne official documentation explains that testing scope is defined per program on the Security Page, not at the platform level.
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
- Main functionality tested: None yet. This record was created to document target selection and prevent duplicate target setup.
- Vulnerability classes attempted: None yet.
- Tools used: Browser research, official program discovery pages, local tracker.
- AI assistance used: Yes.

## 4. Test Steps
1. Confirmed that the teacher-approved starting platform is HackerOne.
2. Confirmed that HackerOne is a directory of many programs, and that actual scope is defined per individual program Security Page.
3. Selected Basecamp as the first candidate program because it appears on HackerOne's bug bounty program listing and Basecamp offers free sign-up on its official site.
4. Deferred active testing until exact scope and rules are checked on the program Security Page.

## 5. Evidence
- Important URLs:
  - `https://www.hackerone.com/bug-bounty-programs`
  - `https://docs.hackerone.com/en/articles/8369900-opportunity-discovery`
  - `https://docs.hackerone.com/en/articles/8490833-security-page`
  - `https://basecamp.com/`
  - `https://basecamp.com/try-basecamp`
- Important requests/responses: None yet.
- Screenshots / recordings: None yet.
- Notes:
  - Current evidence supports target selection only, not a vulnerability.
  - Before testing, the team must verify in-scope assets and any program-specific restrictions.

## 6. Outcome
- Result: In progress
- What worked:
  - Identified a concrete program to start with instead of treating the entire HackerOne platform as one target.
  - Chose a web-first product with visible free sign-up, which is suitable for later IDOR and access-control testing.
- What failed:
  - Could not directly extract the full dynamic Security Page content from HackerOne in this environment.
- Why no finding was confirmed yet:
  - No active security testing has started.
  - Exact in-scope asset list still needs confirmation from the program page.

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
- Questions for teammates:
  - Does the team want Basecamp as the first real target, or should we shortlist 2-3 more HackerOne programs first?
- Whether to keep testing this target: Yes, pending scope confirmation.

## 11. Files in This Folder
- `target-test-log.md`
- `evidence/`
- `requests/`
- `screenshots/`
