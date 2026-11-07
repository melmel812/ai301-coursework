# Plan: Fix TextArea left-padding misalignment in new-item form (kraft #1309)

## Diagnosis

The Description box sits one column to the right because Textual's `TextArea.DEFAULT_CSS`
sets `padding: 0 1`, and `compact=True` only removes the border — not the padding. By
contrast, `Input`'s compact CSS variant explicitly sets `padding: 0`, clearing all padding.

The kraft stylesheet at `src/kraft/console/kraft.tcss` line 508 has:

```tcss
.new-field > Input, .new-field > TextArea {
    width: 1fr;
}
```

This rule sets width for both widgets but provides no `padding` override for `TextArea`, so
the default `padding: 0 1` survives unchanged. The repro pilot confirms it:
`TextArea offset.x=1` vs `Input offset.x=0`.

The Python code in `src/kraft/console/screens/new_item.py` line 75 is correct;
`compact=True` is the right kwarg. The defect is that the calling stylesheet context never
overrides TextArea's padding — not that compact mode itself is wrong.

## Scope

In scope: add `padding: 0` for `.new-field > TextArea` in `kraft.tcss`; add one alignment
assertion to the existing `test_the_form_fits_80x24` test in `test_new_item.py`.

Not in scope: any change to `new_item.py` (instantiation is correct), changes to Textual
internals, changes to any other form field or selector, any behavior change beyond the
left-edge alignment of the Description box.

## Files

- `src/kraft/console/kraft.tcss` — add `.new-field > TextArea { padding: 0; }` after
  the existing combined rule at line 508.
- `tests/console/test_new_item.py` — add `assert form.query_one(TextArea).content_offset.x == 0`
  to the existing `test_the_form_fits_80x24` test, which already queries form widgets via
  headless pilot.

## Approach

After the existing combined rule at line 508–510:

```tcss
.new-field > Input, .new-field > TextArea {
    width: 1fr;
}
```

add one rule:

```tcss
.new-field > TextArea {
    padding: 0;
}
```

This overrides `TextArea`'s default `padding: 0 1` in the new-item form context, matching
the behavior of compact `Input` widgets. The combined width rule is unchanged.

In `test_the_form_fits_80x24`, the scenario already queries `form.query_one(TextArea)` to
check its `.size.height`. Adding:

```python
form.query_one(TextArea).content_offset.x == 0
```

to the returned tuple (and the matching assertion) ties the test directly to the reproduced
failure: `offset.x` must be 0 after the fix.

## Test plan

**Before (from repro evidence):**

```
Input  offset.x=0  padding=Spacing(top=0, right=0, bottom=0, left=0)
TextArea offset.x=1  padding=Spacing(top=0, right=1, bottom=0, left=1)
```

**After fix:** re-run the repro pilot against the patched code; expect:

```
TextArea offset.x=0  padding=Spacing(top=0, right=0, bottom=0, left=0)
```

Then run the targeted test suite to confirm all 22 existing tests still pass and the new
alignment assertion passes:

```bash
just test tests/console/test_new_item.py -n auto
```

A `TextArea offset.x=0` result from the pilot and a green `just test` run confirm the fix.
Any non-zero `offset.x` for TextArea means the padding override did not apply.

## Risks and unknowns

- Textual may have other selectors that set TextArea padding in a compact context; if the
  specificity of the new rule is too low it may lose to a Textual-internal rule. Risk is
  low: `kraft.tcss` is loaded after Textual's DEFAULT_CSS and `.new-field` is a
  local-to-kraft class with no Textual equivalent, so specificity should win.
- The rule applies to every `.new-field > TextArea` in the app. Currently only one
  TextArea exists in a `.new-field` row (the Description field), so there is no collateral
  effect; if a future field adds another TextArea it would also get zero padding, which is
  the right default for this form.

## Deviations

[None yet — record here as they happen.]
