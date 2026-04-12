# ECS 132 — Probability and Statistical Modeling for CS
## Lec - 1 - Part 1/2

> **How to use this doc:** Read through once as a review. Every definition, formula and example problem is here. When something feels fuzzy, that's your cue to go back and drill it.

---

## What This Course Is

Probability is a **tool for reasoning under uncertainty.** Not just math for math's sake — it shows up everywhere in CS.

**First half of the quarter:**
- Probability foundations
- Naive Bayes → observe features and classify objects (intro ML)

**Second half:**
- Use probability to draw conclusions from sample data
- Hypothesis testing (e.g. cancer rates in smokers vs non-smokers)
- Hidden Markov Models

---

## 1. Deterministic vs Non-Deterministic Environments

### Deterministic Environment
The outcome is **predictable given the input.** No uncertainty.

| Example | Why it's deterministic |
|---|---|
| What time does the sun set? | Calculable from physics |
| What is 2 + 3? | Always 5 |
| f(x) = x² | Same input → same output, always |
| Linear search — is the element there? | Defined, computable answer |

### Non-Deterministic Environment (= Experiments)
The outcome is **uncertain.** You cannot predict it exactly before observing it.

| Example | Why it's non-deterministic |
|---|---|
| Speed of a random car outside | Unknown before measuring |
| Rolling a die | Could be 1 through 6 |
| Next email you receive | No way to know in advance |
| Time, speed, weight, height | Continuous measurements with uncertainty |

> **Key insight:** The distinction isn't always about the *type* of thing — it's about what *question* you're asking. "What is the speed of this specific train right now?" is deterministic (measurable). "What is the speed of a *random* train on this track?" is non-deterministic — you're sampling from a population without knowing which outcome you'll get. The word **random** is the trigger.

**This class lives almost entirely in non-deterministic environments.**

---

## 2. Sample Space

### Definition #1 — Sample Space
> **S** is the set of all possible outcomes of an experiment.

### Notation
- Use **{}** (curly braces) for **discrete** sample spaces — listing specific elements
- Use **[]** (square brackets) for **continuous/interval** sample spaces — everything between two endpoints

| Experiment | Sample Space | Notation Type |
|---|---|---|
| Flip a coin | {H, T} | {} discrete |
| Flip 2 coins | {HH, HT, TH, TT} | {} discrete |
| Roll a die | {1, 2, 3, 4, 5, 6} | {} discrete |
| Day you win the lottery | {day1, day2, day3, ...∞} | {} discrete (infinite) |
| Dart on a number line | [0, 1] | [] continuous |

> **{0,1} ≠ [0,1]** — {0,1} means just two elements: zero and one. [0,1] means every real number between 0 and 1 including all decimals. The dart uses [0,1].

### The Dart Paradox — P(exact point) = 0

**Question:** If you shoot a dart at a number line from 0 to 1, what is the probability of landing *exactly* on 0.5?

**Answer: 0.**

Not impossible — but probability zero.

**Why?** In a continuous space, probability is measured by **length (area, volume).** A single point has zero length. So its probability share of the total interval is 0.

Think of it this way: if you slice a 1-inch line into infinitely many points, each point's share of that 1 inch is 0.

> **Real world confirmation:** Even NASA cannot place a measurement at exactly 0.5000000000... Every instrument has finite precision. You might get 0.500000000000001, but never the exact mathematical point. Neil deGrasse Tyson's height example applies — we say 5'10" but the true measurement has infinite decimal places.

> **P = 0 does not mean impossible.** In continuous spaces, P = 0 just means the event has no measurable width. This trips people up later — remember it now.

**Consequence:** For continuous sample spaces, probability only makes sense over **ranges/intervals**, never exact points.
- ❌ P(dart lands exactly on 0.5) = 0
- ✅ P(dart lands between 0.4 and 0.6) = 0.2

---

## 3. Three Categories of Sample Space

### Category 1 — Finite Sample Space
You can **list every outcome and the list ends.**

- Coin toss: {H, T}
- Dice roll: {1, 2, 3, 4, 5, 6}
- How many students get an A in a class of 127: {0, 1, 2, ..., 127}

> The last example looks big but it's bounded by the class size. Can't have 128 students get an A in a 127-person class.

### Category 2 — Countably Infinite
The list **never ends** but you could in theory **number every outcome** (assign it a position: 1st, 2nd, 3rd...).

- Day you win the lottery: {day1, day2, day3, ...}
- Day class gets cancelled: same idea
- Experiment keeps going and you catch the day of occurrence

> The key word is *countable* — you can map outcomes to natural numbers even if you never finish the list.

### Category 3 — Uncountably Infinite (Continuous)
**Exact measurements.** You cannot number these because between any two values there are infinitely more.

- Height, weight, speed
- Exact height of a growing child
- Dart position on [0, 1]

> Between 0.4 and 0.5 there's 0.45. Between 0.4 and 0.45 there's 0.425. It never stops. No numbering system captures this.

### Quick Classification Test
Ask: *Can I list the outcomes and assign each one a number?*
- Yes, and the list ends → **Finite**
- Yes, but the list goes on forever → **Countably infinite**
- No, because continuous measurement → **Uncountably infinite**

---

## 4. Set Theory — Events, Operations, Notation

### Definition — Event
> An **event** is a subset of the sample space S.

**Example:** Roll a 6-sided die. S = {1, 2, 3, 4, 5, 6}

- Event A = odd number of dots → A = {1, 3, 5}
- Event B = dot count ≤ 3 → B = {1, 2, 3}

---

### Complement — Aᶜ or Ā

> **Aᶜ = S − A** — everything in S that is NOT in A.

**Formula:**
```
Aᶜ = S − {A}
```

**Example:**
- S = {1, 2, 3, 4, 5, 6}
- A = {1, 3, 5}
- Aᶜ = {2, 4, 6}

**Football example:**
- S = {Pedri, Messi, Yamal, Cubarsí, Fermín, Alba, Cucurella, Unai, Morata}
- Barca = {Pedri, Messi, Yamal, Cubarsí, Fermín, Alba}
- Barcaᶜ = {Cucurella, Unai, Morata}

> **Critical:** The complement depends entirely on what S is. Always ask — what is my sample space? Complement is always relative to S.

**Key properties:**
- A ∪ Aᶜ = S (together they make the whole sample space — no gaps, no overlap)
- A ∩ Aᶜ = ∅ (always mutually exclusive — can't be in A and not-A simultaneously)

---

### Union — A ∪ B

> **A ∪ B** = all outcomes in A **OR** B (or both).

**Math OR ≠ English OR.** In everyday English, "or" usually means one or the other, not both. In math, OR is **inclusive** — at least one, including both.

**Football example:**
- Barca = {Pedri, Messi, Yamal, Cubarsí, Fermín, Alba}
- Spain = {Fermín, Cucurella, Unai, Morata}
- Barca ∪ Spain = {Pedri, Messi, Yamal, Cubarsí, Fermín, Alba, Cucurella, Unai, Morata}

> Fermín appears in both but is only listed once. He qualified — that's enough.

**Size check:** Union is always ≥ both individual sets. If your union is smaller than one of the sets, something went wrong.

---

### Intersection — A ∩ B

> **A ∩ B** = all outcomes in A **AND** B simultaneously.

**Football example:**
- Barca = {Pedri, Messi, Yamal, Cubarsí, Fermín, Alba}
- Spain = {Fermín, Cucurella, Unai, Morata}
- Barca ∩ Spain = {Fermín}

Only Fermín plays for both. Everyone else only satisfies one condition.

**Size check:** Intersection is always ≤ both individual sets. It shrinks.

---

### Summary — Union vs Intersection

| Operation | Symbol | Logic | Question it answers |
|---|---|---|---|
| Union | ∪ | OR | Who's in at least one of these groups? |
| Intersection | ∩ | AND | Who's in both groups simultaneously? |
| Complement | ᶜ | NOT | Who's outside this group (within S)? |

---

### Mutually Exclusive / Disjoint Events

> Two events are **mutually exclusive** (probability language) or **disjoint** (set theory language) if they **cannot happen at the same time.**

**They are the same concept — just two names from two different fields.**

**Notation:**
```
A ∩ B = ∅
```
The intersection is the **empty set** ∅ — nothing shared.

**Probability consequence:**
```
P(A ∩ B) = 0
```

**Football example:**
- Injured = {Pedri, Morata}
- Playing today = {Yamal, Fermín, Cubarsí}
- Injured ∩ Playing today = ∅

If you're injured, you're not playing. If you're playing, you're not injured.

**Dice example (from lecture):**
- X = number of dots on die
- A = {X = 3} = {3}
- B = {X = 4} = {4}
- A ∩ B = ∅ because no number is simultaneously 3 AND 4
- P(X=3 AND X=4) = 0

> **This is different from the dart paradox.** The dart's P = 0 comes from infinite precision in a continuous space. Mutually exclusive P = 0 comes from logical impossibility — the events simply cannot coexist.

**Two ways to identify mutually exclusive events:**

1. **Set inspection** — look at the two sets. If they share nothing, A ∩ B = ∅.
2. **First principles logic** — ask *"can both happen in one trial of the experiment?"* One die roll cannot produce 2 AND 6. One coin flip cannot be H AND T.

> Method 2 is more powerful — exams often describe scenarios in words without listing sets explicitly.

---

## 5. Inclusion-Exclusion Rule

### Formula
```
|A ∪ B| = |A| + |B| − |A ∩ B|
```

The | | bars = **cardinality** = size of the set (how many elements).

**Why subtract?** If you just add |A| + |B|, outcomes in both A and B get counted twice. Subtracting |A ∩ B| removes the double-count.

### Example — Binary Password Quiz (⚠️ Professor flagged this for quiz)

**Question:** How many binary passwords of length 6 contain 3 zeros at the start OR 2 ones at the end?

**Step 1 — Define sets:**
- A = passwords with 3 zeros at the start
- B = passwords with 2 ones at the end

**Step 2 — Find |A| using blank method:**
```
0 , 0 , 0 , __ , __ , __
```
First 3 slots locked as 0. Last 3 slots are free, each binary (0 or 1).
|A| = 2³ = 8

**Step 3 — Find |B| using blank method:**
```
__ , __ , __ , __ , 1 , 1
```
Last 2 slots locked as 1. First 4 slots are free, each binary.
|B| = 2⁴ = 16

**Step 4 — Find |A ∩ B|** (passwords satisfying BOTH — starts with 3 zeros AND ends with 2 ones):
```
0 , 0 , 0 , __ , 1 , 1
```
Slots 1,2,3 locked as 0. Slots 5,6 locked as 1. Only slot 4 is free.
|A ∩ B| = 2¹ = 2

**Step 5 — Apply formula:**
```
|A ∪ B| = |A| + |B| − |A ∩ B|
         = 8 + 16 − 2
         = 22
```

> **The subtraction is the trap.** Most people get |A| and |B| right then forget to subtract |A ∩ B| and overcount. Don't fall for it.

---

## 6. Basic Principle of Counting (Multiplication Rule)

### Core Idea
If an experiment has multiple **independent stages**, the total number of outcomes is the stages **multiplied together.**

### Formula
```
|S| = |A| · |B| = m · n       (for 2 stages)

|S| = x₁ · x₂ · x₃ · ... · xₙ   (general form)

|S| = ∏ xᵢ  (i = 1 to n)         (product notation)
```

> **∏** is to multiplication what ∑ is to addition. "Multiply all xᵢ from i=1 to n."

These are **three ways of saying the same thing:**
1. The blank method (draw slots, fill in options, multiply across)
2. The formula m·n or x₁·x₂·...·xₙ
3. Product notation ∏xᵢ

### The Blank Method
Draw one blank per stage. Fill in how many options each stage has. Multiply across.

```
__ , __ , __ , ...
x₁   x₂   x₃
```

**This is the most useful tool for counting problems.** Everything else is shorthand for this.

### Examples

**Two dice:**
```
6 , 6  →  6 × 6 = 36
```

**6-digit phone password (digits 0–9):**
```
10 , 10 , 10 , 10 , 10 , 10  →  10⁶ = 1,000,000
```
(Passwords 000000 through 999999)

**License plate — 3 letters then 3 digits:**
```
26 , 26 , 26 , 10 , 10 , 10  →  26³ × 10³ = 17,576,000
```
Note: letters and digits have different option counts so they stay separate.

**Instagram password — 8 characters, each lowercase letter or digit:**
- Lowercase letters: 26 options
- Digits: 10 options
- Total per slot: 26 + 10 = **36**
```
36 , 36 , 36 , 36 , 36 , 36 , 36 , 36  →  36⁸
```

> **When every slot has the same number of options:** total = (options per slot)^(number of slots). This is where the exponential pattern comes from — 2^n for binary, 10^n for decimal digits, etc.

### When to Use the Multiplication Rule
Anytime someone asks "how many possible combinations" and the answer builds stage by stage:
- Passwords
- License plates
- IP addresses
- Binary strings
- Card dealing
- Database primary keys

---

## 7. With Replacement vs Without Replacement

### The Core Question
> After I pick something — does it go back into the pool or not?

| | With Replacement | Without Replacement |
|---|---|---|
| After picking | Item goes **back** in pool | Item is **removed** from pool |
| Pool size | Stays the same | Shrinks by 1 each pick |
| Slots | **Independent** — previous picks don't affect future options | **Dependent** — previous picks reduce future options |
| Same item twice? | ✅ Yes, possible | ❌ No, impossible |

### Examples

**With replacement → Independent:**
- License plate letters — 'A' in slot 1 doesn't remove 'A' from slot 2. AAA is valid.
- Rolling a die twice — rolling a 6 doesn't affect the next roll. All 6 faces still available.
- Spotify shuffle (basic) — same song could play again.

**Without replacement → Dependent:**
- Sandwich line (Pedri, Fermín, Cancelo) — Fermín in slot 1 means Fermín cannot be in slot 2.
- Dealing cards — Ace of Spades dealt means it's gone. 51 cards left.
- Picking a starting XI — Yamal on the pitch can't fill another position. Pool shrinks with each pick.

> **Rule of thumb for this stage of the course:**
> - With replacement → independent → pool size stays same
> - Without replacement → dependent → pool size shrinks

---

## 8. Permutations

### Core Idea
You have **n items.** You want to select **r** of them. **Order matters. No replacement.**

### Why Order Matters
Pedri as President and Fermín as VP is a **different arrangement** from Fermín as President and Pedri as VP. Same two people, completely different outcome. The *position* each person lands in changes the result.

Compare: picking a group of 2 students to attend a conference — Pedri and Fermín going is the **same** as Fermín and Pedri going. The group is identical. That's where order *doesn't* matter (combinations — coming in Part 2).

### Building the Formula from Scratch

**Example:** 25-player squad, pick a starting XI for 11 positions (GK, LB, CB, RB, CDM, CDM, CAM, LW, RW, ST, ST).

**Blank method:**
```
GK  , LB  , CB  , RB  , CDM , CDM , CAM , LW  , RW  , ST  , ST
25  , 24  , 23  , 22  , 21  , 20  , 19  , 18  , 17  , 16  , 15
```
Pool shrinks by 1 each time because it's **without replacement** — a player on the pitch can't fill another position.

Total = 25 × 24 × 23 × 22 × 21 × 20 × 19 × 18 × 17 × 16 × 15

**The formula is just shorthand for this:**
```
25! = 25 × 24 × 23 × ... × 15 × 14 × 13 × ... × 1
```
You want everything from 25 down to 15 — but not below. So divide out the part you don't want:
```
25! / 14! = 25 × 24 × 23 × ... × 15
```
The 14! cancels everything from 14 downward, leaving exactly 11 numbers — one per position.

### The Formula

```
nPr = n! / (n − r)!
```

- **n** = total items to choose from
- **r** = number of positions to fill
- **(n − r)!** cancels the bottom chunk, leaving exactly r numbers in the product

**Soccer example:**
- n = 25 (squad size), r = 11 (starting positions)
- 25P11 = 25! / (25 − 11)! = 25! / 14!

**President/VP/Treasurer from 127 students:**
- n = 127, r = 3
- 127P3 = 127! / (127 − 3)! = 127! / 124! = 127 × 126 × 125

**How many ways can 127 people leave a room one by one?**
- Everyone leaves, r = n = 127
- 127P127 = 127! / 0! = 127! / 1 = 127!

### Identifying When to Use Permutations
All three must be true:
1. Selecting from a group ✅
2. Order matters ✅ (different positions = different outcomes)
3. No replacement ✅ (same item can't be selected twice)

---

## Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| Aᶜ = S − A | Complement — everything in S not in A |
| A ∪ B | Union — outcomes in A OR B (at least one) |
| A ∩ B | Intersection — outcomes in A AND B (both) |
| A ∩ B = ∅ | Mutually exclusive / disjoint — no shared outcomes |
| \|A ∪ B\| = \|A\| + \|B\| − \|A ∩ B\| | Inclusion-exclusion — union size without double counting |
| \|S\| = x₁ · x₂ · x₃ · ... · xₙ = ∏xᵢ | Multiplication counting principle |
| nPr = n! / (n−r)! | Permutations — ordered selection, no replacement |

---

## Notation Glossary

| Symbol | Meaning |
|---|---|
| S | Sample space — all possible outcomes |
| {} | Discrete set — specific listed elements |
| [] | Continuous interval — everything between endpoints |
| ∅ | Empty set — nothing inside |
| Aᶜ or Ā | Complement of A |
| A ∪ B | Union of A and B |
| A ∩ B | Intersection of A and B |
| \|A\| | Cardinality — number of elements in set A |
| ∏ | Product notation — multiply all terms |
| ∑ | Summation notation — add all terms |
| n! | n factorial = n × (n−1) × (n−2) × ... × 1 |
| nPr | Permutation — n choose r, order matters |
| P(A) | Probability of event A |

---

*Part 2/2 continues with: Combinations, nCr formula, HW1 conference room problem, and the complement trick.*
