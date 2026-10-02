# MR writing

Use this wording as the bar. Do not paraphrase it away.

1. You must say what was done here according to the presented ticket (if provided)
2. the explanation must high level (e2e basically) so that can be understood easly without implementation details of the code
3. code explationation can be added only when evaluating trade-offs that drifts from the ticket request, or some workaround that was done and needs to show code, but for 90% of tickets this won't be needed
4. be direct and breef don't waste the reviewer time with shit things
5. the mr title must follow the convention scope(some-optional): message...

## Title

`type(optional-scope): short subject`

- `type`: `feat` | `fix` | `chore` | `refactor` | `docs` | `test`
- `optional-scope`: area if it helps (`requests`, `tenders`); omit if it does not
- subject: what the reviewer gets, not the files you touched

## Body

Write for a reviewer who will not open the diff first.

- Open with the ticket outcome (user-visible / e2e). Ticket given → map the change to that ticket. No ticket → same level, from the branch commits.
- No file lists, no component names, no API paths, no “also refactored X” unless it changes the outcome.
- Add a short code note **only** for a trade-off that drifts from the ticket, or a workaround that needs to be seen. Skip this on ~90% of tickets.
- No Test plan, no checklist padding, no “this PR does X, Y, and Z” inventory.

Done when a teammate can approve from the title + body without reading the implementation.