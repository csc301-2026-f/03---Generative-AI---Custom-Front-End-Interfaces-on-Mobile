# Savi AI Widgets — 404 Team Name Not Found

Savi AI Widgets extends Savi Finance's mobile app with financial widgets created from natural-language requests. Users can describe the information they need, preview a supported design, and reuse it in their dashboard rather than repeatedly configuring charts or prompting the AI.

This repository contains Team 3's CSC301 deliverable documents. The application code lives in the partner's private [Savi Finance codebase](https://github.com/cheapreats/ce). Our scope is the AI-widget workflow; Savi's existing banking integrations and other finance features are provided by the partner.

## Partner

- **Organization:** Savi Finance
- **Primary contact:** Ralph Maamari
- **Email:** ralphpalxyz@gmail.com

Savi Finance helps individuals and households organize accounts, understand spending, and track savings goals. Our work adds customizable financial views to the existing mobile experience.

## D2 Features and Evidence

The following features are implemented in team feature branches and demonstrated through the partner review. Inclusion in a particular app build depends on integration of the corresponding branches; these links identify the implementation evidence, rather than a published D2 build.

| Use case | User-facing behavior | Implementation evidence |
| --- | --- | --- |
| Generate a supported financial view | Describe a view in Ask Savi, confirm the account and period, and preview financial data retrieved by the backend. Supported views include category spending, monthly expense total, expense trend, expense comparison, and transaction list. | [PR #6593](https://github.com/cheapreats/ce/pull/6593) |
| Compare and save designs | Preview alternative presentations and save a preferred widget design. | [PR #6598](https://github.com/cheapreats/ce/pull/6598) |
| Reopen and refresh saved widgets | Load a saved widget without regenerating its design. Refresh its authorized financial data while preserving the saved layout. | [PR #6586](https://github.com/cheapreats/ce/pull/6586) |
| Manage widget visibility | Browse built-in, local, and saved native widgets in one library; show or hide entries on Overview while retaining them in the library. | [US-03 documentation](https://github.com/cheapreats/ce/tree/fcf6f277ca8e46b70e556ecdf5ec0a4777d7229d/docs/ai-widgets/US-03) |
| Refine a saved widget | Request a supported change, compare the preview, and use Undo/Redo while reviewing refinements. | [PR #6736](https://github.com/cheapreats/ce/pull/6736) |
| Remove an unwanted widget | Remove a widget from the library and dashboard with confirmation, cancellation, and undo support. | [PR #6602](https://github.com/cheapreats/ce/pull/6602) |

The team recorded a demonstration of widget creation, refinement, preview comparison, and Undo/Redo. The partner accepted the basic functionality and requested UI improvements. See the [D2 review](deliverables/D2/iteration-02-review.md) for feedback and remaining integration work.

## Using the Widget Workflow

These steps describe the feature workflow in a team-enabled Savi test build. Sign in with an authorized test account containing sample financial data. An ordinary production installation may not contain the team's feature branches.

### Create a financial view

1. Open **Ask Savi** and enter a request such as “Show my monthly expenses by category.”
2. Select or confirm the requested account and month when prompted. Use an account and period containing sample transactions.
3. Review the generated preview. Check its account, period, category labels, and amounts against the selected financial context.
4. Where alternative designs are available, compare their presentations and save the preferred design. Use **Add to Home** when offered to display a generated snapshot on Home.

**Expected result:** The app displays the requested supported view using backend financial data. If the selected period has no eligible expenses, it displays an empty-data message instead of fabricated values.

### Reopen and organize widgets

1. Open **Customize Widgets** and locate a saved widget using the library's search or filters.
2. Open it to view the saved design with refreshed financial data; refreshing does not require generating a new design.
3. Use the eye control to show or hide the widget on Overview. Return to Overview to check the result.

**Expected result:** Hiding a widget removes it from Overview while keeping it available in the library. Saved native widgets and device-local Home snapshots use different storage paths; local snapshots are scoped to the profile and cleared on sign-out.

### Refine or remove a widget

1. Open a saved widget and request a supported refinement to its presentation.
2. Review the proposed preview before accepting it. Use Undo/Redo to compare the available revisions.
3. To remove a widget, open its menu and choose removal. First cancel to check that it remains available, then confirm removal and check the library and dashboard. Use undo if offered.

**Expected result:** Refinement allows a preview before acceptance. Removal affects the selected widget; imported copies remain independent. Drag-to-remove is ongoing work and is not part of the documented D2 release scope.

## How It Works

- **Mobile:** TypeScript, React Native, Expo, Styled Components, and Savi's existing UI components.
- **Backend:** Go services built with Bazel, including RPC handlers for generation and saved-widget operations.
- **AI integration:** The server uses the shared OpenRouter client to interpret requests and produce bounded presentation choices. Financial values come from backend queries and aggregation rather than model-generated amounts.
- **Widget contract:** Native component definitions describe the design, query plans describe the financial data needed, and snapshots provide the values to render. Frontend and backend validation constrain supported components and bindings.
- **Storage:** Saved native widgets support backend loading and refresh. The initial Add to Home flow also supports device-local, profile-scoped snapshots.

For implementation details, see the [widget architecture and user flows](https://github.com/cheapreats/ce/tree/fcf6f277ca8e46b70e556ecdf5ec0a4777d7229d/docs/ai-widgets/US-03) and the implementation PRs above. Developers should follow the partner repository's [setup instructions](https://github.com/cheapreats/ce#readme), which cover its service dependencies and mobile environment. This course repository does not contain a runnable copy of the application.

## Git and GitHub Workflow

1. Split work by use case and agree on the shared widget structures and interfaces before connecting dependent features.
2. Develop focused changes on descriptive feature or fix branches. When work depends on another feature branch, use stacked PRs and make the dependency explicit.
3. Include the change's purpose, relevant tests, and UI screenshots in the PR. Run the partner repository's required checks, including `make presubmit`, and the relevant mobile, backend, or hermetic tests.
4. Review changes through GitHub, resolve comments and integration conflicts, and merge dependencies in order under the partner's repository policies.
5. Keep course deliverable documents in this repository and implementation code in the partner repository. Record process lessons and partner feedback in the iteration review.

Tests contributed across the widget work include mobile component/service tests, backend handler and RPC tests, and hermetic integration scenarios. Test results belong to the individual PR revisions; they do not by themselves establish that every feature has been integrated into one release.

## Deliverable Documents

- [D2 individual contributions](deliverables/D2/Individual_Contributions_Deliverable2.md)
- [D2 review and retrospective](deliverables/D2/iteration-02-review.md)
- [D1 MVP specification](deliverables/D1/mvp.md)
- [D1 technical design](deliverables/D1/technical-design.md)
- [D1 implementation and validation plan](deliverables/D1/implementation-and-validation-plan.md)

The D1 documents record the broader MVP plan. The D2 feature table and implementation evidence above describe this iteration's work; planned sharing and other future capabilities should not be assumed available in a D2 build.
