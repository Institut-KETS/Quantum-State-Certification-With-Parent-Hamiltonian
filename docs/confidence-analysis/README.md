# Confidence analysis of archived hardware results

This companion analysis was moved out of the PRA manuscript's former Appendix F at the authors' request. It analyzes the existing archive; no new hardware measurements are required or performed. `archived-appendix.tex` preserves the former appendix text for provenance (its cross-references refer to the manuscript before relocation). This page is the standalone account.

## Data and assumptions

The archive contains 152 W-state jobs and 15 higher-excitation Dicke-state jobs. All 30 manuscript table entries match the archived excitation number, system size, energy and execution date. W-state results select the lowest measured energy at each size.

All W-state jobs enabled measurement-error mitigation and measurement twirling. Both were disabled for the higher-excitation jobs. Gate twirling, dynamical decoupling, zero-noise extrapolation and probabilistic error cancellation were disabled for all 167 jobs. Configured shots per measurement-basis configuration were 16384 for W states (divided among 32 measurement-twirling randomizations) and 16000 for higher-excitation states (one randomization).

The records contain configured options, expectation values and aggregate uncertainties, but not the executed basis-resolved circuits or outcomes. The calculation below assumes independent preparations, trusted ideal Pauli measurements, and 16000 shots in each of three separately sampled homogeneous X, Y and Z bases. The archive does not independently establish that executed sampling model. Residual readout bias and mitigated W-state values require an additional error model.

## Bound

For a Dicke target with n qubits and k excitations, let M be the sum of the n outcomes (+1 or −1) from one shot in a common Pauli basis. The per-shot energy scores are

$$g_X(M)=g_Y(M)=-\frac{M^2-n}{4n},\qquad g_Z(M)=-\left(\frac n2-k\right)M+\frac{n-1}{4n}(M^2-n).$$

Their means, added to $C=(n/2-k)^2+(2n-1)/4$, give the energy estimate $\widehat E$. Define $R_b=\max_M g_b(M)-\min_M g_b(M)$, with $M\in\{-n,-n+2,\ldots,n\}$. For $N_0$ shots per basis, use

$$E_U=\widehat E+\sqrt{\frac{R_X^2+R_Y^2+R_Z^2}{2N_0}\ln\frac{167}{0.05}}.$$

Hoeffding's inequality gives $\Pr(E>E_U)\le0.05/167$ for each unmitigated result under the assumptions above. A union bound gives simultaneous coverage of at least 95% for the 15 higher-excitation results. The denominator accounts for selection within the full collection of 167 archived candidates; it does not give confidence coverage to the 152 mitigated results. Dependence between different jobs is allowed. Selection from additional, unarchived trials would require a corresponding adjustment.

The Hamiltonian has gap one, so the conditional fidelity lower bound is $\max(0,1-E_U)$. Genuine multipartite entanglement is detected when $E_U<1-\alpha$, where $\alpha$ is the maximal biseparable fidelity of the target.

## Results

| n | k | Energy estimate | Energy upper bound | Witness threshold |
|---|---|---|---|---|
| 4 | 2 | 0.136078 | 0.188890 | 0.333333 |
| 5 | 2 | 0.138325 | 0.265433 | 0.400000 |
| 4 | 3 | 0.078969 | 0.212253 | 0.250000 |
| 5 | 3 | 0.151250 | 0.278358 | 0.400000 |

These four jobs pass the conditional entanglement test. Both higher-excitation families have positive conditional fidelity bounds through eight qubits; larger archived members fail this positivity test. Failure leaves entanglement undetermined.

At seven qubits the k=2 and k=3 energy upper bounds are 0.623486 and 0.528390, above their thresholds 2/7 and 3/7. Six- and seven-qubit higher-excitation crossings and all W-state crossings remain point estimates in this analysis. Aggregate runtime uncertainties do not determine the empirical variances of the basis scores. The normal-error diagnostics included in the CSV outputs are exploratory calculations, not confidence certificates.

## Calculation inputs

The archived manifests and job results are under `data/w_state/results_and_calibrations/` and `data/k2_k3_Dicke/results_and_calibrations/`. Each manifest identifies the experiment and runtime-result JSON files. For each unmitigated job, use its recorded energy and shot count in the score-range formula above. Compare the resulting upper bound with the target's biseparable threshold. The numerical results in this page use the existing archived data only.

The original manuscript source supplement contains the audit script and table-matching outputs. This relocation publishes the explanatory appendix here; it does not change the hardware archive.
