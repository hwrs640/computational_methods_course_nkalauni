# HWRS640 Final Project Proposal
## Symbolic Regression of Neural Aerodynamic Resistance in a Hybrid Land Surface Model

### 1. Motivation and scientific question

Hybrid models that replace uncertain physics components with neural networks have become popular in land-surface modeling and often improve predictive skill over pure-physics baselines. But the learned NN components are black boxes — even when they fit well, we cannot tell whether they have discovered meaningful physical relationships or merely memorized site-specific patterns. **Can we recover an interpretable, closed-form equation for aerodynamic resistance from the neural component of a trained hybrid land-surface model, and how does that equation compare to the textbook physics it replaced?** Answering this would turn opaque hybrid models into a tool for *discovering* parameterizations, not just fitting them.

### 2. Dataset

I will use eddy-covariance and meteorological data from the **AT-Neu (Neustift) FLUXNET2015 grassland site**, which my thesis hybrid model already fits well when the aerodynamic resistance is replaced by a neural network. The relevant half-hourly variables are wind speed at the measurement height (`u_a`), surface temperature (`t_s`), air temperature (`t_a`), atmospheric pressure (`p_a`), and the eddy-covariance turbulent fluxes (H, LE) for downstream evaluation; the site-level roughness length is `z₀ = 0.0019 m`, previously calibrated against the physics-only model. Data is publicly available from the FLUXNET2015 archive and is already loaded and preprocessed (gap masking, unit conversion, normalization) by my thesis pipeline. Two derived "data sources" will be generated for symbolic regression: (a) physics aerodynamic resistance `r_a` computed analytically from the same drivers, and (b) trained-NN predictions of `r_a` sampled densely over the same input distribution.

### 3. Proposed method

The central method is **symbolic regression (SR)** — searching the space of mathematical expressions for compact equations that fit data. I will use **PySR** (genetic programming over operator/feature trees) and, as a sanity check, **SINDy** (sparse regression over a fixed nonlinear feature library). SR is the right tool because it produces *human-readable* expressions, directly addressing the interpretability gap in my thesis hybrid model.

The project has two stages:

1. **Validation on known physics.** Sample inputs from AT-Neu's distribution, compute `r_a` from the analytic formula in `lad_model/utils/physics.py` (a log-law), and run SR on the (inputs → r_a) pairs. Test whether SR recovers an expression structurally close to the analytic form. This calibrates how trustworthy SR's output will be in stage 2 and lets me tune the operator set and parsimony pressure on a case where the answer is known.

2. **Application to the trained NN.** Take the `MLAerodynamicResistance` MLP from my best AT-Neu hybrid run, evaluate it densely over the same input distribution, and run SR on the (inputs → NN-output) pairs. Compare the recovered expression to the physics baseline from stage 1: is it similar or different in interesting ways?

The NN takes four inputs in the order `[u_a, t_s, t_a, p_a]`. The output is scalar `r_a` in s/m.

### 4. Expected outcomes and evaluation

I expect stage 1 to recover an expression *functionally* close to the analytic physics (a log-law-like dependence on `u_a`), giving a ground-truth check on SR in this setting. For stage 2, I expect the NN-derived expression to share gross structure with the physics but include site-specific corrections — possibly a stability correction or a different effective roughness term — that explain why the NN version fits AT-Neu fluxes better than the textbook formula.

**Concrete evaluations:**
- **Pareto front** of expression complexity vs. RMSE on held-out samples for each SR run (standard PySR diagnostic).
- **R² and bias** of the recovered expression against (a) the analytic physics (stage 1) and (b) the NN output (stage 2).
- **Drop-in test:** plug the recovered symbolic `r_a` back into the LaDModel ODE solver and compare H/LE skill against both the physics and the original NN versions on a held-out AT-Neu period. This is the key test: does the *interpretable* expression preserve the predictive gain of the NN?

**Feasibility:** the dataset, the trained `MLAerodynamicResistance` model, and the physics solver are already in hand from my thesis. The new work is wiring PySR/SINDy into the existing pipeline and running the two SR experiments — a focused scope that fits the 5–6 week timeline.