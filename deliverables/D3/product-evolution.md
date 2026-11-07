# D3 Product Evolution Report

## Project: Savi Finance – AI Interfaces and Custom Financial Widgets

## Team: 404 Team Name Not Found

---

## 1. What Needed to Improve

After D2, our team identified several limitations in the existing widget experience and AI-generated financial workflow.

The first issue was the **Overview editing experience**. The Overview screen showed a stack of widgets, but changing that stack was slow and indirect. Removing a widget required leaving the card, navigating through an additional menu, and applying the change. There was also no quick way to undo an accidental removal. Since Overview is intended to show each user the widgets most relevant to them, editing that layout needed to be more direct. We also identified an unstable Add button on the Overview screen. These issues were specifically highlighted in partner feedback after D2.

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

We also fixed the unstable Add button by constraining the bar on its left side so that the control behaves consistently.

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

Validation checks that the financial totals represented
