# Game Logic & Puzzle Guide for `f3a9c7b2.json`

## Overview

This is an **8×8 grid transformation puzzle** where your goal is to learn and apply a simple color-shifting rule. Each training example shows you an input grid and its corresponding output grid. Your task is to identify the transformation pattern and apply it to unseen test inputs.

The puzzle teaches:
- **Pattern Recognition** - Spotting which cells change and which stay the same
- **Rule Inference** - Figuring out the exact transformation logic
- **Generalization** - Applying the learned rule to new grids

---

## The Rule: +2 Color Shift

### Core Logic

**If a cell in the input grid contains a non-zero number, add 2 to that number. If a cell contains 0 (background), it remains 0.**

#### Complete Color Mapping

| Input Color | Output Color | Rule |
|---|---|---|
| 0 | 0 | Background (unchanged) |
| 1 | 3 | 1 + 2 |
| 2 | 4 | 2 + 2 |
| 3 | 5 | 3 + 2 |
| 4 | 6 | 4 + 2 |
| 5 | 7 | 5 + 2 |
| 6 | 8 | 6 + 2 |
| 7 | 9 | 7 + 2 |

### Why This Rule?

- **Simplicity:** The transformation is deterministic and uniform (same rule applied to every single cell).
- **Preservation:** The grid structure, dimensions, and all zero-cells (background) remain unchanged.
- **Invertibility:** Each input color maps to exactly one output color; you can reverse it with -2.

---

## How to Play the Puzzle

### Step 1: Observe the Training Examples

Open `/training/f3a9c7b2.json` and look at the 4 training examples. Notice:

- **Example 1:** Mixed single-color cells scattered across the grid (colors 2, 3, 4, 5, 6, 7, 1).
- **Example 2:** Larger blocks of repeated colors (rows/columns with the same value).
- **Example 3:** Checkerboard pattern (alternating colors like 2, 3, 2, 3, ...).
- **Example 4:** Stripes and structured layouts (horizontal or vertical bands of single colors).

For each example, compare the **input** grid side-by-side with the **output** grid.

### Step 2: Identify What Changed

Pick any non-zero cell from an input and locate it in the output:

- If `input[row][col] = 2`, what is `output[row][col]`?
- If `input[row][col] = 5`, what is `output[row][col]`?
- If `input[row][col] = 0`, what is `output[row][col]`?

Document the differences. Is there a pattern?

### Step 3: Form a Hypothesis

After comparing 5-10 cells, you should start to see it: _"Every non-zero cell gets increased by 2. Zeros stay zero."_

### Step 4: Verify Across All Examples

Test your hypothesis on all 4 training examples:

- Pick 20-30 random cells from different examples.
- Check if each follows the rule: `output[i][j] = input[i][j] + 2` (or stays 0).
- Count successes and failures.
- If 100% match, you've solved it! If not, re-examine your rule.

### Step 5: Apply to Test Input

Once confident, take the test input and apply the +2 rule to **every single cell**:

```
For each cell (i, j) in the test input:
  if input[i][j] == 0:
    output[i][j] = 0
  else:
    output[i][j] = input[i][j] + 2
```

Verify your test output has:
- The same 8×8 dimensions.
- All zeros stay zero.
- All other numbers increased by exactly 2.

---

## Detailed Examples

### Example 1: Simple Scattered Colors (Rows 0-2)

**Input:**
```
[0, 2, 0, 3, 0, 4, 0, 0]
[2, 2, 2, 0, 3, 0, 4, 0]
[0, 0, 0, 0, 0, 0, 0, 0]
```

**Expected Output:**
```
[0, 4, 0, 5, 0, 6, 0, 0]
[4, 4, 4, 0, 5, 0, 6, 0]
[0, 0, 0, 0, 0, 0, 0, 0]
```

**Verification (cell-by-cell):**

| Position | Input | Output | Formula | Status |
|---|---|---|---|---|
| (0, 0) | 0 | 0 | 0 → 0 | ✓ |
| (0, 1) | 2 | 4 | 2 + 2 = 4 | ✓ |
| (0, 2) | 0 | 0 | 0 → 0 | ✓ |
| (0, 3) | 3 | 5 | 3 + 2 = 5 | ✓ |
| (0, 4) | 0 | 0 | 0 → 0 | ✓ |
| (0, 5) | 4 | 6 | 4 + 2 = 6 | ✓ |
| (1, 0) | 2 | 4 | 2 + 2 = 4 | ✓ |
| (1, 1) | 2 | 4 | 2 + 2 = 4 | ✓ |
| (1, 2) | 2 | 4 | 2 + 2 = 4 | ✓ |
| (1, 3) | 0 | 0 | 0 → 0 | ✓ |
| (1, 4) | 3 | 5 | 3 + 2 = 5 | ✓ |
| (2, 0-7) | All 0s | All 0s | 0 → 0 | ✓ |

**Key Insight:** Every non-zero cell is +2 higher. Every zero stays zero. Structure is preserved perfectly.

---

### Example 2: Blocks of Repeated Colors (Rows 0-1)

**Input:**
```
[3, 3, 0, 0, 4, 4, 0, 0]
[3, 0, 3, 0, 4, 0, 4, 0]
```

**Expected Output:**
```
[5, 5, 0, 0, 6, 6, 0, 0]
[5, 0, 5, 0, 6, 0, 6, 0]
```

**Observation:**
- All 3s → 5s
- All 4s → 6s
- All 0s → 0s
- Block structure is preserved; only values change.

---

### Example 3: Checkerboard Pattern (Rows 0-3)

**Input:**
```
[2, 3, 2, 3, 2, 3, 2, 3]
[3, 2, 3, 2, 3, 2, 3, 2]
[2, 3, 2, 3, 2, 3, 2, 3]
[3, 2, 3, 2, 3, 2, 3, 2]
```

**Expected Output:**
```
[4, 5, 4, 5, 4, 5, 4, 5]
[5, 4, 5, 4, 5, 4, 5, 4]
[4, 5, 4, 5, 4, 5, 4, 5]
[5, 4, 5, 4, 5, 4, 5, 4]
```

**Key Observation:**
- The **alternating pattern** is perfectly preserved.
- Only the numeric values shift (+2).
- The visual "checkerboard" looks the same, just with different numbers.

---

### Example 4: Horizontal Stripes (Rows 0-2)

**Input:**
```
[1, 1, 1, 1, 1, 1, 1, 1]  <- All 1s (red stripe)
[0, 0, 0, 0, 0, 0, 0, 0]  <- All 0s (background stripe)
[5, 5, 5, 5, 5, 5, 5, 5]  <- All 5s (purple stripe)
```

**Expected Output:**
```
[3, 3, 3, 3, 3, 3, 3, 3]  <- All 3s (1 + 2)
[0, 0, 0, 0, 0, 0, 0, 0]  <- All 0s (unchanged)
[7, 7, 7, 7, 7, 7, 7, 7]  <- All 7s (5 + 2)
```

**Pattern:** Full rows/stripes maintain their structure; only color values increment.

---

## Edge Cases & Technical Notes

### Zero (Background) Handling

- **Zero is sacred:** Cells with value 0 are **never modified**.
- **Reason:** Zero typically represents empty space or background in ARC puzzles.
- **Impact:** The empty space pattern is completely preserved.

### Value Range

| Aspect | Detail |
|---|---|
| **Input Colors** | 1, 2, 3, 4, 5, 6, 7 |
| **Output Colors** | 3, 4, 5, 6, 7, 8, 9 |
| **Max Value** | 9 (never wraps or exceeds) |

### Grid Dimensions

- All grids are exactly **8 rows × 8 columns**.
- Input and output grids have the same dimensions.
- No reshaping, cropping, or padding.

### Uniqueness & Invertibility

- Each input color maps to **exactly one** output color (injective function).
- No two input colors produce the same output color.
- **Inverse Rule:** Subtract 2 from any output non-zero cell to recover the original input.

---

## Summary Table

| Property | Value |
|---|---|
| **Puzzle Name** | Color-Shift / +2 Transformation |
| **Grid Dimensions** | 8×8 (fixed) |
| **Transformation** | Add 2 to all non-zero cells; preserve 0 |
| **Input Colors** | 1, 2, 3, 4, 5, 6, 7 |
| **Output Colors** | 3, 4, 5, 6, 7, 8, 9 |
| **Training Examples** | 4 |
| **Test Examples** | 1 (unrelated to training) |
| **Difficulty Level** | **Easy** (deterministic, no exceptions) |
| **Learning Curve** | 5-10 min to identify rule; 1-2 min to verify |
| **Key Skill Tested** | Pattern recognition + rule generalization |

---

## Next Steps

### Try It Yourself

1. **Manual Challenge:** Take the test input from `f3a9c7b2.json` and manually apply the +2 rule. Write down the expected output.
2. **Verify:** Check if they match 100%.

### Explore Variations

- What if the rule was **+3** instead of +2?
- What if it was **×2** (multiply) instead?
- What if only even-valued cells were incremented?
- How would the output grids look different?

### Build on This Knowledge

- Explore other ARC puzzles to see more complex transformation rules.
- Try to infer rules from human-created examples (not shown solutions).
- Implement a general "rule inferencer" that detects patterns automatically.

---

## Quick Reference Card

**Rule:** `output[i][j] = (input[i][j] + 2) if input[i][j] != 0 else 0`

**Verification Checklist:**
- [ ] All input zeros → output zeros?
- [ ] All input 1s → output 3s?
- [ ] All input 2s → output 4s?
- [ ] All input 7s → output 9s?
- [ ] Grid dimensions preserved?
- [ ] No values exceed 9 or drop below 0?

If all boxes are checked, you've cracked it!
