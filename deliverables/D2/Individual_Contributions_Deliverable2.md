## Ziheng (Jerry) Wang
**Role:** Owned US-07 (Remove a widget) end to end, from the UI interaction to the removal logic.

**Contributions:**
- Implemented widget removal across both the library and the dashboard: removed widgets can no longer be opened, edited or exported, while imported copies are kept and no data is permanently erased ([PR #6602]).
- - Built the removal interaction: a ⋯ menu on the widget's top-right corner, with confirmation, cancellation with no side effects, and undo ([PR #6602]). Currently reworking it into drag-to-remove for a faster flow (in progress, not part of the D2 release).

