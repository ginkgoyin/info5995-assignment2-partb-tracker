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

Minimum information to record for each tested target:
- Platform and program name
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
