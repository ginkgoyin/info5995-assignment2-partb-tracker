# Part B Execution Brief

Date: 2026-04-28

## 1. What actually gets marks in Part B

Based on `assignment2-spec-2.pdf` and `assignment2-rubric-2.pdf`, Part B is judged mainly on one thing: the team's single highest-impact valid in-the-wild finding.

The finding must be:
- from a legitimate bug bounty platform or one of the explicitly allowed company-run programs in the spec
- clearly in scope
- reproducible
- supported with evidence
- classified as either `Type 1` confirmed non-zero-day or `Type 2` zero-day candidate

The strongest marking path is:
- valid target and scope proof
- reproducible exploit path
- clear impact on real assets or users
- explicit platform severity mapping or CVSS fallback
- novelty evidence that supports a zero-day claim

Top-band reminders:
- `Part B` is required; without a valid in-the-wild finding, the assignment is capped at `15/25`
- the top `9-10` band in the 10-mark impact row is reserved for well-supported zero-day candidates
- the Part B presentation is `5 minutes` maximum and must defend target, root cause, impact, type, novelty evidence, and mitigation

## 2. Current workspace assessment

After reading `PARTB-README.md`, `partb/master-tracker.md`, and the detailed target logs that currently exist in the repository, the current state is:

- the workflow direction is now strong and aligns well with real bug bounty practice
- the project already avoids duplicate testing and records scope/safety better than many student submissions
- the main weakness is no confirmed candidate finding yet
- the main practical blocker is not lack of ideas, but lack of richer authenticated state and, in some cases, lack of a second owned account or second role

Current locally documented detailed targets:
- `Basecamp`
- `Airtable`
- `GitLab`
- `Poe`
- `Privy`
- `Quora`
- `Semrush`
- `Shopify`

Targets that currently look least promising from the existing evidence:
- `Poe`: stop, because a valid program page was not confirmed
- `Quora`: low value at current friction level
- `Privy`: weak public signal compared with stronger candidates
- `GitLab`: real target, but challenge friction is high before meaningful progress starts

Targets that still look worth real time:
1. `Airtable`
2. `Shopify`
3. `Basecamp`
4. `Semrush`

## 3. Recommended primary target order

### Airtable

Why it ranks first:
- valid HackerOne program page already confirmed
- public and authenticated surfaces were both reached previously
- stable object identifiers were already observed
- authenticated developer, token, OAuth, workspace, and account surfaces are all relevant to real bounty bug classes
- anti-automation exists, but not in a way that completely blocked earlier progress

Why it still did not produce a finding:
- current account state is too empty
- there is no richer shared object state
- there is no second account for collaboration, invite, or workspace-boundary checks

Best next path:
- create richer owned test state inside Airtable
- create at least one additional base/workspace/shareable object
- ideally create a second owned account for invitation and cross-boundary validation

### Shopify

Why it ranks second:
- the program explicitly mentions IDOR relevance and self-created store testing
- authenticated partner surfaces already exposed internal numeric IDs and useful object boundaries
- store, collaborator, invite, API-client, and token-related surfaces can become high-value if the account state is richer

Why it still did not produce a finding:
- current partner account is too empty
- no populated owned store
- no second owned account or collaborator state

Best next path:
- create an owned bug-bounty test store
- add real internal state worth testing
- if possible, introduce a second owned account for collaborator or transfer checks

### Basecamp

Why it remains viable:
- collaboration and role boundaries are naturally central to the product
- multi-role access-control testing could be meaningful

Why it is not first:
- it still looks more dependent on multi-account setup than Airtable
- earlier rounds did not reveal a strong enough single-account lead

## 4. Highest-value bug classes from now on

From this point, testing should stop optimizing for broad scanning and instead optimize for the bug classes most likely to create a defendable high-impact finding:

1. `Access control / IDOR / object-boundary failure`
- object IDs in URLs, APIs, shares, invites, teams, workspaces, stores, tokens, apps, reports
- cross-account or cross-role object access
- predictable identifiers only matter if unauthorized access is actually demonstrated

2. `Invite / collaboration / ownership transition flaws`
- invite reuse
- invite acceptance under wrong account
- stale invite after role/state change
- cross-workspace or cross-store object inheritance
- collaborator or transfer boundary confusion

3. `Session / OIDC / OAuth / token binding flaws`
- state or nonce reuse that is actually exploitable
- weak redirect validation
- token scope confusion
- resource-binding flaws between token owner and target objects

4. `Plan-gate / business-logic boundary issues`
- premium-only surface access without entitlement
- cross-tenant behavior reachable through state mismatch
- workflow actions allowed after ownership or state transitions that should revoke them

5. `Unsafe exposure with direct impact`
- exposed secrets, config, or objects that cross account boundaries
- public share or preview behavior that leaks restricted data

## 5. Exact execution order for the next serious round

This is the order that should be followed for the next deep target round:

1. Lock scope again on the program page and capture screenshot-worthy proof.
2. Confirm legal rules such as "only test on your own account/store/workspace".
3. Confirm current account state before touching objects.
4. Create richer owned objects that are safe and in scope.
5. Re-run the baseline `1-5` checklist on those richer objects.
6. Focus first on collaboration, invite, share, ownership, and object-reference boundaries.
7. If a second owned account is available, test cross-account access in a minimal and reversible way.
8. If suspicious behavior appears, stop broad exploration and switch into evidence collection mode immediately.
9. Before calling anything a candidate finding, write down:
   - exact URL and object
   - before/after behavior
   - why access should have been denied
   - what data or action became reachable
   - scope proof
   - likely severity mapping
   - novelty search direction

## 6. Evidence checklist for a submission-ready finding

If a candidate appears, the evidence package needs all of the following:

- target name and exact target URL
- program page URL and scope proof
- legal-boundary note showing the test stayed in scope
- exact reproduction steps
- screenshots or request evidence for each key step
- affected object ID or boundary
- explanation of the authorization expectation
- explanation of the actual broken behavior
- impact statement in attacker language
- platform severity mapping or CVSS fallback
- novelty reasoning:
  - `Type 1` if already public, duplicate-like, or otherwise not novel
  - `Type 2` only if valid and no public disclosure is found at submission time

## 7. Practical stop wasting time rules

Immediately deprioritize a target when:
- the scope proof is unclear
- challenge friction is high and there is no strong signal behind it
- only marketing pages are reachable
- no meaningful objects, roles, shares, or collaboration boundaries exist
- the current account state is too empty and cannot be enriched safely

Keep investing in a target when:
- stable object identifiers already appear
- invites, shares, roles, or ownership flows exist
- the product has collaboration, tenant, or store/workspace boundaries
- the previous round already exposed internal object models but not enough state to validate them

## 8. Recommended immediate next action

The best use of the next serious testing window is:

1. Resume `Airtable` as primary target.
2. Build richer owned state first instead of scanning more public pages.
3. If possible, introduce a second owned account for workspace/base sharing and invite validation.
4. Keep `Shopify` as the second target if Airtable still stalls after richer state is created.

If the team cannot prepare richer authenticated state soon, the project should not pretend that more pre-auth scanning will solve the problem. At that point, effort should shift to:
- upgrading one existing strong target statefully
- capturing better evidence
- preparing the final Part B presentation around the strongest defensible result that can actually be reproduced
