# Integrated Quantum Algorithms and Probability Course

## Purpose

This course provides the learning path for this repository. It combines probability, classical simulation, quantum circuits, and quantum algorithms in a sequence of reproducible notebooks and small programs.

The central project is a comparison of exact calculation, classical Monte Carlo, quantum probability encoding, and amplitude-estimation methods for a Poisson reward. Probability topics are introduced where they support the implementation, analysis, or validation of that project.

## Learning outcomes

By the end of the course, the repository should demonstrate how to:

- build, simulate, and measure quantum circuits with Qiskit;
- work with discrete probability laws, transformations, expectations, and sampling error;
- calculate a target expectation exactly and estimate it with classical Monte Carlo;
- encode a finite probability distribution in quantum-state amplitudes;
- encode a bounded reward as a measurement probability;
- explain the connection between Grover-style amplitude amplification and amplitude estimation;
- compare exact, classical, and quantum results using explicit correctness checks; and
- package the work as a reproducible capstone under `projects/`.

## Core project: Poisson reward estimation

Let

$$
N \sim \operatorname{Poisson}(2)
$$

and define the bounded reward

$$
R = N\mathbf{1}_{\{N\leq 4\}}.
$$

The project estimates $\mathbb{E}[R]$ in several ways:

1. derive the distribution and expectation exactly;
2. estimate the expectation with classical Monte Carlo;
3. encode the reward distribution in a quantum state and estimate it from shots; and
4. apply a small amplitude-estimation example after the baseline circuit is verified.

For a finite quantum register, the reward distribution can be represented directly as

$$
q_k = \Pr(R=k), \qquad k\in\{0,1,2,3,4\}.
$$

The zero-reward probability must include both $N=0$ and the upper tail $N\geq 5$:

$$
q_0 = \Pr(N=0)+\Pr(N\geq 5),
\qquad
q_k = \Pr(N=k) \text{ for } k=1,\ldots,4.
$$

After preparing $\sum_k\sqrt{q_k}\lvert k\rangle$, a reward qubit can be rotated so that its conditional probability of measuring one is $k/4$. The resulting circuit should satisfy

$$
\Pr(\text{reward qubit}=1)=\frac{\mathbb{E}[R]}{4}.
$$

This identity is the main bridge between the probability model and the quantum algorithm.

## Course sequence

| Lesson | Duration | Topic | Repository outcome |
|---:|---:|---|---|
| 1 | 30–60 min | Qiskit setup and a first circuit | A locally simulated Bell circuit with measured counts |
| 2 | 60 min | Discrete laws and the Poisson reward | An exact PMF, normalization check, and calculation of $\mathbb{E}[R]$ |
| 3 | 60–90 min | Classical Monte Carlo | A seeded simulation that reports estimate, error, and convergence data |
| 4 | 60 min | Qubits, measurement, and rotations | A biased-coin circuit using $R_y$ and comparison with a Bernoulli model |
| 5 | 60–90 min | Multi-qubit states and finite encodings | A verified amplitude encoding of $(q_0,\ldots,q_4)$ |
| 6 | 90 min | Reward encoding | A controlled-rotation circuit satisfying $\Pr(1)=\mathbb{E}[R]/4$ |
| 7 | 60–90 min | Joint laws and validation | Marginal and conditional checks derived from simulated circuit results |
| 8 | 60–90 min | Integrated baseline | Exact, Monte Carlo, and quantum-shot estimates exposed through one workflow |
| 9 | 60 min | Sampling error and resource budgets | A comparison of repetitions, shots, RMSE, and circuit cost |
| 10 | 60–90 min | Grover search and amplitude amplification | A small search circuit and a derivation of its success probability |
| 11 | 90 min | Amplitude estimation | A minimal amplitude-estimation reproduction on a known one-qubit problem |
| 12 | 90 min | Capstone integration | Amplitude estimation applied to the Poisson reward circuit, where practical |
| 13 | 60 min | Reproducibility and final report | A clean run, saved results, documented assumptions, and a concise comparison |

Lessons may be split when an implementation needs more time, but each lesson should retain one testable objective.

## Probability coverage

The probability strand is organized around concepts used by the capstone:

- probability mass functions and normalization;
- cumulative distribution functions and inverse-CDF sampling;
- transformations of discrete random variables;
- expectation, variance, and indicator notation;
- joint, marginal, and conditional distributions;
- independence;
- empirical distributions and Monte Carlo estimators; and
- bias, variance, standard error, and root-mean-square error.

Continuous densities and common continuous distributions may be added as separate lessons, but they are not prerequisites for the Poisson reward baseline.

## Quantum-computing coverage

The quantum strand progresses through:

- state vectors, basis measurements, and shot counts;
- single-qubit gates and rotations;
- tensor products, entanglement, and multi-qubit circuits;
- state preparation for a known finite distribution;
- controlled rotations for bounded functions;
- uncomputation where required by an algorithm;
- Grover search and amplitude amplification; and
- a simulator-scale amplitude-estimation method.

Reversible arithmetic is supporting material. It should be introduced only if the selected implementation requires a computed reward oracle rather than a direct finite-state encoding.

## Repository conventions

- `lessons/` contains the course outline and lesson-level notes or notebooks.
- `projects/` contains the integrated Poisson reward capstone.
- Completed examples should run from a clean environment with documented dependencies.
- Randomized experiments should use recorded seeds unless fresh randomness is the subject of the lesson.
- Exact values, simulated probabilities, and shot-based estimates should be kept distinct in output and discussion.
- Numerical comparisons should state their tolerance or uncertainty rather than relying on visual agreement.

Each lesson artifact should include:

1. an objective;
2. the required definitions or derivation;
3. an implementation;
4. at least one correctness check; and
5. a short result summary.

## Reference material

- [IBM: Use a quantum computer today](https://quantum.cloud.ibm.com/learning/en/courses/use-a-qc-today) for the initial Qiskit workflow.
- [IBM: Basics of quantum information](https://quantum.cloud.ibm.com/learning/en/courses/basics-of-quantum-information) for states, circuits, measurement, tensor products, and entanglement.
- [IBM: Fundamentals of quantum algorithms](https://quantum.cloud.ibm.com/learning/en/courses/fundamentals-of-quantum-algorithms/index) for quantum query algorithms, search, and the background to amplitude amplification.
- [IBM Qiskit quickstart](https://quantum.cloud.ibm.com/docs/en/guides/quick-start) for current installation and executable API examples.

These are direct external references; the course does not depend on private notes or an Obsidian vault.

## Completion criteria

The course is complete when the repository contains a reproducible capstone that:

- derives and validates the Poisson reward distribution;
- reports the exact expectation;
- produces a classical Monte Carlo estimate;
- prepares and verifies the corresponding quantum distribution;
- recovers the expectation from reward-qubit measurements;
- includes an amplitude-estimation comparison or clearly documents why it remains an extension; and
- explains the accuracy and resource trade-offs of each method.
