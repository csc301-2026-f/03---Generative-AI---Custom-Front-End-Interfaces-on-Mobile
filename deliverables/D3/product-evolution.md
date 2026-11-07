# D3 Product Evolution Report

## Project: Savi Finance – AI Interfaces and Custom Financial Widgets

## Team: 404 Team Name Not Found

---

## 1. What Needed to Improve

After D2, our team identified two major limitations.

The first was the maturity of the existing widget workflow. D2 proved that users could generate supported financial widgets from natural-language requests, but the broader widget experience was still incomplete. Users needed clearer visibility controls, better ways to search and filter saved widgets, and more reliable behaviour around previewing, saving, retrying, and placing widgets on the dashboard.

The second limitation was scalability. The D2 system primarily relied on a fixed set of supported financial views. This worked well for requests such as spending by category, totals, trends, comparisons, and transaction lists, but it did not handle more customized financial questions well.

For example, a request such as:

> “How much of my income did I save this month?”

does not map naturally to one of the original fixed views. Supporting every new financial question with another hard-coded flow would make the system increasingly difficult to extend and maintain.

We identified these gaps through team reflection, integration testing, and discussions with our partner, Ralph. Partner discussions also helped us narrow the broader idea of “AI Interfaces” into more concrete and realistic use cases, including structured financial calculations, interactive filtering, and hierarchical transaction exploration.

Our D3 goal therefore became twofold: improve the reliability and usability of the existing widget system, while also creating a more scalable direction for future AI-powered financial interactions.

---

## 2. What We Did

### 2.1 Widget Management and Discovery

One area of D3 focused on improving how users manage existing widgets.

The widget-management work in **PR #6586** adds clearer show/hide controls and creator badges. This gives users better visibility into which widgets are active and where they came from.

We also improved reset behaviour. Resetting built-in widgets now preserves custom widgets and their pinned state instead of overwriting the user’s full configuration.

To make saved widgets easier to find, we added **Created** and **Last Updated** filters that work together with search and widget-type filters.

The date picker supports:

- seven-day and thirty-day presets;
- custom calendar selection;
- Apply and Cancel behaviour;
- validation for invalid date ranges.

These changes improve discoverability and make the widget library easier to manage as the number of saved widgets increases.

### 2.2 Widget Design Selection, Saving, and Dashboard Placement

Another major D3 improvement extended the widget-generation workflow beyond the initial request.

US-02 connects a confirmed widget request to chart selection, private saving, and initial dashboard placement.

Users can compare **Bar**, **Donut**, and **Line** previews within the authenticated Ask Savi flow.

Bar and Donut views display category spending, while Line displays daily spending for the same account and month. Validation checks that the underlying currency totals agree before comparison.

We implemented the `previewMonthlyExpenseWidget` and `saveWidgetDesign` endpoints and integrated the chart-selection component with the existing renderer.

The backend checks account permissions and saves only the selected widget definition in MongoDB. Dashboard placement remains profile-scoped and is stored separately on the device.

We also improved retry and cancellation behaviour:

- cancelling before saving creates no widget;
- stable request identifiers prevent duplicate saves;
- repeated save requests remain idempotent;
- if cloud saving succeeds but dashboard placement fails, the user can retry placement without creating a second widget.

This moved the product closer to a complete end-to-end workflow rather than a collection of independent features.

### 2.3 Custom Financial Calculation Prototype

A separate D3 improvement explored how the system could move beyond the fixed financial views from D2.

We built a backend prototype that separates **AI interpretation** from **financial execution**.

The OpenRouter planner interprets the user’s request and proposes a structured formula. After the user confirms the account, month, time zone, and currency, the API validates that formula, verifies account access, retrieves eligible financial transactions, and performs the calculation.

For example, instead of requiring a user to manually calculate their savings rate, the prototype can calculate both monthly net savings and savings rate.

Using seeded transactions in local MongoDB, the prototype verified:

- **CAD 1,800 in net savings**
- **36% savings rate**

The integration test also verified that a request using another user’s account is rejected.

A key architectural decision is that the language model does not directly generate the financial values.

The AI is responsible for interpreting the user’s intent. Financial values are derived from authorized backend data and calculated using controlled application logic.

The flow is therefore:

```text
Natural-language request
        ↓
AI interpretation
        ↓
Structured calculation definition
        ↓
Validation
        ↓
Authorization
        ↓
Authorized financial data
        ↓
Controlled calculation
        ↓
Existing widget preview / renderer
```

This separation improves reliability and reduces the risk of model-generated financial values.

### 2.4 Integration Work

Integration was one of the largest engineering challenges during D3.

Our features interact with several existing parts of Savi:

- Ask Savi;
- backend financial data;
- widget state;
- the native widget renderer;
- dashboard placement;
- accessibility behaviour;
- error handling.

One challenge was aligning US-01’s generated draft response with the component-tree structure and dashboard lifecycle required by later user stories.

Instead of introducing another rendering system, we reused the existing renderer. This helped keep generated previews and saved widgets consistent.

Integration work also included incorporating accessibility and tag-colour updates, reconciling widget-state differences, and fixing undefined invoice error helpers.

Because Savi’s main application continued changing during this iteration, keeping our work compatible with upstream changes required additional debugging and coordination.

### 2.5 Testing and Reliability

D3 also increased our focus on testing edge cases rather than only successful flows.

Regression coverage was added for cases including:

- cancellation before saving;
- repeated saves;
- uncertain save outcomes;
- interrupted dashboard placement;
- user-profile changes;
- account authorization;
- financial-calculation correctness.

This was particularly important because some of the new workflows span both backend and device-local state.

We also added accessibility support to the chart-selection flow, including accessibility labels, selection states, and save-status announcements.

Manual screen-reader verification is still required before D4.

### 2.6 AI-Powered Transaction Explorer Direction

In parallel with the implemented D3 work, we discussed an AI-powered interactive transaction explorer with Ralph.

The proposed experience would allow users to describe how they want their transactions grouped or filtered, for example by asking for different levels of transaction grouping based on a natural-language request.

This remains a **design proposal**, not a completed D3 feature.

However, the discussion helped us refine the longer-term direction of the project.

Instead of pursuing unrestricted AI-generated interfaces, we are focusing more on structured financial interactions in which AI interprets the user’s request but the application controls filtering, grouping, calculations, authorization, and rendering.

Partner discussion:

[Discord discussion](https://discord.com/channels/1550329767989878876/1550329768640258130/1568445544773320807)

### Relevant Pull Requests

- [PR #6586 – Widget management improvements](https://github.com/cheapreats/ce/pull/6586)
- [PR #6593 – Integrate AI widget creation into Ask Savi chat](https://github.com/cheapreats/ce/pull/6593)

---

## 3. Before vs. After

### Before D3

At the end of D2, the system could support the core AI widget-generation workflow:

```text
User request
    ↓
Match a supported financial view
    ↓
Confirm account and month
    ↓
Retrieve authorized financial data
    ↓
Generate widget
```

This demonstrated that the product concept worked, but several limitations remained.

Users had limited widget-discovery and visibility controls.

The design-selection and saving workflow was not yet fully connected to dashboard placement.

Retry and partial-failure handling were still incomplete.

Most importantly, more customized financial questions still depended on adding new hard-coded logic.

### After D3

The widget-management experience is more mature.

Users now have clearer visibility controls and more precise filtering options based on search, widget type, creation date, and update date.

The widget-generation workflow is also more complete.

Users can compare multiple chart presentations, save a selected design, and place the widget on the dashboard.

Saving behaviour is more reliable because retries do not create duplicate widgets, cancellation has no side effects, and dashboard placement can be retried separately after a successful backend save.

The custom-calculation prototype also demonstrates a path beyond fixed views.

Instead of:

```text
Request
→ predefined financial view
→ widget
```

the system can increasingly support:

```text
User request
→ interpretation
→ structured calculation / filtering
→ validation
→ authorization
→ financial data
→ controlled execution
→ reusable widget workflow
```

The improvement is therefore not just the addition of more features.

The underlying architecture is becoming more reusable.

From the user’s perspective, this supports more personalized financial questions and more reliable widget management.

From the engineering perspective, future requests can reuse the same interpretation, validation, authorization, calculation, rendering, and persistence infrastructure rather than requiring an entirely new implementation each time.

---

## 4. Reflection and Next Steps

### What Worked Well

One of the strongest parts of this iteration was our ability to build on top of the existing D2 architecture.

Instead of replacing the previous widget system, later user stories reused the same renderer, confirmation flow, backend data path, and dashboard infrastructure.

This reduced duplication and made the product more cohesive.

Partner discussions were also useful because they helped us narrow the scope of AI Interfaces into more concrete financial use cases rather than keeping the concept too broad.

Another successful design decision was keeping AI interpretation separate from financial execution.

The AI can help understand what the user wants, while financial values, permissions, validation, and calculations remain controlled by application logic.

Testing individual pieces independently also made it easier to verify numerical correctness, account permissions, retry behaviour, cancellation, and partial failures.

### What Was Harder Than Expected

Integration was harder than expected.

Our work exists inside a large partner codebase, so upstream changes can affect widget state, rendering, accessibility, backend APIs, error handling, and dashboard behaviour.

This required additional conflict resolution, testing, and debugging.

Partial failures were also more complicated than simple success cases.

For example, backend saving and dashboard placement are separate operations. If only one succeeds, the system must allow recovery without creating duplicate state.

Financial-language ambiguity was another challenge.

Terms such as “savings,” “combined,” or “recent transactions” can have multiple valid meanings. Supporting more flexible requests therefore requires clarification and validation rather than silently selecting one interpretation.

Accessibility also required more work than simply adding labels. While the selection interface includes accessibility states and announcements, manual VoiceOver/TalkBack verification remains necessary.

CI evidence is also still being finalized. A previously observed failure was addressed and the fix was pushed, but we will only describe CI as confirmed passing once a fresh run verifies it.

### Path Toward D4

For D4, our focus is primarily on stabilization and completing the strongest workflows rather than introducing another major architectural change.

Our priorities are:

- resolve outstanding PR and integration issues;
- verify the full product against the latest partner branch;
- obtain fresh CI evidence;
- complete manual accessibility and screen-reader verification;
- improve retry and error-handling behaviour;
- connect the custom financial-calculation prototype more fully to Ask Savi;
- complete confirmation and preview behaviour for custom calculations;
- finalize the scope of the transaction-explorer concept;
- define supported filtering and grouping levels;
- clearly define where AI interpretation ends and application-controlled behaviour begins;
- expand end-to-end testing;
- polish documentation and the final user experience.

The transaction explorer remains a future direction rather than a completed D3 feature.

Similarly, unfinished CI or accessibility verification will not be presented as complete until we have evidence.

Overall, D3 moved the project from a functional prototype toward a more mature system.

The widget-management and saving workflow became more complete and reliable, while the AI-interface direction became more focused, scalable, and technically grounded.

Instead of simply adding more hard-coded widget types, the project is moving toward a reusable architecture where natural-language financial requests can be interpreted flexibly while financial data access, validation, execution, and rendering remain controlled by the application.

This gives us a clearer and more achievable path toward a stable D4 release.
