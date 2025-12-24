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

**Q-Q Plots (Quantile-Quantile):**
- X-axis: Theoretical quantiles (normal distribution)
- Y-axis: Sample quantiles (log-transformed gaps)
- Expected: Points fall on diagonal line for lognormal data
- Include confidence bands
- One plot per band or composite plot with 6 overlays

**Distribution Comparison:**
- Histogram of log-transformed gaps with fitted normal curve
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

- REPO_STORY.md: Discovery context
- PLAYGROUND_REFERENCES.md: Link to original experiments
- Your published results: 100% lognormal band coverage at 10^8 scale
