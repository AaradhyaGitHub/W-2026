# ECS 132 — Probability and Statistical Modeling for CS
## Lec - 2 - Part 1

> **How to use this doc:** Read through once as a review. Every definition, formula, example problem, and pitfall from the session is here. When something feels fuzzy, that's your cue to go back and drill it. This doc covers the tail end of Lecture 1 review (counting problems) and the first new concept of Lecture 2 — the Naive Definition of Probability.

---

## Where We Left Off

Lecture 1 ended with Permutations and Combinations. Before moving into new material, the session opened with 4 counting problems as a warm-up drill. These problems use everything from Lec 1 — the blank method, binary counting, combinations, and the complement trick.

Quick recap of the tools in play:

| Tool | When to use it |
|---|---|
| Blank method | Anytime you're filling positions — draw a slot per stage, multiply across |
| nPr = n! / (n−r)! | Ordered selection, no replacement |
| nCr = n! / (n−r)! r! | Unordered group, no replacement |
| Complement | "How many do NOT..." → Total − Unwanted |

---

## Part 1 — Counting Problem Drills

### Problem 1 — Password with 4 Spaces (Binary)

**Question:** A password has 4 spaces. How many total possibilities are there? (binary alphabet — only 0 and 1)

**Solution:**

Each slot has 2 choices independently. 4 slots, 2 options each:

```
__ , __ , __ , __
 2    2    2    2   →   2⁴ = 16
```

**If the alphabet were digits 0–9 instead:**

```
__ , __ , __ , __
10   10   10   10  →  10⁴ = 10,000
```

> **Pattern:** When every slot has the same number of options → (options per slot)^(number of slots). Binary = 2^n, decimal = 10^n.

---

### Problem 2 — Password with 4 Spaces, First Can't Be 0 (Binary)

**Question:** Same 4-space binary password. The first space cannot be 0. How many possibilities?

**Solution:**

First slot is locked to 1 — only 1 choice. Remaining 3 slots are free — 2 choices each.

```
__ , __ , __ , __
 1    2    2    2   →   1 × 2³ = 8
```

> **The trap:** Don't add (1 + 2³). The counting principle says multiply across stages, not add. The 1 in slot 1 is still a multiplied stage — it just contributes a factor of 1.

**If the alphabet were digits 0–9 instead:**

First slot can't be 0 → 9 choices (1–9). Remaining 3 slots → 10 choices each.

```
__ , __ , __ , __
 9   10   10   10  →   9 × 10³ = 9,000
```

---

### Problem 3 — Palindrome Passwords (Binary, Size n)

**Question:** Given a password of size n in a binary alphabet (0 and 1), how many palindrome passwords can be created?

**Definition — Palindrome:**
> A string that reads the same forwards and backwards. Examples: "racecar", "kayak", "0110", "10101".

**Key insight:** In a palindrome, the second half is forced by the first half. So only the first ⌈n/2⌉ positions are free choices — the rest are locked.

**Building the answer with small examples:**

**Size 6 (even):**

```
__ , __ , __ , __ , __ , __
 2    2    2    1    1    1
```

Positions 4, 5, 6 must mirror positions 3, 2, 1. So only 3 free choices → 2³ = 2^(6/2) = 2^(n/2).

**Size 5 (odd):**

```
__ , __ , __ , __ , __
 2    2    2    1    1
```

Positions 4 and 5 mirror positions 2 and 1. The middle position (3) is free — it doesn't need a mirror. So 3 free choices → 2³ = 2^⌈5/2⌉.

> **Why ceiling and not floor?** For size 5: n/2 = 2.5. You actually have 3 free positions (the middle counts). So you round **up**. ⌈2.5⌉ = 3. Confirmed by counting directly.

**General Formula:**

$$2^{\lceil n/2 \rceil}$$

| n | Free positions | Formula | Answer |
|---|---|---|---|
| 4 | 2 | 2^⌈4/2⌉ = 2² | 4 |
| 5 | 3 | 2^⌈5/2⌉ = 2³ | 8 |
| 6 | 3 | 2^⌈6/2⌉ = 2³ | 8 |
| 7 | 4 | 2^⌈7/2⌉ = 2⁴ | 16 |

### Notation

| Symbol | Meaning |
|---|---|
| ⌈x⌉ | Ceiling function — round x **up** to the nearest integer |
| ⌊x⌋ | Floor function — round x **down** to the nearest integer |

> ⌈2.5⌉ = 3, ⌊2.5⌋ = 2. We use ceiling here because the middle character of an odd-length palindrome is a free choice, pushing the free count up.

---

### Problem 4 — Camp Counselor Problem

**Question:** You are a camp counselor with 70 students and 3 bunks. How many bunk assignments do NOT place Joe, Jordan, and Chris in the same bunk?

```
Bunk A → fits 40
Bunk B → fits 20
Bunk C → fits 10
```

**Strategy — Complement:**

Counting "not together" directly is messy. Use the complement:

```
Answer = Total − (assignments where all 3 are together)
```

---

**Step 0 — Find Total (no restrictions):**

Treat all 70 students equally. Fill bunks sequentially.

```
70C40  ×  30C20  ×  10C10
```

Let this be **T**.

---

**Step 1 — Find assignments where Joe, Jordan, Chris ARE in the same bunk:**

Three mutually exclusive cases — they're either all in A, all in B, or all in C.

**Case A — All 3 in Bunk A:**

Lock the 3 into A. Pool becomes 70 − 3 = 67. Bunk A has 40 − 3 = 37 seats left.

```
67C37  ×  30C20  ×  10C10
```

**Case B — All 3 in Bunk B:**

Lock the 3 into B. Pool becomes 67. Bunk B has 20 − 3 = 17 seats left.

```
67C17  ×  50C40  ×  10C10
```

**Case C — All 3 in Bunk C:**

Lock the 3 into C. Pool becomes 67. Bunk C has 10 − 3 = 7 seats left.

```
67C7  ×  60C40  ×  20C20
```

Let **G** = Case A + Case B + Case C (add because mutually exclusive).

---

**Final Answer:**

$$T - G = \binom{70}{40}\binom{30}{20}\binom{10}{10} - \left[\binom{67}{37}\binom{30}{20}\binom{10}{10} + \binom{67}{17}\binom{50}{40}\binom{10}{10} + \binom{67}{7}\binom{60}{40}\binom{20}{20}\right]$$

---

**Key mechanics from this problem:**

| Concept | Explanation |
|---|---|
| Locking 3 people into a bunk | Removes them from the pool (70→67) AND fills their seats (e.g. 40→37) simultaneously |
| Multiply within a case | All room assignments happen together — one event, multiply stages |
| Add across cases | Each case (A, B, C) is mutually exclusive — add them |
| Complement | "NOT together" = Total − "Together" |
| nCn = 1 | Only one way to fill a room with exactly the people left |

---

## Part 2 — Naive Definition of Probability

### Why "Naive"?

This is the first concrete, mathematical definition of probability. It's called *naive* because it only works under clean, idealized conditions. A more general definition (the axiomatic definition) will come later in the course — but the naive definition handles a huge class of problems perfectly and is the foundation everything else builds on.

---

### Definition #2 — Naive Probability

> Let **S** be the sample space of an experiment and let **E** be an event. Then the probability of E, written P(E), is:

$$P(E) = \frac{|E|}{|S|} = \frac{\text{number of outcomes in } E}{\text{number of outcomes in } S}$$

**In plain English:** Out of all the things that could happen, what fraction of them are the thing you care about?

---

### Requirements for This Definition

All three must hold or the naive definition gives wrong answers:

1. **S must be finite** — you can't divide by infinity
2. **All outcomes are equally likely** — if some outcomes are more likely than others, a simple count doesn't reflect true probability
3. **Outcomes are mutually exclusive** — each outcome is a distinct, non-overlapping possibility

---

### Notation

| Symbol | Meaning |
|---|---|
| S | Sample space — all possible outcomes |
| E | Event — the subset of outcomes you care about |
| \|S\| | Cardinality of S — number of outcomes in the sample space |
| \|E\| | Cardinality of E — number of outcomes in the event |
| P(E) | Probability of event E |

> **Cardinality** just means the size/count of a set. |{1, 3, 5}| = 3. It's the formal word for "how many things are in here."

---

### When the Naive Definition Fails — The Coin Flip Trap

The naive definition breaks when you define the sample space lazily, in a way that makes outcomes unequal.

**The trap — flipping 2 coins and defining S as {0 heads, 1 head, 2 heads}:**

Someone might say "3 outcomes, so P(1 head) = 1/3." But this is **wrong** because the outcomes are not equally likely:

| Outcome | Actual ways to get it | True probability |
|---|---|---|
| 0 heads (TT) | 1 | 1/4 |
| 1 head (HT or TH) | 2 | **2/4** |
| 2 heads (HH) | 1 | 1/4 |

The equally likely requirement is violated — "1 head" is twice as likely as the others. The naive definition gives you the wrong answer here.

**The fix:** Define S properly as {HH, HT, TH, TT} — all 4 outcomes are equally likely. Now the naive definition works fine.

> **Lesson:** The naive definition doesn't just depend on the experiment — it depends on **how you define your sample space.** Define it carelessly and the equally likely requirement collapses.

---

### Example — Roll 2 Dice, P(sum < 5)

**Setup:**

```
Roll 2 fair dice. What is the probability the sum is less than 5?
```

**|S|:** Each die has 6 faces, 2 dice total → 6² = **36 possible outcomes**

**|E|:** List every pair (die1, die2) where die1 + die2 < 5:

```
{1,1} → sum = 2  ✓
{1,2} → sum = 3  ✓
{1,3} → sum = 4  ✓
{2,1} → sum = 3  ✓
{2,2} → sum = 4  ✓
{3,1} → sum = 4  ✓
```

|E| = **6**

**Answer:**

$$P(\text{sum} < 5) = \frac{|E|}{|S|} = \frac{6}{36} = \frac{1}{6}$$

> **Important:** |E| = 6 is NOT all the possible dice rolls. It is only the ones that satisfy the constraint. |S| = 36 is all the possible rolls.

---

### Practice Problems — Naive Probability

**Problem 1:** Roll a single fair die. What is the probability of rolling an even number?

```
S = {1, 2, 3, 4, 5, 6}  →  |S| = 6
E = {2, 4, 6}           →  |E| = 3

P(even) = 3/6 = 1/2
```

**Problem 2:** Flip 3 fair coins. What is the probability of getting exactly 2 heads?

Listing the full sample space systematically (treat it like binary — all tails to all heads):

```
TTT, TTH, THT, THH, HTT, HTH, HHT, HHH  →  |S| = 2³ = 8
```

Outcomes with exactly 2 heads: {THH, HTH, HHT} → |E| = 3

$$P(\text{exactly 2 heads}) = \frac{3}{8}$$

> **Listing tip:** For n coin flips, |S| = 2^n. Systematically list by fixing the leftmost position and varying the rest — same logic as binary counting.

---

## Part 3 — Simulation and the Law of Large Numbers

### The Alien Analogy

> If you were an alien who had never seen dice and had no concept of probability theory — but you could code — how would you figure out P(sum of 2 dice < 5)?

You'd just run the experiment thousands of times and track how often the event happens. The ratio converges to the true probability.

This is **simulation-based probability** — and it's a direct empirical implementation of P(E) = |E|/|S|.

---

### The Pseudocode (from lecture notes)

```
occurrence = 0
for (i = 1 to 10,000 (nreps)) {
    dice1 = random integer between 1 and 6
    dice2 = random integer between 1 and 6

    if (dice1 + dice2 < 5) {
        occurrence++
    }
}
cout << estimate (count / reps)
```

**Mapping to the formula:**

| Code variable | Formula equivalent | Meaning |
|---|---|---|
| `reps` | approximates \|S\| | total number of trials run |
| `occurrence` (count) | approximates \|E\| | number of times the event happened |
| `count / reps` | P(E) = \|E\|/\|S\| | estimated probability |

---

### C++ Implementation

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>

void estimateProbability(int reps) {
    int count = 0;
    std::srand(std::time(0));

    for (int i = 1; i <= reps; i++) {
        int dice1 = std::rand() % 6 + 1;
        int dice2 = std::rand() % 6 + 1;

        if (dice1 + dice2 < 5) {
            count++;
        }
    }

    double estimate = static_cast<double>(count) / reps;
    std::cout << "Estimate: " << estimate << std::endl;
}

int main() {
    estimateProbability(100000000);
    return 0;
}
```

---

### Convergence — Law of Large Numbers in Action

True answer: 1/6 ≈ **0.16666...**

Running the simulation at increasing reps:

| Reps | Estimate |
|---|---|
| 10 | 0.2 |
| 100 | 0.15 |
| 1,000 | 0.183 |
| 10,000 | 0.1672 |
| 100,000 | 0.16754 |
| 1,000,000 | 0.167401 |

As reps → ∞, estimate → 1/6. That's the **Law of Large Numbers** — not just as a theorem, but visible in your own output.

> **Note:** `reps` is not the same as |S| in the traditional sense — |S| for 2 dice is always 36. `reps` is how many times you *sampled* from that space. But as reps grows large, the ratio converges to the true probability.

---

## Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| 2^n | Total binary strings of length n |
| 10^n | Total decimal digit strings of length n |
| 2^⌈n/2⌉ | Binary palindromes of length n |
| nCr = n! / (n−r)! r! | Combinations — unordered group, no replacement |
| Complement: Answer = Total − Unwanted | When counting "NOT X" |
| P(E) = \|E\| / \|S\| | Naive definition of probability |

---

## Notation Glossary

| Symbol | Meaning |
|---|---|
| ⌈x⌉ | Ceiling — round up to nearest integer |
| ⌊x⌋ | Floor — round down to nearest integer |
| \|S\| | Cardinality of sample space — total outcomes |
| \|E\| | Cardinality of event — favourable outcomes |
| P(E) | Probability of event E |
| S | Sample space |
| E | Event (subset of S) |

---

## Common Pitfalls

**Pitfall 1 — Adding instead of multiplying in the blank method**
When the first slot has 1 choice, you still multiply: 1 × 2³ = 8. Not 1 + 2³ = 9.

**Pitfall 2 — Floor vs Ceiling for palindromes**
The middle character of an odd-length palindrome is a free choice — that pushes the count up. Use ceiling ⌈n/2⌉, not floor. Verify with a small example (size 5 → 3 free slots, not 2).

**Pitfall 3 — Forgetting to remove locked people from the pool**
In the camp counselor problem, locking Joe, Jordan, Chris into a bunk removes them from the pool (70→67) AND fills their seats (e.g. 40→37). One action, two effects. Miss either one and the answer breaks.

**Pitfall 4 — Defining a bad sample space and applying the naive definition**
The naive definition requires all outcomes to be equally likely. If you define S = {0 heads, 1 head, 2 heads} for 2 coin flips, "1 head" is twice as likely as the others — the definition fails. Always define S so outcomes are genuinely equally likely.

**Pitfall 5 — Confusing |E| with |S|**
In the dice example, |E| = 6 is NOT all possible rolls. It's only the rolls that satisfy the condition. |S| = 36 is all possible rolls. These are different things.

---

*End of Lec 2 — Part 1/2. Part 2 continues with the remaining Lecture 2 material.*