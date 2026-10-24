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

**Role:** Implemented the US-01 AI-generated spending widget flow across the mobile app and backend, with a focus on Ask Savi integration, scope confirmation, previews, and adding generated widgets to Home.

I built the initial monthly-expense generation flow and connected it to Savi's chat and mobile interface, working with teammates on the shared widget contracts and subsequent integration.

**Contributions:**

- **Building the initial generation flow:** Implemented the mobile widget creation screen, request parsing and service layer, and `generateSupportedWidgetView` backend RPC. Connected natural-language requests to account/month scope selection, expense aggregation, and structured responses that the mobile app can render.
- **Confirming scope and protecting financial data:** Added request validation and explicit scope confirmation before generating a financial view. Used backend transaction aggregation for expense amounts and kept model interpretation separate from financial calculations, while using the existing account authorization flow.
- **Integrating creation into Ask Savi:** Added the `prepare_spending_widget` chat tool and an interactive widget card that opens the creation flow from a conversation. Preserved the user's original prompt across the chat-to-widget handoff so the requested view and presentation context are retained.
- **Developing the chat and creation interface:** Updated the Ask Savi header, prompt suggestions, composer, history, and New Chat controls, and connected the account/month confirmation flow to widget generation. Fixed navigation from My Widgets so users can reach the intended creation entry point.
- **Rendering widget previews:** Built bar and donut spending previews with category labels, account/month context, currency-specific totals, and an empty-data state. Added composition rendering and validation for supported presentation settings so generated layouts can be displayed through native mobile components.
- **Adding generated widgets to Home:** Implemented device-local, profile-scoped storage and Home rendering for confirmed widget snapshots. Added duplicate detection, a saved-widget limit, serialized storage updates, and clearing of local widget data on sign-out.
- **Connecting model requests and native drafts:** Updated the shared OpenRouter request handling and widget intent transport, fixed model-request routing, and added backend and mobile-service support for native widget draft responses. Kept the generated presentation connected to the validated financial scope.
- **Testing and debugging the workflow:** Added mobile screen, chat-card, preview, and service tests; backend handler, validation, model-transport, and RPC-registration tests; and hermetic coverage for widget generation and chat integration. Added regression tests for prompt preservation and request-routing fixes.
- **Documenting and integrating the work:** Documented the widget request/response contract and chat-tool handoff, resolved conflicts while integrating upstream changes, and fixed local presubmit and Semgrep worktree issues to support validation before review.

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

## Yiting(Tina) Gao

**Role:** Implemented US-02 (Choose from 3 design views), covering preview comparison and saving the selected widget design.

**Contributions:**

- Implemented US‑02’s Bar, Line, and Donut previews inside Ask Savi using the existing renderer.
- Built two backend endpoints for monthly-expense previews and private widget saving in MongoDB, with duplicate-save protection and initial dashboard placement.
- Added regression tests for cancellation, retries, profile isolation, persistence, and authorization. 
- Validated the integration with 127 focused mobile tests and 360 backend integration tests.

**Evidence:** [PR #6598](https://github.com/cheapreats/ce/pull/6598).
