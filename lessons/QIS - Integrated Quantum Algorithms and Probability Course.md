# Integrated Quantum Algorithms and Probability Course

## Purpose

This course provides the learning path for this repository. It combines probability, classical simulation, quantum circuits, and quantum algorithms in a sequence of reproducible notebooks and small programs.

Unit 0 compares exact calculation, classical Monte Carlo, quantum probability encoding, and measured expectation values for a Poisson reward. Amplitude estimation is a possible later extension. Probability topics are introduced where they support the implementation, analysis, or validation of the project.

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
4. optionally apply a small amplitude-estimation example after the baseline circuit is verified.

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

## Unit 0 — Poisson sampling and expectation encoding

Core durations estimate focused work; documentation reading and exploration can extend a session. Lesson 02 took approximately three hours including its interactive extension. Lessons are published as they are used. The revised Lesson 03 combines single-qubit and conditional two-qubit preparation in one notebook with two resumable checkpoints; it does not compress both into a one-hour promise.

| Lesson | Core duration | Topic | Repository outcome |
|---|---:|---|---|
| [1](<QIS - Lesson 01 - Workspace and First Quantum Circuit.md>) | 30 min | Workspace and first circuit | Bell circuit, measurement interpretation, and modeled noise |
| [2](<QIS - Lesson 02 - Poisson Law and Recorded Counts.md>) | 75 min planned; about 3 hr with exploration | Poisson law and recorded counts | Derivation, SciPy verification, NumPy samples, reusable functions, mean markers, and optional interactive controls |
| [3](<QIS - Lesson 03 - Qubits, Rotations, and a Biased Coin.md>) | 105 min (A: 60, B: 45); allow 3–4 hr | Biased coin, then conditional coins | One-qubit rotation and four-outcome preparation from marginal/conditional rotations; exact and sampled checks |
| Former 4 slot | Optional, 0–30 min | Consolidation only | Finish or independently modify Part B; no duplicate required lesson |
| 5 | 90 min | Poisson sampling and expectation encoding | Verified count distribution and controlled reward flag with $\Pr(1)=\mathbb{E}[R]/4$ |

Lesson 05 has two checkpoints and may take more than one sitting. Its three-qubit count register represents 0 through 6 and an explicit overflow category for $N\ge7$. The probabilities are $P(N=k)$ for $0\le k\le6$ and $P(N\ge7)$ for the last category. Counts 1–4 contribute their values to the reward; all other categories contribute zero. A fourth qubit encodes that scaled expectation. This preserves the reward law without claiming to encode every Poisson count separately.

After Lesson 03 Parts A/B, proceed directly to Lesson 05 unless consolidation is useful. The number is retained for continuity. Allow roughly 5–7 hours for the combined Lesson 03 and Lesson 05, with breaks between their checkpoints as needed.

### IBM course boundary for Lesson 03

In [Quantum mechanics basics](https://quantum.cloud.ibm.com/learning/en/courses/use-a-qc-today/quantum-mechanics-basics), read from **Introduction** through **How measurements work**, inclusive, and complete the Hadamard matrix checks. Stop before **Measurement in different bases**. Part B uses the bit-ordering and controlled-rotation references listed in Lesson 03; it does not require the rest of the IBM lesson. Return to the deferred measurement-basis sections before Hamiltonian measurements.

## Further units

Later lessons are planned iteratively around the completed work. Candidate directions include graph Hamiltonians, energy spectra, and a small digitized annealing experiment. Monte Carlo error analysis, Grover search, and amplitude estimation remain available extensions. Specific research questions and later lesson details will be introduced as their scope is established.

Lesson 02's note connecting moments, Gaussian weights, Hermite polynomials, and harmonic-oscillator eigenfunctions is retained as a possible Hamiltonian-unit exploration, not an additional prerequisite for the Poisson circuit.

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
