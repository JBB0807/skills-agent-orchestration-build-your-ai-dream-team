# Project Pulse implementation plan

## 1. Summary

Project Pulse should be built as a small static dashboard in `app/` using plain HTML, CSS, and browser JavaScript, because this repository has no existing app framework or build tooling. The output should match the repository brief and exercise checks:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

Recommended specialist ownership:

- **Designer** owns visual direction, accessibility decisions, responsive layout, and `app/styles.css`.
- **Coder** owns data, markup/behavior, runnable preview setup, and the files `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- **Orchestrator** keeps file scopes separate, sequences dependencies, and validates the integrated result.
- **Planner** owns this plan and gives the Orchestrator the dependency map.

## 2. Ordered implementation steps

1. **Lock the implementation contract — Owner: Orchestrator + Planner**
   - Confirm the static-app approach, file ownership, and success criteria from the Project Pulse brief.
   - Freeze required hooks and outputs: `Project Pulse`, `.dashboard`, `.project-card`, top-level `projects`, and the launch name **Run Project Pulse Dashboard**.

2. **Define the UI contract and implement styling — Owner: Designer**
   - Create the dashboard visual system in `app/styles.css`.
   - Cover layout, project cards, status badges, spacing, typography, contrast, focus states, and responsive behavior.
   - Keep CSS hooks deterministic so Coder can target them from HTML.

3. **Create the project dataset — Owner: Coder**
   - Build `app/project-data.json` as strict JSON.
   - Use a top-level `projects` array.
   - Ensure each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.

4. **Implement the dashboard page — Owner: Coder**
   - Create `app/index.html`.
   - Reference `styles.css` and `project-data.json`.
   - Render visible project cards from the data.
   - Use semantic structure and show at least the project name, owner, status, recent activity, and priority.

5. **Add runnable preview support — Owner: Coder**
   - Create `.vscode/launch.json` as strict JSON with no comments.
   - Add **Run Project Pulse Dashboard**.
   - Serve from `${workspaceFolder}/app`.
   - Open `http://localhost:%s/index.html` so learners see the dashboard frontend instead of a directory listing.

6. **Validate and reconcile — Owner: Orchestrator with Designer + Coder support**
   - Check file-level requirements.
   - Run the launch configuration.
   - Confirm the rendered UI, data loading, and launch behavior all work together.
   - Send any fixes back to the owning specialist only.

## 3. File assignments

| File | Primary owner | Responsibility | Notes |
| --- | --- | --- | --- |
| `app/index.html` | **Coder** | Semantic page structure, title, data loading, project-card rendering, visible status/activity/priority content | Must follow Designer’s class and layout contract |
| `app/styles.css` | **Designer** | Dashboard layout, card styling, badges, spacing, responsive behavior, accessibility-focused presentation | Must include `.dashboard`, `.project-card`, `border-radius`, and `box-shadow` |
| `app/project-data.json` | **Coder** | Source data for the dashboard | Strict JSON; top-level `projects` key |
| `.vscode/launch.json` | **Coder** | VS Code launch configuration for local preview | Strict JSON; serve from `app/`; open `index.html` |
| `docs/project-pulse-plan.md` | **Planner** | Planning artifact for Orchestrator execution | Already the current planning file |

## 4. Dependencies

- Step 1 must happen before all implementation work.
- `app/index.html` depends on:
  - the agreed CSS hook names from Designer
  - the final data shape in `app/project-data.json`
- `app/styles.css` depends on the agreed dashboard/card structure, but not on the final data content.
- `.vscode/launch.json` depends on fixed file paths and launch requirements, not on final styling.
- Final validation depends on all four implementation files existing.

File-level dependencies:

- `app/index.html` → depends on `app/project-data.json`
- `app/index.html` → depends on `app/styles.css` hook names staying stable
- `.vscode/launch.json` → depends on `app/index.html` being the intended entry page

## 5. Parallel work

These items can run in parallel after Step 1:

- **Designer** works on `app/styles.css`
- **Coder** works on `app/project-data.json`
- **Coder** works on `.vscode/launch.json` once the launch contract is confirmed

Parallel-safe because the file scopes do not overlap.

## 6. Sequential work

These items should stay sequential:

- Agree the UI/data contract before finalizing `app/index.html`
- Finish or stabilize `app/project-data.json` before completing HTML rendering logic
- Finish or stabilize CSS hook names before finalizing HTML class names
- Run end-to-end validation only after all files exist

## 7. Edge cases to handle

- `project-data.json` is valid JSON but `projects` is empty
- Fetch/load failure for `project-data.json`
- Missing fields on one or more project objects
- Unknown `status` or `priority` values
- Long project names or recent activity text causing card overflow
- Narrow screens collapsing the layout
- Launch opens the app root instead of `index.html`
- JSON comments or trailing commas breaking either JSON file

## 8. Validation expectations

Minimum validation should include:

- `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` all exist
- `app/project-data.json` parses as JSON and contains top-level `projects`
- Each project contains `name`, `owner`, `status`, `recentActivity`, and `priority`
- `.vscode/launch.json` parses as JSON and includes **Run Project Pulse Dashboard**
- Launch configuration serves from `${workspaceFolder}/app`
- Launch configuration opens `index.html`
- `app/index.html` includes `Project Pulse`, references `styles.css`, and references `project-data.json`
- `app/index.html` renders visible `project-card` content including status, `recentActivity`, and `priority`
- `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`
- Manual preview from **Run Project Pulse Dashboard** shows the dashboard UI, not a directory listing

## 9. Open questions

- How many sample projects should `app/project-data.json` include? Recommendation: at least 3.
- Should `priority` be displayed as priority, risk, or both in the UI copy?
- Is a visible empty/error state required, or is a simpler fallback acceptable?
- Should Designer only own `app/styles.css`, or also provide review feedback on `app/index.html` without editing it directly?

