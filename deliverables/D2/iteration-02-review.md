# Team 3 — 404 Team Name Not Found

## Iteration 02 - Review & Retrospect

- When: Oct 24th
- Where: Online

## Process - Reflection

#### Q1. What worked well

- Splitting use cases during Deliverable 1 gave each member clear feature ownership and time to design and implement their component independently.
- A shared JSON widget format helped connect independently developed components. Stacked PRs, automated checks, and screenshots supported code review and integration.
- Collaborative debugging and testing of the mobile UI and backend widget flow before the demo helped stabilize the main workflow.

#### Q2. What did not work well

- Shared contracts and architecture were not fully agreed on before implementation, and changes were not always communicated early enough. This created extra coordination work between connected use cases.
- Integration happened later than expected, and dependent branches complicated the merge process, leaving less time for end-to-end testing before the demo.
- Local environment and emulator setup, differences between local checks and GitHub CI, and a PR description policy failure required additional troubleshooting and follow-up fixes.

#### Q3(a). Planned changes

- Hold a short interface-design meeting before implementation to agree on shared contracts and architecture, and communicate subsequent changes regularly to reduce integration rework.
- Integrate dependent features and run end-to-end tests throughout the iteration, merging dependencies in order so issues surface earlier.
- Keep local development environments ready and validate CI and PR requirements before submission to reduce avoidable delays.
- Conduct earlier UI walkthroughs to identify usability issues and improve feedback after user actions.
- Follow Ralph's suggestion, rework widget removal into an iOS-style edit mode 

#### Q3(b). Integration & Next steps

We divided widget creation, design selection, management, removal, and refinement across teammates, connecting the mobile UI, backend financial data, and widget generation through shared JSON contracts and stacked PRs. Next, we will complete the remaining integration into the partner's main branch and test the combined workflow.

## Product - Review

#### Q4. How was your product demo?

We tested the main AI widget workflow and prepared a short video showing widget creation, refinement, preview comparison, and Undo/Redo, while explaining each member's role. Our partner accepted the basic functionality and said we were on the right track, while requesting improvements to the UI experience. Some final integration into the partner's main branch was still pending. We learned to allow more time for end-to-end integration and to make the user flow and feedback after each action clearer.
