# ECS 132 — Probability and Statistical Modeling for CS
## Lec - 2 - Part 2

> **How to use this doc:** Read through once as a review. Every definition, formula, example problem, and pitfall from the session is here. When something feels fuzzy, that's your cue to go back and drill it. This doc picks up exactly where Part 1 left off — right after the Naive Definition of Probability and simulation. Part 2 covers the Frequentist Definition, P(A and B), P(A or B), Conditional Probability, and the key probability rules.

---

## Where We Left Off

Part 1 ended with the Naive Definition of Probability and simulation (Law of Large Numbers). The formula was:

```
P(E) = |E| / |S|
```

But this only works when outcomes are finite and equally likely. Part 2 introduces a more powerful definition and builds the full toolkit for working with multiple events.

---

## Part 1 — Frequentist Definition of Probability

### Why a New Definition?

The naive definition has hard requirements — finite sample space, equally likely outcomes. The real world doesn't always cooperate. You need a definition that works even when outcomes aren't equally likely or the space is infinite.

---

### Definition — Frequentist Probability (The Tally Book)

> Imagine a tally book where you track outcomes of an experiment over many runs. Each row is one run. For event A, you mark Yes or No in that row depending on whether A occurred.

**The probability of A is the long run fraction:**

```
P(A) = (# of Yes rows for A) / (# of lines in the notebook)   as lines → infinity
```

**In plain English:** Run the experiment over and over. Keep a tally. Whatever fraction of runs produce the event — that's the probability. As runs approach infinity, the fraction converges to the true probability.

---

### Why This Is Better Than Naive

| Property | Naive Definition | Frequentist Definition |
|---|---|---|
| Requires finite sample space | Yes | No |
| Requires equally likely outcomes | Yes | No |
| Works for loaded dice, biased coins | No | Yes |
| Can handle infinity | No | Yes |

---

### The Connection to Simulation

This is not a new idea — you already implemented it. The C++ simulation from Part 1:

```cpp
count / reps  →  # of Yes / # of lines in notebook
```

As `reps → infinity`, that ratio converges to the true P(E). The simulation was the frequentist definition running empirically. You were doing it before you had a name for it.

---

### Tally Book Example — 3 Runs

**Experiment:** Roll two dice.
- **A:** x + y < 5
- **B:** first die = 3

| Run | Outcome (d1, d2) | A: sum < 5? | B: d1 = 3? |
|---|---|---|---|
| 1 | (1, 1) | Yes | No |
| 2 | (3, 4) | No | Yes |
| 3 | (3, 1) | Yes | Yes |

After 3 runs:
- P(A) ≈ 2/3
- P(B) ≈ 2/3

> **Note:** 3 runs is tiny — the fractions won't match the true probability yet. With infinite runs they converge. The mechanism is identical regardless of run count.

---

## Part 2 — P(A and B) — Joint Probability

### Definition

> **P(A and B) = P(A∩B):** The probability that both A and B occur on the same run.

In the tally book, add a new column "A and B." That column gets a **Yes only when both A and B are Yes.**

---

### The Truth Table

This is a straight AND gate:

| A | B | A and B |
|---|---|---|
| Yes | No | No |
| No | Yes | No |
| No | No | No |
| Yes | Yes | Yes |

---

### Tally Book with A and B Column

| Run | Outcome (d1, d2) | A: sum < 5? | B: d1 = 3? | A and B? |
|---|---|---|---|---|
| 1 | (1, 1) | Yes | No | No |
| 2 | (3, 4) | No | Yes | No |
| 3 | (3, 1) | Yes | Yes | **Yes** |

P(A and B) ≈ 1/3 — only run 3 satisfied both.

---

## Part 3 — P(A or B) — Union Probability

### Definition

> **P(A or B) = P(A∪B):** The probability that at least one of A or B occurs on a given run.

In the tally book, add a new column "A or B." That column gets a **Yes if either A or B (or both) are Yes.**

---

### The Truth Table

This is a straight OR gate:

| A | B | A or B |
|---|---|---|
| Yes | No | Yes |
| No | Yes | Yes |
| No | No | No |
| Yes | Yes | Yes |

---

### Tally Book with A or B Column

| Run | Outcome (d1, d2) | A: sum < 5? | B: d1 = 3? | A and B? | A or B? |
|---|---|---|---|---|---|
| 1 | (1, 1) | Yes | No | No | Yes |
| 2 | (3, 4) | No | Yes | No | Yes |
| 3 | (3, 1) | Yes | Yes | Yes | Yes |

P(A or B) ≈ 3/3 = 1 — small sample, not representative. With more runs it converges.

---

### The Formula — Why You Can't Just Add

**Intuition says:** P(A or B) = P(A) + P(B). But this double counts.

From the example: P(A) = 2/3, P(B) = 2/3. Adding gives 4/3 — impossible, probability can't exceed 1.

**What went wrong:** Run 3 was counted once for A and once for B. But it's one run. It got painted twice.

**The fix — subtract the overlap:**

```
P(A∪B) = P(A) + P(B) - P(A∩B)
```

From the example: 2/3 + 2/3 - 1/3 = 3/3 = 1. Correct.

---

### The Set Analogy

Think of it like sets:

- A = {1, 2, 3}, B = {3, 4, 5}
- A∪B = {1, 2, 3, 4, 5} — the 3 only appears once

If you just "add" the sets you get {1,2,3,3,4,5} — a duplicate. The formula -A∩B removes that duplicate. The set definition "no duplicates" and the formula are enforcing the same rule — one is the rule, the other is the mechanism.

---

### Special Case — Mutually Exclusive Events

If A and B can never happen at the same time, P(A∩B) = 0. Then:

```
P(A∪B) = P(A) + P(B) - 0 = P(A) + P(B)
```

Simple addition is valid **only** when events are mutually exclusive. Always safe to use the full formula.

---

## Part 4 — Conditional Probability P(A|B)

### Definition

> **P(A|B):** The probability of A given that B has already occurred.

You are no longer looking at the full sample space. B already happened. So you **shrink your world down to only the rows where B is Yes**, and then ask — within that smaller world, how often does A happen?

---

### The N/A Rule in the Tally Book

When you add a column for P(A|B), the rules are:

- **B is No** → write **N/A** — the question doesn't apply. B didn't happen so "A given B" is meaningless. Not No. Just irrelevant.
- **B is Yes and A is Yes** → write **Yes**
- **B is Yes and A is No** → write **No**

> **Why N/A and not No?** Because when B is No, asking "did A happen given B?" is like asking "given it rained today, did you use an umbrella?" when it didn't rain. The question doesn't apply. Those rows are invisible to P(A|B).

---

### Tally Book with B/A Column

**A:** x + y < 5 — **B:** x = 3

| Run | Outcome | A: sum < 5? | B: x = 3? | B/A |
|---|---|---|---|---|
| 1 | (1, 2) | Yes | No | N/A |
| 2 | (3, 4) | No | Yes | No |
| 3 | (3, 1) | Yes | Yes | Yes |
| 4 | (3, 5) | No | Yes | No |
| 5 | (3, 1) | Yes | Yes | Yes |

P(A|B) = 2 Yes out of 3 B-Yes rows = 2/3

---

### The Mental Model

> Imagine a tally book out there somewhere. There are runs for some experiment. B has Yes values in some rows. Now look adjacent to those B=Yes rows — how many of those also have A=Yes? That count over the total B=Yes rows is P(A|B).

**Numerator:** rows where both A and B are Yes
**Denominator:** rows where B is Yes (N/A rows are skipped entirely)

---

### The Formula

```
P(A|B) = P(A∩B) / P(B)
```

Your mental model in formula form — numerator is the "adjacent yes" count, denominator is the total B=Yes count.

---

### The Pseudocode — Simulation of P(B|A)

From lecture notes — computing P(x=3 | x+y < 5):

```
countA = 0
countB|A = 0

for (i = 1 to nreps) {
    d1 = sample(1:6)
    d2 = sample(1:6)

    if (d1 + d2 < 5) {
        countA = countA + 1

        if (d1 == 3) {
            countB|A = countB|A + 1
        }
    }
}

print countB|A / countA
```

**Why the nesting matters:** The `if(d1 == 3)` is inside the `if(d1+d2 < 5)` block. The code only checks B when A has already passed. The N/A rows are never reached — the code physically skips them. The nesting is not style. It is the mathematical definition of conditional probability translated directly into logic.

| Code variable | Meaning |
|---|---|
| `countA` | # of rows where A is Yes — the denominator |
| `countB\|A` | # of rows where both A and B are Yes — the numerator |
| `countB\|A / countA` | P(B\|A) |

---

## Part 5 — Key Probability Rules

### Rule 1 — Union Formula

```
P(A∪B) = P(A) + P(B) - P(A∩B)
```

Special case (mutually exclusive): P(A∪B) = P(A) + P(B)

---

### Rule 2 — Multiplication Rule

P(A∩B) can be calculated two ways:

```
P(A∩B) = P(A|B) · P(B)
P(A∩B) = P(B|A) · P(A)
```

Useful when P(A∩B) is hard to find directly but P(A|B) and P(B) are easy.

---

### Rule 3 — Conditional Probability Formula

```
P(A|B) = P(A∩B) / P(B)
```

---

### TH 1 — Mutually Exclusive Special Case for Conditional Probability

> If A and B are mutually exclusive (disjoint), they can never happen at the same time. So P(A∩B) = 0.

Plugging into the conditional formula:

```
P(A|B) = P(A∩B) / P(B) = 0 / P(B) = 0
```

**Intuition:** If A and B can never happen together, knowing B happened tells you A is impossible. P(A|B) = 0 makes perfect sense.

**Example:** Roll one die. A = even {2,4,6}, B = odd {1,3,5}. They can never both occur on one roll. P(A|B) = 0 — if you know the roll was odd, there's zero chance it was even.

---

## Part 6 — Worked Examples

### Example 1 — 7-Sided Dice (From Lecture)

**Setup:** x = result of first 7-sided die, y = result of second 7-sided die.

**Find:** P(x + y ≤ 5 | x = 2)

**Step 1 — Intuition first:**
x is locked at 2. Need 2 + y ≤ 5 → y ≤ 3. So y can be 1, 2, or 3. That's 3 out of 7. Answer should be 3/7.

**Step 2 — Verify with formula:**

Let A = x + y ≤ 5, B = x = 2.

**Find P(A∩B):**
Lock x = 2. List pairs where 2 + y ≤ 5:
- (2,1) → sum = 3 ✓
- (2,2) → sum = 4 ✓
- (2,3) → sum = 5 ✓
- (2,4) → sum = 6 ✗

A∩B = {(2,1), (2,2), (2,3)} → 3 pairs out of 49 total.
P(A∩B) = 3/49

**Find P(B):**
B = x = 2. All pairs where first die is 2:
{(2,1),(2,2),(2,3),(2,4),(2,5),(2,6),(2,7)} → 7 pairs.
P(B) = 7/49

**Apply formula:**
P(A|B) = P(A∩B) / P(B) = (3/49) / (7/49) = 3/7 ✓

Matches intuition. Formula confirmed.

---

### Example 2 — Two 6-Sided Dice, P(A|B)

**Setup:** Roll two 6-sided dice.
- A: sum is less than 7
- B: first die is 4

**Find P(A|B).**

**Find P(B):**
All pairs where first die is 4:
{(4,1),(4,2),(4,3),(4,4),(4,5),(4,6)} → 6 pairs.
P(B) = 6/36

**Find P(A∩B):**
Lock first die = 4. Need sum < 7 → 4 + y < 7 → y < 3.
- (4,1) → sum = 5 ✓
- (4,2) → sum = 6 ✓
- (4,3) → sum = 7 ✗

P(A∩B) = 2/36

**Apply formula:**
P(A|B) = (2/36) / (6/36) = 2/6 = **1/3**

---

### Example 3 — At Least One Six

**Setup:** Roll two 6-sided dice.
- A: at least one die shows a 6
- B: sum is greater than 9

**Find P(B|A).**

**Find P(A):**

> **Key trap:** "At least one die shows a 6" means the 6 can be on either die. List both cases separately.

First die is 6: {(6,1),(6,2),(6,3),(6,4),(6,5),(6,6)} — 6 pairs
Second die is 6: {(1,6),(2,6),(3,6),(4,6),(5,6)} — 5 pairs (exclude (6,6) already counted)

Total: 11 pairs. P(A) = 11/36

**Find P(A∩B):**
Need at least one 6 AND sum > 9. From the 11 pairs:
- (6,4) → sum = 10 ✓
- (6,5) → sum = 11 ✓
- (6,6) → sum = 12 ✓
- (4,6) → sum = 10 ✓
- (5,6) → sum = 11 ✓

P(A∩B) = 5/36

**Apply formula:**
P(B|A) = (5/36) / (11/36) = **5/11**

> **Why 5/36 and not 5/11?** P(A∩B) is always measured against the full sample space (36). The 11 only becomes your denominator at the final division step. The 36s cancel when you divide — that's why the answer is 5/11.

---

## Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| P(A) = # Yes / # lines as lines → ∞ | Frequentist definition |
| P(A∪B) = P(A) + P(B) - P(A∩B) | Union of two events |
| P(A∪B) = P(A) + P(B) | Union when mutually exclusive only |
| P(A∩B) = P(A\|B) · P(B) | Joint probability via conditional |
| P(A∩B) = P(B\|A) · P(A) | Joint probability via conditional (flipped) |
| P(A\|B) = P(A∩B) / P(B) | Conditional probability |
| P(A\|B) = 0 when A,B mutually exclusive | Special case — disjoint events |

---

## Notation Glossary

| Symbol | Meaning |
|---|---|
| P(A∩B) | Probability of A and B — both occur |
| P(A∪B) | Probability of A or B — at least one occurs |
| P(A\|B) | Probability of A given B has occurred |
| ∩ | Intersection — AND |
| ∪ | Union — OR |
| N/A | Row skipped in conditional — B didn't occur, question doesn't apply |

---

## Common Pitfalls

**Pitfall 1 — Adding probabilities without subtracting the intersection**
P(A) + P(B) double counts any outcome where both A and B occur. Always use P(A∪B) = P(A) + P(B) - P(A∩B) unless you've confirmed mutual exclusivity.

**Pitfall 2 — Treating (6,5) and (5,6) as the same outcome**
Dice rolls are ordered pairs. (6,5) means first die is 6, second is 5. (5,6) means the opposite. These are different outcomes. Always list both when counting "at least one."

**Pitfall 3 — Writing No instead of N/A in the conditional column**
When B is No, P(A|B) is not No — it's not applicable. The row is invisible to the calculation. Writing No would wrongly include it in your denominator.

**Pitfall 4 — Using the wrong denominator for P(A∩B)**
P(A∩B) is always over the full sample space. If two 6-sided dice are rolled, the denominator is always 36. The conditional denominator (e.g. 11) only appears at the final division step when you compute P(B|A) = P(A∩B)/P(A).

**Pitfall 5 — Thinking combinations should be bigger than intersections**
A∩B can only be as large as the smaller of A or B. Intersection shrinks. Union grows. Don't mix them up when building your intuition.

**Pitfall 6 — Confusing P(A|B) with P(B|A)**
These are not the same. P(A|B) shrinks the world to B=Yes rows and asks about A. P(B|A) shrinks to A=Yes rows and asks about B. The order matters. Always read the condition carefully.

---

*End of Lec 2 — Part 2/2. Lecture 2 fully documented. Next: Lecture 3.*