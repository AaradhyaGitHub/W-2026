# ECS 132 — Probability and Statistical Modeling for CS
## Lec - 3 - Part 2

> **How to use this doc:** Read through once as a review. Every definition, formula, example problem, and pitfall from the session is here. When something feels fuzzy, that's your cue to go back and drill it. This doc covers the second half of Lecture 3 — the review of key probability rules, Discrete Random Variables, Expected Value, and the Law of Total Probability through a multi-path coin game.

---

## Where We Left Off

Lec 3 Part 1 ended with three full Naive Bayes worked examples — Covid test, cancer test, and the NBA height problem. The big takeaway was:

> A feature that is common in your class means nothing if that feature is also common everywhere else. The prior always matters.

Part 2 opens with a review block — your professor putting core formulas back on the board before moving into new territory. Then it introduces **Discrete Random Variables** and **Expected Value**, and wraps with a multi-path coin game that ties the Law of Total Probability together cleanly.

---

## Part 1 — Review Block: Core Probability Rules

The first thing on the board in this section is a deliberate recap. These are not new — they're from Lec 2. But they're being reloaded here because they are the tools used to build everything that comes next.

### P(A and B) — Two Ways to Write It

$$P(A \cap B) = P(A \mid B) \cdot P(B)$$

$$P(A \cap B) = P(B \mid A) \cdot P(A)$$

Both are correct. Use whichever one has the easier ingredients. The intuition: to find the probability of both happening, zoom into one event first, then ask how often the second happens within that zoomed world.

---

### P(A|B) — Conditional Probability

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$

Shrink the world down to only the B=Yes rows in the tally book. Within that smaller world, ask how often A appears. Numerator = rows where both happened. Denominator = rows where B happened.

---

### Independence and Disjoint — Special Cases

**When independent:**

$$P(A \cap B) = P(A) \cdot P(B)$$

Knowing B happened tells you nothing about A. So P(A|B) collapses to P(A), and the multiplication rule simplifies to straight multiplication.

**P(A or B) in general:**

$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

**When disjoint (mutually exclusive):**

$$P(A \cup B) = P(A) + P(B)$$

Because P(A∩B) = 0 — they can never happen together, so there's nothing to subtract.

> **Don't mix these up.** Independent means knowing one tells you nothing about the other. Disjoint means knowing one makes the other impossible. They are nearly opposite concepts. See Lec 3 Part 1 for the full breakdown.

---

### Naive Bayes — The Formula

$$P(\text{class} \mid \text{feature}) = \frac{P(\text{feature} \mid \text{class}) \cdot P(\text{class})}{P(\text{feature})}$$

The denominator P(feature) is built using the Law of Total Probability — sum over all paths that could produce the feature, each weighted by how likely that path is. Your professor put this on the board here as a bridge — the coin game later in the lecture uses the exact same structure.

---

## Part 2 — Discrete Random Variables

### Why Random Variables Exist

When you run an experiment, the raw outcome might not be a number. Flipping a coin gives you "heads" or "tails." Drawing a card gives you "King of Spades." To do any math — to average outcomes, to compute probabilities of ranges, to build models — you need numbers.

A **random variable** is the function that converts outcomes into numbers.

---

### Definition

> **A random variable is the numeric outcome of an experiment.**

Formally, a random variable is a function:

$$f(\text{outcome}) \rightarrow \mathbb{R}$$

It maps any outcome from the sample space to a real number. That's all the arrow means — give it an outcome, it hands you back a number.

---

### Definition — Support

> **The support of a random variable is the set of all values it can possibly take.**

**Example:** x = result of rolling a 7-sided die.

$$x \in \{1, 2, 3, 4, 5, 6, 7\}$$

The support is {1, 2, 3, 4, 5, 6, 7}. Those are the only numbers x can ever be.

For a fair coin where x = 1 if heads, 0 if tails: support = {0, 1}.

---

### Definition — Discrete Random Variable

> **A discrete random variable is a random variable whose support is discrete** — meaning the values are countable and separated, not continuous.

The 7-sided die is discrete. The possible values are isolated integers — there's no value between 3 and 4. Compare to a continuous random variable like "time until the next bus" which can take any real value in a range.

For this course, Ch-3 is entirely about discrete random variables.

---

### Why This Matters — The Grade Example

Your professor used this example directly:

> "10% of people get an A in this class → not really helpful. If the average is A- → helpful → more likely to enroll."

This is the motivation for random variables and expected value. A single probability like "10% get an A" gives you one slice. But if you turn grades into a random variable and compute its expected value — the average outcome — you get a single number that summarizes the entire distribution. That summary is far more useful for a decision like "should I take this class."

This is what random variables unlock: the ability to talk about the center, spread, and shape of an experiment's outcomes — not just isolated probabilities.

---

## Part 3 — Expected Value

### Definition

> **The expected value (also called the mean or average) of a random variable x is the long run average of the numeric outcomes of the experiment.**

In tally book terms: every run, you write down the numeric value of x. After infinite runs, you add up all those values and divide by the number of runs. Whatever that fraction converges to is E(x).

$$E(x) = \mathbb{E}x = \lim_{n \to \infty} \frac{\sum_{i=1}^{n} x_i}{n}$$

---

### The Tally Book Intuition

Imagine every student in a class of 127 lined up. You walk down the line, talk to each one, and write down their grade as a number. That's your run. You add up all 127 grades and divide by 127. That's your estimate of E(x).

With 127 students you get an estimate. With infinite students (infinite runs), the average converges to the true expected value. Same Law of Large Numbers mechanic from Lec 2 — just applied to averages instead of probabilities.

> **The key difference from probability:** In probability you were tracking Yes/No and computing a fraction. In expected value you are tracking the actual numeric outcome and computing an average. The tally book is the same. The column changes.

---

### Notation

| Symbol | Meaning |
|---|---|
| E(x) | Expected value of random variable x |
| 𝔼x | Alternate notation — same thing |
| lim n→∞ | The limit as runs approach infinity — the true value, not an estimate |

---

## Part 4 — The Coin Game: Law of Total Probability in Action

### Setup

This is the worked example your professor used to bring everything together. Read the setup carefully before touching any math.

> You have a coin where **P(H) = 0.4** and **P(T) = 0.6**.
>
> - If the **first flip is Heads** → flip a **fair coin** next. P(H) = 0.5, P(T) = 0.5.
> - If the **first flip is Tails** → flip an **unfair coin** next. P(H) = 0.2, P(T) = 0.8.
>
> **Question:** What is the probability that the **2nd flip is Tails**?

---

### Why This Is Not Trivial

The two flips are **not independent**. The coin you use on flip 2 depends entirely on what flip 1 produced. You cannot just look up P(T) for a single coin — there isn't one coin. There are two possible coins, and which one you're flipping is determined by the first outcome.

This is the structure that requires the Law of Total Probability.

---

### The Two Paths

There are exactly two ways to reach "tails on flip 2":

```
Path 1: Flip 1 = Heads  →  flip fair coin  →  get Tails
Path 2: Flip 1 = Tails  →  flip unfair coin  →  get Tails
```

These paths are **mutually exclusive** — you cannot take both on the same run. So you can add their probabilities.

---

### Building Each Path

Each path is a joint probability — two things happening in sequence. Use the multiplication rule:

**Path 1:**

$$P(\text{Tails on flip 2 via fair}) = P(T \mid \text{fair}) \cdot P(\text{fair})$$

**Path 2:**

$$P(\text{Tails on flip 2 via unfair}) = P(T \mid \text{unfair}) \cdot P(\text{unfair})$$

---

### Plugging In

| Value | What it is | Number |
|---|---|---|
| P(T \| fair) | Tails on a fair coin | 0.5 |
| P(fair) | Probability of taking Path 1 = P(Heads on flip 1) | 0.4 |
| P(T \| unfair) | Tails on the unfair coin | 0.8 |
| P(unfair) | Probability of taking Path 2 = P(Tails on flip 1) | 0.6 |

$$P(\text{2nd flip is Tails}) = P(T \mid \text{fair}) \cdot P(\text{fair}) + P(T \mid \text{unfair}) \cdot P(\text{unfair})$$

$$= (0.5)(0.4) + (0.8)(0.6)$$

$$= 0.20 + 0.48$$

$$= \mathbf{0.68}$$

---

### The Pattern — Law of Total Probability

What just happened has a name and a structure you will use constantly:

$$P(B) = P(B \mid A_1) \cdot P(A_1) + P(B \mid A_2) \cdot P(A_2) + \ldots$$

Where A₁, A₂, … are all the mutually exclusive paths that could lead to B. You split the event into its paths, compute each path's probability using the multiplication rule, and add them up because the paths are mutually exclusive.

This is identical to what you did in the Naive Bayes denominator. P(feature) = P(feature | class)·P(class) + P(feature | not class)·P(not class). Same structure. Same logic.

---

### The Connection to Bayes

Your professor put Naive Bayes on the board right before this example for a reason. Now that you've done the coin game, the denominator of Bayes should feel mechanical rather than mysterious:

> The denominator is just the Law of Total Probability — split the event into all the ways it could happen, weight each by its path probability, add them up.

And if someone then asked: *given that the 2nd flip was tails, what's the probability it came from the fair coin path?* — that would be a Bayes question. You'd use 0.68 as the denominator, and P(fair and tails) = 0.20 as the numerator. P(came from fair | 2nd flip was tails) = 0.20 / 0.68 ≈ 29.4%.

---

## Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| P(A∩B) = P(A\|B)·P(B) = P(B\|A)·P(A) | Joint probability — two ways to write it |
| P(A\|B) = P(A∩B) / P(B) | Conditional probability |
| P(A∩B) = P(A)·P(B) when independent | Joint probability shortcut — independence only |
| P(A∪B) = P(A) + P(B) - P(A∩B) | Union — general case |
| P(A∪B) = P(A) + P(B) when disjoint | Union — mutually exclusive only |
| f(outcome) → ℝ | Random variable definition |
| E(x) = lim(n→∞) Σxᵢ / n | Expected value — long run average |
| P(B) = Σ P(B\|Aᵢ)·P(Aᵢ) | Law of Total Probability — sum over all paths |

---

## Notation Glossary

| Symbol | Meaning |
|---|---|
| x | Random variable — a numeric outcome of an experiment |
| Support | The set of all values x can possibly take |
| E(x) or 𝔼x | Expected value / mean / average of x |
| ℝ | The real number line — all real numbers |
| f(outcome) → ℝ | Random variable as a function mapping outcomes to numbers |
| lim n→∞ | As the number of runs approaches infinity |
| P(B\|Aᵢ) | Probability of B given path Aᵢ was taken |
| Discrete | Support is countable and separated — finite or countably infinite values |

---

## Common Pitfalls

**Pitfall 1 — Treating non-independent flips as independent**
In the coin game, flip 2 depends on flip 1. You cannot just use P(T) = 0.5 or 0.8 alone — you don't know which coin you're on until flip 1 is resolved. Always identify whether events are independent before applying the multiplication shortcut.

**Pitfall 2 — Forgetting to split into paths**
When an outcome can happen multiple ways, you must account for all of them. P(2nd flip is tails) cannot be computed with a single multiplication — there are two routes to tails and both contribute. Missing one path gives an answer that is systematically too low.

**Pitfall 3 — Adding paths that are not mutually exclusive**
You can only add the path probabilities because Path 1 and Path 2 are mutually exclusive — you can't take both on the same run. If paths can overlap, you'd need to subtract the intersection before adding. Always check.

**Pitfall 4 — Confusing E(x) with P(some outcome)**
Expected value is the long run average of the numeric outcome. It is not the probability of any particular outcome. P(x=3) and E(x) are completely different things — one is a fraction of Yes rows, the other is an average of actual values.

**Pitfall 5 — Thinking support is infinite when it's finite**
The support is exactly the set of values x can take — no more, no less. A 7-sided die has support {1,2,3,4,5,6,7}. x = 8 is not in the support. x = 3.5 is not in the support. Discrete means countable, separated values only.

**Pitfall 6 — Confusing P(fair) with P(T|fair)**
P(fair) = 0.4 is the probability of even arriving at the fair coin (flip 1 = heads). P(T|fair) = 0.5 is the probability of tails given you're using the fair coin. These are different. The multiplication rule needs both — one without the other gives you nothing.

---

## The Big Picture — End of Lecture 3

By the end of this lecture, the full foundation is in place:

**From Lec 2:** Frequentist probability, conditional probability, the multiplication rule, the union formula.

**From Lec 3 Part 1:** Independence vs mutual exclusivity (they are not the same), Naive Bayes as a classification tool, the Law of Total Probability as the denominator builder.

**From Lec 3 Part 2:** Random variables as functions that turn outcomes into numbers, support as the set of possible values, expected value as the long run average, and the Law of Total Probability applied mechanically through the coin game.

The thread connecting all of it: **the tally book**. Every concept — probability, conditional probability, independence, expected value — is just a different question you ask about the same tally book of experimental runs.

---

*End of Lec 3 — Part 2/2. Lecture 3 fully documented. Next: Lecture 4.*