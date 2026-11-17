# Task API contract review

This review describes the implementation currently in the repository. It was based on source inspection; the API was not run during this review.

## Endpoints

### `GET /tasks`

- **Request:** No path, query, or body fields are declared.
- **Success:** `200 OK`; JSON array of task objects. The array can be empty. Each task contains:
  - `id`: integer
  - `title`: string
  - `completed`: boolean
- **Behavior:** Returns all tasks currently held in the in-memory store.
- **Errors and access:** No endpoint-specific errors or authentication/authorization requirements are defined in the route.

### `POST /tasks`

- **Request:** JSON object described by `TaskCreate`:
  - `title`: required string, minimum length 1 and maximum length 120.
  - `completed`: optional boolean, defaults to `false`.
- **Success:** `201 Created`; JSON task object with `id` (integer), `title` (string), and `completed` (boolean). The ID is assigned by the service.
- **Validation:** Invalid request bodies are rejected by FastAPI/Pydantic request validation (normally `422 Unprocessable Entity`). There is no custom validation error handler in the route.
- **Errors and access:** No endpoint-specific errors or authentication/authorization requirements are defined in the route.

### `GET /healthz`

- **Request:** No path, query, or body fields are declared.
- **Success:** `200 OK`; JSON object `{"status":"ok"}`.
- **Errors and access:** No endpoint-specific errors or authentication/authorization requirements are defined in the route.

## Contract details and gaps

- Task data is stored in memory and is lost when the service process restarts. IDs are assigned starting at 1 for a fresh process.
- The model does not declare a rule to reject extra request fields, so no such rule should be assumed from this implementation.
- There is a discrepancy around whitespace-only titles: `TaskCreate.title` declares a minimum string length of 1, which does not itself trim or reject whitespace, while `.github/tests/test_title_validation.py` expects a title containing only spaces to be rejected with status 422. The code and that test do not establish the same behavior; the whitespace rule needs to be resolved or verified against the running version.
- No custom authentication, authorization, or error-response format is shown in the reviewed route code.

## Manually inspecting through `/docs`

From the repository root, start the app with the command documented in `.github/copilot-instructions.md`:

```text
uvicorn app.main:app --reload
```

Open `http://127.0.0.1:8000/docs`. In Swagger UI, expand `GET /tasks` and `POST /tasks` to inspect the generated parameters, request-body schema, response schema, and documented status codes. Use **Try it out** on `POST /tasks` with a body such as `{"title":"Try a task"}`; execute it and inspect the response status and JSON fields. Then execute `GET /tasks` to see the in-memory list, and try a title of 121 characters or an empty title to inspect validation behavior. `GET /healthz` can also be expanded and executed there. The whitespace-only case is especially worth checking because the source and the dedicated test disagree as noted above.

## Repository evidence

- `app/main.py` — FastAPI app, `TaskCreate` and `Task` models, all three route definitions, in-memory store, and task-ID generation.
- `tests/test_api.py` — expected health response, initial empty task list, default `completed` value, and created-task listing.
- `.github/tests/test_title_validation.py` — test expectation for whitespace-only title validation; this conflicts with the explicit `min_length=1` constraint in the model.
- `tests/conftest.py` — test fixture resets the in-memory list and ID sequence for each test.
- `.github/copilot-instructions.md` — documented run command and `/docs` inspection location; notes in-memory persistence.
- `pyproject.toml` — declares FastAPI as a project dependency.
