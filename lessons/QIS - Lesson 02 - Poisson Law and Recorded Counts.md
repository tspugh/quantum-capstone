# Lesson 02 — Poisson Law and Recorded Counts

- Status: core work recorded; final notebook polish pending.
- Core duration: 75 minutes originally planned; approximately three hours including documentation and the interactive extension.
- Notebook: [Poisson distribution](../projects/00-foundations/00-02-poisson-distribution.ipynb)
- Prerequisites: basic Python and discrete probabilities; no new quantum gates are required.

## Objective

Derive the complete probability mass function of

$$
N\sim\operatorname{Poi}(2),
\qquad
R=N\mathbf 1_{\{N\le4\}},
$$

verify that it is normalized, and compute $E[R]$ exactly before using SciPy, NumPy, and Matplotlib to check and visualize the result.

By the end of the block, you should be able to explain why $R=0$ has more than one preimage, why this is not a conditioned or renormalized Poisson distribution, and how a probability library represents the same law you derived by hand. A small empirical comparison supplies the classical baseline for Unit 0's quantum circuit; a later estimation extension can investigate Monte Carlo convergence.

> **Required reading and stopping points**
>
> The original lesson uses *A Short Course in Basic Probability*, pp. 27–28 (Definition 4.16 through discrete expectation), p. 46 (Poisson PMF, moments, and CDF), and the counting-model box on p. 47. Those course notes are not redistributed here.
>
> Public alternative covering the same core definitions:
>
> 1. [MIT 18.600, Lecture 9: Expectations of discrete random variables](https://ocw.mit.edu/courses/18-600-probability-and-random-variables-fall-2019/resources/mit18_600f19_lec9/) — read the definition of discrete expectation; defer the remaining examples and extensions.
> 2. [SciPy's Poisson reference](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.poisson.html) — read **Notes** for the PMF and parameter range, and inspect `mean` and `var` in **Methods**. Save the code examples for the Python block below.
>
> A Poisson variable counts events in a fixed interval or region. For parameter $\lambda$, its mean and variance are both $\lambda$. Keep a count distinct from a waiting time.
>
> Skip geometric distributions, the binomial-limit derivation, and Poisson-process theory in this session.

> **IBM course position**
> Do not advance the IBM course during this lesson. IBM Lesson 1 is complete for the current track; IBM Lesson 2 resumes in QIS Lesson 03 under the revised Unit 0 map. This block is deliberately probability-only.

## The 75-minute block

This is the original core outline, not an elapsed-time log. The completed exploration also developed reusable functions and an interactive parameter experiment; those additions are recorded in Session notes rather than added as prerequisites.

### 0–8 minutes — retrieve before reading

Without opening the sources, write brief answers:

> **Initial retrieval**
>
> 1. What kind of quantity does a Poisson random variable model?
> 2. What is the support of $N\sim\operatorname{Poi}(2)$?
> 3. What does the parameter $2$ mean?
> 4. What is the difference between a random variable, its law, and its probability mass function?

Do not correct the answers yet. Preserve the first attempt in the notebook so later revisions remain visible.

### 8–18 minutes — read the probability material

Read the sections specified under Required reading. Then write, in your own notation,

$$
p_k=P(N=k)=e^{-2}\frac{2^k}{k!},\qquad k=0,1,2,\ldots,
$$

and one sentence interpreting $N$, its support, and the parameter $2$.

> **First exact probabilities**
> Calculate $p_0,p_1,p_2,p_3,p_4$ without Python. Keep the common factor $e^{-2}$ visible before using decimal approximations.

### 18–33 minutes — transform the law through preimages

Define the reward function explicitly:

$$
g(n)=n\mathbf1_{\{n\le4\}},\qquad R=g(N).
$$

Start from the preimage of each output value. Complete this table on paper or in a Markdown cell before checking the solution:

| Reward value $r$ | Event $\{R=r\}$ written using $N$ | $q_r=P(R=r)$ |
|---:|---|---|
| 0 |  |  |
| 1 |  |  |
| 2 |  |  |
| 3 |  |  |
| 4 |  |  |

> **The essential transformation question**
> Why is $P(R=0)$ not merely $P(N=0)$? Name every part of the preimage $g^{-1}(\{0\})$.

Your result should have the form

$$
q_0=1-\sum_{k=1}^{4}p_k,
\qquad
q_k=p_k\quad(k=1,2,3,4).
$$

Verify $\sum_{k=0}^4q_k=1$. Do not renormalize $p_0,\ldots,p_4$: conditioning on $N\le4$ would define a different random variable and discard the upper-tail outcomes rather than mapping them to reward zero.

### 33–45 minutes — compute the exact expectation

Use the transformed law, not a simulation:

$$
E[R]=\sum_{r=0}^{4}r q_r.
$$

Simplify the expression symbolically until it has the form $c e^{-2}$ for a rational number $c$. Then calculate a decimal value to four places.

> **Check only after completing the derivation**
> Since the zero term vanishes and $q_k=p_k$ for $1\le k\le4$,
> $$
> \begin{aligned}
> E[R]
> &=\sum_{k=1}^{4}k e^{-2}\frac{2^k}{k!}\\
> &=2e^{-2}\sum_{j=0}^{3}\frac{2^j}{j!}\\
> &=\frac{38}{3}e^{-2}\\
> &\approx1.7142.
> \end{aligned}
> $$

### 45–50 minutes — add the scientific Python tools

Continue in the [Poisson notebook](../projects/00-foundations/00-02-poisson-distribution.ipynb); the mathematical derivation should appear before the code that verifies it. From the repository root, add the libraries as direct project dependencies:

```bash
uv add numpy scipy matplotlib
```

This updates `pyproject.toml` and `uv.lock`. Ensure Jupyter uses the project's environment, then restart the kernel if needed. Existing installations can subsequently use `uv sync --locked`.

> **External library references**
> Keep these official pages open and use them instead of guessing method names:
>
> - [SciPy: `scipy.stats.poisson`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.poisson.html) — read the PMF definition and scan the `pmf`, `cdf`, `sf`, `mean`, and `var` methods.
> - [NumPy: random `Generator`](https://numpy.org/doc/stable/reference/random/generator.html) — find `default_rng` and `Generator.poisson`.
> - [Matplotlib: `pyplot.bar`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.bar.html) — use this only as a plotting reference.

### 50–59 minutes — represent the exact law with SciPy

SciPy uses `mu` for the Poisson parameter that we have called $\lambda$. Create a frozen distribution and inspect it:

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import poisson

lam = 2.0
poisson_model = poisson(mu=lam)

poisson_model.support(), poisson_model.mean(), poisson_model.var()
```

Before proceeding, predict the three outputs. Then calculate the first five probabilities:

```python
n_values = np.arange(5)
p_0_to_4 = poisson_model.pmf(n_values)
p_0_to_4
```

> **Read the library mathematically**
>
> 1. Which mathematical object does `poisson_model` represent?
> 2. Why does `poisson_model.pmf(n_values)` return an array?
> 3. What event is computed by `poisson_model.sf(4)`? Check the documentation: `sf` means survival function.

Now construct the exact reward law. The `...` entries are exercise placeholders: replace both before running this cell. Fill in the two missing expressions before checking earlier results:

```python
r_values = np.arange(5)
q_exact = poisson_model.pmf(r_values)

# Every count above 4 is also mapped to reward zero.
q_exact[0] += ...

assert np.isclose(q_exact.sum(), 1.0)

expected_reward = ...
q_exact, expected_reward
```

The first blank should express $P(N>4)$ using SciPy, and the second should compute $\sum_r r q_r$ as an array operation. Confirm that the result agrees with your handwritten values, including

$$
E[R]=\frac{38}{3}e^{-2}\approx1.7142.
$$

Do not use `poisson_model.pmf(r_values) / poisson_model.cdf(4)`. That would construct the conditional distribution $N\mid N\le4$, not the reward law.

### 59–69 minutes — preview sampling with NumPy

Use a named random-number generator with a fixed seed so the public notebook is reproducible:

```python
rng = np.random.default_rng(20260917)
sample_size = 2_000

n_samples = rng.poisson(lam=lam, size=sample_size)
r_samples = np.where(n_samples <= 4, n_samples, 0)

q_empirical = np.bincount(r_samples, minlength=5) / sample_size
empirical_reward = r_samples.mean()

q_empirical, empirical_reward
```

Explain each transformation in a Markdown cell. In particular, say why every sampled count greater than four becomes zero and why `np.bincount(..., minlength=5)` has one position for each possible reward.

Then compare the exact and empirical laws:

```python
width = 0.38
fig, ax = plt.subplots(figsize=(8, 4.5))

ax.bar(r_values - width / 2, q_exact, width, label="Exact law")
ax.bar(r_values + width / 2, q_empirical, width, label="Empirical law")

ax.set(
    xlabel="Reward r",
    ylabel="Probability",
    title=f"Poisson reward law: exact vs. {sample_size:,} samples",
    xticks=r_values,
)
ax.legend()
fig.tight_layout()
```

> **Interpret the figure**
> Why do the bar heights differ slightly? Which difference is most visually noticeable with this seed? Would another seed produce exactly the same empirical bars? Do not merely say “randomness”; connect the discrepancy to finite sampling.

This is only a simulation preview. Do not yet run many sample sizes or make a log-log error plot; the revised course map places those experiments in a later estimation extension.

### 69–75 minutes — explain the model and library choices

> **Exit questions**
>
> 1. State the support of $N$ and the support of $R$.
> 2. Why is $q_0>p_0$?
> 3. What problem would renormalizing $p_0,\ldots,p_4$ solve, and why is it not this problem?
> 4. What roles did SciPy and NumPy play, and why did neither replace the derivation?
> 5. In one sentence, what does the classical Monte Carlo estimate target, and what exact value should it approach?

Answer in five to eight sentences without reading the code.

> **Evidence required to complete Lesson 02**
>
> - The initial retrieval answers are preserved before correction.
> - The preimage table gives the complete law of $R$ and the probabilities sum to one.
> - The exact derivation reaches $E[R]=\frac{38}{3}e^{-2}\approx1.7142$.
> - SciPy independently verifies the PMF and expectation without replacing the derivation.
> - A reproducible NumPy sample is transformed into rewards and compared with the exact law in a labeled Matplotlib figure.
> - The exit explanation distinguishes transformation from conditioning or truncation.

> **Optional, outside the 75-minute block**
> Use `poisson_model.expect(...)` to evaluate the reward expectation directly, then explain what function SciPy is integrating or summing. Alternatively, derive $E[R]$ a second way from $E[N\mathbf1_{\{N\le4\}}]=2P(N\le3)$, or compute $E[R^2]$ and $\operatorname{Var}(R)$ in preparation for Monte Carlo error analysis. Leave the convergence study for the later estimation extension.

## Session notes

- **Generalization:** the notebook separates exact-law evaluation, sampled experiments, and frequency/mean summaries into functions. Exact and empirical mean markers make the expectation visible alongside the probability bars.
- **Interactive exploration:** controls vary the Poisson parameter, reward cutoff, and sample count. This is an optional extension; future lessons reuse the documentation-reading and validation habits without requiring another plotting or widgets tutorial.
- **Reproducibility:** retain a recorded-seed baseline separately from the interactive plot, which deliberately uses `seed=None` for new samples. Pseudorandom generation is deterministic given its initial state, implementation, and call sequence; reproducing it does not require Bayesian inference. Keep dependency versions as well as seeds.
- **Numerical stability:** the exact identities `1 - sum(p[1:c+1])` and `poisson.pmf(0, mu=lam) + poisson.sf(c, mu=lam)` represent the same zero-reward probability. At `lam=100, c=200`, the subtractive form produced a value near `-6.13e-14` in a review check, whereas the additive form gave about `4.63e-19`. Prefer the latter for wide parameter ranges and report floating-point tolerances. This note records a recommended improvement; it does not claim that the notebook was edited.

- **Transformation versus conditioning:** counts above four remain part of the experiment and contribute to reward zero. Renormalizing the first five Poisson probabilities changes the target law.
- **Exact law versus empirical frequencies:** SciPy evaluates the theoretical probabilities numerically; NumPy generates finite samples. Their discrepancy is sampling variation.
- **Notation:** use `\mathbb{R}` and `\mathbb{Z}_{\ge 0}` in Markdown mathematics. The discrete support includes zero.

> **Note of interest — return in a later lesson**
>
> Moments can be understood as expectations of polynomial functions of a random variable: $E[p(X)]$ is determined by the moments appearing in the polynomial $p$. For Gaussian variables, the corresponding natural orthogonal polynomial family is the Hermite polynomials. This is a worthwhile bridge from probability to quantum mechanics: in the quantum harmonic oscillator, a Gaussian is the ground-state wavefunction and the excited-state wavefunctions are Hermite polynomials multiplied by that Gaussian. Return to this connection when studying Schrödinger systems, Hamiltonians, and Hermitian (self-adjoint) operators; it is intentionally outside the scope of the present Poisson lesson.

For that later investigation, distinguish the wavefunction from its squared-magnitude position density, the physicists' and probabilists' Hermite scalings, and Hermite polynomials from Hermitian operators. The polynomial-expectation statement assumes the required moments exist. [Michael Fowler's harmonic-oscillator lecture](https://galileo.phys.virginia.edu/classes/751.mf1i.fall02/SimpleHarmOsc.htm) is a possible external starting point, not required reading for Lesson 03.

## Next lesson

[Lesson 03 — Qubits, Rotations, and a Biased Coin](<QIS - Lesson 03 - Qubits, Rotations, and a Biased Coin.md>) resumes IBM's quantum-mechanics basics. Part A constructs a chosen Bernoulli bias; Part B absorbs the former Lesson 04 four-outcome circuit into the same notebook using controlled rotations. Pause after A if needed. The old Lesson 04 slot is optional consolidation, not an extra requirement. Unit 0 still culminates in Poisson sampling and expectation encoding together in Lesson 05. A more extensive Monte Carlo error study is a later extension.

See the [course map](<QIS - Integrated Quantum Algorithms and Probability Course.md>) and [Lesson 01](<QIS - Lesson 01 - Workspace and First Quantum Circuit.md>).

## Sources

- *A Short Course in Basic Probability*, pp. 27–28 and 46–47 — original course reading, not redistributed.
- [MIT 18.600: Expectations of discrete random variables](https://ocw.mit.edu/courses/18-600-probability-and-random-variables-fall-2019/resources/mit18_600f19_lec9/) — public expectation reference.
- [SciPy: Poisson distribution](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.poisson.html).
- [NumPy: random Generator](https://numpy.org/doc/stable/reference/random/generator.html).
- [Matplotlib: bar charts](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.bar.html).

## Provenance

This lesson plan and its code scaffolds are AI-assisted, consistent with the repository's tutoring methodology. The notebook is learner-authored. The saved work includes the derivation, numerical experiment, and interactive extension; selected numerical cells were checked during review. These checks do not constitute a full widget-interface test, and the lesson plan does not substitute for the notebook's results.
