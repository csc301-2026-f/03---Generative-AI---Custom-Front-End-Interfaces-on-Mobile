# D3 Product Evolution Report

## Project: Savi Finance – AI Interfaces and Custom Financial Widgets

## Team: 404 Team Name Not Found

---

## 1. What Needed to Improve

After D2, our team identified several limitations in the existing widget experience and AI-generated financial workflow.

The first issue was the **Overview editing experience**. The Overview screen showed a stack of widgets, but changing that stack was slow and indirect. Removing a widget required leaving the card, navigating through an additional menu, and applying the change. Since Overview is intended to show each user the widgets most relevant to them, editing that layout needed to be more direct. We also found a layout bug in the shared Add button on the Transactions and Wallet screens: on Android, it kept widening until it covered the bottom of the screen. These issues were specifically highlighted in partner feedback after D2 and the preparations of the next phase.

The second issue was **widget management and completion of the widget creation workflow**. Users needed better ways to find saved widgets, manage their visibility, compare generated designs, save a selected widget safely, and place it on the dashboard. Retry, cancellation, and partial-failure behaviour also needed to be more reliable.

The third issue was **scalability of AI-generated financial views**. D2 demonstrated that users could request predefined financial views such as spending by category, expense totals, trends, comparisons, and transaction lists. However, a more customized financial question could still require another hard-coded implementation.

For example:

> “How much of my income did I save this month?”

does not directly correspond to one of the original fixed views. If every new type of financial question required another dedicated flow, the system would become increasingly difficult to extend and maintain.

We identified these gaps through partner feedback, testing, integration work, and team reflection on the D2 implementation.

Our goal for D3 was therefore not simply to add more individual features. We wanted to make the existing widget experience more direct and reliable while also creating a more reusable foundation for future financial interactions.

---

## 2. What We Did

### 2.1 Direct Overview Editing

One of the most visible D3 improvements was a new edit mode for the Overview screen, implemented in **PR #6602**.

The interaction was designed to behave similarly to the iOS home-screen editing experience.

A long press on a widget starts edit mode. While editing:

- widgets jiggle to indicate that the layout can be changed;
- widgets that can be hidden display a `−` badge;
- tapping the badge removes the widget directly from Overview;
- a toast provides an **Undo** action for four seconds;
- Undo restores the widget to its previous position;
- the **Done** button exits edit mode;
- the Android back button also exits edit mode;
- leaving the Overview screen ends editing;
- taps on the widget itself are disabled while editing, preventing accidental navigation.

The same long press that begins edit mode can also lift the widget for drag-and-drop. This allows removal and reordering to begin from the same interaction while keeping the actual remove action separate.

Hidden widgets can still be restored through **Customize widgets**, preserving the existing workflow.

We also fixed the Add button that Add transaction (Transactions) and Add Account (Wallet) share. It measured its own container, so its width grew on every layout pass; it now measures the label with a hidden, absolutely positioned copy.

#### Design Decision

Instead of introducing a separate deleted-widget state, removal reuses the existing:

```text id="3dkwe0"
isVisible: false
```

state through `uiStateManager.setWidgetVisibility`.

That visibility state was already stored locally per user.

Reusing it provided several advantages:

- no backend change was required;
- no data migration was required;
- Customize widgets continues to work;
- Undo uses the same state;
- edit mode and existing widget visibility settings remain consistent.

Edit-mode behaviour was also isolated in a dedicated `useWidgetEditMode` hook.

The hook owns:

- entering edit mode;
- exiting edit mode;
- Android back-button handling;
- stopping edit mode when Overview loses focus.

This keeps `HomeScreen` and `WidgetRenderer` simpler because those components only need to read the resulting `isEditing` state.

---

### 2.2 Widget Management and Discovery

We also improved the broader widget-management experience.

The work in **PR #6586** adds show/hide controls and creator badges, making widget state and origin clearer to the user.

Reset behaviour was also improved. Resetting built-in widgets preserves custom widgets and their pinned state instead of resetting the user's entire configuration.

To improve discovery, we added **Created** and **Last Updated** filters that can be combined with existing search and widget-type filters.

The date picker supports:

- seven-day presets;
- thirty-day presets;
- custom date selection;
- Apply behaviour;
- Cancel behaviour;
- validation of invalid date ranges.

These changes make the widget library easier to navigate as users accumulate more saved widgets.

---

### 2.3 Widget Design Selection and Saving

Another D3 improvement continued the widget-generation workflow after the initial request had been confirmed.

US-02 connects a confirmed widget request to chart selection, private saving, and initial dashboard placement.

Users can compare:

- **Bar**
- **Donut**
- **Line**

previews inside the authenticated Ask Savi workflow.

Bar and Donut views display category spending, while Line displays daily spending for the same account and month.

Validation checks that the financial totals represented across the views remain consistent before allowing the user to compare them.

We implemented the:

```text id="y5b75h"
previewMonthlyExpenseWidget
saveWidgetDesign
```

endpoints and connected the selection interface to the existing widget renderer.

The backend verifies account permissions before saving and stores only the selected widget definition in MongoDB.

Dashboard placement remains profile-specific and is stored separately on the device.

We also added stronger behaviour around cancellation and retries:

- cancelling before saving creates no widget;
- stable request identifiers prevent duplicate widgets;
- repeated saves are handled safely;
- if cloud saving succeeds but dashboard placement fails, placement can be retried without saving the widget again.

This was important because backend persistence and dashboard placement are separate operations and can fail independently.

---

### 2.4 Custom Financial Calculation Prototype

Another area of D3 explored how we could move beyond the fixed financial views supported in D2.

We developed a backend prototype that separates **AI interpretation** from **financial execution**.

The OpenRouter planner interprets a user's natural-language request and proposes a structured formula.

After the user confirms the relevant:

- account;
- month;
- time zone;
- currency;

the API validates the formula, verifies account access, retrieves eligible financial transactions, and performs the calculation.

For example, instead of requiring a user to calculate a savings rate manually, the system can calculate both monthly net savings and savings rate.

Using seeded transactions in local MongoDB, the prototype verified:

- **CAD 1,800 net savings**
- **36% savings rate**

An integration test also verified that the API rejects attempts to perform the calculation using another user's account.

A key design decision is that the model does **not** generate financial values directly.

The model helps interpret what the user is requesting, while actual financial values come from authorized backend data and are processed through controlled application logic.

The architecture therefore follows:

```text id="jrylad"
Natural-language request
        ↓
AI interpretation
        ↓
Structured financial operation
        ↓
Validation
        ↓
Authorization
        ↓
Authorized financial data
        ↓
Controlled calculation
        ↓
Existing widget renderer
```

This makes the system more flexible without giving the language model unrestricted control over financial calculations.

---

### 2.5 Integration Challenges

Integration was one of the most significant engineering challenges during D3.

Our changes interact with several existing parts of Savi:

- Overview;
- Ask Savi;
- widget state;
- drag-and-drop;
- local UI state;
- backend financial data;
- MongoDB persistence;
- the widget renderer;
- dashboard placement;
- accessibility;
- error handling.

One challenge was that the same long press on an Overview widget now has two responsibilities: starting edit mode and beginning drag-and-drop.

Instead of choosing one behaviour, we allow the same long press to do both. Removal remains a separate tap on the `−` badge. This ensures that dragging a card can never accidentally remove it.

Another challenge came from upstream changes in Savi's main branch.

For example, **PR #6075 (Finance Wrapped)** changed test fixtures and caused failures in `HomeScreen.spec.tsx` and `InsightsScreen.spec.tsx` unrelated to our work.

Instead of ignoring or skipping these tests, we built on top of the fix from **PR #6603**, which repaired the affected fixtures.

We also needed to reconcile:

- widget-state differences;
- accessibility changes;
- tag-colour updates;
- existing error-handling behaviour;
- undefined invoice error helpers;
- differences between cloud-saved widget state and device-local dashboard state.

Because the partner repository receives frequent changes, our feature branches can quickly fall behind main. This resulted in additional merge conflicts, CI failures caused by unrelated changes, and integration work.

---

### 2.6 Testing, Reliability, and Accessibility

D3 increased our focus on testing not only successful user flows but also edge cases and recovery behaviour.

For Overview edit mode, tests cover:

#### `useWidgetEditMode.spec.ts` (new)

- starting edit mode;
- stopping edit mode;
- handling Android Back only while editing;
- stopping editing when Overview loses focus.

#### `WidgetRenderer.spec.tsx`

- starting edit mode when a long press lifts a widget;
- showing the drag grip only in edit mode;
- showing a remove badge on each hideable widget while editing;
- showing no remove badge outside edit mode, or on a widget that cannot be hidden.

#### `useWidgetDragAndDrop.spec.ts`

- calling `onLongPress` when a long press lifts an item.

#### `uiStateManager.spec.ts`

- hiding a widget in place and persisting it;
- showing a hidden widget back in its original slot after the other widgets were reordered.

For the wider widget workflow, regression tests also cover:

- cancellation before saving;
- repeated saves;
- uncertain save outcomes;
- interrupted dashboard placement;
- profile changes;
- account authorization;
- financial-calculation correctness.


Accessibility work also included:

- accessibility labels;
- selection states;
- save-status announcements.

Manual VoiceOver/TalkBack verification still remains before D4.

CI evidence is also still being finalized. A previously recorded failure was addressed and the fix was pushed, but we will only describe the pipeline as confirmed passing once a fresh CI run provides that evidence.

---

### 2.7 Future AI Interface Direction

In parallel with the implemented D3 work, we refined the longer-term direction of AI-powered financial interfaces.

One direction under consideration is an interactive transaction explorer that could allow users to request different filtering or grouping structures for transaction data.

For example, the user could ask the system to organize recent transactions by different levels depending on the financial question being asked.

This remains a **design proposal**, not a completed D3 feature.

The important outcome of this exploration was that our direction became more specific.

Rather than allowing AI to generate unrestricted interfaces, we are moving toward structured financial interactions where AI interprets intent while application logic controls:

- available filters;
- grouping behaviour;
- calculations;
- permissions;
- data access;
- rendering.

---

### Relevant Pull Requests

- **PR #6586** – Widget-management improvements
- **PR #6593** – AI widget creation and Ask Savi integration
- **PR #6602** – Overview edit mode and direct widget removal

---

## 3. Before vs. After

### Before D3

At the end of D2, the basic AI widget concept worked.

A typical financial-widget workflow was:

```text id="trb8rh"
User request
    ↓
Match a supported view
    ↓
Confirm account and month
    ↓
Retrieve authorized data
    ↓
Generate widget
```

However, several parts of the surrounding product experience remained incomplete.

On Overview, removing a widget was indirect. A user had to leave the screen they were actively editing, navigate through another menu, apply the change, and repeat the process if the removal was accidental.

The shared Add button also kept widening until it covered the bottom of the screen.

Widget discovery was limited.

Design selection, private saving, and dashboard placement were not yet fully integrated.

Retry and partial-failure behaviour needed improvement.

The AI workflow also remained constrained by predefined financial views.

---

### After D3

The Overview experience is now much more direct.

A user can:

```text id="9kfzj3"
Long press
→ enter edit mode
→ tap −
→ widget disappears
```

without leaving Overview.

If the removal was accidental:

```text id="cvp6am"
Undo
→ widget returns to the same position
```

within four seconds.

The same long press also begins drag-and-drop, so removing and reordering widgets now start from one interaction rather than requiring separate workflows.

The Add button now keeps a stable width.

The widget library now offers stronger discovery and visibility controls, including search, widget type, creation date, update date, show/hide controls, and creator information.

The widget-generation workflow has also become more complete.

Users can move from a confirmed request to:

```text id="fshcjh"
Confirmed request
→ compare chart designs
→ select one
→ save privately
→ place on dashboard
```

with duplicate-save protection, cancellation handling, and recovery from partial placement failures.

The custom financial-calculation prototype also demonstrates how the system can move beyond fixed financial views.

Instead of relying only on:

```text id="9xtww9"
Request
→ predefined view
→ widget
```

the architecture can increasingly support:

```text id="icqun3"
User request
→ interpretation
→ structured operation
→ validation
→ authorization
→ financial data
→ controlled execution
→ existing widget workflow
```

The improvement from D2 to D3 is therefore both **user-facing and architectural**.

From the user's perspective, widget editing, discovery, saving, and recovery are more direct.

From the engineering perspective, the widget system is increasingly based on reusable state, shared rendering infrastructure, validation, and controlled financial execution rather than independent hard-coded flows.

---

## 4. Reflection

### What Worked Well

Reusing existing state and infrastructure worked particularly well.

For Overview edit mode, using the existing `isVisible` state avoided introducing another persistence mechanism. This meant we needed:

- no backend change;
- no migration;
- no duplicate widget-visibility state.

Customize widgets, Undo, and direct Overview editing can all operate on the same value.

Similarly, reusing the existing renderer for generated-widget previews and saved widgets reduced duplication and kept different parts of the product visually consistent.

Keeping edit-mode behaviour inside `useWidgetEditMode` also improved testability and reduced the amount of state-management logic inside larger screen components.

Another successful decision was separating AI interpretation from financial execution. This gives the system more flexibility while keeping account permissions, financial values, and calculations under application control.

Testing isolated behaviours before complete integration also helped us identify errors more precisely.

---

### What Was Harder Than Expected

Integration was harder than expected.

Savi's main repository changes frequently, sometimes with more than ten pull requests being merged in a day. As a result, a branch that worked correctly could quickly fall behind main.

This created:

- merge conflicts;
- unrelated test failures;
- CI failures;
- fixture incompatibilities;
- additional rebasing and coordination.

The failure caused by the Finance Wrapped fixture changes is one example. Instead of disabling the affected tests, we integrated the corresponding upstream test repair.

The interaction between editing and drag-and-drop also required more design work than expected because one gesture needed to support both behaviours without allowing accidental removal.

Partial failure in the save-and-place workflow was another challenge. Backend saving and local dashboard placement can succeed or fail independently, so recovery needed to be designed without creating duplicate state.

---

### Effect on the Path to D4

D3 gave us a clearer direction for D4.

Our team plans to split into smaller groups to work on the next phase of custom interfaces, including the **Transactions** and **Wallet** areas.

Several pieces of D3 work can be reused directly.

For example, Wallet section reordering and visibility can reuse the same edit-mode and visibility-state pattern introduced for Overview.

We also learned an important process lesson from D3: large dependency chains between unmerged pull requests slow integration and make CI harder to interpret.

For D4, we plan to:

- keep PRs smaller;
- base new work more closely on main;
- reduce dependencies between unmerged branches;
- merge related work in a clearer order;
- obtain fresh CI evidence earlier;
- complete accessibility verification;
- verify behaviour on additional platforms where possible;
- improve end-to-end testing;
- resolve remaining integration issues;
- continue connecting flexible financial interactions to the existing Savi workflow.

For the AI side of the project, D4 will also require us to make the boundary between AI interpretation and application-controlled behaviour explicit.

We need to determine which filtering, grouping, and calculation operations are supported and make sure those operations remain predictable and testable.

Overall, D3 moved the project from a working prototype toward a more mature product.

The user-facing workflow is more direct, the underlying widget state is more reusable, error and retry behaviour is more deliberate, and the AI-interface direction is more constrained and technically realistic.

The remaining D4 work is therefore focused less on proving that the concept works and more on making the complete system stable, testable, accessible, and ready for final integration.
