# agentic-engineering-lab

## Task priority requirements (draft)

**Status:** Draft — not approved
**Source:** [GitHub issue #1: Clarify requirements for task priority](https://github.com/gabrielCrosman/agentic-engineering-lab/issues/1)
**Approval owner:** Unassigned

### Request

The request is to give the team a simple way to prioritize work so users can see what is urgent. Issue #1 also asks that current behavior and related issues be inspected, explicit requirements be separated from unknowns, and focused questions be asked before feature acceptance criteria are finalized. The intended result is a draft requirements record, not an implementation.

### Evidence reviewed

| Source | Evidence | Limits |
|---|---|---|
| Issue [#1](https://github.com/gabrielCrosman/agentic-engineering-lab/issues/1) | Requests task priority so users can see urgency; calls for a draft with evidence, assumptions, acceptance criteria, non-goals, open questions, and status. | Does not define priority values, default, editing, or presentation behavior. |
| Repository issue list (GitHub MCP, read-only) | Issue #1 was the only issue returned; no separate related issue was found. | Search is limited to the issues visible in this repository. |
| `app/main.py` | `GET /tasks` lists in-memory tasks; `POST /tasks` creates one. Current task fields are `id`, `title`, and `completed`; no priority field is implemented. | Does not establish desired priority behavior. |
| `tests/test_api.py` | Covers health response, initial empty list, default incomplete state, and listing a created task. | No priority behavior is tested. |
| `.github/tests/test_title_validation.py` | Expects whitespace-only titles to return 422. | The `title` field in `app/main.py` only declares length constraints and does not explicitly reject whitespace; this discrepancy is unrelated to priority. |

### Confirmed facts

- The requested user outcome is to identify urgent work by adding priority to tasks (issue #1).
- Existing Task API responses have no priority field (`app/main.py`).
- No priority-specific behavior or related issue is established by the evidence reviewed.

### Assumptions and inferences

- “Keep it simple” indicates a preference for a small feature, but does not define its values, rules, or UI/API surface. No specific priority model is assumed.

### Draft acceptance criteria

No feature-level acceptance criteria can yet be confirmed without deciding the open product questions below. Before implementation, the requester should confirm the priority model and how users interact with it; then the observable criteria can be finalized. This draft does not treat any proposed priority values or behavior as approved.

### Non-goals and constraints

- Do not implement or change application behavior as part of this draft.
- Do not create, update, or close GitHub issues.
- Keep unresolved product decisions open rather than selecting defaults on the requester's behalf.

### Open questions

1. What priority levels and ordering should be supported (for example, a small ordered set or another model)?
2. What priority should a newly created task receive if the requester does not specify one?
3. Where must users be able to set and see priority: task creation, task listing, or both? Is editing required?
4. Should task lists be sorted or filterable by priority, or is displaying priority sufficient?

### Review record

- **Decision:** Draft; not approved.
- **Reviewed by:** Unassigned.
- **Notes:** Answer open questions before finalizing feature acceptance criteria.