# Savi AI Widgets — 404 Team Name Not Found

## Partner Intro

- **Partner organization:** Savi Finance
- **Primary contact:** Ralph Maamari
- **Title:** Chief Executive Officer
- **Email:** ralphpalxyz@gmail.com
- **Secondary contact:** Not designated

Savi Finance is a personal finance platform that helps individuals and households organize accounts, understand spending, and track savings goals. Our team is extending its existing mobile application with customizable AI-generated financial widgets.

## Description about the Project

Savi AI Widgets allows users to describe the financial information they want to see and create a personalized dashboard widget within the Savi Finance mobile app.

Fixed dashboards do not suit every user's needs. Our feature helps users create, save, refine, and reuse useful financial views without manually configuring charts and other components. The intended users are individuals and households who want to understand their spending through views suited to their needs.

This repository contains Team 3's CSC301 deliverable documents. The application code lives in the partner's private [Savi Finance codebase](https://github.com/cheapreats/ce). Our scope is the AI-widget workflow; Savi's existing banking integrations and other finance features are provided by the partner.

## Key Features

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

### Key Use Cases

#### Generate a financial widget

**User story:** As a Savi user, I want to describe a financial view so I can see the information that matters to me.

The user submits a request through Ask Savi and confirms the account and period. The backend retrieves authorized financial data and returns a supported view. The generation work supports category spending, monthly expense total, expense trend, expense comparison, and transaction lists. Financial amounts come from backend calculations; AI interprets the request and selects supported presentation options.

#### Preview and choose a design

**User story:** As a Savi user, I want to compare presentations before saving a widget so I can choose one that is easy to understand.

The design-selection flow provides alternative previews of the requested information and saves the selected design. Supported presentations use native components rather than executing arbitrary AI-generated code.

#### Save, reopen, and refresh

**User story:** As a Savi user, I want to reopen a saved widget so I can reuse it without repeating my request.

Saved native widgets retain the design separately from their financial query and data snapshot. Loading and refreshing retrieve authorized financial data while preserving the layout. The library brings built-in, local, and saved native widgets together with search and filters.

#### Organize the dashboard

**User story:** As a Savi user, I want to choose which widgets appear on Overview so useful information is easy to reach.

Show/hide controls connect library entries to Overview preferences. Hiding a widget keeps it available in the library. The initial generation flow also supports adding a device-local snapshot to Home. Pinning and reordering remain part of the broader MVP plan; the D2 evidence above specifically documents visibility management.

#### Refine a widget and undo changes

**User story:** As a Savi user, I want to request a supported refinement and compare it before accepting so I can improve a design without starting over.

The refinement work supports preview comparison and Undo/Redo while reviewing changes. Section-level refinement and preservation of unrelated content are part of the intended workflow; availability depends on the integrated feature revision.

#### Remove an unwanted widget

**User story:** As a Savi user, I want to remove a widget I no longer need so my collection stays organized.

Removal updates the library and dashboard, with confirmation, cancellation, and undo support. Imported copies remain independent. Drag-to-remove is ongoing work rather than part of the documented D2 scope.

### Scope and Remaining Work

The broader MVP includes reusable design sharing, dashboard reordering, and additional financial contexts such as balances and savings-goal progress. These should not be assumed available in a D2 build solely because they appear in the D1 plan. Remaining work includes completing branch integration, testing the combined flow, and addressing the partner's UI feedback.

## Instructions

### Access the System

The feature runs within the existing Savi Finance mobile application. Reviewers need a team-enabled test build and an authorized account with sample financial data. This course repository contains documentation rather than a runnable application. Distribution links and test-account details are outside this README update.

### User Walkthrough

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

## Development Requirements

### Technologies

| Technology | Purpose |
| --- | --- |
| TypeScript and React Native | Mobile screens, widget previews, and interaction logic. |
| Expo / EAS | Mobile development and build tooling used by the partner application. |
| Styled Components and Savi UI components | Styling and consistency with the existing app. |
| Go and Bazel | Backend RPC handlers, financial queries, validation, builds, and tests. |
| OpenRouter | Shared server-side model client for bounded request interpretation and presentation choices. |
| Git and GitHub | Source control, pull requests, review, and automated checks. |

The earlier plan mentioned Gemini Flash / Antigravity for AI-related work. The widget implementation described here uses the shared OpenRouter client; development assistants are separate from the application's runtime dependencies.

### Prerequisites

Developers need Git, Node.js and npm compatible with the partner repository, the Go/Bazel toolchain, Docker for the local service dependencies, and a supported mobile test device or emulator. iOS simulator development requires macOS and Xcode; Android development requires the corresponding Android SDK and emulator setup.

Use the versions and platform instructions in the partner repository rather than installing arbitrary latest releases. Access to the private codebase and authorized development configuration is required.

### Local Setup

1. Clone the [partner repository](https://github.com/cheapreats/ce) and check out the feature or integration revision being tested.
2. Follow its [setup instructions](https://github.com/cheapreats/ce#readme) to install the repository and mobile dependencies and prepare service configuration.
3. Configure the backend's database, infrastructure, and model-service settings using the partner's environment templates. Keep API keys on the server and out of committed files.
4. Start the backend development services following the partner's local-development instructions, and check their status before launching the app.
5. Configure the mobile app to reach that backend. From `sba-mobile/`, use the existing platform scripts:

   ```bash
   npm run ios
   # Or, for Android:
   npm run android
   ```

6. Sign in with an authorized test profile, then follow the user walkthrough above using sample transactions.

The platform scripts do not replace the partner's dependency, environment, or native build setup. Backend services use the repository's Go/Bazel workflow rather than treating this course repository as a standalone Go project.

### External Dependencies and Third-Party Software

The mobile app uses React Native, Expo, Styled Components, native chart/rendering components, and local storage packages. Backend services use external Go modules and the shared OpenRouter client. Frontend dependencies are tracked in package manifests and lockfiles; backend dependencies are tracked in the partner's Go module and Bazel configuration.

Required environment variables and service credentials follow the partner repository's setup documentation. Secret values must not be added to the course documentation or source control. Third-party dependencies remain subject to their respective licenses.

### Architecture

- **Mobile:** TypeScript, React Native, Expo, Styled Components, and Savi's existing UI components.
- **Backend:** Go services built with Bazel, including RPC handlers for generation and saved-widget operations.
- **AI integration:** The server uses the shared OpenRouter client to interpret requests and produce bounded presentation choices. Financial values come from backend queries and aggregation rather than model-generated amounts.
- **Widget contract:** Native component definitions describe the design, query plans describe the financial data needed, and snapshots provide the values to render. Frontend and backend validation constrain supported components and bindings.
- **Storage:** Saved native widgets support backend loading and refresh. The initial Add to Home flow also supports device-local, profile-scoped snapshots.

For implementation details, see the [widget architecture and user flows](https://github.com/cheapreats/ce/tree/fcf6f277ca8e46b70e556ecdf5ec0a4777d7229d/docs/ai-widgets/US-03) and the implementation PRs above. Developers should follow the partner repository's [setup instructions](https://github.com/cheapreats/ce#readme), which cover its service dependencies and mobile environment. This course repository does not contain a runnable copy of the application.

### Testing

Run the checks relevant to the modified subsystem and finish with the partner repository's required `make presubmit` checks. Mobile testing uses the configured Jest suite; backend testing uses Bazel. Hermetic scenarios exercise RPC behavior and require the partner's local test services.

For manual verification, check a populated account/month, an empty period, cancellation before removal, show/hide behavior, and reopening a saved widget. Verify that profile changes and sign-out do not expose another profile's local snapshots. See each feature PR for its exact validation results and limitations.

## Deployment and GitHub Workflow

### Task Management

Jira is the main task-management tool. Larger features are divided into smaller tickets with assigned owners and priorities. GitHub pull requests track code changes, while Slack and meeting minutes record blockers, decisions, and partner feedback.

### Branching, Pull Requests, and Review

1. Split work by use case and agree on the shared widget structures and interfaces before connecting dependent features.
2. Develop focused changes on descriptive feature or fix branches. When work depends on another feature branch, use stacked PRs and make the dependency explicit.
3. Include the change's purpose, relevant tests, and UI screenshots in the PR. Run the partner repository's required checks, including `make presubmit`, and the relevant mobile, backend, or hermetic tests.
4. Review changes through GitHub, resolve comments and integration conflicts, and merge dependencies in order under the partner's repository policies.
5. Keep course deliverable documents in this repository and implementation code in the partner repository. Record process lessons and partner feedback in the iteration review.

Tests contributed across the widget work include mobile component/service tests, backend handler and RPC tests, and hermetic integration scenarios. Test results belong to the individual PR revisions; they do not by themselves establish that every feature has been integrated into one release.

Use descriptive branch names such as `feat/<ticket-id>-<description>`, `fix/<ticket-id>-<description>`, or `docs/<description>`. Keep commits focused and PR descriptions clear about the behavior changed, dependencies, and validation. This workflow keeps reviews manageable and makes integration problems easier to identify.

### Deployment

Development and testing use the partner's local environments and authorized test accounts. Mobile and backend distribution follow Savi's existing release process. A feature PR or successful local test does not by itself mean the feature is included in the partner's production app.

For D2, the course documents and tested application revision need to identify the same submission scope. The submission should reference a fixed D2 release snapshot so later development does not change what reviewers evaluate. Creating the release and distributing an app build are separate from this README update.

## Coding Standards and Guidelines

Follow the existing Savi codebase conventions, use descriptive names, and keep changes focused. Format Go code with `gofmt` and use the frontend's configured formatting and linting tools. Validate generated widget definitions before rendering them and document important API or configuration changes.

Keep UI behavior consistent with the existing Savi components. Keep financial values backed by authorized queries, preserve supported widget contracts across the mobile/backend boundary, and add regression coverage when fixing a defect. Comments and documentation should describe the behavior implemented by the reviewed revision.

## Deliverable Documents

- [D2 individual contributions](deliverables/D2/Individual_Contributions_Deliverable2.md)
- [D2 review and retrospective](deliverables/D2/iteration-02-review.md)
- [D1 MVP specification](deliverables/D1/mvp.md)
- [D1 technical design](deliverables/D1/technical-design.md)
- [D1 implementation and validation plan](deliverables/D1/implementation-and-validation-plan.md)

The D1 documents record the broader MVP plan. The D2 feature table and implementation evidence above describe this iteration's work; planned sharing and other future capabilities should not be assumed available in a D2 build.

## D3 Improvement Highlight

The next iteration will focus on completing integration, testing the full widget lifecycle, and improving the user flow based on partner feedback. Completed D3 changes will be documented with their implementation evidence and testing instructions in the D3 deliverables.
