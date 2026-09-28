# Savi AI Widgets — MVP Specification

## 1. Overview

Users describe a financial view; Savi composes a custom widget from supported components. They can save, reopen, organize, refine, and share the design for another user to reuse.

**Hypothesis:** Users can create, understand, and reuse a useful financial view without manually configuring its components.

## 2. Target Users

Everyday Savi users who want financial views suited to their needs, without needing to understand chart terminology or application internals.

## 3. Core Problem

A fixed dashboard cannot fit every user's workflow. Repeated prompting may also produce inconsistent designs. Users need to create a personalized view once and reliably reuse it.

**Representative requests — Not yet discussed:** Agree on 3–5 real user requests with the partner for the demo and evaluation.

## 4. Core User Flow

**Describe → clarify if needed → generate and check → preview up to three designs → choose and save → reopen, organize, refine, or share.**

The generator understands the request, identifies the information needed, asks focused questions when necessary, selects reusable components, and composes and validates the design.

Illustrative request: “Show my savings balance, vacation-goal progress, and recent travel purchases.” Clarify the account or goal if needed.

The [D1 mockup](<Mockup/CSC301 — AI Widgets Prototype/readme.md>) illustrates earlier interactions; it does not cover this entire specification.

## 5. In-Scope Features

| Capability | MVP commitment |
| --- | --- |
| Financial information | Transactions, income and expenses, account balances, and savings-goal progress available through Savi. |
| Supported requests | Summarize, compare periods, show trends or category breakdowns, and filter or rank transactions. |
| Composition | Combine summaries, charts, tables, lists, and progress indicators within one widget using supported components and mobile layouts. |
| Alternatives | Offer one to three useful designs. Preserve explicit preferences and requested information; vary components, arrangement, and emphasis. |
| Interaction | Inspect displayed values and navigate to existing account, transaction, or goal details. |
| Persistence | Save the selected design privately; reopen without AI regeneration and refresh authorized financial data. |
| Organization | Pin, unpin, reorder alongside existing dashboard widgets, and remove saved widgets. |
| Refinement | Select a section, request a supported change, and Undo an accepted refinement. Preserve unrelated content. |
| Reusable sharing | Let another Savi user preview the design, select their own financial context, and save an independent private copy. |

Reusable-design sharing is the primary sharing feature, reflecting the partner's feedback.

**Editing boundary — Not yet discussed:** Whether refinement changes presentation only or can also change the selected section's information and filters.

**Sharing route — Not yet discussed:** Link, in-app invitation, or file delivery.

**Time behavior — Not yet discussed:** How fixed dates and relative requests such as “this month” behave as time passes. Data refresh must preserve the saved design.

## 6. Out-of-Scope Features

- Generating unsupported component types or executing arbitrary AI-generated code while the app is in use.
- New banking integrations, money transfers, transaction/goal creation or modification, financial advice, forecasts, and arbitrary financial calculations.
- Marketplace discovery, collaborative widget editing, and public web views of financial data.
- Embedded account/category/period filter controls; the initial interaction supports inspection and navigation.

## 7. User Stories

- **US-01:** As a Savi user, I want to describe a financial view so I can see what matters to me.
- **US-02:** As a Savi user, I want to preview and save a design so I can choose a useful presentation.
- **US-03:** As a Savi user, I want to reopen and refresh widgets so I can reuse them without repeating requests.
- **US-04:** As a Savi user, I want to pin, unpin, and reorder widgets so useful views are easy to reach.
- **US-05:** As a Savi user, I want to change a selected section so I can refine it without rebuilding everything.
- **US-06:** As a Savi user, I want to Undo a refinement so I can recover the previous design.
- **US-07:** As a Savi user, I want to share and reuse widget designs so I can apply useful ideas to my own finances.
- **US-08:** As a Savi user, I want to remove unwanted widgets so my dashboard and library stay organized.
- **US-09:** As a Savi user, I want clear feedback and recovery options so failures do not lose my work.
