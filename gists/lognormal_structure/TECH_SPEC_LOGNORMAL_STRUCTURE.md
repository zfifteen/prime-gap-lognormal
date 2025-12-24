# Technical Specification: Lognormal Structure Discovery Gist

## Objective

Create a self-contained, executable demonstration that proves prime gaps follow lognormal distributions across all scales with 100% band coverage, contradicting the classical assumption of exponential gap distributions.

## Discovery Statement

**Gap distributions are lognormal across all scales and bands — 100% of 6 tested bands pass distributional tests with p-values > 0.05. This implies multiplicative randomness in gap formation.**

## Implementation Requirements

### 1. Core Functionality

**Input:**
- Scale range: 10^6 to 10^8 (configurable)
- Band partitioning: 6 distinct magnitude ranges

**Processing:**
1. Generate primes using optimized sieve (or load pre-computed)
2. Extract consecutive prime gaps
3. Partition gaps into bands by scale
4. Apply log-transformation to gaps
5. Fit lognormal distribution to log-transformed data
6. Run statistical tests for normality/lognormality

**Output:**
- Statistical test results (p-values) for each band
- Lognormal parameters (μ, σ) per band
- Visual Q-Q plots showing lognormal fit
- Summary: X/6 bands pass distributional tests

### 2. Statistical Tests

Must implement at least 2 of:
- **Kolmogorov-Smirnov test**: Compare empirical vs theoretical lognormal CDF
- **Anderson-Darling test**: Weighted goodness-of-fit emphasizing tails
- **Shapiro-Wilk test**: Test normality of log-transformed gaps
- **Jarque-Bera test**: Test normality via skewness and kurtosis

**Success criteria:** p-value > 0.05 indicates lognormal fit

### 3. Visualization Requirements

**Q-Q Plots (Quantile-Quantile):****Q-Q Plots (Quantile-Quantile)** [ESSENTIAL - Gold Standard Proof]
- X-axis: Theoretical quantiles (normal distribution)
- Y-axis: Sample quantiles (log-transformed gaps)
- Expected: Points fall on diagonal line for lognormal data
- Include confidence bands
- One plot per band or composite plot with 6 overlays

- **Why This Matters:**
This is the **definitive visual proof** of lognormality. If data is lognormal, log-transformed values are normally distributed, and Q-Q plot points will fall on a perfect diagonal line. A viewer can instantly see linearity = lognormal fit, requiring no statistics background.

**Technical Details:**
- Layout: 2×3 grid showing all 6 bands, or single composite with color-coded bands
- Include 95% confidence intervals (gray shaded region)
- Target R² > 0.95 for linear fit
- Label axes clearly with units

---

**Plot 2: Distribution Comparison Histograms** [HIGH IMPACT - Refutes Cramér]

**Why This Matters:**
Shows **both** that lognormal fits perfectly AND exponential (Cramér model) fails. The side-by-side comparison demonstrates universal lognormal structure across all bands while systematically refuting the classical model.

**Technical Details:**
- Format: 2×3 grid (6 subplots, one per band)
- Each subplot shows:
  - Histogram of raw gaps (gray bars)
  - Overlaid lognormal fit curve (red solid line)
  - Overlaid exponential fit curve (blue dashed line) for contrast
  - Band label and scale range
- Clearly show lognormal hugs data while exponential deviates
- Optional: Add chi-squared or log-likelihood comparison text

---

**Plot 3: Parameter Evolution** [REVEALS STRUCTURE]

**Why This Matters:**
Demonstrates that lognormal parameters **shift predictably** with scale, proving this isn't random coincidence—there's systematic structure. μ increasing = gaps get larger at higher scales (expected). σ stability = consistent multiplicative randomness across scales.

**Technical Details:**
- X-axis: Band number or prime scale (10^6.0, 10^6.33, 10^6.67, ...)
- Y-axis: Parameter value  
- Two lines:
  - μ (mean of log-gaps): Increasing trend (blue circles)
  - σ (std dev of log-gaps): Stable or systematic trend (red squares)
- Include error bars if variance is measured
- Add trend lines to show systematic behavior

---

**Plot 4: P-Value Summary** [STATISTICAL PROOF]

**Why This Matters:**
Visually proves **100% pass rate** across all bands and tests. This is the numerical backing for "brutal consistency"—even non-statisticians can see the wall of green confirming complete validation.

**Technical Details:**
- Format: Heatmap or grouped bar chart
- Rows: Statistical tests (KS, Anderson-Darling, Shapiro-Wilk, Jarque-Bera)
- Columns: Bands (1-6)
- Color coding:
  - Green if p > 0.05 (passes lognormal test)
  - Red if p < 0.05 (fails)
  - Intensity based on p-value magnitude
- Annotate cells with actual p-values
- Target: Complete green grid = 100% validation

---

**Plot 5: Log-Scale Gap Distribution** [SHOWS MULTIPLICATIVE NATURE]

**Why This Matters:**
Lognormal distributions appear symmetric/bell-shaped on log scale, while exponential distributions look completely different. This visually demonstrates multiplicative randomness—gaps scale geometrically, not additively, revealing the fundamental process generating gaps.

**Technical Details:**
- X-axis: Gap size (log scale)
- Y-axis: Frequency/density (log scale for log-log plot, or linear)
- Show histogram of gaps with log-transformed x-axis
- Overlay lognormal PDF (appears as normal curve on log-x axis)
- Compare to exponential PDF (appears as different shape)
- Single composite plot or one representative band

---

**Plot 6: Residual Analysis** [OPTIONAL - GOODNESS OF FIT]

**Why This Matters:**
Addresses potential skepticism about Q-Q plot quality. Shows deviations from perfect lognormal fit. Tight clustering around zero = excellent fit with minimal systematic bias.

**Technical Details:**
- X-axis: Theoretical quantiles
- Y-axis: Residuals (observed - expected)
- Scatter plot with horizontal line at y=0
- Points should cluster tightly around zero
- Look for patterns (none = good fit, systematic curve = model misspecification)
- One plot per band or composite

---

### Recommended Composite Figure Layout

**Single publication-ready figure with 4 panels:**

```
+------------------+------------------+
|                  |                  |
|   Q-Q Plots      | Distribution     |
|   (2×3 grid)     | Comparisons      |
|   All 6 bands    | (2×3 grid)       |
|                  | Lognormal vs Exp |
+------------------+------------------+
|                  |                  |
| Parameter        | P-Value          |
| Evolution        | Heatmap          |
| (μ, σ vs scale)  | (100% pass rate) |
+------------------+------------------+
```

This layout tells the complete story in one figure:
- **Left column**: Visual and systematic proof (Q-Q + parameters)
- **Right column**: Comparative and statistical proof (distributions + p-values)

**File outputs:**
- `lognormal_structure_complete.png` (composite figure, 300 DPI)
- `qq_plots.png` (standalone Q-Q plots)
- `distribution_comparison.png` (standalone histograms)
- `parameter_evolution.png` (standalone parameter plot)
- `pvalue_heatmap.png` (standalone statistical summary)

**Distribution Comparison:** Histogram of log-transformed gaps with fitted normal curve
- OR: Histogram of raw gaps with fitted lognormal curve
- Show deviation from exponential distribution for contrast

### 4. Technical Stack

**Language:** Python (preferred for reproducibility)

**Required libraries:**
- **Prime generation**: `sympy`, `gmpy2`, or custom sieve
- **Statistics**: `scipy.stats` (kstest, anderson, shapiro)
- **Visualization**: `matplotlib`, `seaborn`
- **Numerical**: `numpy`

**Optional:**
- `statsmodels` for advanced tests
- `pandas` for data handling

### 5. Code Structure

```python
# High-level structure
def generate_primes(start, end):
    """Generate primes in range using sieve"""
    
def extract_gaps(primes):
    """Calculate consecutive gaps"""
    
def partition_by_band(gaps, primes, n_bands=6):
    """Split gaps into magnitude-based bands"""
    
def test_lognormal_fit(gaps, band_name):
    """
    1. Log-transform gaps
    2. Fit lognormal (equivalent to fitting normal to log-gaps)
    3. Run statistical tests
    4. Return p-values and parameters
    """
    
def visualize_qq_plots(log_gaps_by_band):
    """Generate Q-Q plots for each band"""
    
def main():
    primes = generate_primes(1e6, 1e8)
    gaps = extract_gaps(primes)
    bands = partition_by_band(gaps, primes)
    
    results = {}
    for band_name, band_gaps in bands.items():
        results[band_name] = test_lognormal_fit(band_gaps, band_name)
    
    visualize_qq_plots(results)
    print_summary(results)
```

### 6. Performance Considerations

**Prime generation:**
- Use segmented sieve for large ranges
- Consider pre-computing and caching primes
- Target: < 5 minutes for 10^6 to 10^8 range

**Memory:**
- Store gaps as numpy arrays (int32 sufficient)
- Stream processing for scales > 10^8

### 7. Validation Targets (from your research)

**Expected results:**
- 100% of bands pass lognormal tests (p > 0.05)
- Lognormal parameters shift predictably with scale:
  - μ increases with prime magnitude
  - σ remains relatively stable or shows systematic trend
- Q-Q plots show strong linear correlation (R² > 0.95)

**Comparison to exponential:**
- If gaps were exponential (Cramér model), p-values would be << 0.05
- Include negative control: test exponential fit (should fail)

### 8. Output Format

**Terminal output:**
```
=== Lognormal Structure Test ===
Scale: 10^6 to 10^8
Bands: 6

Band 1 [10^6 - 10^6.33]:
  μ: 2.45, σ: 0.62
  KS test: p = 0.23 ✓
  AD test: p = 0.18 ✓
  
Band 2 [10^6.33 - 10^6.67]:
  μ: 2.51, σ: 0.59
  KS test: p = 0.31 ✓
  AD test: p = 0.27 ✓
  
[... bands 3-6 ...]

Summary: 6/6 bands (100%) pass lognormal tests
Conclusion: Gaps follow lognormal distribution
```

**Visual output:**
- Save Q-Q plots as PNG: `lognormal_qq_plots.png`
- Save distribution histograms: `lognormal_distributions.png`

### 9. Documentation

**In-code comments:**
- Explain log-transformation rationale
- Document statistical test interpretation
- Note deviations from classical Cramér model

**README section:**
```markdown
## Running the Demonstration

```bash
python lognormal_structure_demo.py
```

### What This Proves

This demonstration shows that prime gaps follow **lognormal** 
distributions, not exponential distributions as predicted by 
the Cramér random model. Key findings:

1. 100% of tested bands pass lognormal goodness-of-fit tests
2. Log-transformed gaps fit normal distributions (Q-Q linearity)
3. This implies **multiplicative randomness** in gap formation

### Interpretation

Lognormal distributions arise from multiplicative processes.
The presence of lognormal gap structure suggests prime gaps
are generated by a multiplicative stochastic process, not
the additive/independent process assumed by classical models.
```

### 10. Acceptance Criteria

- [ ] Code runs without errors on Python 3.9+
- [ ] All 6 bands show p-values > 0.05 for lognormal tests
- [ ] Q-Q plots demonstrate visual linearity
- [ ] Execution completes in < 10 minutes
- [ ] Output clearly states: "X/6 bands pass lognormal tests"
- [ ] Includes comparison to exponential fit (should fail)
- [ ] Self-contained: no external data files required

## Stretch Goals

1. **Interactive visualization**: Plotly/Bokeh for web-based exploration
2. **Scale parameter sweep**: Test multiple ranges (10^5-10^6, 10^7-10^8, etc.)
3. **Parallel processing**: Speed up band analysis with multiprocessing
4. **Publication-ready plots**: High-resolution figures with LaTeX labels
5. **Docker container**: Fully reproducible environment

## References

### Reference Implementation (Playground)

The complete validated implementation is available in the playground repository:

**Primary Source:** [PR-0002_prime_gap_analysis](https://github.com/zfifteen/playground/tree/main/experiments/PR-0002_prime_gap_analysis)

This experiment contains:
- **Core gap analysis logic**: `src/` directory with extraction, banding, and statistical testing
- **Lognormal fitting procedures**: Statistical distribution fitting for gaps
- **Ljung-Box autocorrelation testing**: ACF calculation and significance testing  
- **Validated results**: [RESULTS.md](https://github.com/zfifteen/playground/blob/main/experiments/PR-0002_prime_gap_analysis/RESULTS.md) with complete empirical data
- **Methods specification**: [SPEC.md](https://github.com/zfifteen/playground/blob/main/experiments/PR-0002_prime_gap_analysis/SPEC.md) detailing implementation approach
- **Cross-scale analysis**: [CROSS_SCALE_ANALYSIS.md](https://github.com/zfifteen/playground/blob/main/experiments/PR-0002_prime_gap_analysis/CROSS_SCALE_ANALYSIS.md) showing convergence patterns

**Supporting Libraries:**
- **lognormal_prefilter**: [GitHub link](https://github.com/zfifteen/playground/tree/main/experiments/lognormal_prefilter) - Gap transformation pipeline (extract → log-transform → normalize)
- **loglog_kernel**: [GitHub link](https://github.com/zfifteen/playground/tree/main/experiments/loglog_kernel) - Doubly-logarithmic transforms (if needed for scale analysis)

**Performance Reference:** [PR-0003_prime_log_gap_optimized](https://github.com/zfifteen/playground/tree/main/experiments/PR-0003_prime_log_gap_optimized)
- Optimized implementations for larger scales
- [PERFORMANCE_ANALYSIS.md](https://github.com/zfifteen/playground/blob/main/experiments/PR-0003_prime_log_gap_optimized/PERFORMANCE_ANALYSIS.md) with benchmarks
- Memory-efficient banding strategies

**Expected Results (from playground validation):**

| Scale | N Primes | Lognormal Bands Pass | ACF Significance |
|-------|----------|---------------------|------------------|
| 10^6  | 78,498   | 100% (6/6)          | 63% lags         |
| 10^7  | 664,579  | 100% (6/6)          | 85% lags         |
| 10^8  | 5,761,455| 100% (6/6)          | 98% lags         |

Your implementation should reproduce these results.



- REPO_STORY.md: Discovery context
- PLAYGROUND_REFERENCES.md: Complete catalog of playground experiments
- PLAYGROUND_REFERENCES.md: Link to original experiments
- Your published results: 100% lognormal band coverage at 10^8 scale
