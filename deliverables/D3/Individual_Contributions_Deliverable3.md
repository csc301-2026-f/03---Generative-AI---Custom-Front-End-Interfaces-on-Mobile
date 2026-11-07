# 03-Generative AI Custom Interface- Individual Contributions (D3)


## Kaiwen Yang

**Role:** Built and tested the backend prototype that turns natural-language requests into validated financial formulas and returns the results in Savi’s existing widget preview format.

**Contributions:**

- **Formula planning and confirmation:** Built the `calculateFinancialFormula` JSON-RPC endpoint and connected it to OpenRouter. The planning step proposes a formula without reading transaction data; execution starts only after the caller confirms the account, month, time zone, and currency.
- **Deterministic calculation:** Implemented addition, subtraction, and percentages over backend income and expense totals. The endpoint rechecks transaction access and rejects invalid expressions, incompatible currencies, division by zero, and overflow.
- **Widget preview integration:** Returned the computed result and input totals in the existing native widget preview format. Kept the versioned formula definition separate so a later recalculation can revalidate it and reauthorize the account.
- **Testing and demo:** Built a local demo and added formula, RPC, and MongoDB-backed integration coverage. Seeded cases covered CAD 1,800 in monthly net savings, a 36% savings rate, and rejection of a request for another user’s account.

**Status:** Backend prototype in the local `feat/custom-financial-calculations` worktree. Live model verification and integration into the Ask Savi mobile flow are pending.

Evidence: https://github.com/cheapreats/ce/pull/6593

## Yifei Yang

**Role:** Worked on extending the AI-generated financial widget system toward more flexible custom financial calculations and integrating the new approach with the existing Savi workflow.

**Contributions:**

- **Extended the D2 widget approach:** Helped identify the limitation of relying on fixed supported financial views and contributed to moving toward a more flexible calculation-based design.
- **Contributed to custom financial calculation design:** Worked on representing natural-language financial requests as structured calculations while keeping execution controlled by application logic.
- **Maintained separation between AI and financial logic:** Kept AI focused on interpreting user intent while financial data retrieval and calculations remain backend-backed and validated.
- **Supported integration with the existing widget system:** Worked on connecting the new calculation approach with the existing confirmation, widget generation, and rendering workflow instead of building a separate standalone feature.
- **Contributed to testing and debugging:** Helped validate the interaction between request interpretation, financial data handling, calculation logic, and widget generation as the system evolved from D2 to D3.

**Evidence:** [PR #6593](https://github.com/cheapreats/ce/pull/6593)

## Yucheng Duan (Ethan)

**Role:** Continued developing US-03 (Reopen and refresh saved widgets), focusing on widget-library usability, saved-widget state, and integration with the existing Savi mobile app.

**Contributions:**

- **Refined the unified widget library:** Continued integrating built-in, local, and saved native widgets through the existing Customize widgets entry. Improved search and type filtering, compact previews, creator badges, and show/hide controls connected to Overview.
- **Added date-based discovery:** Implemented combinable Created and Last updated filters with quick presets, custom calendar selection, and open-ended ranges. Added Apply/Cancel behavior, inclusive-day boundaries, and validation for reversed date ranges.
- **Improved saved-widget state and recovery:** Added built-in reset safeguards that preserve custom-widget pins. Continued integrating saved-widget loading and refresh while retaining the saved design and query plan, with error and retry handling for failed operations.
- **Tested and integrated the changes:** Added and extended mobile, backend, and hermetic tests for filtering and saved-widget operations. Resolved widget-state integration issues and resolved upstream accessibility and tag-color changes.
- **Documented and coordinated the workflow:** Maintained the widget architecture and flow documentation, before/after evidence, and PR verification notes. Participated in team and partner discussions to keep the library and saved-widget behavior aligned with related features.

**Evidence:** [PR #6586](https://github.com/cheapreats/ce/pull/6586)

## Yiting(Tina) Gao
**Role:** 
Refine US-02 (print widget previews in chatbox), including CI fixes, regression testing; Phase 2 custom-logic design.

**Contributions:**
- Fixed the widget pinning TypeScript error by explicitly typing the updated state as `UIState`.
- Added Go tests verifying that widget save fields survive database serialization and internal save metadata is excluded from JSON responses.
- Expanded RPC tests to verify widget endpoint registration and rejection of unauthenticated requests.
- Drafted the Phase 2 proposal for converting natural-language financial calculations into structured, validated calculation plans.
- Defined the proposed architecture: cloud services handle authorization, data retrieval, and definition storage; the device executes validated calculations and passes results to the renderer.
- Specified proposed security controls, including restricted operations, authorized data inputs, and execution limits to reduce unauthorized access and resource abuse.

**Evidence:** [PR #6598](https://github.com/cheapreats/ce/pull/6598).

## Junhua Zhou

**Role:** Built AI-generated app themes in Ask Savi, extending D2's widget refinement (US-05/US-06) to the whole app.

**Contributions:**

- **Theme agent:** Added an Ask Savi agent that turns a description ("a calm deep ocean at night") into a full app theme (colours, corner roundness, text size), shown as a preview card. Built on the Light/Dark options from [PR #6284](https://github.com/cheapreats/ce/pull/6284).
- **Validation:** Matching TypeScript and Go validators enforce WCAG AA contrast and keep Savi's gold accent. Invalid AI output goes back to the model, never to the user.
- **Try, Keep, Undo:** Try it previews the theme across the app without saving it; Keep saves it, with Revert and Undo.
- **Storage and rollout:** Added `getAppTheme`/`saveAppTheme` RPCs with revision checks. The feature sits behind the `app_theme_generation` flag, and older app versions never get a card they can't draw.
- **Testing:** Added Jest, Go and hermetic tests for the agent, validation, saves and chat card. Verified the flow end to end in the iOS simulator with a real OpenRouter model.

**Evidence:** [PR #7727](https://github.com/cheapreats/ce/pull/7727) (four commits: theme foundation, account storage, Ask Savi theme agent, chat card and Settings). D2: [PR #6736](https://github.com/cheapreats/ce/pull/6736).

## Ziheng Wang

**Role:** Continuing working on the use case that users can remove widgets straight from the Overview, the way they edit an iOS home screen. Tested and fixed existing bugs and produce prototype on the next phase

**Contributions:**
- Built iOS-style widget removal on Overview ([#6602](https://github.com/cheapreats/ce/pull/6602)), following Ralph's feedback after D2: a long press starts edit mode, cards jiggle (not with reduced motion), a − badge removes a card, and the toast's Undo puts it back in the same slot. Done, the Android back button or leaving Overview ends edit mode.
- In the same PR, fixed the shared Add button (`SplitAddAction.tsx`), which made Add transaction and Add Account widen until they filled the screen on Android; it now measures its label with a hidden copy.
- Wrote tests: the new `useWidgetEditMode.spec.ts` (start and stop editing, Android Back only while editing, stop when Overview loses focus) and edit-mode cases in `WidgetRenderer.spec.tsx`, `useWidgetDragAndDrop.spec.ts` and `uiStateManager.spec.ts`.
- Verified #6602 on the Android emulator and updated `sba-mobile/docs/overview-widgets.md` to describe edit mode.
- Prepared the Phase 2 custom views (Savi's Custom Interfaces): audited the Transactions and Wallet screens on the Android emulator, checked what `main` already supports (sort fields, grouping, category and vendor totals), wrote use cases UC-1 to UC-7, designed 8 Figma screens ([mockup](https://www.figma.com/design/qaNDF00w6vbCEhpD3r1tbG/Savi-Phase-B--mockup?node-id=0-1&p=f&t=I6t60Ynh7xgyLMlD-0)) and listed the backend changes needed.

**Evidence:** https://www.figma.com/design/qaNDF00w6vbCEhpD3r1tbG/Savi-Phase-B--mockup?node-id=0-1&t=I6t60Ynh7xgyLMlD-1
https://github.com/cheapreats/ce/pull/6602
