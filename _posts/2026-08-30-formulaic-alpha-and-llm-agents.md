---
layout: post
title: "The Mechanics of Formulaic Alpha: AST Parsers, Search Failures, and Execution Lags"
categories: [theory, math]
---

In quantitative equity research, a formulaic alpha is an algebraic function evaluated on a panel of market and fundamental data:

$$
s_t = f\left(X_{t-L+1:t}, U_t\right) \in \mathbb{R}^{\vert U_t\vert}. \tag{1}
$$

It maps historical observations $X$ across active names $U_t$ to a relative score vector $s_t$. 

A common issue in quantitative pipelines is evaluating $s_t$ without enforcing realistic execution timing. If scores are evaluated against forward returns over holding horizon $h$:

$$
y_{i,t}^{(h)} = \frac{P^{\mathrm{exec}}_{i,t+\delta+h}}{P^{\mathrm{exec}}_{i,t+\delta}} - 1, \tag{2}
$$

setting $\delta = 0$ assumes fills occur at day $t$'s market close. Because signal calculation requires close prices, orders in live production execute at day $t+1$'s open or morning VWAP ($\delta = 1$). In short-term reversal factors, this 1-day execution lag substantially degrades raw backtest returns as overnight price corrections diminish the intraday edge.

Converting scores $s_t$ into portfolio weights $w_t$ requires an optimizer rather than direct position mapping:

$$
w_t = \arg\max_{w\in\mathcal C_t}\left\{w^\top\hat\mu_t - \lambda w^\top\Sigma_t w - \eta\,\widehat{\operatorname{TC}}(w - w_{t-1})\right\}, \tag{3}
$$

where $\hat\mu_t$ aggregates alpha forecasts, $\Sigma_t$ is the risk covariance matrix, $\widehat{\operatorname{TC}}$ estimates transaction costs, and $\mathcal C_t$ specifies portfolio constraints.

Consider Kakushadze (2016) Alpha #101:

$$
\operatorname{Alpha\#101} = \frac{\operatorname{close} - \operatorname{open}}{(\operatorname{high} - \operatorname{low}) + 0.001}.
$$

The formula scales intraday price movement by the trading range. Under a $\delta = 1$ execution lag and standard trading friction, the factor's standalone performance is minimal.

## AST Representation and Static Typing

In automated search frameworks, formulas are parsed into Abstract Syntax Trees (ASTs). For the 5-day reversal expression $\alpha_t = -\operatorname{cs\_rank}\left(\operatorname{ts\_rank}(r_{1,t}, 5)\right)$:

- Leaf node: 1-day returns $r_{1,t}$.
- Time-series node: `ts_rank(·, 5)` computing rolling 5-day percentile ranks per stock.
- Cross-sectional node: `cs_rank(·)` computing cross-sectional ranks across $U_t$.
- Root: `negate(·)` inverting the sign.

A typed AST parser checks arity, dimensions, and operator validity before execution, preventing invalid operations like passing 2D matrices to scalar operations or mixing incompatible units.

## Combinatorial Search and Mode Collapse

A standard grammar combining 20 operators, 15 input fields, and 5 lookback windows yields a large search space. Several search paradigms explore this space:

* **Genetic Programming (GP):** Unconstrained crossover often creates deep expression trees that fit in-sample noise, requiring depth penalties or quality-diversity niching (Zhang et al., 2020).
* **Reinforcement Learning (RL):** Because intermediate trees yield no reward until completion, training policies directly on standalone Information Coefficient (IC) can lead to high-turnover reward hacking. Conditioning rewards on marginal portfolio utility (Eq. 3) helps penalize redundancy (Yu et al., 2023).
* **Generative Flow Networks (GFlowNets):** Standard RL frequently converges to a single dominant mode (e.g., generating minor variants of a single reversal pattern). GFlowNets (Chen et al., 2025) sample trees proportional to reward $p(\alpha) \propto R(\alpha)$ across directed acyclic graphs to maintain multiple distinct factor modes.
* **LLM-Guided MCTS:** Language models propose candidate structures based on economic priors. Pairing them with Monte Carlo Tree Search (Shi et al., AAAI 2026) allows systematic exploration while backtest feedback prunes unpromising branches.

## Multi-Agent Systems and Contextual Improvement

Automated factor mining systems often partition tasks across specialized components:
1. **Hypothesis Proposer:** Generates candidate AST expressions.
2. **Type Validator:** Verifies grammar compliance and node arity.
3. **Execution Sandbox:** Runs vectorized evaluations against fixed data panels.
4. **Portfolio Evaluator:** Assesses incremental predictive power after transaction costs.

In these frameworks, self-improvement occurs at the prompt and retrieval context layer rather than through model weight updates: diagnostic logs of failed trials (such as high turnover or rapid IC decay) are recorded in memory to guide subsequent candidate proposals.

## Sources of Empirical Leakage

Backtest performance frequently overstates live results due to data integrity issues:
* **Survivorship Bias:** Using current index constituent memberships for historical simulations rather than historical point-in-time constituent lists.
* **Restatement Leakage:** Evaluating fundamental signals using post-restatement financial figures prior to their actual publication dates.
* **Lookahead in Normalization:** Computing cross-sectional statistics (e.g., `zscore`) across entire time series rather than contemporaneous daily cross-sections.
* **Overlap Leakage:** Evaluating multi-day holding horizons without purging overlapping validation windows.

## Measuring Marginal Contribution and Multiple Testing

Initial predictive power is measured using the Spearman Rank Information Coefficient against forward returns:

$$
\operatorname{RankIC}_t = \operatorname{corr}_{\mathrm{Spearman}}(s_t, y_t^{(h)}). \tag{4}
$$

To determine incremental contribution, candidate scores are residualized against existing factor models and risk exposures ($B_t$):

$$
\tilde{s}^{(k)}_t = (I - P_{B_t})s^{(k)}_t, \qquad P_{B_t} = B_t(B_t^\top B_t)^{-1}B_t^\top. \tag{5}
$$

If $\tilde{s}^{(k)}_t$ exhibits negligible predictive power, the candidate factor provides no orthogonal diversification.

Furthermore, evaluating thousands of candidate formulas increases the rate of false discovery. Multiple testing controls—such as logging candidate trials, adjusting significance thresholds ($t > 3.0$), and maintaining unexposed out-of-sample holdout datasets (Harvey, Liu, and Zhu, 2016)—are necessary to control false positive rates in automated factor search.

---

### References

- Z. Kakushadze, [*101 Formulaic Alphas*](https://arxiv.org/abs/1601.00991), Wilmott Magazine, 2016.
- T. Zhang et al., [*AutoAlpha: an Efficient Hierarchical Evolutionary Algorithm for Mining Alpha Factors*](https://arxiv.org/abs/2002.08245), 2020.
- S. Yu et al., [*Generating Synergistic Formulaic Alpha Collections via Reinforcement Learning*](https://arxiv.org/abs/2306.12964), 2023.
- B. Chen et al., [*AlphaSAGE: Structure-Aware Alpha Mining via GFlowNets for Robust Exploration*](https://openreview.net/pdf?id=zRKF4ln2VE), ICLR 2026 / arXiv:2509.25055.
- Y. Shi et al., [*Navigating the Alpha Jungle: An LLM-Powered MCTS Framework for Formulaic Factor Mining*](https://ojs.aaai.org/index.php/AAAI/article/download/37069/41031), AAAI 2026.
- C. Harvey, Y. Liu, and H. Zhu, [*… and the Cross-Section of Expected Returns*](https://www.nber.org/papers/w20592), 2016.

*Educational research notes only; not investment advice.*
