# Part B Tracking Workspace

This workspace is used to track every allowed target tested for INFO5995 Assignment 2 Part B.

Goals:
- Record every tested website, even when no vulnerability is found.
- Prevent duplicate testing across teammates.
- Preserve scope evidence, test steps, findings, and rejection reasons.
- Keep records ready for AI log, activity log, presentation, and tutor review.

Recommended workflow:
1. Copy `partb/templates/target-test-log-template.md` into a new folder under `partb/targets/`.
2. Name the folder using the date and target name, for example `2026-04-22-hackerone-example`.
3. Update `partb/master-tracker.md` after each testing session.
4. Store screenshots, request samples, and notes in the same target folder.
5. Commit and push after meaningful updates.

Enhanced testing order for each program:
1. Program intake and scope lock.
   - Read the HackerOne program page, overview, and scope before testing.
   - Record the exact program URL, scope page URL, official site URL, welcomed vulnerability types, and any rules such as `use only your own account(s)`.
2. Attack surface mapping.
   - Map the product's reachable surfaces before deep testing: homepage, login, signup, password reset, invitation flows, onboarding, reports, app/developer areas, billing, sharing, and API or docs surfaces.
3. Pre-auth UI review.
   - Review public pages and public flows for obvious unauthenticated issues such as exposed objects, suspicious parameters, public share links, reflection points, and unsafe redirects.
4. Pre-auth DevTools deep dive.
   - Use browser DevTools to inspect rendered DOM, XHR/fetch traffic, object IDs, tokens, nonces, `state` values, hidden parameters, embedded state objects, and exposed routes or endpoints.
5. Baseline common-vulnerability pack.
   - `1) Access control / IDOR / object exposure`
   - `2) Authentication / session / invite / reset / verification / OAuth flow flaws`
   - `3) Token / nonce / randomness / binding quality`
   - `4) Input handling / XSS / unsafe reflection`
   - `5) Misconfiguration / unsafe exposure / business logic boundary`
6. Decide whether login is worth it.
   - Only ask for manual registration/login after the pre-auth and DevTools rounds suggest that authenticated testing is likely to be valuable.
7. Post-auth state confirmation.
   - After login, confirm account state, visible roles, trial/subscription state, newly unlocked navigation, and any project/workspace/report/app surfaces.
8. Post-auth core checks.
   - Re-run the baseline common-vulnerability pack against authenticated features, focusing on object references, session behavior, token flows, plan gates, and deeper input surfaces.
9. Decide whether a second account or second role is worth it.
   - Escalate to multi-account or multi-role testing only when the target shows meaningful collaboration, invitation, sharing, or ownership boundaries.
10. Evidence and submission-readiness check.
   - Before calling something a candidate finding, confirm reproducibility, scope proof, impact explanation, screenshots/requests, and enough evidence to defend it in the presentation and Q&A.
11. Stop and decide.
   - After each meaningful round, stop and decide whether to continue the same program, request login, request a second account, or move on to the next program.

Minimum information to record for each tested target:
- Platform and program name
- HackerOne program URL
- Official website URL
- Proof that the program overview/rules were read before testing
- Which baseline categories `1-5` were tested
- What customized testing ideas were attempted
- Scope proof
- Exact target URL or asset tested
- Date and team member(s)
- Test types attempted
- What was tested
- Result, including failures and dead ends
- Evidence and screenshots
- Initial impact assessment
- Novelty notes
- Next actions

Why this workflow is stronger:
- It aligns testing with the assignment's real scoring needs: scope handling, evidence quality, impact clarity, and novelty readiness.
- It treats DevTools review as a required step rather than an optional helper.
- It explicitly covers common bounty-heavy areas such as access control, auth/session flaws, token quality, XSS/input handling, and business-logic boundaries.
- It separates single-account testing from multi-account testing so time is not wasted on the wrong setup.
