# ECS 132 — Probability and Statistical Modeling for CS
## Lec 1 — Part 2/2

> **How to use this doc:** Read through once as a review. Every definition, formula, example problem, and pitfall from the session is here. When something feels fuzzy, that's your cue to go back and drill it. This doc picks up exactly where Part 1 left off — right at Permutations.

---

## Where We Left Off

Part 1 ended with the Permutation formula introduced but not fully digested. The formula was on the board:

```
nPr = n! / (n - r)!
```

But the *why* behind it wasn't clear yet. That's where Part 2 starts.

---

## 1. Permutations — Full Picture

### Definition

> **Permutation:** Given n items, select r of them. **Order matters. No replacement.**

The three conditions that must all be true for permutation to apply:
1. Selecting from a group ✅
2. Order matters — different positions = different outcomes ✅
3. No replacement — same item can't be selected twice ✅

---

### The Soccer Example — You Already Did One

**Setup:** Starting XI = { Lewandowski, Raphinha, Lamine Yamal, Pedri, De Jong, Fermín, Cancelo, Cubarsí, Martin, Koundé, Joan Garcia }

**Problem:** Assign 3 distinct roles — Captain, Vice Captain, Aggressor — to 3 distinct players.

**Blank method:**

```
Captain     Vice-Captain     Aggressor
  11      ×      10        ×     9      =  990
```

Pool shrinks by 1 each time because it's **without replacement** — a player with a badge can't hold another badge simultaneously.

**This is permutation.** You didn't realise it but you did it correctly just by thinking through the problem.

---

### What Does 990 Actually Mean?

> "In a team of 11 players, if we want to assign 3 distinct roles to 3 distinct players, there are **990 different ways to do it.**"

Think of it as a pile of cards. Every possible assignment is written on one card:

```
Card 1:   Captain=De Jong,  Vice=Pedri,    Aggressor=Fermín
Card 2:   Captain=De Jong,  Vice=Fermín,   Aggressor=Pedri
Card 3:   Captain=Pedri,    Vice=De Jong,  Aggressor=Fermín
...
Card 990: Captain=Yamal,    Vice=Koundé,   Aggressor=Martin
```

990 = the total number of cards in that pile. Each card is one valid, unique assignment. That's it.

---

### Why Order Matters — The Test

Is **De Jong = Captain, Pedri = Vice** the same as **Pedri = Captain, De Jong = Vice**?

**No.** Different outcome. De Jong is wearing a different badge. The position each person lands in changes the result entirely.

Compare to: *"Pick 3 players to go to a press conference."* De Jong, Pedri, Fermín going is the **same** as Fermín, De Jong, Pedri going. The group is identical. That's combinations — coming next.

**The test:** Can swapping two people's positions produce a different outcome? If yes → order matters → Permutation.

---

### Why the Factorial Formula Appears

You want to write `11 × 10 × 9` in a compact form using factorials. Here's the logic:

```
11! = 11 × 10 × 9 × 8 × 7 × 6 × 5 × 4 × 3 × 2 × 1
```

You only want the top 3 numbers. So divide out everything below 9:

```
8! = 8 × 7 × 6 × 5 × 4 × 3 × 2 × 1
```

Therefore:

```
11 × 10 × 9  =  11! / 8!  =  11! / (11 - 3)!
```

**The formula is just a compact way to write "multiply from n downward, stopping after r steps."** The `(n-r)!` in the denominator cancels the tail you don't want.

```
nPr = n! / (n - r)!

11P3 = 11! / (11-3)! = 11! / 8! = 990  ✓
```

---

### What Insight Does Permutation Give You?

The formula matters because manually counting gets impossible fast.

**127 students, assign President + VP + Treasurer:**

```
127 × 126 × 125  =  127! / 124!  =  2,000,250
```

You cannot list those out. The formula collapses a massive counting problem into a single expression.

**In CS:** You use this whenever you need to count the size of a search space, analyse complexity of ordered selections, or compute probabilities — since probability = favourable outcomes / total outcomes, and you need both counts.

---

### A Clean Direct Example

> A streaming app has 50 songs. It plays 5 of them in order as your "daily mix preview." How many distinct previews are possible?

```
50P5 = 50! / 45! = 50 × 49 × 48 × 47 × 46 = 254,251,200
```

**Without replacement** — same song can't play twice in one preview.

254 million is **not** how many times you can play 5 songs before running out. It is the number of distinct ordered playlists that exist. Each playlist is one card in the pile. The 50 songs absolutely repeat across cards — that's fine. Each card is a unique *arrangement*.

---

### The Shortcut / Mental Hack

> After `(n - r)`, count up to n inclusive and multiply.

**P1 — 8 runners, award 1st/2nd/3rd:** n=8, r=3, 8-3=5 → product from 6 to 8 → `8 × 7 × 6`

⚠️ **Pitfall on direction:** The blank method goes **downward** from n, not upward. `8 × 7 × 6` not `6 × 7 × 8`. Multiplication is commutative so the answer is the same, but your mental model should match the actual process — you start at n and count down r steps.

---

### Practice Problems — Permutations

| Problem | Answer |
|---|---|
| 8 runners, award 1st/2nd/3rd | 8 × 7 × 6 |
| 6 songs, arrange all 6 in a playlist | 6! = 6 × 5 × 4 × 3 × 2 × 1 |
| 10 employees, assign CEO/CFO/COO | 10 × 9 × 8 |
| 30 students, pick 4 to answer questions one at a time | 30 × 29 × 28 × 27 |
| 5 books, place exactly 3 on a shelf (position matters) | 5 × 4 × 3 |

**⚠️ The "arrange all 6" trap:** When r = n, every slot is filled. Don't confuse the playlist (1 thing) with the positions in the playlist (n slots). The playlist has 6 position slots. So r = 6, not r = 1.

```
6P6 = 6! / (6-6)! = 6! / 0! = 6! / 1 = 6!
```

---

## 2. Combinations

### The One Question That Splits Them

Same squad. **"Pick 3 players to do a post-match interview."**

Is this the same as picking Captain, Vice, Aggressor?

**No.** Because:

```
Interview group: De Jong, Pedri, Fermín
Interview group: Fermín, De Jong, Pedri
Interview group: Pedri, Fermín, De Jong
```

These are all **the same group.** It doesn't matter who gets named first. They're all three guys going to the same room to talk to journalists.

With roles, swapping positions = different card. With a group, swapping positions = same card.

**That's the entire difference:**
- Roles → order matters → **Permutation**
- Group → order doesn't matter → **Combination**

---

### Definition

> **Combination:** Given n items, choose a group of size r without replacement. Order does not matter.

---

### Why Combinations Are Always Smaller Than Permutations

**The intuition trap:** "If you remove the order restriction, shouldn't there be more possibilities?"

**No. This is backwards.**

Order doesn't restrict — order **creates distinction.** When you add order, "De Jong then Pedri" and "Pedri then De Jong" count as two different things. When you remove order, they collapse into one. Collapsing = fewer results. Always.

**Feel it directly — 3 players, pick 2:**

With order (permutation):
```
De Jong → Pedri
De Jong → Fermín
Pedri → De Jong
Pedri → Fermín
Fermín → De Jong
Fermín → Pedri
= 6 outcomes
```

Without order (combination):
```
{De Jong, Pedri}
{De Jong, Fermín}
{Pedri, Fermín}
= 3 outcomes
```

Rows 1 and 3 from permutation collapsed into one. Rows 2 and 5 collapsed into one. Rows 4 and 6 collapsed into one. Removing order merges existing outcomes — it doesn't create new ones.

---

### Where the r! Comes From

Take one group: **{De Jong, Pedri, Fermín}**

How many ways can you arrange these 3 people in ordered slots?

```
De Jong, Pedri, Fermín
De Jong, Fermín, Pedri
Pedri, De Jong, Fermín
Pedri, Fermín, De Jong
Fermín, De Jong, Pedri
Fermín, Pedri, De Jong
= 6 arrangements
```

And 3! = 3 × 2 × 1 = 6 ✓

Every single group of 3 players generates exactly **6 cards** in the permutation pile — but they all represent the same group. So in the permutation count of 990, every unique group was counted 6 times.

To get unique groups, divide out the overcounting:

```
990 / 6 = 165
```

**165 unique interview groups** from 11 players.

---

### The Formula

```
nCr = nPr / r!  =  n! / (n - r)! × r!
```

The `r!` literally means: **"divide out the overcounting."** Every group of r items was counted r! times in the permutation count, because there are r! ways to arrange r items. You don't want that, so you kill it.

**Permutation vs Combination — formula comparison:**

```
nPr = n! / (n-r)!            ← stop here, order preserved

nCr = n! / (n-r)! × r!      ← divide by r! to kill order
```

Same numerator. Combinations just have one extra thing in the denominator — the correction for not caring about order.

**The relationship in one sentence:**

> Every combination, when you care about order, becomes r! permutations. So combinations = permutations ÷ r!

nCr is always ≤ nPr. Always.

---

### Notation

`nCr` is shorthand. It doesn't show the expansion — it assumes you know the formula behind it.

```
15C3  →  15! / (15-3)! × 3!  →  (13 × 14 × 15) / 3!
```

Same way 6! doesn't show 1×2×3×4×5×6 every time. The notation is the compressed form. The expansion lives in your head.

Alternative stacked notation (means the same thing):
```
(15)
( 3)
```

---

### Practice Problems — Combinations

| Problem | Answer |
|---|---|
| Class of 15, pick group of 3 for conference | 15C3 = (13×14×15) / 3! |
| 10 songs, pick 4 (order doesn't matter) | 10C4 = (7×8×9×10) / 4! |
| 20 players, pick group of 5 for photo | 20C5 = (15×16×17×18×19×20) / 5! |
| 52 cards, deal hand of 5 | 52C5 = (48×49×50×51×52) / 5! |
| 30 students, form study group of 4 | 30C4 = (27×28×29×30) / 4! |

**The mental hack carries over:** n=15, r=3 → 15-3=12 → product from 13 to 15 → divide by 3!

---

## 3. HW1 — Conference Room Problem (Full Worked Solution)

### Setup

```
Total people: 250
Two special people: Sam Altman and Elon
Regular people: 250 - 2 = 248

Room A → fits 100
Room B → fits 70
Room C → fits 80
```

---

### Q1: How many assignments have Sam and Elon in the same room?

**Key insight:** There are 3 mutually exclusive cases — Sam and Elon in A, in B, or in C. We calculate each separately and add them.

**Why 248 and not 250 when filling the remaining seats?**

Locking Sam and Elon into a room is one action with two simultaneous effects:
- People pool: 250 → 248 (they are placed, out of the pool)
- Seat count: e.g. 100 → 98 (their seats are taken)

---

**Case 1: Both in Room A**

- Lock Sam + Elon in A → 98 seats left, pick from 248 normals: `248C98`
- 150 normals remain → fill B: `150C70`
- 80 remain → fill C: `80C80 = 1`

```
248C98 · 150C70 · 80C80
```

---

**Case 2: Both in Room B**

- Lock Sam + Elon in B → 68 seats left, pick from 248 normals: `248C68`
- 180 normals remain → fill A: `180C100`
- 80 remain → fill C: `80C80 = 1`

```
248C68 · 180C100 · 80C80
```

---

**Case 3: Both in Room C**

- Lock Sam + Elon in C → 78 seats left, pick from 248 normals: `248C78`
- 170 normals remain → fill A: `170C100`
- 70 remain → fill B: `70C70 = 1`

```
248C78 · 170C100 · 70C70
```

---

**Q1 Final Answer — add all 3 cases:**

$$\binom{248}{98}\binom{150}{70}\binom{80}{80} + \binom{248}{68}\binom{180}{100}\binom{80}{80} + \binom{248}{78}\binom{170}{100}\binom{70}{70}$$

**Why add?** Each case (A, B, C) is mutually exclusive — Sam and Elon can only be in one room. Mutually exclusive cases get added.

**Why multiply within each case?** All room assignments happen simultaneously as one event. Independent stages get multiplied (basic counting principle from Lecture 1 Part 1).

---

### Q2: How many assignments do NOT have Sam and Elon in the same room?

**Use the complement trick:**

```
Q2 = Total − Q1
```

Counting "not together" directly is messy. Flipping it to Total minus Together is clean. This is the complement move — Aᶜ = S − A from Part 1.

**Total (no restrictions — treat all 250 people equally):**

- Pick 100 for A: `250C100`
- Pick 70 of remaining 150 for B: `150C70`
- 80 remain → C: `80C80 = 1`

```
Total = 250C100 · 150C70 · 80C80
```

**Q2 Final Answer:**

$$\binom{250}{100}\binom{150}{70}\binom{80}{80} - \left[\binom{248}{98}\binom{150}{70}\binom{80}{80} + \binom{248}{68}\binom{180}{100}\binom{80}{80} + \binom{248}{78}\binom{170}{100}\binom{70}{70}\right]$$

---

### Key Concepts from this Problem

| Concept | Explanation |
|---|---|
| Multiply within a case | All room assignments happen together — one event, multiply stages |
| Add across cases | Each case (A, B, C) is mutually exclusive — add them |
| Complement | "NOT together" = Total − "Together" |
| Why 248? | Locking 2 people removes them from the pool AND fills 2 seats — one action, two effects |
| Why Total uses 250? | No restrictions — every person treated equally |
| Why nCn = 1? | Only one way to choose everyone. Always write it out while learning to show you accounted for that room |

---

## 4. Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| nPr = n! / (n-r)! | Permutations — ordered selection, no replacement |
| nCr = n! / (n-r)! r! | Combinations — unordered group, no replacement |
| nCr = nPr / r! | Combinations as permutations with overcounting removed |
| nCn = 1 | Only one way to choose everyone |
| nC0 = 1 | Only one way to choose nobody |

---

## 5. Permutation vs Combination — Decision Guide

**Ask: Does swapping two people's positions produce a different outcome?**

| Yes → different outcome | No → same outcome |
|---|---|
| Order matters | Order doesn't matter |
| **Permutation** | **Combination** |
| nPr | nCr |
| Roles, rankings, sequences, playlists (ordered) | Groups, committees, teams, hands of cards |

---

## 6. Notation Glossary (additions from Part 2)

| Symbol | Meaning |
|---|---|
| nPr | Permutation — n items, select r, order matters |
| nCr | Combination — n items, select r, order doesn't matter |
| r! | r factorial — also the number of ways to reorder r items |
| nCn | = 1, only one way to select everyone |

---

## Common Pitfalls — Part 2

**Pitfall 1 — Confusing the playlist with its positions**
"Arrange all 6 songs in a playlist" → r = 6 (the 6 position slots), not r = 1 (the one playlist). The thing you're filling is slots, not playlists.

**Pitfall 2 — Thinking combinations should be bigger than permutations**
Removing order collapses outcomes together. Fewer distinctions = fewer results. nCr ≤ nPr always.

**Pitfall 3 — Forgetting to remove special people from the pool**
In the conference room problem, locking Sam and Elon into a room removes them from the available pool (248) AND takes their seats simultaneously. Missing either effect breaks the calculation.

**Pitfall 4 — Adding when you should multiply (or vice versa)**
Within one case → multiply (independent stages, counting principle).
Across mutually exclusive cases → add.

**Pitfall 5 — Not using the complement**
When a problem says "how many do NOT..." — don't count the NOT directly. Compute Total and subtract the unwanted count. Almost always cleaner.

---

*End of Lecture 1. Next: Lecture 2 — Probability Foundations, Axioms, and Conditional Probability.*