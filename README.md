# Quantum Computing and Probability — Learning Notebooks

A self-study project connecting probability with quantum computing through worked derivations, classical sampling, and local Qiskit experiments. I am developing this alongside my mathematics master's studies at the University of L’Aquila.

**Status:** two completed foundational notebooks. The Poisson reward model is currently implemented classically; quantum state preparation and amplitude estimation are future work. This repository does not demonstrate quantum advantage or a completed capstone.

## Start here

| Notebook | Question | Implemented work |
| --- | --- | --- |
| [01 — Bell states and noise](projects/00-foundations/00-01-ibm-introduction.ipynb) | How do finite-shot variation and modeled device noise differ? | Bell-state circuit, ideal sampling, and a simulated device noise model. |
| [02 — Poisson rewards](projects/00-foundations/00-02-poisson-distribution.ipynb) | How does transforming a count change its distribution and expectation? | Exact reward law, Monte Carlo comparison, and interactive plots. |

For the second notebook, the example is $N \sim \mathrm{Poi}(2)$ and $R=N\mathbf{1}_{\{N\leq4\}}$. The exact expectation is $\mathbb{E}[R]=\frac{38}{3}e^{-2}\approx1.714247$. Counts above four contribute to the probability of **zero reward**; this is not a conditional, renormalized Poisson distribution.

## Run locally

Requires [uv](https://docs.astral.sh/uv/) and Python 3.12 (recorded in `.python-version`). From a fresh clone:

```bash
git clone https://github.com/tspugh/quantum-capstone.git
cd quantum-capstone
uv sync --locked
uv run jupyter lab
```

Open the notebooks in the order above and select the project Python kernel. Both run entirely on a local CPU simulator; no IBM account, API key, or hardware job is needed. GitHub shows saved static outputs; use JupyterLab for the interactive controls.

To execute the published notebooks from clean kernels without changing the committed copies:

```bash
mkdir -p /tmp/quantum-capstone-executed
uv run jupyter nbconvert --to notebook --execute --ExecutePreprocessor.timeout=180 \
  --output-dir=/tmp/quantum-capstone-executed \
  projects/00-foundations/00-01-ibm-introduction.ipynb \
  projects/00-foundations/00-02-poisson-distribution.ipynb
```

Recorded simulator and NumPy seeds make the examples repeatable in the locked environment. Results may differ across library versions and simulator methods. The fake-device model illustrates modeled noise; it is not a current hardware benchmark.

## Learning approach and roadmap

The notebooks retain predictions, questions, hand derivations, and interpretations. The [lesson plans](lessons/) provide the learning sequence; planned lessons are not completed results. See the [curriculum](<lessons/QIS - Integrated Quantum Algorithms and Probability Course.md>) for the longer plan.

Next steps are biased quantum coins, encoding a finite probability law, and comparing exact expectations, classical Monte Carlo, and quantum amplitude estimation. A future comparison must account for state-preparation cost and the error introduced by representing an infinite-support law with finitely many qubits.

## Sources and assistance

The first notebook follows IBM's [Build and run your first quantum program](https://quantum.cloud.ibm.com/learning/en/courses/use-a-qc-today/build-and-run-your-first-quantum-program), with additional local sampling and noise experiments. The second is based on probability coursework taught by Prof. Ida Germana Minelli and my worked reward example. [SOURCES.md](SOURCES.md) identifies these references and the scope of adaptations.

I wrote the original learning code and responses while using AI as a tutor. Lesson plans were AI-generated. This publication pass also used AI-assisted editing to correct explanations, improve reproducibility, and organize documentation; it does not represent additional completed coursework.

No repository-wide open-source license is granted at present. Referenced material remains attributed to its original authors; links and attribution do not relicense their work.
