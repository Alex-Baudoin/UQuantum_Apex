# Africa Quantum Consurtium Hack the Horizon - Quantum Finance Challenge Portfolio Optimization
# Team 16 : UQuantum Apex

## Background
In traditional finance, portfolio optimisation involves selecting assets to maximize returns while minimising risk. The Markowitz model formulates this as a quadratic optimisation problem. With quantum computing, we can solve these complex optimisation problems more efficiently, especially when dealing with many assets and constraints.

## Team

| Name | Role | Level | GitHub |
|------|------|-------|--------|
| [Alex Baudoin Nguetsa Tankeu](mailto:anguetsa@aimsric.org) | Mentor | Masters | [@Alex-Baudoin](https://github.com/Alex-Baudoin) |
| [Michael Ndai](mailto:michaelndai7@gmail.com) | Student | Bachelors | [@Mjndai7](https://github.com/Mjndai17) |
| [Victor Cedric Ng'ang'a](mailto:cedricvictor450@gmail.com) | Student | Bachelors | [@ced-sys](https://github.com/ced-sys) |

## Tech Stack
- QUBO/Ising problem formulation
- Qiskit
- Google Colab
- Python 3.8+
- Numpy
- Pandas
- Matplotlib
## Data Preprocessing and Asset Screening
The optimization pipeline begins with comprehensive preprocessing of our raw financial data to ensure consistency and suitability for optimization. All relevant financial fields are cleaned and standardized, including the normalization of column names and the conversion of key attributes such as expected returns, market capitalization, and transaction costs into numeric formats. This step ensures robustness against missing or malformed input data and establishes a uniform representation across assets.

To model portfolio risk in scenarios where long historical price series may be unavailable, the system synthesizes a covariance matrix using sector-level relationships and relative market capitalization. Assets within the same sector are assumed to exhibit higher correlation, while cross-sector correlations are attenuated. This approach enables realistic risk estimation while remaining computationally efficient and compatible with both classical and quantum optimization methods.

Given the current limitations of quantum hardware and the exponential growth of combinatorial search spaces, a smart asset screening stage is applied prior to optimization. Assets are ranked using a risk-adjusted performance metric (Sharpe ratio), and only the top-ranked candidates are retained for optimization (e.g., selecting 30 assets from an initial universe of 50). Sector diversity constraints are respected during this screening process to prevent over-concentration and to preserve portfolio balance.

## QUBO Formulation (Mathematical Translation)
Once the candidate asset set has been reduced, the portfolio optimization problem is mathematically translated into a Quadratic Unconstrained Binary Optimization (QUBO) formulation. In this representation, each asset is encoded as a binary decision variable indicating whether it is included in the portfolio. The financial objective is expressed as a single scalar “energy” function to be minimized.

The objective function integrates three competing components: expected portfolio return (modeled as a negative linear term to reflect maximization), portfolio risk (modeled via quadratic terms derived from the covariance matrix), and transaction costs associated with changing positions from a previous portfolio state. Together, these terms capture the trade-off between performance, stability, and operational cost.

To enforce hard constraints especially the requirement that the portfolio contains exactly a fixed number of assets, a dynamically scaled penalty weight is introduced. This penalty sharply increases the energy value when the selected asset count deviates from the target size (e.g., exactly 20 assets), effectively discouraging infeasible solutions. For quantum execution, the resulting QUBO matrix is further transformed into an Ising Hamiltonian, mapping binary decision variables onto quantum spin operators (Pauli-Z). This transformation enables direct compatibility with quantum algorithms such as QAOA.

## Hybrid Optimization Layer

The system employs a hybrid optimization architecture that dynamically bridges quantum and classical solvers. At its core, an orchestration layer selects the most appropriate optimization strategy based on resource availability and problem scale, allowing seamless fallback when quantum execution is impractical.

When quantum resources are available, the Quantum Approximate Optimization Algorithm (QAOA) is used to approximate the ground state of the Ising Hamiltonian. QAOA constructs a parameterized quantum circuit whose measurement outcomes correspond to candidate portfolios, iteratively adjusting circuit parameters to minimize the expected energy. This approach leverages quantum superposition and interference to explore the solution space efficiently.

In the absence of usable quantum backends, the system defaults to classical optimization strategies. Simulated Annealing is employed as a quantum-inspired stochastic search method that probabilistically escapes local minima, while a Greedy heuristic serves as a fast baseline by selecting assets based on risk-adjusted returns. This hybrid design ensures robustness, reproducibility, and practical deployability across diverse computational environments.

## Constraint Repair and Analysis

Because optimization solvers, particularly stochastic and quantum-based methods may occasionally produce solutions that slightly violate hard operational constraints, a post-optimization repair stage is applied. The Constraint Enforcer systematically adjusts the solution to guarantee compliance with portfolio size requirements, maximum allowable position changes, and sector exposure limits. Corrections are applied in a minimal-impact manner to preserve the quality of the optimized portfolio.

Following constraint enforcement, the Portfolio Analyzer computes a comprehensive set of performance metrics. These include expected portfolio return, overall risk (volatility), Sharpe ratio, transaction costs, and sector allocation percentages. In addition, the system generates an explicit Buy/Sell/Hold action list relative to the previous portfolio state, translating the optimization output into actionable investment decisions.

## Output
Our solution provides:
- List of Assets to hold
- Transaction list (buy/sell decisions)
- Expected Portfolio Return
- Portfolio risk
- Total transaction cost (in billion of USD)
- Sector Allocation breakdown

## Acknowledgements

A special thanks to the students, mentors and judges involved in the UQuantum Apex project. We express our gratitude to the AQC-PAQC organization comittee for putting African researchers on the quantum map through this hackathon. We welcome contributions from the community.



















