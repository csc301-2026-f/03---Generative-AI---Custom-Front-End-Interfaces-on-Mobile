# Savi AI Widgets — Implementation and Validation Plan

**Working draft.** This plan supports the [MVP specification](mvp.md) and [technical design](technical-design.md). It proposes a delivery sequence and validation criteria. It does not report assigned work, completed implementation, passed tests, or partner acceptance of unresolved decisions.

## 1. Decisions to resolve

| Topic | Status | What it affects |
| --- | --- | --- |
| Representative requests | **Not yet discussed:** Choose 3–5 real requests with the partner. | Demo scenarios, component priorities, financial operations, and usability tasks. |
| Editing boundary | **Not yet discussed:** Presentation only, or changes to a selected section's information and filters. | Refinement UI, validation, and test cases. |
| Sharing route | **Not yet discussed:** Link, in-app invitation, or file. | Recipient entry point, access rules, and sharing tests. |
| Time behavior | **Not yet discussed:** Fixed and relative periods, including what changes at a period boundary. | Saved data requests, refresh behavior, and expected values. |
| Delivery milestone | **Not yet discussed:** Production-ready handoff, limited rollout, or public launch. | Release evidence, ownership, environment, and support. |

Detailed component, data, API, persistence, and generator decisions are recorded in the technical design. Owners, estimates, dates, and dependencies on partner staff are **Not yet discussed**. The sequence below is a proposal, not a schedule.

## 2. Proposed implementation sequence

| Stage | Work | Exit evidence |
| --- | --- | --- |
| 1. Establish the foundation | Confirm controlled test access, select representative requests, review financial semantics, and agree on an initial component/data contract. Identify reusable Savi code and required extensions. | Recorded decisions, verified environment, and reviewed example definitions with known expected values. |
| 2. Prove one integrated flow | Connect request handling, necessary clarification, validated composition, preview, selection, private save, and reopening in the Savi app. Start with one agreed request. | A saved design survives restart and loads authorized values without regeneration. This is an intermediate milestone, not the complete MVP. |
| 3. Cover the agreed financial views | Extend coverage across transactions, income/expenses, balances, and savings-goal progress, including combined widgets. Add one to three meaningful alternatives, value inspection, and detail navigation. | Representative requests preserve all requested information; alternatives agree on financial values and honor explicit preferences. |
| 4. Complete personal reuse | Add pin/unpin/reorder, selected-section refinement, Undo, removal, and recovery across the lifecycle. | Saved state persists; unrelated sections remain unchanged; failures preserve accepted work. |
| 5. Complete reusable sharing | Build the agreed delivery route, template validation, recipient context selection, preview, and independent saving. | Two test users can reuse the same design with different financial data without sharing private values or modifying each other's copies. |
| 6. Validate and prepare delivery | Run functional checks, real-model evaluation, and a usability pilot. Resolve failures and assemble the evidence required for the agreed delivery milestone. | Recorded results, remaining limitations, and a handoff or release package appropriate to the milestone. |

Define sharing requirements early so that private account references and prompt history are not built into a supposedly reusable format. Develop failure handling alongside each stage. Mobile rendering and backend work can proceed independently once their shared contract is agreed; team assignments remain open.

## 3. Functional acceptance checklist

These are proposed observable checks mapped to the MVP's user stories. They are distinct from the usability measures in section 5. Detailed expected values depend on agreed financial rules and controlled test data.

| ID | Stories | Required observation |
| --- | --- | --- |
| V-01 | US-01, US-09 | A supported request produces an appropriate view. Missing essential context triggers a focused clarification. Unsupported requests produce understandable feedback without a broken saved widget. |
| V-02 | US-02 | The user previews one to three valid designs, chooses one, and saves it privately. Alternatives preserve requested information and explicit preferences, with consistent underlying values. Unchosen previews do not become saved widgets. |
| V-03 | US-03 | After restart, the selected design reopens without a generation call. Changing controlled source data and refreshing updates the relevant values without changing the design. |
| V-04 | US-04 | Pinning, unpinning, and reordering work alongside built-in dashboard widgets and persist after restart. Unpinning keeps the saved widget available. |
| V-05 | US-05 | A supported refinement changes the selected section while unrelated design content and data bindings remain unchanged. An unsupported or failed edit preserves the prior accepted design. Exact permitted edits are **Not yet discussed**. |
| V-06 | US-06 | Undo restores the design preceding an accepted refinement without AI regeneration. It does not undo changes in the underlying financial accounts. |
| V-07 | US-07 | A second user previews the shared design, selects their own context, and saves a private copy. Each copy shows its owner's authorized data; subsequent edits remain independent. Private records, account/goal identifiers, financial values, and prompt history are absent from the reusable payload. |
| V-08 | US-08 | Removal clears the widget from the library and dashboard and remains effective after restart. It does not remove another user's independent copy. Record retention and outstanding-share behavior are **Not yet discussed**. |
| V-09 | US-09 | Generation, network, validation, access, and save failures provide clear recovery options. Existing saved designs survive failures; unavailable data is not shown as a legitimate zero. |
| V-10 | US-01, US-02, US-03 | Displayed totals, lists, balance values, and progress match independently checked expectations. Inspection and navigation reach the relevant existing details. Combined widgets retain every requested supported element. |

Use real authenticated Savi services with controlled test accounts for end-to-end acceptance. Deterministic model fakes and component fixtures support development and automated tests; they do not establish real-model quality or integrated completion by themselves.

## 4. Test data and engineering verification

Prepare two isolated users with deliberately different accounts, transactions, and savings goals. Record expected results independently of the generator. Include populated data, a genuine zero, no matching transactions, ambiguous account/goal choices, unavailable context, and denied access. Add transfer, refund, currency, and date-boundary cases after the corresponding financial rules are settled.

Verify the feature at four levels:

- **Data and definition checks:** Component/property validation, permitted data operations, correct calculations, complete composition, and rejection of unsupported actions or definitions.
- **API and persistence checks:** Ownership, preview/save behavior, repeated requests, restart persistence, edit boundaries, Undo, removal, and two-user sharing. Concurrent-update and retry expectations require the persistence decisions first.
- **Mobile checks:** Loading/empty/failure states, readable combined layouts, selection, value inspection, navigation, and dashboard organization on the agreed devices.
- **Real-model checks:** Representative requests and paraphrases, explicit presentation preferences, ambiguity, unsupported requests, and malformed output. Record completeness, correctness, latency, and failures rather than judging only whether JSON was returned.

Existing [generator tests][generator-tests] and [widget RPC hermetic tests][widget-tests] provide starting points. Extend tests to the agreed behavior; existing tests are not evidence that the new composition or sharing flow works.

### Repository checks for implementation work

Follow the CE repository's [contribution and test instructions][repo-instructions] and applicable local instructions. The commands below are verified entry points, not results from this documentation task.

| Check | Directory within CE | Entry point |
| --- | --- | --- |
| Dependency readiness | Repository root | `make check-deps` |
| Backend unit/build checks | `sf1/` | Relevant Bazel targets; use Bazel for Go work. |
| API integration | Repository root | `make hermetic`, with the controlled local development services running. |
| Mobile unit tests and lint | `sba-mobile/` | `npm test -- --runInBand` and `npm run lint` |
| Repository presubmit | Repository root | `make presubmit` |
| Device walkthrough | `sba-mobile/` | Existing [iOS][ios-harness] and [Android][android-harness] harnesses on the selected test devices. |

`make presubmit` does not replace hermetic tests or mobile checks. Verify the app's API environment and test identity before running integration scenarios. Record failed or blocked checks honestly. For mobile implementation PRs, follow the repository's before/after visual verification and Savi Moment requirements.

**Test setup — Not yet discussed:** Fixture ownership, test-account provisioning, supported devices/OS versions, required model-evaluation repetitions, and performance/cost thresholds.

## 5. Usability pilot

Functional acceptance asks whether the feature works. The pilot asks whether users understand the resulting views and find them useful enough to reuse.

| Measure | Collection method |
| --- | --- |
| Independent task completion | Observe whether testers can describe a view, choose and save a design, and reopen it without assistance. Record where help was needed. |
| Interpretation | Ask testers to explain what the displayed values, period, and components mean. Compare answers with the test scenario's expected meaning. |
| Ease of use | Ask for a 1–5 rating after the task and a short explanation of the main difficulty. |
| Usefulness and reuse | Ask whether the saved view answers their original question and when they would return to it. Treat stated intent as feedback, not evidence of long-term use. |

**Pilot details — Not yet discussed:** Representative participants, participant count, final tasks, target thresholds, and whether sharing/refinement receive separate usability tasks. No usability outcomes have been measured for this plan.

## 6. Completion evidence and handoff

For each validation case, record the build/revision, environment, scenario and test data, expected result, actual result, and pass/fail/blocked status. Keep reproducible automated results and device walkthrough evidence with the implementation review. Record unresolved limitations separately from passed checks.

Completion requires evidence for the entire agreed MVP flow, including financial correctness, persistent reuse, selected-section refinement, Undo, organization, removal, and independent sharing. A successful generation demo alone does not establish completion.

**Release and support — Not yet discussed:** Acceptance/release owners, rollout audience, feature flag, monitoring, rollback procedure, and post-handoff maintenance. This plan does not commit the team to a production launch date or imply that the partner has approved those decisions.

[generator-tests]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sf1/api/widgetgen/generator_test.go
[widget-tests]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/smoke-tests/hermetic/definitions/createWidget-rpc.hermetic.ts
[repo-instructions]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/AGENTS.md
[ios-harness]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/agent-harness/README-ios.md
[android-harness]: https://github.com/cheapreats/ce/blob/604122cc8bc3bd13a16c34fe8c43192f33f82fa5/sba-mobile/agent-harness/README.md
