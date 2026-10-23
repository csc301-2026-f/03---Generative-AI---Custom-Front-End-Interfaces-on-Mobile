# Team 12 — Savi Finance

## Iteration 02 - Review & Retrospect

- When: To be confirmed by the team.
- Where: To be confirmed by the team.

## Process - Reflection

#### Q1. What worked well

**Yucheng (Ethan) — US-03**

- **Agreeing on a shared widget structure:** After I proposed the widget architecture, my teammates were willing to discuss it and adjust their implementations around a common definition. This helped connect features that had initially been developed separately. Agreeing on the structures and interfaces also let everyone retain flexibility in how they implemented their own functionality.
- **Reusing the existing application:** Building on Savi's components and infrastructure reduced unnecessary changes and helped the new library and saved-widget functionality remain compatible with the existing system. Examining working code also gave me useful examples while learning the stack.
- **Regular, supportive communication:** We generally held weekly online meetings lasting 30–40 minutes to share progress and discuss questions. I used these meetings to explain the architecture and coordinate how other features would use it. Responsibilities felt clear, and I did not experience major delays caused by teammates or communication problems.

#### Q2. What did not work well

**Yucheng (Ethan) — US-03**

- **We started without a shared definition of a widget:** Some features could begin independently, but the common representation became important as they needed to work together. My biggest challenge was deciding what belonged in the widget data type, component tree, and query plan, and how to separate the saved design from financial data. I explored alternatives, examined the existing code, and discussed the proposed structure with the team. Agreeing on these boundaries earlier would have made the initial development more straightforward.
- **Learning the stack alongside implementation:** Before this iteration, I had not formally learned TypeScript, JavaScript, or Go. I often needed to understand unfamiliar language features and existing code before making changes. This added effort, particularly when tracing how frontend state, backend responses, and rendering connected. Working through the existing implementation gradually helped me understand those relationships.


#### Q3(a). Planned changes

**Yucheng (Ethan) — US-03**

For the next iteration, I would like to:

- Agree on shared data structures and interfaces before dependent features develop too far, and document a small example that everyone can use. Keep implementation choices flexible within those boundaries.
- Continue checking the overall user experience with Ralph while making technical decisions through investigation and team discussion. His feedback helped clarify how the feature should fit into the product.
- Take on work beyond US-03 to understand more of the application and contribute more flexibly to the team.

#### Q3(b). Integration & Next steps

**Yucheng (Ethan) — US-03**

I completed my US-03 implementation and technical documentation. For the demo, a teammate combined the relevant branches into an integration branch because some pull requests were still awaiting approval or merge. My proposed next step is to verify a continuous generation, saving, reopening, and refresh journey as the work is merged.

## Product - Review

#### Q4. How was your product demo?

**Yucheng (Ethan) — US-03**

The team demonstrated the combined branch in a simulator, including my saved-widget functionality alongside teammates' features. We showed the different parts working together. My impression was that Ralph was satisfied about our progress.

Across the iteration, I communicated with Ralph approximately four or five times through Slack and online discussions. His feedback focused on bringing widget types together and making management consistent with the existing app. Specific requests included using Customize Widgets as the entry point, reusing the search and filter row, removing the manual refresh button, using eye icons, simplifying menus, and adding Savi or creator badges. I revised existing code in response while retaining automatic refresh and error recovery. These discussions taught me to consider how a feature fits the product, alongside its technical correctness.

**Supporting work:** [US-03 implementation and review — PR #6586](https://github.com/cheapreats/ce/pull/6586).
