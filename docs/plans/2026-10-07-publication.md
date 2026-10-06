# Quantum notebook publication plan

**Goal:** Publish the two completed learning notebooks with accurate explanations and reproducible local execution.

**Architecture:** Retain the notebook-led repository and the original learning sequence. Separate completed work from planned quantum algorithms; preserve untracked lesson 3 drafts outside this publication.

**Tech stack:** Python 3.12, uv, NumPy, SciPy, Qiskit Aer, Jupyter.

1. Correct the notebook explanations, standardize numbering, and use explicit simulator seeds.
2. Replace placeholder documentation and metadata; add a notebook index and source/assistance attribution.
3. Execute both notebooks from clean kernels in the locked environment; check numerical invariants and rendered output.
4. Inspect tracked content and Git history for publication blockers, then commit only the reviewed files.
5. Push the verified commit, add GitHub description/topics, and make the repository public.

No hardware jobs, credentials, unfinished exercise solutions, or new licensing terms are part of this change.
