# ECS 132 — Midterm 2 Master Review

> **Note on `y`:** Your personal variable `y` = (last digit of Student ID) % 5 + 8, so `y ∈ {8, 9, 10, 11, 12}`.
> The official Fall 2025 solution key uses **y = 8**.
> **Do not leave y as a variable — always substitute your constant before solving.**

---

## Sources Key

| Tag | Source |
|-----|--------|
| `[F25]` | Fall 2025 Exam (file labeled "Winter 2024") |
| `[P1]` | Practice Exam 1 (Winter 2022) |
| `[IS]` | IntroSheet sample problems |
| `[HW]` | HW-related problems from IntroSheet |
| `[QP]` | Quiz Prep problems from Canvas |
| `[ADD]` | Additional problems (confidence interval, hypothesis testing) |

---

## Topics Covered (From IntroSheet)

- **Ch2** — Basic Probability & Counting: P(A|B), P(A∩B), P(A∪B), Bayes; permutations, combinations
- **Ch3 & Ch10** — Discrete RVs: E[X], Var[X], Covariance, Independence
- **Ch4** — Parametric Families: Uniform (discrete), Indicator, Geometric, Binomial, Poisson; R-functions
- **Ch7** — Continuous RVs: PDF, CDF, E[X], Var[X]; Uniform (cont.), Exponential, Normal; CLT; Z-table

> **CLT has been removed from the test** per the Canvas page note — but CLT-style questions still appeared in Fall 2025, so know it.

---

## Formula Quick Reference

### Discrete Families

| Family | PMF | E[X] | Var[X] |
|--------|-----|------|--------|
| Indicator(p) | P(X=1)=p, P(X=0)=1-p | p | p(1-p) |
| Geometric(p) | P(X=k) = (1-p)^(k-1) · p | 1/p | (1-p)/p² |
| Binomial(n,p) | C(n,k) · p^k · (1-p)^(n-k) | np | np(1-p) |
| Uniform Disc.(a,b) | 1/(b-a+1) for k∈[a,b] | (a+b)/2 | (b-a)(b-a+2)/12 |
| Poisson(λ) | e^(-λ)λ^k / k! | λ | λ |

### Continuous Families

| Family | PDF | E[X] | Var[X] |
|--------|-----|------|--------|
| Uniform(q,r) | 1/(r-q) | (q+r)/2 | (r-q)²/12 |
| Exponential(λ) | λe^(-λt), t>0 | 1/λ | 1/λ² |
| Normal(μ,σ²) | (1/√(2πσ))·e^(-0.5((t-μ)/σ)²) | μ | σ² |

### Key Formulas

```
Z-score:            Z = (X - μ) / σ
CLT (sum T):        T ~ N(n·m, n·v²),   Z = (T - n·m) / √(n·v²)
CLT (average X̄):   X̄ ~ N(m, v²/n),    Z = (X̄ - m) / √(v²/n)

Cov(X,Y)        = E[XY] - E[X]·E[Y]
Var(X+Y)        = Var(X) + Var(Y) + 2·Cov(X,Y)
Var(aX)         = a²·Var(X)
Cov(aX+bY, cU+dV) = ac·Cov(X,U) + ad·Cov(X,V) + bc·Cov(Y,U) + bd·Cov(Y,V)
If independent: Cov(X,Y) = 0, E[XY] = E[X]·E[Y]

Sample mean:     X̄ = (X₁+...+Xₙ)/n
Sample variance: s² = (1/n)·Σ(Xᵢ - X̄)²  [or divide by n-1 for unbiased]

CI for μ:  X̄ ± z_c · √(s²/n)
```

### R Functions Reference

```r
d<dist>(i)             # P(X = i)    — PMF
p<dist>(i)             # P(X ≤ i)   — CDF
q<dist>(q)             # Find c s.t. P(X ≤ c) = q   (quantile)
r<dist>(nreps, ...)    # Generate nreps random variates

# Examples:
dbinom(k, n, p)        # P(X=k), X~Binom(n,p)
pbinom(k, n, p)        # P(X≤k)
dgeom(k, p)            # Geometric
pnorm(z)               # P(Z≤z), standard normal
qnorm(0.95)            # z-value for 95th percentile
runif(n)               # Uniform random numbers (used in simulation)
```

---

---

# PROBLEM BANK

---

## TYPE 1 — Binomial Distribution

> Pattern: n trials, success probability p, count k successes.
> **When you see:** "probability that exactly k out of n…", "batch acceptance/rejection", "basketball players", "lottery", "casino wins"

---

### 1A — Batch Quality Control `[F25, IS]`

**`[F25]` Problem 1 (15 pts)**

A very large batch of electronic components has arrived at a distributor. The batch is acceptable only if the proportion of defective components is at most **(10+y)%**. The distributor randomly selects **(y+1)** components and rejects the batch only if the number of defective components is **2 or more**. What is the probability that the batch will be **rejected** when the actual proportion of defective is **(10+y)%?**

---

**`[IS]` Sample 1**

A very large batch of electronic components has arrived at a distributor. The batch is acceptable only if the proportion of defective components is at most **0.10**. The distributor randomly selects **10** components and **accepts** the batch only if the number of defective components is **at most 2**. What is the probability that the batch will be **accepted** when the actual proportion of defective is **0.10**?

> ⚠️ *Note the difference: F25 asks for P(reject), Sample 1 asks for P(accept). Also, F25 uses y-based parameters.*

---

### 1B — Basketball Players `[P1]`

**`[P1]` Problem 1 (25 pts)**

The probability of any person being a basketball player is **(10+y)%**.

a) What is the probability that out of **120 students** in ECS 132, exactly **20** are basketball players?

b) How many students in ECS 132 should you expect to be basketball players?

c) If X = number of basketball players in ECS 132, what is the **PMF** of X?

d) *(10 pts)* Given **1000 students**, what is the probability that **100+10y or more** are basketball players? Provide the **exact answer** (simplify) and show your work.

---

### 1C — Casino Wins + CLT `[F25]`

**`[F25]` Problem 6a (part of 30 pts)**

The probability of winning a casino game is **30%**. You sample **(100+y)** people. What is the probability that **more than 40** won? Do not simplify.

---

### 1D — Communication System `[IS]`

**`[IS]` Sample 3 (part)**

In a noisy communication system, each message is sent error-free with probability **0.86**, independent of all other messages.

a) What is the probability that in **10 transmissions**, exactly **8** will succeed?

---

### 1E — Lottery `[IS]`

**`[IS]` Problem 3 (15 pts)**

You are playing the lottery for **30 days** in a row, assuming each day is independent and your probability of winning on any given day is **p = 0.7**.

a) What is the probability that you will win on **exactly 5** of the 30 days?

b) What is the **expected value** of the number of times you will win in 30 days?

c) What is the **variance** of the number of times you will win in 30 days?

---

---

## TYPE 2 — Geometric Distribution

> Pattern: Keep rolling/trying until first success.
> **When you see:** "first time you win", "expected number of rolls to win", "win on the kth roll"

---

### 2A — Y-Sided Die `[F25]`

**`[F25]` Problem 2 (20 pts)**

You are rolling a fair **y-sided die** in a casino where you win **$100** if you roll a **1 or a 2**.

a) What is the probability that you will win on the **7th roll AND again on the 9th roll** (regardless of whether you won before the 7th or on the 8th roll)?

b) What is the **expected value of the number of rolls** it will take you to win?

c) What is the **expected value of the amount of money** you will win if you play **8 games**?

d) What is the **expected value of the amount of money** you will win if you play **(y+20) games**?

---

### 2B — Six-Sided Die `[IS]`

**`[IS]` Problem 2 (20 pts)**

You are rolling a fair **six-sided die** in a casino where you win **$100** if you roll a **3**.

a) What is the probability that you will win on the **10th roll** (regardless of whether you won before)?

b) What is the probability that the **first time you win is on the 11th roll**?

c) What is the **expected value of the number of rolls** it will take to win?

d) What is the **variance of the number of rolls** it will take to win?

> ⚠️ *2A vs 2B: F25 asks about winning on 7th AND 9th (two independent events); IS asks about first win on 11th (pure geometric). Different setups — know both.*

---

---

## TYPE 3 — Continuous Uniform Distribution

> Pattern: X ~ Unif(a, b), pdf = 1/(b-a), P(X=c) = 0 for continuous, P(X<c) = (c-a)/(b-a)

---

### 3A — Checkout Time `[F25]`

**`[F25]` Problem 3 (20 pts)**

The time a person takes at checkout is **continuously uniformly distributed between 1 and (20+y) minutes**. Let X be the checkout time.

a) What is **P(X = 9)**?

b) What is **P(X < 9)**?

c) You randomly selected **50 people** and collected their checkout times. What is the probability that the **average checkout time is more than 16**? Do not need to simplify.
*(Hint: Use CLT on the sample average.)*

---

### 3B — Target Store Visitors `[F25]`

**`[F25]` Problem 4 (20 pts)**

You are counting the number of people entering Target. Each day the count is **uniformly distributed between 10 and 80y** people.

a) What is the probability that the **total number of people in the last 7 days is less than (42 + 40y)**? No need to simplify.
*(Hint: Sum of 7 i.i.d. uniforms → CLT.)*

b) What is the **top 5%** of the total number of people entering for a **31-day month**? No need to simplify.
*(Hint: Find z s.t. P(Z > z) = 0.05, then invert.)*

---

### 3C — Computer Execution Time `[P1]`

**`[P1]` Problem 2 (25 pts)**

The execution time of a job in the Edison supercomputer is **X ~ Unif(12, 16+y) seconds**.

a) What is the probability that the computer executes **between 15 and 50 seconds**?

b) What is the probability that the computer executes **between 8 and 25 seconds**?

c) What is the probability that the computer executes **more than 12 seconds**?

d) **50% of jobs take more than x seconds.** What is x? *(Find the median.)*

e) Imagine all tasks now take **5 seconds longer**. What is the **new E[X]**?

---

### 3D — Computer Execution Time (Detailed) `[IS]`

**`[IS]` Problem 6 (30 pts)**

The execution time is **X ~ Unif(11, 20) seconds**.
*(Note: problem text says Unif(10,19) in one place and 11–20 in another — use the range that makes the problem consistent.)*

a) What is the probability that the computer executes **between 12 and 14 seconds**?

b) What is the probability that the computer executes **between 8 and 9.5 seconds**?

c) What is the probability that the computer executes **in exactly 13 seconds**?

d) What is the probability that the computer executes **more than 20 seconds**?

e) All tasks now take **5 seconds longer** — what is the **new E[X]**?

f) What is the **new Var[X]**?

h) What is the **new PMF**?

---

### 3E — Uniform Probability Computations `[IS]` Sample 3

**`[IS]` Sample 3 (part)**

The execution time is **X ~ Unif(10, 25)**.

a) What is **P(X < 15)**?

b) What is **P(13 ≤ X ≤ 21)**?

---

---

## TYPE 4 — Custom / Non-Standard PDF

> Pattern: Given a PDF formula f(t), integrate to find E[X], Var[X], and probabilities.

---

### 4A — PDF f(t) = t/20 `[F25]`

**`[F25]` Problem 5 (10 pts)**

The PDF for a continuous random variable X is:

```
f_X(t) = t/20,   for t ∈ (4+y, 10+y),   and 0 otherwise.
```

a) What is **E[X]**?

b) What is **Var[X]**?

c) What is **P(5.01+y < X < 7+y)**?

> ⚠️ *The official solution notes this PDF is technically ill-defined (doesn't integrate to 1) but the exam treats it as valid. Proceed anyway.*

---

---

## TYPE 5 — Normal Distribution & CLT

> Pattern: Either X is given as Normal, or CLT is applied to sums/averages of i.i.d. RVs.

---

### 5A — Network Flow (Pure Normal) `[P1]`

**`[P1]` Problem 6 (20 pts)**

Network flow size is normally distributed: **X ~ N(μ=55 GB, σ=2.5 GB)**. Flows less than **52 GB** are classified as "slow". **Simplify** using the z-table.

a) What **percentage of flows** are less than 52 GB? *(i.e., P(X < 52))*

b) What **percentage of slow flows** are less than 49 GB? *(i.e., P(X < 49 | X < 52))*

c) What **percentage of slow flows** are more than **75 GB**? *(i.e., P(X > 75 | X < 52))*

d) What **percentage of slow flows** are more than **55 GB**? *(i.e., P(X > 55 | X < 52))*

---

### 5B — Store Visitors (Normal, Individual + CLT) `[QP]`

**`[QP]` Quiz 5 Prep, Question 2**

The number of people entering a store is **normally distributed with μ=70 and σ=16**.

a) What is the probability that **60 or fewer people** enter the store **today**?

b) What is the probability that the **total number** of people over **30 days** is **(60 × 30) or less**?

c) What is the probability that the **average** number of people over **30 days** is **less than 60**?

---

### 5C — Baby Weight Difference (Two Samples, CLT) `[F25]`

**`[F25]` Problem 6b (part of 30 pts)**

- US babies: **E[weight] = 20 lbs**, **std dev = (Y+1) lbs**, sample size **n = 30**
- Canadian babies: **E[weight] = (20 + Y/100) lbs**, **std dev = Y lbs**, sample size **n = 20**

What is the probability that the **difference in sample averages** (US minus Canada) is **more than 2 lbs**? Do NOT simplify.

*(Hint: D = W_US - W_CA. Find μ_D and σ²_D, then use CLT.)*

---

### 5D — Casino CLT (Binomial → Normal Approximation) `[F25]`

*(See Type 1C — Problem 6a above. Same problem, same technique.)*

---

### 5E — CLT Simulation `[QP]`

**`[QP]` Quiz 5 Prep, Question 1**

You play **100 games** with probability of winning a single game **p = 0.3**. Let X = total games you win.

a) What is **E[X]**?

b) **Simulate Var[X] using R** — you may only use `runif()` and `sample()`. You cannot use built-in expected value or variance of an array.

---

---

## TYPE 6 — Exponential Distribution

---

### 6A — Web Server Arrivals `[IS]`

**`[IS]` Sample 7**

Let X = time between two successive arrivals of requests at a web server. X has an **exponential distribution with λ = 1**.

a) What is the **expected time** between two successive arrivals?

b) What is the **standard deviation** of the time between successive arrivals?

c) What is **P(X < 4)**?

d) What is **P(2 < X < 5)**?

---

---

## TYPE 7 — Sample Mean, Variance, and Confidence Intervals

---

### 7A — Salary Sample Statistics `[F25]`

**`[F25]` Problem 6c (part of 30 pts)**

You sample salaries of 4 random graduates: **44, 88, (120+y), and 0** dollars per hour.

a) What is the **sample mean**?

b) What is the **sample standard deviation**?

---

### 7B — Baby Weight Confidence Interval `[ADD]`

**`[ADD]` Additional Problem**

You sample **4 babies** with replacement. Their weights are: **42, 43, 44, (44+y) lbs**.

Find the **((67+y)%)** confidence interval for the **population average weight**.
Show your work or the equation you use. No need to simplify; just use the z-table.

*(Hint: μ ∈ X̄ ± z_c · √(s²/n), where z_c corresponds to the confidence level.)*

---

### 7C — Sample Mean & Variance Simulation `[QP]`

**`[QP]` Quiz 5 Prep, Question 3**

Randomly generate **10 numbers**. Find the **sample mean and variance**.

---

---

## TYPE 8 — Coin Toss Game (Consecutive Heads)

> Pattern: Toss a fair coin until r consecutive heads OR s total tosses (whichever comes first). X = number of tosses. Find E[X] = minimum fair fee.
> **This appears on EVERY exam and practice. Know it cold.**
> **Key insight:** Enumerate all possible values of X and their probabilities, then compute E[X].

---

### 8A — r = y, s = 2+y `[F25]`

**`[F25]` Problem 7 (15 pts)**

r = **y** consecutive heads, s = **(2+y)** total tosses.

a) Find the **minimum fee** that should be charged for this game. No need to simplify.
*(Hint: X can only take values y, y+1, …, 2+y. List each and find its probability.)*

b) Find **E[X²]** via **simulation in R**. You may NOT use the built-in function of expected value of an array of numbers.

---

### 8B — r = 3, s = 6 `[P1, IS]`

**`[P1]` Problem 3 (15 pts) / `[IS]` Problem 2**

*(Same problem appears in both sources.)*

r = **3** consecutive heads, s = **6** total tosses.

Find the **minimum fee** that should be charged for this game.
*(X can be 3, 4, 5, or 6. Enumerate all paths.)*

---

### 8C — r = 8, s = 10 `[F25 Solution]`

*(This is the solved version from the y=8 solution key, for reference when checking your y=8 work.)*

r = **8**, s = **10**. Possible X values: 8, 9, 10.

- P(X=8) = (0.5)^8
- P(X=9) = 0.5 · (0.5)^8
- P(X=10) = 1 − P(X=8) − P(X=9)

E[X] = 8·P(X=8) + 9·P(X=9) + 10·P(X=10)

---

---

## TYPE 9 — Covariance Problems

> Pattern: Two random variables with some dependency structure. Compute Cov(X,Y) = E[XY] - E[X]·E[Y].
> **When you see:** joint distributions, dice + coin, conditional coins.

---

### 9A — Two Coins Sequentially `[P1]`

**`[P1]` Problem 4 (32 pts)**

Flip 2 coins sequentially.
- Coin 1 is **fair**.
- If Coin 1 = **Heads** → Coin 2 is **fair**.
- If Coin 1 = **Tails** → Coin 2 is **unfair** with P(Tails) = **y/100**.

Let **X** = indicator(Heads on Coin 1), **Z** = indicator(Heads on Coin 2).

a) Compute **COV(X, Z)**

b) Compute **VAR[X]**

c) Compute **COV(X, 4X)**

---

### 9B — Carnival Dice + Coin (Two Versions) `[IS]`

**`[IS]` Problem 7 (30 pts)**

Roll a **3-sided die**:
- P(1) = 1/3, P(2) = 1/6, P(3) = 1/2

If you land on face **2** → flip a **fair coin**.  
Else → flip an **unfair coin** with P(Heads) = **0.2**.

Let **X** = number of dots on die, **Y** = indicator(Heads).

a) Compute **COV(X, Y)**

b) Now assume **no matter what face** you land on, you always get the **unfair coin**. Compute **COV(X, Y)**.

---

### 9C — Dice + Conditional Coin (y-based) `[IS, HW]`

**`[IS]` Simulation Problem**

Roll a fair **3-sided die** with sample space **{y, y+1, y+2}** (equal probability 1/3 each).

- If you roll **y** → flip a **fair coin**.
- Else → flip an **unfair coin** with P(Heads) = **(y+15)/100**.

Let **X** = number of dots on die, **Y** = indicator(Tails).

a) Find **COV(X, Y) analytically**.

b) Provide **R code** to simulate and verify COV(X, Y).

> ⚠️ *Substitute your y value before solving. Don't solve with y as a variable.*

---

---

## TYPE 10 — Bus Ridership Problem

> Pattern: People board and exit at stops. Analyze L₁, L₂, L₃ (riders after each stop). Requires tracking joint distributions across stops.
> **This is the big recurring HW-style problem. Different versions change the boarding probabilities.**

---

### The Setup (Standard)

At each stop, some people **exit** (each independently with probability p_off), then some people **board**. Define:
- **Lᵢ** = number of passengers **after** boarding at stop i
- **Mᵢ** = number of passengers **before** boarding at stop i (after exit)

---

### 10A — Modified Boarding: {0 or 2}, with E[L₂·L₁] `[F25]`

**`[F25]` Problem 8 (25 pts)**

At every stop, either **0 or 2** people board:
- P(0 board) = **(25−y)%**
- P(2 board) = **(74+y)%**
- P(any person exits) = **(12+y)%**
- Start: **L₀ = 0**

**Part a)** Find **E[L₂ · L₁]** analytically. You may use column notation and do not need to simplify.

*(Hint: Build the joint distribution of (L₁, L₂) by tracing all paths through the Markov graph.)*

**Part b)** Show how you would **simulate and calculate E[L₂ − L₃]** and **P(L₃ = 2)** using R. You may augment the reference code provided.

---

### 10B — Modified Boarding: {0 or 2}, Var(L₂−L₁) `[P1]`

**`[P1]` Problem 5 (20 pts)**

At every stop, either **1 or 2** people board:
- P(1 boards) = **(30−y)%**
- P(2 board) = **(70+y)%**
- P(any person exits) = **(2+y)%**

Find **Var(L₂ − L₁)** analytically.

Also: how would you **update the R simulation code** to test your analytical result?

---

### 10C — Modified Boarding: {0 or 2}, E, COV, VAR `[HW]`

**`[HW]` HW-Related Problem**

At every stop, either **0 or 2** people board:
- P(0 board) = **0.4**, P(2 board) = **0.6**
- P(any person exits) = **0.2**

Find:
- **E(L₂ − L₁)**
- **COV[L₁, L₂]**
- **VAR[2·L₂]**

---

### 10D — R Simulation Template (Bus)

```r
bus_step <- function(num_passengers, p_leave, p_new) {
  # passengers leave
  if (num_passengers > 0) {
    for (i in 1:num_passengers) {
      if (runif(1) < p_leave) {
        num_passengers <- num_passengers - 1
      }
    }
  }
  # new passengers board: p_new is a probability vector
  # e.g. c(0.17, 0, 0.83) means P(0 board)=0.17, P(1)=0, P(2)=0.83
  num_new <- sample(c(0:(length(p_new)-1)), 1, prob = p_new)
  num_passengers + num_new
}

sim_bus <- function(num_iter = 100000) {
  p_leave <- (12 + y) / 100               # adjust for your y
  p_new   <- c((25-y)/100, 0, (75+y)/100) # adjust for your y

  sum_diff <- 0
  count_l3_eq_2 <- 0

  for (i in 1:num_iter) {
    l <- 0
    l1 <- bus_step(l,  p_leave, p_new)
    l2 <- bus_step(l1, p_leave, p_new)
    l3 <- bus_step(l2, p_leave, p_new)

    sum_diff <- sum_diff + (l2 - l3)
    count_l3_eq_2 <- count_l3_eq_2 + (l3 == 2)
  }

  cat("E[L2 - L3] =", sum_diff / num_iter, "\n")
  cat("P(L3 = 2)  =", count_l3_eq_2 / num_iter, "\n")
}
```

---

---

## TYPE 11 — Hypothesis Testing

> Pattern: Two-sample test comparing means. Compute Z-score, compare to critical value z_c from table.
> **When you see:** "reject H₀", "significance level α", "test whether..."

---

### 11A — Smoking vs Non-Smoking Lung Index `[ADD]`

**`[ADD]` Problem 8 (20 pts)**

Examining whether to reject H₀: lung destructive index is the same for smokers and non-smokers.

Sample statistics:
- **Non-smokers (a):** x̄_a = (17+y), s²_a = 4, n_a = **9**
- **Smokers (b):** x̄_b = (16+y), s²_b = 4.5, n_b = **16**

You **do** need to get applicable values from the z-table. Don't forget the P-value.

**Part a) (10 pts)**  
H₀: μ_a − μ_b = 0 (no difference)  
Test H_alt: **μ_a ≠ μ_b** (two-tailed) with **α = (4+y)/100**.

**Part b)**  
Test H_alt: **μ_a − μ_b > 0** (one-tailed) with **α = (4+y)/100**.

*(Hint: Z = (x̄_a − x̄_b − 0) / √(s²_a/n_a + s²_b/n_b). Compare |Z| to z_{α/2} for two-tailed, Z to z_α for one-tailed.)*

---

---

## TYPE 12 — R Coding / Simulation Problems

---

### 12A — E[X²] via Simulation for Coin Game `[F25]`

**`[F25]` Problem 7b**

Find **E[X²]** via simulation in R for the coin toss game (r=y, s=2+y). You **may not** use the built-in function for expected value of an array of numbers.

*(Hint: Run many iterations, accumulate x² values manually, divide by num_iter at the end.)*

---

### 12B — Simulate Var[X] for Binomial `[QP]`

**`[QP]` Quiz 5 Prep, Q1b**

100 games, p=0.3. **Simulate Var[X]** using only `runif()` and `sample()`. Cannot use `var()` or `mean()` on an array.

---

### 12C — Custom Dice Functions `[QP]`

**`[QP]` Quiz 6 Prep, Problem 3**

Create **DDice, PDice, QDice, RDice** — analogous to d/p/q/r distribution functions — where X is the random variable for a **six-sided die**.

---

### 12D — Simulate Cov(X,Y) for Dice + Coin `[IS]`

**`[IS]` Simulation Problem**

After finding COV(X,Y) analytically (Problem 9C above), provide **R code to simulate** and verify the result.

---

---

## Repeated / Overlapping Questions (Quick Reference)

| Problem Type | Sources | Notes |
|---|---|---|
| Batch quality (binomial) | F25 P1, IS Sample 1 | F25 = reject, IS = accept; different n |
| Coin toss game (r consec. heads) | F25 P7, P1 P3, IS | F25 uses y-based r/s; P1+IS use r=3,s=6 |
| Bus ridership simulation | F25 P8, P1 P5, HW | Different boarding probs each time |
| Geometric die | F25 P2, IS P2 | F25 = y-sided, IS = 6-sided |
| Covariance (dice+coin) | P1 P4, IS P7, IS Simulation | All different setups but same method |
| Uniform execution time | P1 P2, IS P6, IS S3 | Different ranges; all same technique |
| CLT on sample average | F25 P3c, F25 P4, QP Q5 Q2 | All normal approx via CLT |
| Normal distribution (z-table) | P1 P6, QP Q5 Q2 | Straightforward z-score lookup |

---

---

## Common Pitfall Checklist

- [ ] **Substitute y before starting** — never solve symbolically.
- [ ] **P(X = c) = 0 for continuous** random variables. Don't forget this.
- [ ] **CLT:** Use for sums/averages of many i.i.d. RVs. Know when T is a sum vs X̄ is an average.
- [ ] **Geometric vs Binomial:** Geometric = first success; Binomial = exactly k successes in n trials.
- [ ] **Covariance:** If X, Y independent → Cov = 0. But Cov = 0 does NOT mean independent.
- [ ] **Conditional probability** in normal: P(A|B) = P(A∩B)/P(B). If A and B are disjoint → 0.
- [ ] **Bus problem:** Track L (after boarding), not M (before boarding). Build the full probability tree.
- [ ] **Coin game:** Enumerate ALL valid sequences for each X value. Use complement where helpful.
- [ ] **R simulation:** Don't use built-in `mean()` or `var()` unless told you can. Accumulate sums manually.
- [ ] **Confidence interval z_c:** 95% CI uses z_c = 1.96; 90% uses 1.645; for (67+y)%, find the right z.
- [ ] **Hypothesis testing:** Two-tailed uses α/2 on each side; one-tailed uses full α on one side.

---

*Last compiled: ECS 132 Fall 2025 Midterm 2 prep. Good luck!*