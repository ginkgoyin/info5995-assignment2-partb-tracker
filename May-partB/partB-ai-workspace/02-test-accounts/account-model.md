# Account Model

Account A: resource owner
Account B: non-owner / non-follower / removed member / blocked user
Anonymous: logged-out browser, no cookies

## Expected rule

Account A should access the target resource.
Account B and anonymous should be blocked from private resources unless the program explicitly allows access.
