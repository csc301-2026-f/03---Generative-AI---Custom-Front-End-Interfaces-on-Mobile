# Savi AI Widgets — 404 Team Name Not Found

## Partner Intro

- **Partner organization:** Savi Finance
- **Primary contact:** Ralph Maamari
- **Title:** Chief Executive Officer
- **Email:** ralphpalxyz@gmail.com
- **Secondary contact:** Not designated

Savi Finance is a personal finance platform that helps individuals and households organize accounts, understand spending, and track savings goals. Our team is extending its existing mobile application with customizable AI-generated financial widgets.

## Description about the Project

Savi AI Widgets allows users to describe the financial information they want to see and create a personalized dashboard widget.

Fixed dashboards do not suit every user's needs. This feature helps users create, save, refine, and reuse useful financial views without manually configuring charts and other components.

The project is currently under development. The proposed features and final release scope remain subject to team and partner review.

## Key Features

The planned MVP includes:

- **Generate a financial widget:** Describe a financial view in natural language. The system asks clarification questions when needed and creates a design using supported components.
- **Preview and choose a design:** Compare up to three presentations of the requested information and save the preferred design.
- **Save, reopen, and refresh:** Reopen saved widgets without regenerating their designs and refresh the associated financial data.
- **Organize the dashboard:** Pin, unpin, and reorder widgets alongside existing dashboard widgets.
- **Refine selected sections:** Select part of a widget and request a supported change while preserving unrelated content.
- **Undo refinements:** Restore the previous saved design after an accepted change.
- **Remove widgets:** Remove unwanted widgets from the saved collection and dashboard.

Supported financial information may include transactions, income and expenses, account balances, and savings-goal progress. The exact supported requests, editing boundaries, and sharing method will be confirmed with the partner.

## Instructions

### Access the System

The feature is being developed within the existing Savi Finance mobile application. A deployed version of the AI-widget feature is not yet available.

For development and testing, obtain access to the project repositories and an authorized test account, then follow the development setup below.

### Planned User Flow

Once implemented, users will be able to:

1. Sign in to Savi Finance and open the AI-widget creation interface.
2. Describe the financial view they want to create.
3. Answer any clarification questions about the account, date range, or information required.
4. Review the generated designs and choose one to save.
5. Reopen the saved widget and refresh its financial data.
6. Pin, unpin, or reorder the widget on the dashboard.
7. Select a section to refine and use Undo to restore the previous design if needed.
8. Remove unwanted widgets after confirming the action.

Exact screen names, account setup instructions, and feature walkthroughs will be added as implementation progresses.

## Development Requirements

### Technologies

- **React Native:** Mobile application frontend.
- **Go:** Backend API.
- **Styled Components:** Styling for React Native components.
- **Git:** Source control and collaboration.
- **Gemini Flash / Antigravity:** Currently identified for AI-related work; their role in development and the application's runtime configuration requires confirmation.

### Local Setup

Developers should install:

- Node.js and the package manager required by the frontend repository.
- Go.
- Git.
- A compatible Android emulator, iOS simulator, or physical test device.
- Any additional tools required by the existing Savi application.

To prepare the development environment:

1. Obtain access to the frontend and backend repositories.
2. Clone the repositories to your local machine.
3. Install frontend dependencies using the repository's package manager and lockfile.
4. Download backend dependencies using `go mod download` from the directory containing `go.mod`.
5. Configure the required environment variables using the project's setup instructions.
6. Start the backend and configure the frontend to connect to it.
7. Run the mobile application on a supported emulator, simulator, or test device.

Exact commands, supported software versions, and configuration examples will be documented once the project setup is finalized.

### External Dependencies and Third-Party Software

The project uses React Native, Styled Components, and external Go modules. Additional mobile packages and AI services will be documented as they are confirmed.

Frontend dependencies will be tracked in the package manifest and lockfile. Backend dependencies will be tracked in `go.mod` and `go.sum`. Required API keys and environment variables will be documented without committing secret values to the repository.

## Deployment and GitHub Workflow

### Task Management

Jira is the main task-management tool. Larger features will be divided into smaller tickets with assigned owners and priorities. GitHub pull requests will track code changes, while Slack and meeting minutes will record blockers, decisions, and partner feedback.

### Proposed GitHub Workflow

1. Create a branch from the repository's agreed integration branch for each task.
2. Use descriptive branch names such as `feat/<ticket-id>-<description>` or `fix/<ticket-id>-<description>`.
3. Make focused commits and complete the checks relevant to the change.
4. Open a pull request linking the Jira ticket and describing the change and validation.
5. Have at least one other team member review the pull request.
6. Address review comments before the author or designated maintainer merges it, subject to repository permissions.

This workflow keeps changes reviewable and helps identify integration problems before they enter the shared codebase. Final branch rules and partner review requirements will follow Savi's repository policies.

### Deployment

Development and testing will initially use local environments and authorized test accounts. The feature is intended to be integrated into Savi's existing mobile and backend release process.

Deployment tools, release responsibilities, and distribution instructions will be confirmed with the partner.

## Coding Standards and Guidelines

Follow the existing Savi codebase conventions, use descriptive names, and keep changes focused. Format Go code with `gofmt` and use the frontend's configured formatting and linting tools. Validate generated widget definitions before rendering them and document important API or configuration changes.

## Licenses

The project's license and distribution permissions have not yet been confirmed. The team will clarify these with Savi Finance before adding a license or distributing the code.

Third-party dependencies remain subject to their respective licenses.

## Deployed URL / Access Instructions

The AI-widget feature is not yet deployed. Developers and reviewers currently require repository access and an authorized local testing environment.

TestFlight, APK, emulator, and API testing instructions will be added when a testable build is available.

## D3 Improvement Highlight

To be completed for D3. This section will briefly describe the changes since D2 and explain where reviewers can find and test them.
