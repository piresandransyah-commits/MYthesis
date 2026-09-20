## Abstract
This study optimizes the operational performance of the Kamojang Direct Steam Geothermal Power Plant (PLTP Unit 4) using a data-driven metaheuristic approach. A 4th-degree Polynomial Regression model with 2 selected features (turbine inlet flow rate and turbine outlet pressure) was developed as a surrogate function to predict net power output, achieving an $R^2$ of 0.9508 and a MAPE of 0.57% on test data. To maintain operational constraints, a Mahalanobis distance-based trust region ($D_{max} = 2.618$) was implemented. Three optimization algorithms—Particle Swarm Optimization (PSO), Simulated Annealing (SA), and Hybrid PSO-SA—were evaluated across multiple hyperparameter settings. All methods consistently converged to an optimal power output of approximately 61.47 MW (turbine inlet flow rate $\approx 437.6\text{ T/h}$ and outlet pressure $\approx 0.149\text{ bara}$), demonstrating high solution robustness ($\Delta P_{net} \le 0.0006\text{ MW}$). The optimized operational state improved net power by 2.21 MW (+3.72%) and increased system exergy efficiency from 49.67% (median operating condition) to 51.52%.

### 1. Machine Learning Surrogate Model Construction

1. **Feature Selection & Correlation Analysis:**
   - Evaluated 45 operational sensor parameters.
   - Filtered out redundant features using Variance Inflation Factor (VIF) analysis to eliminate multicollinearity.
   - Selected two primary predictors: **Turbine Inlet Flow Rate** ($R = 0.945$) and **Turbine Outlet Pressure** ($R = 0.564$).

2. **Data Preprocessing & Cleaning:**
   - Removed sensor dropout entries (zero values).
   - Verified data distribution using Interquartile Range ($3 \times \text{IQR}$ threshold) to retain valid operational boundary conditions.
   - Applied `StandardScaler` within a pipeline structure to normalize continuous features.

3. **Train-Test Split & Hyperparameter Tuning:**
   - Split dataset into **80% training set** (2,320 samples) and **20% testing set** (580 samples)[cite: 1].
   - Performed 5-fold cross-validation across polynomial degrees 1 through 5[cite: 1].
   - Selected a **4th-degree Polynomial Regression** as the optimal model, avoiding overfitting observed at degree 5[cite: 1].

4. **Model Evaluation Metrics:**
   - $R^2$ Score: **0.9508**
   - Mean Absolute Error (MAE): **0.3372 MW**
   - Root Mean Squared Error (RMSE): **0.4286 MW**
   - Mean Absolute Percentage Error (MAPE): **0.57%**

---

### 2. Optimization Framework & Execution

1. **Objective Function & Trust Region Boundary:**
   - The trained polynomial surrogate model served as the objective function to maximize net power output ($P_{net}$) and system exergy efficiency ($\eta_{ex}$).
   - **Mahalanobis Distance Constraint:** Applied a 97% confidence threshold ($D_M(x) \le 2.618$) to constrain search agents within historical operational bounds and prevent false mathematical extrapolation.

2. **Optimization Algorithms Implemented:**
   - **Particle Swarm Optimization (PSO):** Evaluated global search capability across swarm sizes (30 vs. 80 particles) and inertia weights ($w = 0.7$ vs. $0.95$).
   - **Simulated Annealing (SA):** Evaluated local/probabilistic search across initial temperatures ($T_0 = 10$ vs. $50$) and cooling schedules ($\alpha = 0.995$ vs. $0.999$).
   - **Hybrid PSO-SA:** Combined initial PSO global exploration (60 iterations) followed by fine-grained SA local refinement (800–2000 iterations).

3. **Key Optimization Results & Performance:**
   - **Optimal Parameters:** Turbine Inlet Flow Rate $\approx 437.62\text{ T/h}$, Turbine Outlet Pressure $\approx 0.1489\text{ bara}$.
   - **Optimal Net Power Output:** $\approx 61.4704\text{ MW}$.
   - **Exergy Efficiency Gain:** Increased exergy efficiency from **49.67%** (median operational baseline) to **51.52%** (+1.85% absolute increase / +3.72% net power gain).
   - **Algorithm Robustness:** Maximum power output variation across all 9 algorithm configurations was less than $0.0006\text{ MW}$ ($0.6\text{ kW}$), confirming high consistency and convergence toward the global optimum.
