---
id: uipsq25v5l3lm0w2owpgr2h
title: AI-summary
desc: ''
updated: 1773286050974
created: 1773276082753
---

Disclaimer: Generated with Chatgpt.

# Scaling in IDAES: Methods and Evolution

IDAES (Integrating Data and Numerical Simulation) supports several scaling strategies, reflecting both legacy Pyomo-style scaling and newer “Scaling Toolbox” features.  Historically, models use *scale‐factor suffixes* on variables and constraints.  Modelers manually set scaling factors (via `idaes.core.util.scaling.set_scaling_factor`) based on expected magnitudes【43†L57-L66】, and models often implement a `calculate_scaling_factors()` method to compute remaining factors.  Constraints are automatically transformed so that a unit residual of 1e-8 is reasonable【43†L79-L88】.  For example, setting a pressure variable (∼1×10^6 Pa) to magnitude ~1 suggests a scale factor ≈1e-6【43†L63-L70】.  These suffixes are defined alongside each variable/constraint block to avoid conflicts【43†L57-L66】.  In practice, legacy flowsheets (e.g. early WaterTAP/PrOMMiS models) follow a four-step process: set known variable scales, call `calculate_scaling_factors()` to fill in the rest (transforming constraints by default)【4†L145-L154】, and finally ensure the solver (IPOPT) uses “user-scaling” (so solver applies these suffixes)【47†L631-L634】.  IPOPT’s `nlp_scaling_method = user-scaling` must be set so that the scaling factors are honored【47†L631-L634】【45†L362-L370】; otherwise IPOPT’s internal scaling overrides them.  

In IDAES v2.0+, a **Scaling Toolbox** was introduced to automate and structure this process.  The toolbox distinguishes **Auto-Scalers** (which use the current model solution to generate factors) from **Custom Scalers** (model-specific, “best-guess” heuristics)【22†L226-L235】【22†L272-L280】.  Auto-scalers (via the `AutoScaler` class) can scale an entire model in one step using metrics like Jacobian norm, but require an initial solution and can *overscale* (since they target the current state)【22†L226-L235】.  Custom Scalers (via `CustomScalerBase`) let developers encode knowledge of a unit or property model so scaling can be applied before initialization; this is more robust for uninitialized or unknown states but must be written per model【22†L258-L267】【22†L272-L280】.  Most IDAES unit models now come with their own `calculate_scaling_factors` or a custom scaler, but users can also mix approaches (e.g. use auto-scaling to fill gaps after manual guesses).

Several utility functions assist both old and new approaches.  For example, IDAES provides generators to list all variables or constraints missing a scaling factor【45†L264-L273】【52†L369-L378】, and functions to identify *badly scaled* variables (very large or small scaled values)【47†L483-L492】.  Constraint transforms can be applied explicitly with `constraint_scaling_transform(c,s)`【43†L149-L158】.  You can save or load scale factors via JSON (with `scaling_factors_to_dict`/`_from_dict`)【52†L429-L438】【52†L489-L498】, which is handy for documenting a custom scaling or re-using it in another run.  Newer IDAES also offers a “Scaling Profiler” for advanced analysis (e.g. sensitivity of objective to scaling), though documentation is sparse. 

**Evolution of approach:** In early IDAES (v1.x) and Pyomo days, users did everything manually with suffixes and constraint transforms【43†L57-L66】【43†L79-L88】.  Around v2.0, the scaling toolbox added programmatic autoscalers, profilers, and diagnostics (Jacobian analysis, SVD)【22†L226-L235】【17†L187-L196】.  However, many WaterTAP and PrOMMiS models still rely on the old pattern (as illustrated in [PrOMMiS/WaterTAP examples](#)) since it’s straightforward: set flows, call `calculate_scaling_factors(m)` and solve.  Newer guidance emphasizes combining modeler insight with tools: e.g. *first guess scaling for key variables based on expected operating conditions* (feeds, design parameters, etc.), then use autoscaling or Jacobian-based tools to refine the rest【62†L203-L212】【62†L217-L227】.  A key detail often omitted in brief docs is that **scaling need not be exact** – only order-of-magnitude accuracy is needed to avoid bad conditioning【22†L214-L223】. 

## Legacy (Pyomo) Scaling Methods

Under the legacy IDAES scheme, you use the *`scaling_factor` suffix* on variables/constraints.  For example:
```python
iscale.set_scaling_factor(m.fs.stream.flow_vol, 1e-3)
```
ensures the flow variable is treated as `x_scaled = 1e-3 * x_original`.  All fixed variables default to scale 1.0.  IDAES typically attaches this suffix in the same block as the variable for organization【43†L57-L66】.  It’s critical that scaling factors be **positive, nonzero floats**; negative or zero factors are invalid【52†L542-L550】.  Pyomo/IPOPT only sees these suffixes if you tell IPOPT to use “user-scaling” (the default **IDAES** setting)【47†L631-L634】【45†L362-L370】.  Otherwise IPOPT’s auto-scaling may ignore or override them.  (For other solvers, Pyomo may transform the problem internally via `TransformationFactory('core.scale_model')`, but IPOPT is the standard, so set `nlp_scaling_method = user-scaling`.) 

IDAES also provides helper functions for common tasks.  `calculate_scaling_factors(model)` – when defined by a unit model – will set many factors automatically based on process heuristics (e.g. mass balances scaled by flow rates)【39†L1-L9】【39†L12-L16】.  After applying scaling, you should transform constraints so that solver tolerances are meaningful.  IDAES’s standard is to scale each constraint such that a 1e-8 violation is reasonable【43†L79-L88】.  This is typically done automatically when `calculate_scaling_factors()` runs.  If you write custom constraints, you can call `constraint_scaling_transform(c, s)` manually to scale an individual constraint by `s`【43†L149-L158】.

In practice, a common legacy workflow (as in WaterTAP v1.0) is:

- **Step 1:** Set scale factors on known “extensive” variables (feeds, flow rates, holdup) to reflect typical orders of magnitude.  
- **Step 2:** Call the model’s `calculate_scaling_factors()` method.  This populates any unset scales (often using heuristics coded in unit models) and applies constraint transforms【4†L145-L154】.  If factors are missing, it issues warnings (and uses defaults).  
- **Step 3:** Optionally inspect and refine: use `badly_scaled_var_generator` or the newer `extreme_jacobian_columns/rows` to find variables/constraints with extreme scaled values【47†L483-L492】【47†L435-L444】.  Add or adjust scales as needed.  
- **Step 4:** Solve (with IPOPT user-scaling).   

A couple of *subtleties* that are often overlooked in docs: 

- **Constraint Scaling vs. Variable Scaling:**  Variables get suffix factors, but constraints get *transforms*.  The transform multiplies the original constraint so that its typical residual ≈1e-8 (independent of the suffix factors)【43†L79-L88】.  In effect, constraint transforms and suffix factors are separate: even if you give a constraint a suffix, IDAES still tries to transform it to 1e-8.  If you use your own transform (via `constraint_scaling_transform`), it “sticks” to the original constraint (undoing previous ones if rerun)【43†L149-L158】.  Many modelers find this confusing: essentially, variable suffixes condition variables, while constraint transforms adjust equations for solver tolerance.  

- **Unscaled Components:**  It’s easy to forget a variable or constraint. IDAES provides `list_unscaled_variables(block)` and `list_unscaled_constraints(block)` to list any missing scaling factors【52†L369-L378】.  Unscaled variables (scale=1) or constraints usually indicate you need to set a factor.  Likewise, `badly_scaled_var_generator(block)` flags variables whose **scaled value** is astronomically large or small【47†L483-L492】 (by default >1e4 or <1e-3).  This heuristic is useful but imperfect (e.g. it may mis-identify signed variables like enthalpy).  Newer functions `extreme_jacobian_columns/rows` report very large/small Jacobian norms for vars/constraints, which is a *more reliable* sign of poor scaling【47†L443-L452】【47†L463-L472】.  

- **Aggregate Scaling Info:** After setting or computing scales, use `report_scaling_factors(model)`【52†L406-L415】 to dump all scales.  This often catches typos (e.g. a scale of 1e-20 on the wrong component).  You can also save scales to a JSON dict/file (`scaling_factors_to_json_file`) for reproducibility【52†L483-L492】【52†L493-L502】.  

- **Solver Options:** Remember to disable solver auto-scaling.  For IPOPT, set:
  ```python
  solver.options["nlp_scaling_method"] = "user-scaling"
  ```
  This ensures IPOPT uses the provided suffixes.  If you don’t, IPOPT’s own heuristics may re-scale the problem unpredictably【47†L631-L634】.  

## IDAES Scaling Toolbox (v2+)

Since IDAES v2, the *Scaling Toolbox* adds classes and more automation.  The toolbox provides: **AutoScalers** that analyze the Jacobian, **CustomScalerBase** for model-specific routines, and a **ScalingProfiler** for tuning.  The [“Scaling Toolbox” guide](#) explains these:

- **Auto-Scalers:** Implemented in `idaes.core.scaling.AutoScaler`, these can run on a solved model to pick scales that optimize a criterion (e.g. minimize Jacobian norm)【22†L226-L235】【22†L244-L253】.  You simply call something like `AutoScaler(model).calculate()` and it sets scales for you.  This is fast and model-agnostic, but must be used *after* finding a solution (so it knows the current state) and may “over-correct” for that snapshot【22†L226-L235】【22†L244-L253】. 

- **Custom Scalers:** The base class `CustomScalerBase` lets a model developer encode scaling logic. For example, a Pump model might know to scale its power consumption by flow⋅pressure drop.  Many unit models in IDAES ship with a `_scaler` or `calculate_scaling_factors()` built on this.  The workflow is to create an instance of the scaler and call its methods.  For flowsheet-level scaling, the toolbox suggests a hierarchical approach: scale feed conditions first, propagate those scales along `Arcs`, then scale each unit in turn【62†L239-L247】.  Recycle loops require iteration or scaling to a guessed recycle state.

- **Utility Functions:** All older utilities are still available and extended.  Notably, `idaes.core.scaling.get_jacobian(model, scaled=True)` returns the current Jacobian matrix (in sparse CSR) and the underlying Pynumero NLP object【47†L584-L593】.  One can then compute the Jacobian’s condition number using `idaes.core.scaling.jacobian_cond(model)`【47†L604-L613】.  The condition number (ratio of largest to smallest singular values) gauges ill-conditioning: values ≳1e10 are warning signs【30†L185-L194】.  IDAES also supplies generators `unscaled_variables_generator` and `unscaled_constraints_generator` (similar to the list functions)【52†L529-L538】【54†L579-L588】, and `min_scaling_factor(iterable)`/`map_scaling_factor()` to get extremal scales in a collection【45†L336-L345】【47†L535-L543】.

- **Diagnostics Toolbox:** Although not strictly “scaling”, the IDAES DiagnosticsToolbox (accessible via `idaes.core.util.model_diagnostics`) is central for evaluating scaling effectiveness.  After solving or partially solving the model, `DiagnosticsToolbox.report_numerical_issues()` will flag scaling-related symptoms【30†L179-L188】【30†L196-L202】.  These include huge Jacobian condition numbers, variables at bounds or near zero, mismatched constraint terms, or potential cancellations【30†L185-L194】.  If any appear, scaling is needed or the model formulation may need change (note: some issues like mismatched terms cannot be fixed by scaling【27†L208-L213】).  Conversely, `report_structural_issues()` (run first, before solving) checks index/set consistency and *does not* rely on scaling factors【64†L249-L258】【64†L254-L263】.  A recommended workflow is to resolve all structural issues first (often involving indexing or fix/unfix mistakes), then solve as far as possible, then diagnose numerical issues【64†L249-L258】【64†L276-L283】.

- **SVD Toolbox:** For deeper insight, IDAES offers an SVD-based tool.  The `idaes.core.util.model_diagnostics.SVDToolbox` performs a singular value decomposition of the Jacobian【17†L187-L196】.  Small singular values (below a tolerance, default 1e-6) indicate near-dependencies or poor scaling.  The toolbox can highlight which constraints and variables dominate the nullspace of the Jacobian (via `display_underdetermined_variables_and_constraints`) and show ranks【17†L257-L266】【17†L273-L283】.  SVD is more expensive but can pinpoint subtle issues in large models.  It’s most useful when other diagnostics (extreme rows/columns, condition number) already suggest trouble.  

## Diagnosing Scaling Issues

To **evaluate scaling quality**, follow an iterative diagnostic workflow (adapted from IDAES guidance【62†L200-L209】【30†L179-L188】):

1. **Structural checks first:** Use `DiagnosticsToolbox.report_structural_issues()` to catch indexing errors, unused expressions, or infeasibilities unrelated to scaling【64†L249-L258】【64†L276-L283】.  Fix any such issues before worrying about scaling.

2. **Initial solution:** Attempt to solve or initialize the model (even if imprecisely) so that variables have values.  Many scaling diagnostics require a model state.  

3. **Basic numerical diagnostics:** Run `DiagnosticsToolbox.report_numerical_issues()`【30†L179-L188】.  Look for warnings/cautions about:
   - **Jacobian condition number >1e10** (a strong red flag)【30†L185-L194】.
   - **Variables with extreme values** (very large or small magnitude) or near bounds【30†L185-L194】.
   - **Constraints with mismatched or canceling terms** (may indicate formulation issues)【30†L188-L196】.
   - **Extreme Jacobian entries** or row/column norms (via `display_constraints_with_extreme_jacobians` / `display_variables_with_extreme_jacobians`)【62†L208-L215】.  Very large Jacobian rows often mean a constraint is much “stiffer” than others (badly scaled).  

   Warnings here tell you *where* to focus.  For example, if a particular flow variable’s scaled value is 1e12, its scale factor is too small.  Or a constraint shows a cancellation, hinting at a rewriting need.  

4. **Jacobian analysis:** Use `idaes.core.scaling.get_jacobian(model)` to fetch the Jacobian matrix at current values【47†L584-L593】.  Compute its condition number via `jacobian_cond()`.  This quantifies overall scaling: values ≳1e12 suggest serious imbalance.  You can also inspect extreme columns/rows as above【47†L443-L452】【47†L463-L472】.  

5. **SVD analysis (advanced):** If the model has a large condition number or you suspect degeneracy, invoke the SVDToolbox.  Call `SVDToolbox(model).run_svd_analysis()`, then use methods like `display_underdetermined_variables_and_constraints()`【17†L257-L266】【17†L273-L283】.  This will show which variables/constraints correspond to the smallest singular values (e.g. an entire equation might be nearly redundant).  Use it if simpler checks don’t fully identify the issue.

6. **Iterate scaling:** Based on diagnostics, adjust scaling factors (via `set_scaling_factor`) or apply auto-scaling.  Re-run diagnostics after each iteration.  Note that partial scaling can *increase* the count of warnings temporarily, as mismatches become more apparent【62†L208-L215】.  What matters is eventual convergence: after adequate scaling, the Jacobian condition number should drop (e.g. <1e6 ideally) and no variables/constraints should appear in extreme-value reports.  

7. **Validation:** Once scaling is applied, solve the model again and check that solver tolerances (usually 1e-8) correspond to physically acceptable residuals.  All variable values should be “normal” in magnitude (neither astronomically large nor tiny).  If you plan to optimize, check that varying objectives still converge; sometimes scaling must be retuned if operating conditions change substantially.  

Throughout this process, keep in mind that **scaling factors need only be approximate**: in many cases rounding them to 1 or a power of ten is fine【22†L214-L223】.  The goal is to avoid gross mis-scaling (e.g. one variable 10^20 times larger than another) rather than perfect normalization.

## Uncommon Caveats and Tips

- **Constraint Residual Targets:** IDAES assumes after scaling, each constraint’s residual of 1e-8 is acceptable【43†L79-L88】. If your constraints combine terms of very different units, check if the 1e-8 criterion makes sense; you may need to adjust the scaling or tolerances.  

- **Bound and Zero Issues:** Diagnostics flags variables near zero or bounds【30†L188-L194】. A variable exactly zero is okay, but if a scaled variable is 1e-12 (not intended to be zero) it may trigger warnings.  If a non-negativity or bound is active at solution, consider re-scaling so that the bound is not “near zero” in scaled space.

- **Descent in Block Hierarchy:** Many inspection tools take `descend_into=True` to recurse into sub-blocks.  If you have a large hierarchical model, explicitly descending can reveal unscaled leaves.  Use `report_scaling_factors(descend_into=True)` to see all.  

- **Propagation on Flowsheets:** The new toolbox’s `propagate_state_scaling` (in CustomScalerBase) can automatically send scale information along Arcs.  This is useful when scaling one unit and wanting connected units to inherit its outlet scales【62†L239-L247】.

- **SVD Callback:** By default the SVDToolbox uses a dense SVD via SciPy (good for moderate-size Jacobians)【17†L208-L217】.  For very large models, one can swap in `svd_sparse` (Lanczos) via the `svd_callback` argument.  However, sparse SVD may be less stable for poorly conditioned matrices.

- **Order of Operations:** Always apply **structural fixes first**, then scale variables (especially fixed inputs) before constraints【62†L203-L212】【62†L217-L227】.  Don’t try to auto-scale from scratch; start with physically meaningful scales.  If you rush into auto-scaling without a good base, you may “overshoot” and get worse conditioning.

In summary, IDAES supports **manual suffix-based scaling** and a modern **Scaling Toolbox** that automates much of it.  Effective scaling often involves using both: supply your knowledge of expected magnitudes (through initial factors or custom scalers), then use Jacobian-based utilities and the SVD toolbox to diagnose and refine.  Following the suggested workflow (structural checks → initial solve → diagnostics → scaling adjustments) will systematically lead to well-scaled, robust models. 

**Sources:** Official IDAES and WaterTAP documentation (scaling theory, methods, toolbox, and diagnostic guides)【43†L57-L66】【30†L179-L188】【22†L226-L235】【17†L187-L196】【45†L362-L370】【52†L369-L378】, which outline the available tools and recommended practices for scaling and diagnosing models.