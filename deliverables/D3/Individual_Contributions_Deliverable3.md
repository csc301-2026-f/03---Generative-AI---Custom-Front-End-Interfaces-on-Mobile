# YOUR PRODUCT/TEAM NAME - Individual Contributions (D3)

> For each team member: name, role, and specific contributions. Be precise about what you did, not how much you wrote.

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

**[Name]**, [Role]
> [Specific contributions]

**[Name]**, [Role]
> [Specific contributions]

**[Name]**, [Role]
> [Specific contributions]
