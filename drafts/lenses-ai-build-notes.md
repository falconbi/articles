# Lenses and AI-built TM1 applications — working notes

Not published. These notes were moved out of the Lens showcase when it was returned to
its original purpose: showing the Workforce Planning UI. They are raw material for a later
article about AI-assisted application development.

## The build loop

The build loop is a sequence of tool calls, model checks, page edits and review:

1. **Read the model.** List cubes, dimensions, attributes, rules, processes and views.
2. **Change the model.** Build dimensions, cubes, attributes, rules, processes, views and subsets through the MCP inside a change set. Read the result, run a process, check the numbers.
3. **Build the Lens.** Write a single HTML page using the toolkit and save it with `save_lens`. The save gate test-runs the complete MDX literals it can find and rejects bad names. The page declares the cubes it writes and the processes it runs.
4. **Test and review.** Assertions, page checks and a person reviewing the change set decide whether the result is acceptable. Nothing is released automatically.

## Division of labour

| Part | Does | Does not |
|---|---|---|
| AI | Writes model artefacts, TI, rules, MDX and the Lens page; reads errors and iterates inside the session | Establish business rules as correct, choose the model shape, or release changes |
| Reusable layer | Supplies the grid, charts, pivot, forms, history and theme; the runtime supplies access checks, declared writes and process calls | Remove the need for design judgement |
| People | Own the model shape, business rules, application behaviour, review, tests and release | — |

## Vertical slice: setting a hire month

On the Positions page, an empty Position has an editable **Hire month** column. Changing it calls
`WFP Set Hire Month`; the page declares that process in its `lens-runs` metadata. The process checks
whether the month is in the plan, whether someone is already in the Position and whether the
Position is closed. If it refuses, the page shows the process's reason and does not keep the typed
value. The change is recorded in History, and cost is marked for recalculation.

This is a useful unit of AI-assisted development: a page, a process, a model rule and a visible
result. It is not a whole application. The AI can write each part; it can propose the rule but
cannot establish that a closed Position should be refused or that a company move changes tax
treatment. Those are model decisions, encoded in TI and reviewed like any other change.

## What is still missing

There is no application manifest or service catalog that describes the pages, queries, actions and
workflow in one place. Each page writes its own MDX. The agent or developer still holds the sequence
of the application in their head. A service-catalog design is a proposal, not a shipped layer.

## Security and operational notes

| Concern | Implementation | Limit |
|---|---|---|
| Runtime identity | The Lens server holds one TM1 session per viewer and makes every bridge call with it; Lens access can be limited to TM1 groups. | The MCP builder is a separate identity; the runtime never uses the builder login. |
| Cell writes | Limited to declared cubes; checks every coordinate, refuses calculated cells and stale values; TM1 cell security decides the rest. | The page can declare a broad cube; the declaration is a scope, not a row-level policy. |
| Process runs | Declared processes only; parameter names and types checked; outcome written to Lens Messages. | A running TI process is not limited by the caller's cell security; the process must check the caller. |
| Page isolation | Sandboxed page with no network access; token never enters the page; undeclared writes and runs are rejected. | Protects the runtime, not every query or model design. |
| Build access | MCP is read-only by default; build mode is explicit; model changes use a change set and builder identity; reads can use a separate reader identity on supported connections. | The MCP tool surface is early and has not seen heavy production use. |
| Validation | Lens saves test-run complete MDX literals; writes check values and staleness; assertions check agreed numbers. | Catches known failure modes, not every semantic mistake. A passing test does not prove the model is right. |
| History | Successful writes and process runs, and refused process runs, are recorded with user and Lens. | Refused cell writes are not written to History by the current write route. |
| Versioning | Model changes governed by change sets; Workforce page sources exported to git; toolkit has a version gate. | Export is manual; Lens deployment to PROD is still parked. |
| Asynchronous cost | Actions mark cost out of date; a five-minute chore rebuilds it; pages show the cost state. | A page can be read before the chore catches up. |
| AI-generated change | Artefacts reviewed in a change set; assertions and page checks run before release. | AI does not guarantee correctness. Review is the control. |

## Implemented

- Lens runtime: model-stored pages, per-user server, sandboxed browser page, declared writes and process runs, access by TM1 group and shared toolkit.
- MCP tools for model objects, rules, TI, views, subsets, diagnostics and Lens saving, with a change-set gate and a save-time query check.
- Workforce application: positions, people, assignments, hiring, approvals, transfers, absence, cover, payroll tax, cost and control checks.
- Tests for runtime, writes, access, toolkit, pivot, history and model assertions.

## Demonstrated

- An AI agent can build a realistic model and its pages in the same project, with a person reviewing each step.
- A page can carry a workflow through selection, action, model validation, refusal and cost recalculation without losing the record context.
- The same model supports views by cost centre, location, function and month without copying data.

## Not yet solved

- An application-level contract describing pages, queries, actions, navigation and workflow as one artifact. The service-catalog proposal exists; it is not implemented.
- Whole-application design from a model profile or Cube Map. Current builds are still directed by a person or an agent in a sequence.
- Governed Lens deployment to production. Model deployment is real; Lens release is parked, and page export is manual.
- General reuse across arbitrary planning applications. The kit is shared; the application layer is not proven general.
- A correctness guarantee. Tests are evidence, not proof, and a model can pass the tests written for it and still be wrong.

## Design notes — around the work

A planning application should present the next action in the context of the work already on screen.
In Lenses, the interface is built around the business task, not simply the underlying TM1 structures.
Select a person and the available actions relate to that person. Select a planned hire and its hire
month, cost and approval status are visible together. Select a step in the budget-to-forecast bridge
and the cost centres are ranked by that driver.

The same principle applies throughout the application. Lists come from the model and respect the
viewer's access. Status appears alongside the relevant records. Actions run against the selected
item, and when TM1 refuses a change, the reason returns to the page. The user does not have to
translate every business task into the structure of the model.

The reusable Lens kit makes this approach repeatable. The same grid, pivot, charts, forms and
interaction patterns are used across pages and, potentially, across different planning applications.
