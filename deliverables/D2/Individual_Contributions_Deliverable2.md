## Ziheng (Jerry) Wang
**Role:** Owned US-07 (Remove a widget) end to end, from the UI interaction to the removal logic.

**Contributions:**
- Implemented widget removal across both the library and the dashboard: removed widgets can no longer be opened, edited or exported, while imported copies are kept and no data is permanently erased ([PR #6602]).
- - Built the removal interaction: a ⋯ menu on the widget's top-right corner, with confirmation, cancellation with no side effects, and undo ([PR #6602]). Currently reworking it into drag-to-remove for a faster flow (in progress, not part of the D2 release).
## Yifei Yang

**Role:** Owned US-01 (Generate a supported financial view) end to end, from natural-language request interpretation to backend-backed widget generation and mobile integration.

**Contributions:**

- Implemented and integrated the US-01 generation flow, including natural-language request handling, account/month scope confirmation, backend financial data retrieval, and structured widget responses for the mobile app.

- Extended the supported financial views beyond category spending to include monthly expense total, expense trend, expense comparison, and transaction list, while keeping the data generation deterministic and backend-backed rather than model-generated.

- Worked on the READY response structure used by the mobile UI, including the widget artifact, query plan, and financial snapshot, and helped validate that the generated results could be rendered correctly in the existing Savi workflow.

- Contributed to integration testing, debugging, and validation of the complete US-01 flow before the partner demo, including resolving issues between the AI request flow, backend logic, and mobile interface.

## Kaiwen Yang

**Role:** Generating Support View（US-01) for AI-generated spending widgets in the Savi mobile app.

**Contributions:**

- Implemented the monthly-expense widget creation flow, including the mobile creation screen, request/service layer, and `generateSupportedWidgetView` backend RPC with expense aggregation, intent parsing, and request validation.
- Built chart previews and widget composition rendering; added local widget saving and display on the home screen.
- Integrated widget creation into Ask Savi through an interactive chat card and the `prepare_spending_widget` tool; preserved the user's original prompt when opening the creation flow from chat.
- Fixed widget-creation navigation and model-request routing; added support for native widget draft responses in the backend and mobile service.
- Added mobile component/service tests, backend handler and RPC-registration tests, and hermetic tests for widget generation and chat integration; documented the widget request contract.

**Evidence:** [PR #6593](https://github.com/cheapreats/ce/pull/6593).

## Yucheng Duan (Ethan)

**Role:** Owned US-03 and designed the shared widget architecture, helping the team agree on structures and interfaces for related features.

I built on the existing AI Widgets work and Savi components, combining implementation of saved-widget reuse with architecture design and team coordination.

**Contributions:**

- **Designing the widget architecture:** Defined the shared widget data types and how native component trees, query plans, and financial-data snapshots work together. Separated the saved design from changing values so refresh does not require AI regeneration. Organized rendering through a reusable component registry with matching frontend and backend validation.
- **Establishing a shared structure with the team:** Presented the proposed architecture in our weekly meetings, explained how related features could use it, and helped the team agree on common structures and interfaces. Teammates adapted their implementations around this agreement while retaining flexibility in their own approaches.
- **Reopening and refreshing widgets:** Connected the mobile interface to backend operations for listing, loading, and refreshing saved widgets. Refresh uses the saved financial query and preserves the widget's design, while checking access and ownership on every request.
- **Bringing widgets into one library:** Extended the existing library to show built-in, local, and saved native widgets together. Reused Savi's search and filter components, added consistent previews and show/hide controls, and connected saved widgets to the existing Overview layout preferences. Hiding a widget keeps it available in the library.
- **Error handling:** Added retries for failed loads and refreshes, recovery from definition changes, and safeguards against outdated responses after switching profiles. Isolated invalid widgets so they do not prevent healthy entries from opening.
- **Fixing financial edge cases:** Corrected currency conversion and balance allocation for goals sharing an account, and preserved actual date spacing in sparse trend charts. Excluded goals without linked accounts from the catalog and added regression tests.
- **Responding to partner feedback:** Applied Ralph's suggestions by using Customize Widgets as the main entry point, keeping refresh automatic, replacing checkmarks with eye icons, simplifying menus, and adding Savi and creator badges where creator information is available.
- **Testing and documenting the work:** Added and extended mobile and backend tests, including four saved-widget hermetic integration scenarios. Checked the interface in the iOS simulator, prepared screenshots for review, documented the architecture, API contract, and user flows, and resolved conflicts while integrating upstream changes.

**Evidence:** [PR #6586](https://github.com/cheapreats/ce/pull/6586) and [US-03 documentation](https://github.com/cheapreats/ce/tree/fcf6f277ca8e46b70e556ecdf5ec0a4777d7229d/docs/ai-widgets/US-03).
