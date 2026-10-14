<!-- solve-problem:b1c51924-e2a6-406a-8a07-fbe4fda6b5fa -->
# Attack composition with endogenous miner exit

A [Tangle Solve problem](https://solve.tangle.tools/problems/b1c51924-e2a6-406a-8a07-fbe4fda6b5fa).

## What is the problem?

In proof-of-work blockchains, a miner with hashpower fraction α can deviate from honest mining by running withholding attacks (selfish mining and variants), and can also exit the mining market or relocate hashpower. The classical literature fixes α exogenously and computes an attack's revenue in a static environment. The open problem is to analyze **attack composition under endogenous entry/exit**: when the attacker withholds, honest miners' per-unit revenue changes, which changes which competitors stay, enter, or leave, which in turn changes α and the attacker's future profitability.

Formal setting: a dynamic game over discrete rounds. State variables: attacker hashpower αₜ, total network hashpower Hₜ, attacker private-chain length, and a population of (continuum or few large) small miners each choosing ∈ {mine honestly, exit, enter}. The large attacker chooses a policy over actions {honest, withhold/selfish-mine-with-parameter τ, exit for T rounds}, maximizing discounted long-run revenue R = Σ δᵗ rₜ, where exit and re-entry carry costs. Small miners' entry/exit is a rational response to realized per-hash revenue (block reward + fees minus operating cost, possibly with difficulty-adjustment lag). Question: characterize equilibria (Markov perfect, or mean-field if miners are a continuum) — existence, the stationary distribution of αₜ, optimal attack policy as a function of state, and conditions under which withholding is an equilibrium action at all versus honest mining versus exit.

Ambiguity to note: the census record does not specify the market structure (competitive fringe vs. few strategic miners), the difficulty-adjustment model, or whether exit is reversible. These are modeling choices the solver must fix and state.

## Why does it matter?

This determines whether withholding attacks are rational in realistic markets rather than the frictionless fixed-α models of the classic literature. A solution would let one predict post-attack hashpower redistribution (e.g., whether an attack self-deflates by pushing out honest miners, or self-reinforces by raising the attacker's relative share), inform protocol design of difficulty-adjustment and fee mechanisms, and give exchanges/miners a model of attack-driven market exits. It also connects to the empirically observed miner shutdowns during revenue shocks, which static models cannot capture.

## What's already known?

Classical results (Eyal–Sirer 2014; Sapirshtein et al. 2016; follow-ups to 2025) characterize optimal selfish-mining revenue as a function of a fixed α, typically showing profit above honest share for α ≳ 1/4 (lower with network-level advantages). This line treats total hashpower and competitor behavior as exogenous. The cited recent work moves toward dynamics: Doger and Ulukus (2025, arXiv:2511.11538) analyze how incentive attacks change short-term revenue and long-run mining efficiency, i.e., the revenue trajectory an attack induces; Finkbeiner et al. (2026, AFT) (verify exact content) reportedly incorporate temporal analyses relevant to attack profitability under changing conditions. What is missing: nobody has closed the loop by making competitor entry/exit a rational endogenous response, nor computed the resulting equilibrium attack policy. Related tools exist — mean-field games for continuum populations, regime-switching stochastic control, real-options models of firm exit under stochastic revenue — but have not been assembled for this setting. Main known obstacle: the difficulty-adjustment lag creates a non-stationary reward process during transitions, complicating existence arguments for stationary equilibria.

## What would count as solving it?

A referee would accept any of:

1. **Existence + characterization**: a proof that the attacker–fringe dynamic game admits a Markov-perfect (or mean-field) equilibrium, with a characterization of the equilibrium attack policy (e.g., a threshold rule: withhold iff αₜ ≥ α*(cost parameters)).
2. **Comparative statics**: proven statements on how exit costs, difficulty-adjustment period, and discount factor shift the equilibrium — e.g., "exit options make withholding strictly less attractive iff δ < δ̄."
3. **Matching bound / counterintuitive regime**: proof that endogenous exit either expands the profitable-withholding region beyond the classical α threshold or eliminates it in a stated parameter regime.
4. **Numerical construction**: a computed equilibrium policy and stationary α distribution with convergence guarantees, validated against the model (acceptable as a partial result for the mean-field variant).

## How to start

1. Read Eyal–Sirer (2014) §III–IV for the selfish-mining revenue formula and Sapirshtein et al. (2016) for optimal-withholding MDP formulation — you need this as the attacker's stage-game payoff kernel.
2. Read Doger and Ulukus (2025) for the temporal revenue analysis; this is your bridge from static to dynamic payoffs. Then Finkbeiner et al. (2026) (verify availability).
3. Build the simplest solvable model first: continuum of identical small miners with linear operating cost c and myopic exit/entry on per-hash revenue, difficulty adjustment every D blocks, attacker solving an MDP. Solve numerically (value iteration over (α, private length, position in epoch)) to build intuition for threshold structure.
4. Main obstacles: non-stationary rewards during difficulty transitions; the attacker's state space with private chains blows up (borrow truncated-chain abstractions from Sapirshtein et al.); proving existence in the mean-field version needs continuity of the exit flow in the reward process.

## Key references

- Eyal, I., Sirer, E.G. (2014). "Majority is not Enough: Bitcoin Mining is Vulnerable." FC 2014. Derives selfish-mining revenue for fixed α and the classical profitability threshold. https://arxiv.org/abs/1311.0243
- Sapirshtein, A., Schapira, Y., Zohar, A. (2016). "Optimal Selfish Mining Strategies in Bitcoin." FC 2016. Computes the optimal withholding policy as an MDP over truncated private-chain states. https://arxiv.org/abs/1507.06183
- Doger, B., Ulukus, S. (2025). "Incentive Attacks in BTC: Short-Term Revenue Changes and Long-Term Efficiencies." arXiv:2511.11538. Analyzes how incentive attacks reshape short- and long-term miner revenue, the temporal foundation this problem builds on. https://arxiv.org/abs/2511.11538 (verify)
- Finkbeiner, B. et al. (2026). DOI:10.4230/LIPIcs.AFT.2026.29. Temporal analysis of mining-attack profitability; content per census record, not independently verified. (verify)
- Carlson (1973) / Lasry–Lions (2007) mean-field game papers: standard machinery for continuum-population dynamic games, the natural tool for the miner fringe.

## Warnings

- The census record is sparse; the 2025–2026 citations are beyond my reliable knowledge — verify they exist and read them before assuming their results.
- Do not assume total hashpower H is constant; that assumption silently recovers the static problem and trivializes the question.
- Difficulty adjustment timing is critical: with instant adjustment, attacks are far less profitable than with the real ~2016-block Bitcoin epoch. State your assumption explicitly.
- Common dead end: modeling exit as exogenous decay of Hₜ; that is a stochastic-control exercise, not the game demanded here. The problem requires competitor best responses.
- Ambiguity in "relocate": cross-coin hashpower mobility changes fee/reward assumptions; pick one interpretation and defend it.

---
*`BCGT-000229` · flagged for review*

## Acceptance guidance

A referee would accept any of:

1. **Existence + characterization**: a proof that the attacker–fringe dynamic game admits a Markov-perfect (or mean-field) equilibrium, with a characterization of the equilibrium attack policy (e.g., a threshold rule: withhold iff αₜ ≥ α*(cost parameters)).
2. **Comparative statics**: proven statements on how exit costs, difficulty-adjustment period, and discount factor shift the equilibrium — e.g., "exit options make withholding strictly less attractive iff δ < δ̄."
3. **Matching bound / counterintuitive regime**: proof that endogenous exit either expands the profitable-withholding region beyond the classical α threshold or eliminates it in a stated parameter regime.
4. **Numerical construction**: a computed equilibrium policy and stationary α distribution with convergence guarantees, validated against the model (acceptable as a partial result for the mean-field variant).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/b1c51924-e2a6-406a-8a07-fbe4fda6b5fa?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000229.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000229)
