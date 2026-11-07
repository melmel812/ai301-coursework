Hi — picking this up (I commented to claim it earlier).

The issue body names the fix precisely: `padding: 0` on `.new-field > TextArea` in
`kraft.tcss`. My repro confirms it — the headless pilot shows `TextArea offset.x=1` vs
`Input offset.x=0`, which is exactly the one-column left pad from `TextArea`'s default
`padding: 0 1` surviving through `compact=True`.

My plan:

1. In `src/kraft/console/kraft.tcss` after the `.new-field > Input, .new-field > TextArea`
   width rule, add `.new-field > TextArea { padding: 0; }`.
2. In `tests/console/test_new_item.py`, add an `offset.x == 0` assertion to the existing
   `test_the_form_fits_80x24` test.
3. Run `just test tests/console/test_new_item.py -n auto` and verify the pilot shows
   `TextArea offset.x=0` after the change.

This touches only `kraft.tcss` and `test_new_item.py`; `new_item.py` is correct as-is.
I'll open a PR with `Fixes #1309` when the tests are green.

(Assisted by Claude Code for codebase exploration and plan drafting.)
