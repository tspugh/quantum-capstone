# Lesson 01 — Workspace and First Quantum Circuit

- Status: complete
- Planned duration: 30 minutes
- Actual exploration time: approximately two hours

## Objective

Verify the `uv`-managed project environment, build and sample a two-qubit Bell-state circuit, and interpret the measurement results as a joint probability distribution.

This lesson follows IBM Quantum's [Build and run your first quantum program](https://quantum.cloud.ibm.com/learning/en/courses/use-a-qc-today/build-and-run-your-first-quantum-program) and uses the [Qiskit Aer simulator documentation](https://qiskit.github.io/qiskit-aer/tutorials/1_aersimulator.html) for the simulator extension.

## Repository setup

The project uses Python 3.12 and `uv`:

```bash
uv sync --locked
uv run jupyter lab
```

The completed notebook is [Unit 0, Lesson 1: Foundations](../projects/00-foundations/00-01-ibm-introduction.ipynb).

## Bell-state circuit

The circuit starts in $|00\rangle$, applies a Hadamard gate to qubit 0, and then applies a controlled-X gate from qubit 0 to qubit 1:

```python
from qiskit import QuantumCircuit

bell = QuantumCircuit(2)
bell.h(0)
bell.cx(0, 1)
bell.measure_all()
```

Before measurement, the ideal state is

$$
|\Phi^+\rangle=\frac{|00\rangle+|11\rangle}{\sqrt2}.
$$

The Born rule converts amplitudes into measurement probabilities:

$$
P(00)=P(11)=\frac12,
\qquad
P(01)=P(10)=0.
$$

The individual bits each have Bernoulli$(1/2)$ marginals, but they are not independent. For example,

$$
P(B_0=0,B_1=1)=0
\ne
P(B_0=0)P(B_1=1)=\frac14.
$$

## Ideal sampling

Two ideal Aer simulation methods produced different finite-shot proportions while sampling the same theoretical law:

| Simulation | Shots | Observed proportions |
|---|---:|---|
| `method="automatic"` | 1024 | `00`: 0.4492, `11`: 0.5508 |
| `method="statevector"` | 1024 | `00`: 0.4980, `11`: 0.5020 |

The `method` argument selects an internal simulation method; it does not add noise by itself. Both runs are compatible with the same ideal 50/50 distribution. Counts fluctuate because a finite collection of shots is a random sample.

## Device-derived noise model

As an extension, the notebook constructs a noise model from `FakeVigoV2`, including the backend's basis gates and coupling map, and samples 10,000 shots. The observed proportions were approximately:

| Outcome | Proportion |
|---|---:|
| `00` | 0.4560 |
| `11` | 0.4459 |
| `01` | 0.0495 |
| `10` | 0.0486 |

Unlike a 55/45 split between the two allowed outcomes, the appearance of `01` and `10` is evidence of modeled error because those outcomes have zero probability under the ideal Bell-state law. A backend-derived model may include gate, relaxation, and readout errors; the experiment does not attribute every erroneous outcome to one gate.

## Technical takeaways

- One shot returns one classical bit string.
- The ideal support is $\{00,11\}$, even though the full two-bit outcome space is $\{00,01,10,11\}$.
- Amplitudes combine coherently and can interfere; probabilities are nonnegative squared magnitudes.
- More shots reduce sampling uncertainty but do not change the underlying circuit distribution.
- Simulation method, simulation device, and noise model are separate configuration choices.
- A joint distribution can reveal dependence even when both marginal distributions look like fair coins.

## Sources

- [IBM Quantum: Build and run your first quantum program](https://quantum.cloud.ibm.com/learning/en/courses/use-a-qc-today/build-and-run-your-first-quantum-program)
- [IBM Quantum: Qiskit quickstart](https://quantum.cloud.ibm.com/docs/en/guides/quick-start)
- [Qiskit Aer simulator tutorial](https://qiskit.github.io/qiskit-aer/tutorials/1_aersimulator.html)
- [Qiskit Aer noise models](https://qiskit.github.io/qiskit-aer/apidocs/aer_noise.html)

## Provenance

The lesson structure and synthesis are AI-assisted. The notebook implementation, experiments, and written responses were completed by the repository author.
