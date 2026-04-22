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

Standard testing order for each program:
1. Read the HackerOne program page, overview, and scope before testing.
2. Test `IDOR / horizontal access control`.
3. Test `authentication / authorization flow mistakes`.
4. Test `input handling issues`, prioritizing reflected XSS and then obvious stored XSS.
5. Test `misconfiguration / unsafe exposure` with real security impact.
6. Run `customized testing` based on the product's specific workflows and trust boundaries.

Minimum information to record for each tested target:
- Platform and program name
- HackerOne program URL
- Official website URL
- Proof that the program overview/rules were read before testing
- Which baseline categories `1-4` were tested
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
