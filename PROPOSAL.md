# Benchmark Paper

- **Paper:** N. C. Rubin, D. W. Berry, A. Kononov, F. D. Malone,
  T. Khattar, A. White, J. Lee, H. Neven, R. Babbush and
  A. D. Baczewski, "Quantum computation of stopping power for inertial
  fusion target design," *PNAS* 121(23), e2317772121 (2024),
  <https://doi.org/10.1073/pnas.2317772121> (arXiv:
  <https://arxiv.org/abs/2308.12352>).

- **Venue rank:** PNAS, Q1 in Scimago (Multidisciplinary category).

- **Code and data:** Google Quantum AI's open-source (Apache 2.0)
  verifier and reference circuit for the Kaggle competition *Verifiable
  Quantum Circuit Compilation*; the `kickmix` library is in the Qualtran
  GitHub repository.

- **Scope:** only Benchmark I (`invsqrt`). The other two competition
  benchmarks are excluded to keep the project feasible.

# Result to Reproduce

The paper's Newton--Raphson circuit applies the phase
$\exp(2\pi i/\sqrt{x})$ to a 22-qubit register for every integer $x$ in
$[1,\,3{,}133{,}452]$, with relative error below $2^{-24}$. I will
reproduce its cost and accuracy using the reference code as a guide. The
problem definition and tolerance follow the verifier's documentation
(Google Quantum AI, Apache 2.0, Kaggle competition files); the algorithm
is the paper's, and the register-width names come from the reference
code's `Precision` class.

- **Metrics:** worst-case relative error, qubit count, Toffoli count,
  and score $=$ qubits $\times$ Toffoli-equivalents (lower is better).

- **Paper's number:** the paper estimates the inverse-square-root
  arithmetic at about 2,395 Toffolis for $n=6$ (14-bit $x$), using the
  formula $2137 + 4n^2 + 19n$ (Eq. 50 and the text around it in
  Section II-D of arXiv:2308.12352), with a relative error of about one
  part in $4\times10^{8}$. For the benchmark's $n=10$, the formula gives
  about 2,727 Toffolis (my own calculation). The paper counts the
  forward computation only, so I will compare it with one direction of
  my circuit and explain any difference.

# Research Question

Given the same budget of $N = 50$ circuit evaluations, does an LLM agent
find a verified circuit with a lower score than the reference, and than
random search and grid search?

**Method:** I will build a Python agent loop. Each round, the agent
reads the history of past attempts and proposes new settings for the
five register widths of the reference circuit (`delta_bits`,
`coef_bits`, `square_bits`, `product_bits`, `result_bits`). A harness
then checks the worst-case error with the fixed-point model (it must
stay below $2^{-24}$), builds the circuit, and runs the official
verifier for pass/fail and the score (with a reduced number of shots per
$x$ during the search, and the full default number for the final best
circuits). The best verified circuit is kept and every attempt is
logged. All three search methods use the same metrics and the same
budget, and the reference files are never modified.

# Data and Compute

No dataset is needed, since every $x$ in the range is enumerated. I will
use Python, NumPy, `kickmix`, pytest, and an LLM API with the key kept
outside the repository, on Kaggle notebooks or my laptop CPU. Quick
checks use the model (seconds) and a reduced-shot verifier run; final
candidates get the official verifier. Full verification is slow, so it
is used only on finalists. I have run the official verifier on Kaggle
notebooks; I have not tested Colab.

# Plan

- **By 4 December (checkpoint):** rebuild the reference circuit from
  source, verify it, and compare its cost and accuracy with the paper's
  numbers; write the evaluation harness and the random and grid search
  baselines.

- **By 18 December:** finish the agent loop, run all three methods with
  the same budget, build and verify the best circuits, compare with a
  table and a score-versus-evaluations plot, write the IEEE paper (8+
  real references I have read), finish the README, and publish 2 Kaggle
  notebooks for the skills profile.

- **Fallback:** if the circuit builder will not run in my environment, I
  will run the agent on the fixed-point model only, verify Google's
  provided sample circuit, and report cost changes only for circuits I
  can build and verify.

# Use of LLMs {#use-of-llms .unnumbered}

An LLM helped me understand the code and plan this project, and the
agent itself calls an LLM at run time. All results come from my own
runs.
