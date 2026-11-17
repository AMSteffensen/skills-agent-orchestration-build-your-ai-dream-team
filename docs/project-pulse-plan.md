# Project Pulse dashboard implementation plan

## Summary

Build Mona's lightweight **Project Pulse** dashboard as a static HTML/CSS/JSON application. The dashboard will present contributor-friendly project cards showing project name, owner, status, recent activity, priority or risk, and an optional summary.

The repository has no existing `app/` implementation or `launch.json`, so these files must be created. The project has no package manager or frontend framework; implementation should use browser-native HTML, CSS, JavaScript, JSON, and Python's built-in HTTP server.

## Ordered implementation phases

### Phase 1: Confirm requirements and ownership

**Owner:** Orchestrator, with Planner research

- Use the Project Pulse brief as the product source of truth.
- Follow the custom agent definitions in `.github/agents/`.
- Preserve non-overlapping file ownership.
- Confirm the launch name, command, port, working directory, and URL.
- Confirm that the dashboard opens `index.html`, not a server directory listing.

**Output:** An approved implementation brief for Designer and Coder.

### Phase 2: Define visual and accessibility direction

**Owner:** Designer  
**Primary file:** `app/styles.css`

The DesStage docs/project-pulse-plan.md.

Commit it with the message "Plan the Project Pulse dashboard".

Push the commit.or risk emphasis, readable typography, spacing, contrast, rounded corners, and shadows.
- Define deterministic selectors including `.dashboard` and `.project-card`, plus status and priority modifier hooks as needed.
- Ensure layouts work on narrow and wide screens.
- Recommend semantic structure and accessible visual states to the Coder.
- Ensure status and priority are not communicated by color alone.
- Validate readable spacing, contrast, focus visibility, and responsive behavior.

### Phase 3: Create the project data contract

**Owner:** Coder  
**Primary file:** `app/project-data.json`

The Coder will create valid JSON with:

- A top-level `projects` array.
- Multiple representative project entries.
- `name`, `owner`, `status`, `recentActivity`, and `priority` on every entry.
- Optional contributor-friendly `summary` text, included consistently if used.
- Deterministic values suitable for visual review.

### Phase 4: Implement the dashboard page

**Owner:** Coder  
**Primary file:** `app/index.html`

The Coder will:

- Use the exact visible title **Project Pulse**.
- Reference `styles.css` and `project-data.json`.
- Use semantic elements such as `header`, `main`, `section`, and `article`.
- Load the JSON through browser JavaScript.
- Render one `.project-card` per project.
- Display each project's name, owner, status, recent activity, and priority.
- Include accessible labels and meaningful text alternatives.
- Use safe DOM insertion such as `textContent` for data values.
- Provide loading, empty-data, and data-load-error states.
- Keep the implementation framework-free and self-contained.

This phase depends on the data shape from Phase 3 and the CSS hooks and design direction from Phase 2.

### Phase 5: Add the runnable VS Code configuration

**Owner:** Coder  
**Primary file:** `.vscode/launch.json`

Create strict JSON with no comments containing a configuration named **Run Project Pulse Dashboard**. The configuration should:

- Run `python3 -m http.server 5500`.
- Set `cwd` to `${workspaceFolder}/app`.
- Use the appropriate VS Code server launch type.
- Configure `serverReadyAction`.
- Open `http://localhost:%s/index.html`.
- Ensure the browser opens the dashboard page rather than the `/app/` directory listing.

Python's `http.server` is appropriate for local preview only; it is not a production server.

### Phase 6: Integrate and validate

**Owner:** Orchestrator, with Coder and Designer review

- Review all four implementation files together.
- Confirm HTML class names match CSS selectors.
- Confirm JSON property names match the rendering code.
- Confirm the launch working directory and URL match the app location.
- Run automated syntax and repository checks.
- Launch the dashboard manually through VS Code.
- Stop the preview server after verification.

## File assignments

| File | Owner | Responsibilities |
| --- | --- | --- |
| `app/index.html` | Coder | Semantic page structure, exact Project Pulse title, JSON loading, visible project-card rendering, loading/error/empty states, and accessibility hooks |
| `app/styles.css` | Designer | Dashboard visual system, responsive layout, `.dashboard`, `.project-card`, badges, spacing, contrast, rounded cards, shadows, and focus states |
| `app/project-data.json` | Coder | Valid top-level `projects` array and complete project records |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration, Python server command, `cwd`, server-ready browser action, and `index.html` target |

The Designer may provide HTML structure recommendations but should not edit `app/index.html` unless the Orchestrator explicitly reassigns that file. The Coder should not modify design-only CSS except to fix an integration defect agreed with the Designer.

## Agent responsibilities

### Designer

- Own information hierarchy and contributor usability.
- Design project-card composition and visual emphasis.
- Define responsive behavior.
- Make accessibility-oriented visual decisions.
- Establish consistent status and priority treatment.
- Provide deterministic CSS hooks required by repository checks.
- Review the integrated UI for polish and readability.

### Coder

- Implement the static HTML page.
- Create the project data JSON.
- Connect the page to the JSON data.
- Render all required project fields visibly.
- Handle loading, empty, malformed, and failed-data states.
- Create `.vscode/launch.json` as strict JSON.
- Use `${workspaceFolder}/app` as the launch working directory.
- Run syntax and browser-preview validation.
- Report changed files, validation results, and remaining risks.

### Orchestrator

- Coordinate the Planner, Designer, and Coder.
- Break work into phases with explicit file ownership.
- Run tasks in parallel only when scopes and dependencies allow it.
- Integrate the result and verify that the complete dashboard hangs together.

## Dependencies

1. The repository brief and agent definitions must be read before implementation.
2. The Designer's CSS hooks and layout direction must be available before final HTML markup is completed.
3. `app/project-data.json` must establish field names before the rendering logic is finalized.
4. `app/index.html` depends on both the CSS selectors and JSON schema.
5. `.vscode/launch.json` depends on the final app location and entry point, but is otherwise independent of page styling.
6. Browser validation depends on all four files existing and the launch configuration being syntactically valid.

## Parallel work decisions

### Work that can run in parallel

After Phase 1:

- Designer can implement `app/styles.css`.
- Coder can create `app/project-data.json`.
- Coder can draft `.vscode/launch.json`.

These tasks use separate files and have no direct edit conflicts.

### Work that must run sequentially

- Phase 1 must precede all implementation.
- Final `app/index.html` implementation must follow agreement on the JSON schema and CSS hooks.
- Integration review must follow completion of all four files.
- Browser launch validation must follow JSON validation and file integration.
- Fixes discovered during manual preview must be completed before final validation.

## Edge cases and risks

- `project-data.json` is missing, malformed, or inaccessible.
- The JSON exists but `projects` is not an array.
- A project is missing a required field.
- The projects array is empty.
- Long project names, owner names, activity text, or priority labels overflow cards.
- Status or priority colors have insufficient contrast or are the only visual indicator.
- Narrow viewport layouts become unreadable.
- Browser access through `file://` prevents `fetch()` from loading JSON; validation must use the HTTP server.
- Port `5500` is already occupied.
- `serverReadyAction` fails to detect the Python server output.
- The launch opens `/` instead of `/index.html`.
- CSS selectors and HTML class names drift apart.
- JSON values must be inserted safely to avoid accidental markup injection.
- Python's development server is suitable only for local preview, not production hosting.

## Validation expectations

### Static validation

- Confirm these files exist:
  - `app/index.html`
  - `app/styles.css`
  - `app/project-data.json`
  - `.vscode/launch.json`
- Run:
  - `python3 -m json.tool app/project-data.json`
  - `python3 -m json.tool .vscode/launch.json`
- Confirm `launch.json` contains no comments.
- Confirm the required JSON fields exist for every project.
- Confirm repository workflow checks pass, including checks for:
  - `Project Pulse`
  - `styles.css`
  - `project-data.json`
  - `project-card`
  - `status`
  - `recentActivity`
  - `priority`
  - `.dashboard`
  - `.project-card`
  - `border-radius`
  - `box-shadow`
  - `Run Project Pulse Dashboard`
  - `index.html`

### Manual UI validation

- Start **Run Project Pulse Dashboard** from VS Code Run and Debug.
- Confirm the browser opens `http://localhost:5500/index.html`.
- Confirm the page shows the dashboard UI rather than a directory listing.
- Confirm multiple project cards are visible.
- Confirm every required project field is displayed.
- Resize the viewport to verify responsive behavior.
- Check keyboard focus visibility and readable contrast.
- Test loading failure and empty-data behavior where practical.
- Stop the preview server after testing.

## Open questions

- Should the optional `summary` field be included in every project record? Recommended: yes, because the brief requests summaries.
- Should the dashboard include filtering or sorting? Recommended: no for the initial implementation; those are future enhancements.
- Should the dashboard use an external browser or VS Code's integrated browser? Recommended: use the repository-required `serverReadyAction` URL and the default supported browser action.
- What should happen if port `5500` is unavailable? Recommended: report the launch failure clearly and retry after stopping the conflicting process rather than silently changing the required port.
