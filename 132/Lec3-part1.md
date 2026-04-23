# ECS 132 — Probability and Statistical Modeling for CS
## Lec - 3 - Part 1

> **How to use this doc:** Read through once as a review. Every definition, formula, example problem, and pitfall from the session is here. When something feels fuzzy, that's your cue to go back and drill it. This doc covers the first half of Lecture 3 — Properties of Probability, Independence vs Mutual Exclusivity, and Naive Bayes with full worked examples.

---

## Where We Left Off

Lecture 2 ended with the full conditional probability toolkit:

| Formula | What it's for |
|---|---|
| P(A∪B) = P(A) + P(B) - P(A∩B) | Union of two events |
| P(A∩B) = P(A\|B) · P(B) | Joint probability via conditional |
| P(A\|B) = P(A∩B) / P(B) | Conditional probability |

Lecture 3 picks up by reviewing the Naive Definition, then introduces **Independence**, corrects a common misconception about **Mutual Exclusivity**, and builds into **Naive Bayes** — the first real CS application of everything covered so far.

---

## Part 1 — Quick Review: Naive Definition and the Sample Space Trap

### Naive Definition (Recap)

$$P(A) = \frac{|A|}{|S|}$$

Three requirements — all must hold:
1. S must be finite
2. All outcomes are equally likely
3. Outcomes are mutually exclusive (disjoint)

---

### The 2-Coin Toss Trap

**Setup:** Flip 2 coins. Define S = {0H, 1H, 2H}. A = single head.

Someone computes P(A) = 1/3. **Wrong. Breaks rule 2.**

| Notation | Meaning | Ways it happens |
|---|---|---|
| 0H | Zero heads — TT | 1 way |
| 1H | One head — HT or TH | **2 ways** |
| 2H | Two heads — HH | 1 way |

"1H" is secretly two outcomes squished into one label. It's twice as likely as 0H or 2H — the equally likely requirement collapses.

**The fix:** Define S properly as {HH, HT, TH, TT}. Now every outcome is one way, equally likely.

- P(exactly 1 head) = 2/4 = **1/2**
- P(at least 1 head) = 3/4

> **Key lesson:** The naive definition is just a counting machine. It gives you a number no matter what you feed it. Your job is to feed it a properly defined sample space. The answer 1/3 isn't wrong because of bad math — it's wrong because the sample space was defined sloppily.

---

## Part 2 — Properties of Probability

### The Boundary Rule

$$0 \leq P(A) \leq 1$$

Probability can never be negative and can never exceed 1. If your answer is 1.3 or -0.2, something broke upstream. Use this as a gut-check on every answer.

---

## Part 3 — Independence

### Definition

> **A and B are independent if knowing B occurred tells you nothing about A.**

Formally:

$$\text{If A and B are independent} \Rightarrow P(A|B) = P(A)$$

The conditional probability equals the unconditional probability. Shrinking the world down to B=Yes rows doesn't change how often A appears. B gave you zero information.

---

### The Multiplication Shortcut for Independent Events

You already know the general rule:

```
P(A and B) = P(A|B) · P(B)
```

If A and B are independent, substitute P(A) for P(A|B):

```
P(A and B) = P(A) · P(B)
```

Straight multiplication. No conditional needed.

---

### Examples of Independent Events

**Example 1 — Coin and Die:**
Flip a coin, roll a die. A = heads, B = die shows 6.
Does knowing the die showed 6 change whether the coin landed heads? No. Separate physical objects.
P(A|B) = P(A) = 1/2. **Independent.**

**Example 2 — Two Dice:**
Roll two dice. x1 = dots on die 1, x2 = dots on die 2.
P(x1=3 and x2=4) = P(x1=3) · P(x2=4) = 1/6 · 1/6 = **1/36**
One die doesn't affect the other. Independent — straight multiplication applies.

**Example 3 — Repeated Rolls/Flips:**
- P(rolling a 2 on the 2nd try | first roll was a 1) = 1/6. The first roll is irrelevant.
- P(second coin is heads | first coin was heads) = 1/2. The first toss is irrelevant.

> **The test for independence:** Does the outcome of one event change the probability of the other? If no → independent. Constraints that link two events together (like "given the sum is less than 5") are what create dependence.

---

## Part 4 — Mutual Exclusivity (Disjoint Events)

### Definition

> **A and B are mutually exclusive (disjoint) if they cannot happen at the same time.**

$$P(A \cap B) = 0$$

---

### The Critical Misconception — Disjoint ≠ Independent

This is one of the most common mix-ups in the course. They are not the same thing. They are almost opposites.

**Apply the independence test to disjoint events:**

Roll one die. A = shows 3, B = shows 4. These are disjoint — one roll can't produce both.

```
P(A|B) = P(A∩B) / P(B) = 0 / (1/6) = 0
```

But P(A) = 1/6. Since P(A|B) ≠ P(A), the independence test **fails**. They are **dependent**.

Knowing B happened (die showed 4) told you A is completely impossible. That's the furthest thing from "knowing B tells you nothing about A."

| | Independent | Mutually Exclusive (Disjoint) |
|---|---|---|
| What it means | Knowing one gives zero info about the other | Cannot happen at the same time |
| P(A∩B) | P(A) · P(B) | 0 |
| P(A\|B) | P(A) — unchanged | 0 — A becomes impossible |
| Relationship | Separate, unlinked | Maximally dependent in one direction |

> **One line summary:** Independent = knowing B happened → A's probability unchanged. Disjoint = knowing B happened → A's probability collapses to zero. Zero ≠ unchanged.

> **The "two sides of the same coin" model:** Disjoint events are like two sides of a coin. If you know one side is showing, you know the other is impossible. That's not independence — that's the most informative thing one event can tell you about another.

---

### P(A or B) for Disjoint Events

When A and B are mutually exclusive, P(A∩B) = 0, so the union formula simplifies:

```
P(A∪B) = P(A) + P(B) - 0 = P(A) + P(B)
```

**Example:** P(x1=3 or x1=4) on a single die:
```
= P(x1=3) + P(x1=4) = 1/6 + 1/6 = 2/6
```
Simple addition is valid **only** when events are mutually exclusive.

---

### P(A and B) for Disjoint Events — Impossible Case

**Example:** P(x1=3 and x1=4) on a single die.
One roll cannot produce both 3 and 4. **P = 0.**

---

## Part 5 — Naive Bayes

### The Big Picture

> **Naive Bayes helps with classification given observed features.**

You observe something (a feature — a test result, a symptom, a word in an email). You want to classify the reality behind it (a class — sick or healthy, spam or not spam). The problem is you can only directly measure the feature, not the class.

Bayes flips this around. You use what you know about how features behave given each class, combined with how common each class is in the population, to work backwards to the probability of the class given the feature.

$$P(\text{class} \mid \text{feature}) = \frac{P(\text{class and feature})}{P(\text{feature})}$$

---

### The Two Ingredients You Always Need

For every Bayes problem you will ever see, you need exactly these two types of information:

1. **Population priors** — how common is each class in the real world?
2. **Test/feature behavior** — given each class, how does the feature behave?

From those two ingredients, you build everything.

---

### The Law of Total Probability — Building P(feature)

P(feature) is always the denominator. You can't observe it directly — you have to build it by accounting for every possible class that could produce the feature:

$$P(\text{feature}) = P(\text{feature} \mid \text{class}) \cdot P(\text{class}) + P(\text{feature} \mid \bar{\text{class}}) \cdot P(\bar{\text{class}})$$

In plain English: the probability of a feature showing up = the probability it shows up in sick people times how many sick people there are, plus the probability it shows up in healthy people times how many healthy people there are.

Every "and" term needs **two** ingredients — the feature behavior AND the population prior multiplied together.

---

## Part 6 — Worked Example 1: Covid Test

### Setup

You take a Covid test. It comes back **negative**. What is the probability you actually have Covid?

**Population prior:**
- P(covid) = 0.30
- P(no covid) = 0.70

**Test behavior on Covid patients (left pool — 100 confirmed Covid patients):**
- 90 tested positive → P(pos | covid) = 0.9
- 10 tested negative → P(neg | covid) = 0.1 ← false negatives, test missed them

**Test behavior on healthy people (right pool — 100 confirmed healthy people):**
- 5 tested positive → P(pos | no covid) = 0.05 ← false positives, test lied
- 95 tested negative → P(neg | no covid) = 0.95

**Find:** P(covid | neg)

---

### Solution

**Step 1 — Set up the formula:**

$$P(\text{covid} \mid \text{neg}) = \frac{P(\text{covid and neg})}{P(\text{neg})}$$

**Step 2 — Build the numerator:**

```
P(covid and neg) = P(neg | covid) · P(covid)
                 = 0.1 × 0.3
                 = 0.03
```

**Step 3 — Build the denominator (two ways to test negative):**

```
P(neg) = P(neg and covid) + P(neg and no covid)
       = [P(neg | covid) · P(covid)] + [P(neg | no covid) · P(no covid)]
       = (0.1 × 0.3) + (0.95 × 0.7)
       = 0.03 + 0.665
       = 0.695
```

**Step 4 — Apply Bayes:**

$$P(\text{covid} \mid \text{neg}) = \frac{0.03}{0.695} \approx 4.3\%$$

**Interpretation:** Even though you tested negative, there is still a 4.3% chance you have Covid. The test has a 10% false negative rate and Covid is common enough (30%) that a small slice of negative tests are still hiding real cases.

---

### The Fully Expanded Bayes Formula

The denominator P(neg) written out in full gives you the classic Bayes' Theorem form:

$$P(\text{covid} \mid \text{neg}) = \frac{P(\text{neg} \mid \text{covid}) \cdot P(\text{covid})}{P(\text{neg} \mid \text{covid}) \cdot P(\text{covid}) + P(\text{neg} \mid \overline{\text{covid}}) \cdot P(\overline{\text{covid}})}$$

$$= \frac{(0.1)(0.3)}{(0.1)(0.3) + (0.95)(0.7)}$$

---

## Part 7 — Worked Example 2: Cancer Test

### Setup

A cancer test is applied to a patient. The patient tests **negative**. What is the probability they are actually cancer free?

**Population prior:**
- P(cancer) = 0.01 → 1% of the population has cancer
- P(C̄) = 0.99

**Test behavior on cancer patients:**
- P(pos | cancer) = 0.5 → catches cancer only 50% of the time (terrible test)
- P(neg | cancer) = 0.5 → misses cancer 50% of the time (false negative)

**Test behavior on healthy people:**
- P(pos | C̄) = 0.1 → falsely alarms 10% of the time (false positive)
- P(neg | C̄) = 0.90 → correctly clears them 90% of the time

**Find:** P(C̄ | neg) — probability the patient is cancer free given a negative test

---

### Solution

**Step 1 — Set up the formula:**

$$P(\bar{C} \mid \text{neg}) = \frac{P(\bar{C} \text{ and neg})}{P(\text{neg})}$$

**Step 2 — Build the numerator:**

```
P(C̄ and neg) = P(neg | C̄) · P(C̄)
              = 0.90 × 0.99
              = 0.891
```

**Step 3 — Build the denominator:**

```
Case 1 — has cancer AND tests negative:
P(neg and cancer) = P(neg | cancer) · P(cancer) = 0.5 × 0.01 = 0.005

Case 2 — no cancer AND tests negative:
P(neg and C̄) = P(neg | C̄) · P(C̄) = 0.90 × 0.99 = 0.891

P(neg) = 0.005 + 0.891 = 0.896
```

**Step 4 — Apply Bayes:**

$$P(\bar{C} \mid \text{neg}) = \frac{0.891}{0.896} \approx 99.4\%$$

**Interpretation:** A negative test result means there is a 99.4% chance the patient is actually cancer free. This makes sense — cancer is rare (1%) and the test is decent on healthy people (90% accurate), so a negative test is very reassuring despite the test being mediocre on actual cancer patients.

---

## Part 8 — Worked Example 3: NBA Basketball

### Setup

You see a tall person walking down the street. What is the probability they play in the NBA?

**Population priors:**
- P(B) = 0.001 → only 0.1% of the world plays in the NBA
- P(B̄) = 0.999

**Feature behavior:**
- P(T | B) = 0.95 → 95% of NBA players are tall
- P(T | B̄) = 0.15 → 15% of non-NBA people are tall

**Find:** P(B | T) — probability a tall person plays in the NBA

---

### Solution

$$P(B \mid T) = \frac{P(T \mid B) \cdot P(B)}{P(T \mid B) \cdot P(B) + P(T \mid \bar{B}) \cdot P(\bar{B})}$$

```
Numerator:   0.95 × 0.001 = 0.00095

Denominator: (0.95 × 0.001) + (0.15 × 0.999)
           = 0.00095 + 0.14985
           = 0.1508

P(B | T) = 0.00095 / 0.1508 ≈ 0.63%
```

**Interpretation:** Even though 95% of NBA players are tall, a tall person has only a 0.63% chance of being an NBA player. Tall people are common in the world (15%) and NBA players are extremely rare (0.1%) — seeing a tall person tells you almost nothing about whether they're in the NBA.

---

### The Key Insight from All Three Examples

> A feature that is common in your class means nothing if that feature is also common everywhere else.

This is why the **prior probability** (how rare or common the class is in the population) matters so much. A test can be highly accurate and still be misleading if the thing you're testing for is very rare. Bayes accounts for this automatically.

---

## Part 9 — The General Bayes Formula

### Abstract Form

Replace Covid/cancer/basketball with any class and feature:

$$P(\text{class} \mid \text{feature}) = \frac{P(\text{feature} \mid \text{class}) \cdot P(\text{class})}{P(\text{feature})}$$

Where:

$$P(\text{feature}) = P(\text{feature} \mid \text{class}) \cdot P(\text{class}) + P(\text{feature} \mid \bar{C}) \cdot P(\bar{C})$$

**In the Covid problem:**
- class = covid, feature = test result
- You observed the feature (test said negative), you want the class (do you have covid?)

**In CS / Machine Learning:**
- class = spam or not spam, feature = words in an email
- class = fraud or not fraud, feature = transaction pattern
- class = disease or healthy, feature = symptoms or test result

Same formula, same structure, every time.

---

## Key Formulas — Quick Reference

| Formula | What it's for |
|---|---|
| 0 ≤ P(A) ≤ 1 | Boundary rule — sanity check every answer |
| P(A\|B) = P(A) | Definition of independence |
| P(A and B) = P(A) · P(B) | Joint probability when independent |
| P(A\|B) = 0 when disjoint | Disjoint events are dependent, not independent |
| P(class\|feature) = P(class and feature) / P(feature) | Bayes' Theorem |
| P(feature) = P(f\|class)·P(class) + P(f\|C̄)·P(C̄) | Law of Total Probability — denominator expansion |
| P(A and B) = P(A\|B) · P(B) | Multiplication rule — building joint probabilities |

---

## Notation Glossary

| Symbol | Meaning |
|---|---|
| P(A\|B) | Probability of A given B has occurred |
| P(A∩B) | Probability of A and B — both occur |
| P(A∪B) | Probability of A or B — at least one occurs |
| C̄ | Complement of C — "not C" |
| P(B) | Prior probability — how common class B is in the population |
| P(feature\|class) | Likelihood — how often the feature appears given the class |
| P(class\|feature) | Posterior — what you actually want to find |

---

## Common Pitfalls

**Pitfall 1 — Confusing Disjoint with Independent**
Disjoint events are NOT independent. Knowing one happened makes the other impossible — that's the most informative relationship two events can have. Always apply the test: does P(A|B) = P(A)? If not, they're dependent.

**Pitfall 2 — Thinking the test result is the same as reality**
Testing negative ≠ not having the disease. The test is imperfect. These are two separate things — the reality (class) and the observation (feature). Bayes is the tool that connects them.

**Pitfall 3 — Forgetting the population prior**
Every "and" term in Bayes needs two ingredients — the feature behavior AND the population prior. P(neg | covid) is not the same as P(neg and covid). You must multiply by P(covid).

**Pitfall 4 — Using P(no disease) as P(negative test)**
P(no covid) = 0.70 and P(test negative) = 0.695 are different numbers. Testing negative and not having covid are not the same event — false negatives exist. Don't substitute one for the other.

**Pitfall 5 — Thinking a highly accurate test is always reliable**
A test can be 95% accurate and still be mostly wrong if the disease is very rare. The prior (population rate) pulls the posterior toward the base rate. This is why the NBA example gives 0.63% despite 95% of NBA players being tall — tall people are too common everywhere else.

**Pitfall 6 — P(A and B) vs P(B and A)**
These are identical. "And" has no order. P(covid and neg) = P(neg and covid). Don't get confused when the label order flips between the numerator and the denominator expansion.

---

*End of Lec 3 — Part 1/2. Part 2 continues with pages 7 and 8 of Lecture 3.*