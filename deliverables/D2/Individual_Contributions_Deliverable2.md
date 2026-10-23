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

## Kaiwen Yang — 1010855200

**Role:** Generating Support View for AI-generated spending widgets in the Savi mobile app.

**Contributions:**

- Implemented the monthly-expense widget creation flow, including the mobile creation screen, request/service layer, and `generateSupportedWidgetView` backend RPC with expense aggregation, intent parsing, and request validation.
- Built chart previews and widget composition rendering; added local widget saving and display on the home screen.
- Integrated widget creation into Ask Savi through an interactive chat card and the `prepare_spending_widget` tool; preserved the user's original prompt when opening the creation flow from chat.
- Fixed widget-creation navigation and model-request routing; added support for native widget draft responses in the backend and mobile service.
- Added mobile component/service tests, backend handler and RPC-registration tests, and hermetic tests for widget generation and chat integration; documented the widget request contract.
