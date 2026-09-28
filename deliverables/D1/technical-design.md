# Savi AI Widgets — Technical Design

**Working draft.** The [MVP specification](mvp.md) defines the product scope. This document describes the existing foundation and a proposed implementation approach. The [implementation and validation plan](implementation-and-validation-plan.md) describes the work and evidence needed. Unresolved decisions are marked **Not yet discussed**; proposed designs are not claims of completed implementation.

## 1. Design objective

Let users compose useful financial views from supported Savi components, save the selected design, and reuse it reliably. A widget may combine transaction information, income and expenses, account balances, and savings-goal progress.

The saved design should be stable while its financial values can refresh. Reopening, refreshing, and Undo must not require generating the design again. AI helps interpret requests and compose or refine designs; application code controls rendering, financial calculations, permissions, and persistence.

## 2. Existing foundation and gaps

These observations come from the CE source revision `604122cc8bc3bd13a16c34fe8c43192f33f82fa5`. Source links require access to the partner's repository.

| Area | Existing behavior | Work needed for this MVP |
| --- | --- | --- |
| Mobile application | React Native/Expo, shared components, Styled Components, and an established service/API boundary. [Architecture][mobile-architecture] | Integrate widget creation, a renderer for composed designs, and lifecycle screens into the existing app. |
| Generation | `widgetgen` makes one model call and returns a component tree plus data-request declarations. Validation checks the root card, resolver names, and selected parameters. [Generator][generator] | Add clarification, composition, alternatives, and validation of every component, property, and data binding. The current checks do not establish that an entire design is renderable. |
| Saved widgets | The widget model stores prompt history and serialized designs. `createWidget` generates and immediately saves a widget. [Model][widget-model], [creation][create] | Separate candidate previews from saving the user's selected design. Decide how the existing storage and API contract should evolve. |
| Refinement and Undo | Updating regenerates the whole design from prompt history; Undo removes the latest history entry. [Update][update], [Undo][undo] | Enforce selected-section boundaries and preserve unrelated content. Define reliable version handling. |
| Dashboard | The home renderer handles built-in widget IDs; layout restoration filters against the built-in list. [Renderer][dashboard], [layout state][layout] | Support references to saved generated widgets and retain their visibility and order alongside built-in widgets. |
| Sharing and removal | Public widgets can be read through `getWidgetCode`; updating a public widget clones its history. `deleteWidget` removes its library reference but retains the stored document. [Read][read], [update][update], [remove][remove] | Define reusable templates, recipient-owned data binding, and removal behavior. These existing operations do not establish the required sharing contract. |

The mobile chat renderer uses a separate set of typed chat components, and configurable device home-screen widgets are another separate feature. Neither establishes an end-to-end renderer for the saved widget manifests above. [Chat renderer][chat-renderer], [device widgets][device-widgets]

Savi also has a separate chat-agent implementation. Whether the widget generator should reuse its orchestration is **Not yet discussed**. [Chat agent][chat-agent]

## 3. Proposed system responsibilities

| Part | Responsibility |
| --- | --- |
| Mobile interface | Collect requests and clarification answers; preview alternatives; select sections; expose save, organize, Undo, share, and removal actions. |
| Widget service and renderer | Load saved definitions, map approved component types to native components, display data and failure states, and navigate to existing detail screens. |
| Backend generation workflow | Interpret the request, determine required information, compose candidate designs, and coordinate validation. |
| Financial services | Authorize access and supply deterministic financial values through reviewed Savi operations. |
| Widget storage and sharing | Preserve private saved designs and revisions; deliver reusable definitions without granting access to the creator's finances. |

Follow Savi's existing mobile service/API separation and authenticated backend integration. Exact API operations and storage changes are **Not yet discussed**. These responsibilities do not imply a new service or a separate agent for every step.

### Generation workflow

1. **Understand:** Identify the requested information, explicit presentation preferences, and relevant financial context.
2. **Clarify:** Ask focused questions when an account, goal, period, or other necessary choice is ambiguous. Do not silently substitute a different financial question.
3. **Plan the data:** Select approved financial operations and bind them to the user's authorized context. Identify unavailable information before promising a design.
4. **Compose:** Produce one to three useful designs using supported components and mobile layouts. Alternatives preserve the requested information and use consistent financial inputs; their arrangement or presentation can differ.
5. **Validate and preview:** Check supported types, properties, layout, data bindings, navigation actions, and completeness against the request. Display only valid candidates. Financial correctness and authorization cannot depend on the model reviewing itself.
6. **Save:** Persist the selected valid design. Candidate generation alone must not add widgets to the user's saved library.

For example, a request combining savings balance, vacation-goal progress, and recent travel purchases needs several data sources and components within one design. It is not satisfied by choosing a chart for only one part of the request.

**Orchestration — Not yet discussed:** Provider/model, framework, agent structure, clarification state, repair strategy, retry limits, latency, and cost budgets. A single unchecked model response is insufficient for the workflow above, but the number of model calls or agents is not fixed.

## 4. Separate design, financial context, and values

The proposed representation distinguishes four concepts:

- **Design:** Component hierarchy, labels, presentation settings, and permitted interactions.
- **Private instance:** A saved design bound to its owner's selected accounts, goals, and other required context.
- **Current values:** Authorized financial results loaded for that instance. Refreshing values does not rewrite the design.
- **Reusable template:** The shareable design and descriptions of the context a recipient must supply.

A recipient maps the template to their own financial context and saves an independent private instance. The creator's account or goal identifiers must not become the recipient's data source. Financial records, amounts, credentials, and private prompt history must not be included in the reusable payload. User-written labels may also contain personal information; how sharing previews identify or remove that content is **Not yet discussed**.

**Representation — Not yet discussed:** Exact schema, component identifiers, version compatibility, template format, candidate storage, and expiry. These concepts describe required separation, not an approved wire format.

### Financial data

Existing starting points include [transaction queries][transactions], [period analytics][analytics], [account details and balances][accounts], and [saving-goal responses][goals]. Availability of an API does not automatically make it an approved generator tool: the current generator's resolver list does not directly include `getAccount` or `listSavingGoals`.

Use Savi's domain logic to calculate amounts and progress; the model must not invent totals or formulas. Review each supported operation for permissions, filtering, units, currency, and date semantics before connecting it to components.

**Financial rules — Not yet discussed:** Supported account combinations and profile contexts; currency conversion; treatment of transfers and refunds; period boundaries and time zones; which balance and goal-progress measures to expose.

**Time behavior — Not yet discussed:** Fixed dates versus relative periods such as “this month,” and how those periods advance on refresh.

## 5. Components and interaction

Build a reviewed component catalog from Savi's existing primitives and any necessary ordinary application development. The catalog must support the MVP's summaries, charts, tables, lists, and progress indicators in combined mobile layouts. Each entry needs defined properties, data inputs, supported actions, and loading, empty, and error behavior.

The renderer interprets validated definitions using native components; it does not execute generated application code. Reuse Savi's styling and accessibility conventions. Interaction supports inspecting values and navigating to existing account, transaction, and goal details. Embedded filter controls remain outside the MVP.

Validate shared definitions against the same catalog before previewing or saving them. Recheck authorization whenever financial data is loaded, including after a saved widget is reopened.

**Component contract — Not yet discussed:** Exact component and chart types, layout options, styling controls, size limits, accessibility checks, and behavior when an older app cannot render a saved definition.

## 6. Saved-widget lifecycle

| Operation | Required behavior and proposed approach |
| --- | --- |
| Save and reopen | Save the chosen design privately and restore it after restart without AI generation. Preview and save must refer to the same validated candidate. |
| Refresh | Fetch authorized values for the saved definition. Distinguish missing data or failed loading from a genuine zero. Keep the design unchanged. |
| Pin, unpin, reorder | Maintain visibility and order alongside built-in dashboard widgets. Unpinning preserves the saved widget. |
| Refine a section | Identify the selected section explicitly, propose a supported change, and verify that unrelated components and bindings remain unchanged before accepting it. Stable section identifiers are a proposed mechanism. |
| Undo | Restore the previous accepted design from stored history without another model call. Values can then be loaded for that restored design. |
| Share and reuse | Let the recipient preview a reusable design, supply their own context, and save a private copy. Later edits to either copy do not modify the other. |
| Remove | Remove the widget from the user's saved collection and dashboard. Retention of the underlying record and effects on outstanding shares require a decision. |

**Editing boundary — Not yet discussed:** Presentation-only refinement versus changes to the selected section's information or filters.

**Sharing route — Not yet discussed:** Link, in-app invitation, or file; recipient access checks and any expiry or revocation behavior. Existing public-widget access must not be treated as permission to publish financial data.

**Persistence details — Not yet discussed:** Revision identifiers, concurrent edits, repeated save requests, recovery from partial writes, history limits, dashboard storage, and removal/retention policy. The implementation must preserve existing saved work when an operation fails.

## 7. Failure handling and delivery decisions

Unsupported requests should explain the supported boundary. Invalid generation should allow correction or retry without saving a broken widget. Failed edits preserve the last accepted design. Missing recipient context should request a valid selection; it must not fall back to the creator's data. Loading and access failures must remain visible instead of producing plausible-looking values.

Validation must cover these behaviors through the [implementation and validation plan](implementation-and-validation-plan.md), including two-user sharing and restart tests.

**Operational decisions — Not yet discussed:** Observability, diagnostic-data retention, generation quotas, performance thresholds, feature-flag strategy, and compatibility with existing stored widgets.

**Delivery milestone — Not yet discussed:** Production-ready handoff, limited rollout, or public launch; partner/team responsibilities for release and ongoing support.

[mobile-architecture]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/ARCHITECTURE.md
[generator]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/widgetgen/generator.go
[widget-model]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/models/widget.go
[create]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/create_widget/handler.go
[update]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/update_existing_user_widget_prompt/handler.go
[undo]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/undo_widget_prompt/handler.go
[dashboard]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/screens/tabs/home/home-screen/WidgetRenderer.tsx
[layout]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/shared/ui/uiStateManager.ts
[read]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/get_widget_code/handler.go
[remove]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/delete_widget/handler.go
[chat-renderer]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/components/ask-savi/ChatComponentRenderer.tsx
[device-widgets]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/services/widgets/customWidgetTypes.ts
[chat-agent]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/agents/agents/chat/agent.go
[transactions]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/list_transactions/types.go
[analytics]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/get_analytics_by_time_ranges/types.go
[accounts]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/get_account/types.go
[goals]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/rpc/handlers/list_saving_goals/types.go
