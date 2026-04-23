# ECS 132 — Probability and Statistical Modeling for CS
## Lec - 4 - Part 1

> **How to use this doc:** Read through once as a review. Every definition, formula, example problem, and pitfall from the session is here. When something feels fuzzy, that's your cue to go back and drill it. This doc covers the first half of Lecture 4 — the attendance quiz (Law of Total Probability with a 3-sided die and coins), the Kjn bookkeeping framework, and Property A: the closed-form formula for Expected Value of a discrete random variable.

---

## Where We Left Off

Lec 3 Part 2 ended with three things locked in:

- **Random variables** — functions that map outcomes to numbers
- **Support** — the set of all values a random variable can take
- **Expected value** — the long run average of the numeric outcome, defined as:

$$E(x) = \lim_{n \to \infty} \frac{x_1 + x_2 + \ldots + x_n}{n}$$

Lecture 4 opens with an attendance quiz that forces you to apply the Law of Total Probability in a new shape — a dice choosing which coin to flip. Then it introduces a smarter way to organize your runs (Kjn), and uses that reorganization to derive a clean closed-form formula for E(x).

---

## Part 1 — Attendance Quiz: Dice + Coins + Law of Total Probability

### Setup

> Roll a 3-sided die. S = {1, 2, 3}.
>
> - If you see a **2** → flip a **fair coin**: P(H) = 0.5, P(T) = 0.5
> - **Else** (you see a 1 or 3) → flip an **unfair coin**: P(H) = 0.4, P(T) = 0.6
>
> Let **y = 1** if you see heads, **y = 0** if you see tails.
>
> Let **x** be the random variable for the dice roll.
>
> **Question: What is P(y = 1)?**

---

### Breaking Down the Experiment

The physical process:

1. Roll the die → get 1, 2, or 3 (each with probability 1/3)
2. If you got 2 → use the fair coin. Otherwise → use the unfair coin.
3. Flip that coin → record y.

The key trap here: **you cannot just say P(y=1) = 0.5 or 0.4.** Which number you use depends entirely on what the die showed. The coin is chosen by the die. These two stages are not independent.

---

### Identifying the Paths

To get y = 1, you need heads. There are exactly two routes to heads:

```
Path 1: Roll 2  →  flip fair coin   →  get Heads
Path 2: Roll 1 or 3  →  flip unfair coin  →  get Heads
```

These paths are **mutually exclusive** — you can only take one per run. So you can add their probabilities.

---

### Setting Up the Solution

$$P(y = 1) = P(y=1 \text{ and fair}) + P(y=1 \text{ and unfair})$$

Expanding each using the multiplication rule:

$$P(y=1) = P(y=1 \mid \text{fair}) \cdot P(\text{fair}) + P(y=1 \mid \text{unfair}) \cdot P(\text{unfair})$$

---

### Plugging In

| Value | What it is | Number |
|---|---|---|
| P(y=1 \| fair) | P(heads on fair coin) | 0.5 |
| P(fair) | P(rolling a 2) = P(x=2) | 1/3 |
| P(y=1 \| unfair) | P(heads on unfair coin) | 0.4 |
| P(unfair) | P(rolling a 1 or 3) = P(x≠2) | 2/3 |

$$P(y=1) = (0.5)\left(\frac{1}{3}\right) + (0.4)\left(\frac{2}{3}\right)$$

This is the answer left in closed form — which is acceptable and clean.

---

### The Pattern — Law of Total Probability (Again)

This is the same structure as the coin game from Lec 3:

$$P(B) = P(B \mid A_1) \cdot P(A_1) + P(B \mid A_2) \cdot P(A_2) + \ldots$$

The signal that you need this: **the probability of your outcome depends on which path you took to get there, and you don't get to choose the path — it's determined by a prior random event.**

Split into all paths. Multiply within each path. Add across paths (because they're mutually exclusive).

---

### P(y = 0) — The Other Side

The same structure applies for y = 0 (tails):

$$P(y=0) = P(y=0 \text{ and fair}) + P(y=0 \text{ and unfair})$$

$$= \left(\frac{1}{3}\right)(0.5) + \left(\frac{2}{3}\right)(0.6)$$

> **Sanity check:** P(y=0) + P(y=1) should equal 1. Verify this holds — it always will if you set up the paths correctly.

---

## Part 2 — Designing a Game: The Kjn Framework

### The Game Setup

> Toss **10 fair coins**. The outcome of one run = **number of heads** out of the 10 coins.
>
> Run the game **n times**.
>
> Let **xᵢ** = result of experiment i (number of heads on run i).

This is still the same E(x) definition from Lec 3 — xᵢ is the numeric outcome of run i. The support of x is {0, 1, 2, 3, ..., 10}.

---

### The Raw Tally (Run-by-Run)

If you run the game 7 times, you might get:

| Run (i) | xᵢ (# of heads) |
|---|---|
| 1 | 2 |
| 2 | 0 |
| 3 | 5 |
| 4 | 4 |
| 5 | 0 |
| 6 | 5 |
| 7 | 10 |

The raw expected value from this:

$$E(x) \approx \frac{2 + 0 + 5 + 4 + 0 + 5 + 10}{7}$$

That's the definition straight from Lec 3 — add up all the outcomes, divide by number of runs.

---

### Definition — Kjn

> **Kjn** = the number of times outcome j occurred across n runs of the game.

In other words: instead of organizing by run, you organize by **outcome**. For each possible value j (0 through 10), Kjn counts how many runs produced that value.

From the 7 runs above:

| j (outcome) | Kjn (# of times j occurred) |
|---|---|
| 0 | 2 (runs 2 and 5) |
| 1 | 0 |
| 2 | 1 (run 1) |
| 3 | 0 |
| 4 | 1 (run 4) |
| 5 | 2 (runs 3 and 6) |
| 6 | 0 |
| 7 | 0 |
| 8 | 0 |
| 9 | 0 |
| 10 | 1 (run 7) |

**Sanity check:** All Kjn values must sum to n (total runs). Here: 2+0+1+0+1+2+0+0+0+0+1 = 7. ✓

---

### Why Reorganize This Way?

The traditional tally tracks each run row by row. Kjn flips it — you're grouping by **outcome** instead. Same information, different organization.

The payoff: once you group by outcome, you can rewrite the sum for E(x) in a smarter way.

---

### Rewriting E(x) Using Kjn

The raw sum:

$$E(x) \approx \frac{x_1 + x_2 + \ldots + x_n}{n}$$

With Kjn, instead of listing every run result, you group identical outcomes together:

$$E(x) \approx \frac{0 \cdot K_{0,n} + 1 \cdot K_{1,n} + 2 \cdot K_{2,n} + \ldots + 10 \cdot K_{10,n}}{n}$$

From the example: (0×2) + (1×0) + (2×1) + (3×0) + (4×1) + (5×2) + ... + (10×1) = 2+0+2+0+4+10+0+... = same numerator as before. Same sum, just grouped smarter.

$$E(x) = \lim_{n \to \infty} \frac{\sum_{j=0}^{10} j \cdot K_{j,n}}{n} = \lim_{n \to \infty} \sum_{j=0}^{10} j \cdot \frac{K_{j,n}}{n}$$

---

### The Key Step — Kjn/n as n→∞

What does Kjn/n converge to as n → ∞?

It's the fraction of runs where outcome j occurred. By the frequentist definition of probability (from Lec 2), that's just:

$$\lim_{n \to \infty} \frac{K_{j,n}}{n} = P(x = j)$$

Swap the limit inside the sum (this is the "switching order of the limit" step on the board):

$$E(x) = \sum_{j=0}^{10} j \cdot P(x = j)$$

That's **Property A**.

---

## Part 3 — Property A: Closed-Form Formula for E(x)

### Definition — Property A

> **If x is a discrete random variable, the expected value of x is:**

$$E(x) = \sum_{c \in A} c \cdot P(x = c)$$

> **where A is the support of x** — the set of all possible values x can take.

In plain English: for each possible outcome, multiply that outcome by its probability, then add everything up. That's the expected value.

---

### Why This Is the Same Thing You Already Knew

The raw definition: add up all your run results and divide by runs.
Property A: for each outcome value, multiply it by how often it shows up (its probability), add those up.

Same thing. Just swapping "raw counts of runs" for "probabilities." The Kjn argument above shows exactly why they're equivalent.

> **The tell for when to use Property A:** Any time a question asks for "on average" or "expected" or "in the long run" — and you have a random variable with known probabilities for each outcome.

---

### Notation

| Symbol | Meaning |
|---|---|
| x | Discrete random variable |
| A | Support of x — all possible values |
| c | A specific value in the support |
| P(x = c) | Probability that x takes the value c |
| E(x) | Expected value of x |
| Kjn | Number of times outcome j occurred in n runs |
| Kjn/n | Fraction of runs producing outcome j → converges to P(x=j) |

---

### Example 1 — 3-Sided Unfair Die

> Die with support {1, 2, 3}. Probabilities: P(1) = 0.5, P(2) = 0.25, P(3) = 0.25.

$$E(x) = \sum_{c \in \{1,2,3\}} c \cdot P(x=c)$$

$$= 1(0.5) + 2(0.25) + 3(0.25)$$

$$= 0.5 + 0.5 + 0.75 = \mathbf{1.75}$$

---

### Example 2 — 4-Sided Fair Die

> Die with support {1, 2, 3, 4}. All equally likely: P(c) = 0.25 for each.

$$E(x) = 1(0.25) + 2(0.25) + 3(0.25) + 4(0.25)$$

$$= 0.25 + 0.50 + 0.75 + 1.00 = \mathbf{2.5}$$

> **Note:** 2.5 is not in the support. The expected value doesn't have to be a value x can actually take — it's an average, not a prediction of any single run.

---

### When You'll Use Property A — 5 Real Situations

| Situation | x | Use Property A to find... |
|---|---|---|
| Lottery ticket — win $100 w/ prob 0.01, $10 w/ prob 0.05, $0 otherwise | Payout | Average payout per game |
| App load time — 1s (p=0.5), 3s (p=0.3), 10s (p=0.2) | Load time in seconds | Average load time users experience |
| Bug fixing — 0 bugs (p=0.2), 1 bug (p=0.5), 2 bugs (p=0.3) | Bugs fixed | Expected bugs fixed per day |
| Roll a fair 6-sided die, get paid that many dollars | Payout | Expected payout per roll |
| API retries — 1 retry (p=0.6), 2 (p=0.3), 3 (p=0.1) | # retries | Expected retries per call |

**The signal:** "on average," "expected," "in the long run" → Property A.

---

## Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| P(y=1) = P(y=1\|fair)·P(fair) + P(y=1\|unfair)·P(unfair) | Law of Total Probability — two path version |
| P(B) = Σ P(B\|Aᵢ)·P(Aᵢ) | Law of Total Probability — general form |
| Kjn = # of times outcome j occurred in n runs | Definition of Kjn |
| Kjn/n → P(x=j) as n→∞ | Kjn/n converges to probability by frequentist definition |
| E(x) = Σ xᵢ / n as n→∞ | Expected value — raw run-by-run definition |
| E(x) = Σ c·P(x=c) for c ∈ A | **Property A** — closed form, use this in practice |

---

## Notation Glossary

| Symbol | Meaning |
|---|---|
| x | Random variable — numeric outcome of experiment |
| xᵢ | Outcome of the i-th run |
| A | Support — set of all values x can take |
| c | A specific value in the support |
| E(x) | Expected value / mean / long run average of x |
| Kjn | # of times outcome j appeared across n runs |
| n | Total number of runs |
| j | A specific outcome value (subscript in Kjn) |
| P(x=c) | Probability x takes value c |
| P(y=1\|fair) | Probability of heads given you're using the fair coin |
| P(fair) | Probability of arriving at the fair coin (= P(x=2) = 1/3) |

---

## Common Pitfalls

**Pitfall 1 — Grabbing one coin's probability when the coin depends on the die**
In the attendance quiz, you can't just say P(y=1) = 0.5. Which coin you use depends on the die roll. Always check: is this outcome's probability conditional on a prior random event? If yes → Law of Total Probability, split into paths.

**Pitfall 2 — Forgetting that Kjn subscript is the outcome, not the run**
k₀, k₁, k₂... k₁₀ are indexed by **outcome value** (0 through 10 heads), not by run number. The list runs from 0 to 10 because those are the possible outcomes of the experiment, not because you ran it 11 times.

**Pitfall 3 — Kjn values must sum to n**
A quick sanity check: add up all your Kjn values. They must equal n (total runs). If they don't, you've miscounted somewhere.

**Pitfall 4 — E(x) doesn't have to be in the support**
The expected value of a 4-sided fair die is 2.5. You can never roll a 2.5. That's fine — E(x) is a long run average, not a prediction of any single outcome. Don't confuse E(x) with P(x=c).

**Pitfall 5 — Property A requires you to sum over ALL values in the support**
If x ∈ {0,1,2,...,10}, you need all 11 terms. Dropping a value (especially 0, since 0·P(x=0)=0 and feels tempting to skip) is technically fine numerically but sloppy — make sure you're accounting for the full support.

**Pitfall 6 — Confusing P(y=1|fair) with P(fair)**
P(fair) = 1/3 is the probability of even arriving at the fair coin (rolling a 2). P(y=1|fair) = 0.5 is the probability of heads given you're already using it. The multiplication rule needs both. Neither one alone gives you P(y=1 and fair).

---

## The Big Picture — End of Lecture 4 Part 1

The thread through this entire session:

**Attendance quiz** — Law of Total Probability in a new shape (die selecting coin). The structure is identical to the Lec 3 coin game and the Naive Bayes denominator. Same logic, different costume.

**Kjn** — a smarter way to organize your tally book. Instead of row-by-row runs, group by outcome. Same data, reorganized so the math flows more cleanly.

**Property A** — the Kjn reorganization, taken to infinity. Kjn/n becomes P(x=j). The raw average becomes a weighted sum of outcomes times their probabilities. This is the formula you will actually use every time someone says "expected value" or "on average."

The through-line from Lec 2 to here: **the tally book is always the same. You're just asking smarter questions about it each lecture.**

---

*End of Lec 4 — Part 1/2. Part 2 continues with the remainder of Lecture 4.*