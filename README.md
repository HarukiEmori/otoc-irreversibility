# otoc-irreversibility

Code and data accompanying the paper

> **Irreversibility-based framework for quantifying error, disturbance, and out-of-time-ordered correlators**
> Haruki Emori, Hiroyasu Tajima
> arXiv / DOI: not yet assigned (to be added after submission). Companion Letter: H. Emori and H. Tajima, arXiv:2309.14172.

The notebooks implement the three OTOC evaluation protocols compared in the demonstrations of the paper,
for a four-qubit XXZ chain prepared in an approximate Gibbs state by a variational algorithm, on the trapped-ion quantum computer
`reimei` (accessed through Quantinuum Nexus), with exact numerics and noiseless `aer-simulator` circuits as references.

This repository was created on 2026-10-21 by copying the files from the author's local virtual-environment folder
(`venv/pytket-dev/otoc/paper/`), where they were not under version control.
The output of the account-login cell (which displayed the login e-mail address) was cleared in the copies; no other change was made.

## Contents

| File | Role |
| :-- | :-- |
| `OTOC_ISM_Delta.ipynb` | Irreversibility-susceptibility method (ISM): Hamiltonian, VQA Gibbs-state preparation, fidelity check, exact OTOC, simulation, job submission to `reimei`, result retrieval and plots |
| `OTOC_RTM_Delta.ipynb` | Rewinding-time method (RTM), same structure |
| `OTOC_WMM_Delta.ipynb` | Weak-measurement method (WMM), same structure |
| `xxz_gibbs_optimized_params_delta_{0.1,...,0.9}.npy` | Optimized VQA parameters of the approximate Gibbs states, one file per anisotropy Delta (written by phase 1 of the notebooks) |
| `otoc_handles_delta_{Delta}_{ism,rtm}_reimei.txt`, `otoc_handles_delta_{Delta}_wmm_H2-2_exp.txt` | Result handles of the jobs submitted to the backend (read by phase 2 to retrieve the counts). The WMM file name contains `H2-2`, while the notebook code uses `QuantinuumBackend("reimei")`; which backend the stored WMM handles refer to is unverified |
| `OTOC_Comparison_{ISM,RTM,WMM}_EXP.pdf` | Figures produced by phase 3 (numerical / simulation / experiment) |
| `requirements.txt` | Package versions read from the local environment (see the comment in the file) |

Settings stored in the notebooks: `SHOTS = 500`, `ITERATION = 10` (shots per circuit and number of iterations), Delta in {0.1, 0.3, 0.5, 0.7, 0.9}.

## Environment

- Python 3.12 (notebook metadata: 3.12.12)
- pytket 2.10.1, pytket-quantinuum 0.54.0, qermit 0.8.2, qiskit 2.2.2, qiskit-aer 0.17.2 and others: see `requirements.txt`
  (read from the local `pytket-dev` environment on 2026-10-21; the exact versions at the time of the runs are unverified)

## Reproduction

```bash
python3.12 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Each notebook has three phases (cells under "8. メイン実行ブロック"):

1. VQA optimization of the Gibbs state, saving `xxz_gibbs_optimized_params_delta_*.npy` (local simulator; can be skipped by using the stored files).
2. OTOC computation: exact values and `aer-simulator` circuits locally; submission of the experiment to `reimei` and retrieval through the stored handle files.
   Submitting or retrieving jobs requires a Quantinuum account with access to `reimei` (`backend_exp.login()` asks for credentials interactively; never store credentials in the notebook).
3. Plots.

The stored handle files allow the account holder to retrieve the original counts; other users can reproduce the numerical and simulation curves without an account.

## License

MIT License (see `LICENSE`).
