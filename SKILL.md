---
name: data-mystic
version: 2.1.0
description: Advanced pattern recognition and insight generation from structured/unstructured data
author: SMOUJBOT
tags:
  - pattern-recognition
  - data-alchemy
  - insight-generation
  - anomaly-detection
  - correlation-analysis
  - time-series
dependencies:
  - python>=3.9
  - pandas>=2.0.0
  - numpy>=1.24.0
  - scikit-learn>=1.3.0
  - prophet>=1.1.0
  - matplotlib>=3.7.0
  - seaborn>=0.12.0
  - networkx>=3.0.0
  - scipy>=1.10.0
  - imbalanced-learn>=0.10.0
  - ydata-profiling>=4.5.0
system_requirements:
  memory_min: 2GB
  memory_recommended: 8GB
  disk_space: 500MB
  python_packages_install: pip install -r requirements.txt
---

# Data Mystic

Pattern recognition engine that reveals hidden structures, correlations, and anomalies in seemingly random datasets. Transforms raw data into actionable insights through statistical alchemy and machine learning.

## Purpose

Data Mystic addresses real-world scenarios where data appears chaotic but contains latent patterns:

- **User behavior analysis**: Detect subtle behavioral shifts in app usage logs before churn occurs
- **Financial anomaly detection**: Identify fraud patterns in transaction streams that evade rule-based systems
- **IoT sensor correlation**: Find hidden dependencies between sensor readings that predict equipment failure
- **Time-series forecasting**: Extract seasonality and trends from noisy operational metrics
- **Customer segmentation**: Discover natural groupings in transaction data beyond simple RFM analysis
- **Supply chain optimization**: Identify bottlenecks and cascading effects in logistics data
- **Security log mining**: Detect APT patterns in firewall logs that span weeks

## Scope

### Commands

```bash
# Analyzes CSV/JSON for correlation patterns with statistical significance
data-mystic correlate --input data.csv --target conversion_rate --threshold 0.3 --method pearson/spearman/kendall

# Detects anomalies using isolation forest or local outlier factor
data-mystic anomalies --input metrics.jsonl --contamination 0.01 --method isolation/LOF --output anomalies.json

# Time-series decomposition and forecasting with Prophet
data-mystic forecast --input timeseries.csv --date-column timestamp --value-field revenue --periods 30 --interval daily/weekly/monthly

# Clustering analysis with automatic optimal cluster determination
data-mystic cluster --input user_features.csv --method kmeans/dbscan/hierarchical --auto-k true --max-clusters 10

# Pattern mining in sequential/categorical data
data-mystic sequence --input clickstream.log --pattern-length 3-5 --min-support 0.02 --algorithm prefixspan/span

# Network/graph analysis for relationship data
data-mystic graph --input relationships.csv --source user_id --target product_id --detect-communities true --centrality pagerank

# Full data profiling with automated insight generation
data-mystic profile --input dataset.csv --report-format html/markdown/json --minimal true/false
```

## Work Process

### 1. Data Ingestion and Validation
```bash
# Real command with actual flags
data-mystic profile --input /var/log/app/events_2024.jsonl \
  --date-column @timestamp \
  --categorical-threshold 0.05 \
  --missing-threshold 0.3
```

Validates file existence, format compatibility, and basic schema. Rejects files with excessive missing values (>30% by default). Auto-detects CSV/JSON/JSONL/Parquet.

### 2. Exploratory Analysis
Computes:
- Distribution statistics (skewness, kurtosis)
- Missing value patterns (MCAR/MAR/MNJ)
- Cardinality per column
- Correlation matrix with p-values
- Temporal span for date columns

### 3. Pattern Detection Phase
Executes selected analysis:

**Correlation analysis**:
```bash
data-mystic correlate --input sales_data.csv \
  --target revenue \
  --threshold 0.25 \
  --method spearman \
  --p-value 0.05 \
  --partial-control month,region
```
Runs partial correlation controlling for confounders. Bonferroni correction for multiple testing.

**Anomaly detection**:
```bash
data-mystic anomalies --input server_metrics.jsonl \
  --method isolation \
  --contamination 0.001 \
  --features cpu,memory,network_in,network_out \
  --time-window 5m
```
Returns anomaly scores and binary flags. Exports to JSON with timestamps.

### 4. Insight Generation
Applies natural language generation templates:
- "Column X shows strong correlation (r=0.82, p<0.001) with target"
- "Detected 147 anomalies in last 24h (expected: 12)"
- "Optimal cluster count: 5 (silhouette score: 0.73)"
- "Seasonal pattern: weekly cycle with 42% variance explained"

### 5. Output and Visualization
Generates:
- Summary report (markdown/HTML)
- Anomaly CSV with scores
- Correlation heatmap (PNG)
- Cluster visualization (t-SNE/UMAP)
- Forecast plot with confidence intervals

## Golden Rules

1. **Never assume Gaussian distributions**: Always test normality (Shapiro-Wilk) before applying parametric methods. Default to non-parametric (Spearman, Mann-Whitney) when p<0.05 for normality tests.

2. **Multiple comparison correction mandatory**: When testing >5 hypotheses, apply Benjamini-Hochberg FDR. Never report raw p-values without correction.

3. **Anomaly thresholds must be validated**: Contamination parameter cannot exceed 5% without explicit `--force` flag. Always review top 20 anomalies manually.

4. **Temporal leakage prevention**: When forecasting, split data temporally (no random splits). Use `--test-size` as last N periods, not random sample.

5. **Feature scaling context**: Standardize only within training split. Never scale using future data. Document scaling method in outputs.

6. **Categorical encoding**: Use target encoding for high-cardinality (>20 levels) with cross-validation. Never use label encoding for nominal data.

7. **Cluster stability**: Run clustering with 10 random seeds; coefficient of variation < 0.1 required for stable clusters. Report instability if violated.

8. **Minimum viable sample**: Require minimum 30 samples per group for statistical tests. Abort with error if violated.

## Examples

### Example 1: Detect Fraud in Transaction Stream

```bash
# Input: transactions.csv (10M rows, columns: user_id, amount, merchant_category, timestamp, ip_country)
data-mystic anomalies \
  --input transactions.csv \
  --features amount,merchant_category_freq \
  --method isolation \
  --contamination 0.001 \
  --time-column timestamp \
  --user-column user_id \
  --output fraud_indicators.csv
```

Output fraud_indicators.csv:
```
timestamp,user_id,amount,anomaly_score,is_anomaly,reason
2024-01-15 14:23:01,user_8892,15000,0.89,true,amount-outlier
2024-01-15 14:25:17,user_1120,8900,0.76,true,geographic-jump
```

Report excerpt:
```
=== ANOMALY DETECTION REPORT ===
Total transactions analyzed: 10,234,561
Anomalies detected: 12,345 (0.12%)
Expected under null: 10,234 (0.10%)
P-value (Poisson): 0.03

Top patterns:
- 67% of anomalies involve amounts >5x user's 30-day average
- 23% involve country changes within 2 hours
- 11% involve merchants never previously used

Recommendation: Trigger step-up authentication for transactions scoring >0.75
```

### Example 2: Forecast Server Capacity

```bash
# Input: cpu_metrics.csv (2 years hourly data)
data-mystic forecast \
  --input cpu_metrics.csv \
  --date-column hour \
  --value-field cpu_utilization \
  --periods 168 \
  --interval hourly \
  --seasonality-mode multiplicative \
  --changepoint-prior-scale 0.05 \
  --output forecast_2024_Q1.json
```

Output forecast_2024_Q1.json:
```json
{
  "model": "Prophet",
  "metrics": {
    "mape": 8.3,
    "rmse": 4.2,
    "coverage_95": 0.89
  },
  "forecast": [
    {"ds": "2024-01-01 00:00:00", "yhat": 45.2, "yhat_lower": 41.1, "yhat_upper": 49.3},
    {"ds": "2024-01-01 01:00:00", "yhat": 42.1, "yhat_lower": 38.0, "yhat_upper": 46.2}
  ],
  "seasonality": {
    "weekly": {
      "period": 168,
      "mode": "multiplicative",
      "fourier_order": 5
    }
  }
}
```

### Example 3: Customer Segmentation from Behavior Logs

```bash
# Input: user_features.csv (features: session_count, avg_session_duration, feature_usage_vector)
data-mystic cluster \
  --input user_features.csv \
  --method kmeans \
  --auto-k true \
  --max-clusters 12 \
  --elbow-threshold 0.15 \
  --features session_count,avg_duration,feature_1,feature_2,feature_3 \
  --scale-standard true \
  --output clusters/
```

Generates:
- `clusters/centers.csv`: cluster centroids
- `clabels assignments.csv`: user_id → cluster mapping
- `clusters/visualization.png`: t-SNE plot colored by cluster
- `clusters/profile.md`: cluster characteristics

```
# Cluster Profiles

## Cluster 0 (Power Users) - 2,341 users (5.2%)
- Avg sessions/day: 8.3 (vs population 2.1)
- Feature adoption: 92% (vs 47%)
- Churn rate: 2%/month (vs 8%)
**Action**: Premium upsell candidates

## Cluster 3 (At-Risk) - 5,112 users (11.4%)
- Session duration declining 15% WoW
- Feature usage dropped 60% in 30 days
- Last login >14 days: 67%
**Action**: Winback campaign
```

## Environment Variables

```bash
# Set for reproducible results
DATA_MYSTIC_RANDOM_SEED=42

# Configure memory limits (in MB)
DATA_MYSTIC_MEMORY_LIMIT=4096

# Enable debug logging
DATA_MYSTIC_LOG_LEVEL=DEBUG

# Custom correlation threshold default
DATA_MYSTIC_DEFAULT_CORRELATION_THRESHOLD=0.2

# Parallel processing threads
DATA_MYSTIC_N_JOBS=-1  # all cores

# Disable auto-visualization (faster processing)
DATA_MYSTIC_NO_PLOT=true
```

## Requirements

```
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
prophet>=1.1.0
matplotlib>=3.7.0
seaborn>=0.12.0
networkx>=3.0.0
scipy>=1.10.0
imbalanced-learn>=0.10.0
ydata-profiling>=4.5.0
joblib>=1.3.0
```

Install:
```bash
pip install data-mystic
```

## Verification

Check installation and capabilities:
```bash
# Basic smoke test
data-mystic --version

# Validate dependencies
data-mystic doctor

# Run benchmark on sample data
data-mystic profile --input tests/sample/sales_sample.csv --report-format markdown --minimal true

# Verify all subcommands available
data-mystic list-commands
```

Expected output for `data-mystic doctor`:
```
✓ Python 3.11.5 detected
✓ pandas 2.0.3 (OK)
✓ scikit-learn 1.3.0 (OK)
✓ prophet 1.1.5 (OK)
✓ matplotlib 3.7.1 (OK)
✓ All dependencies satisfied
✓ Memory available: 15.2GB (min: 2GB)
✓ Disk space: 142GB (min: 500MB)
Ready for data alchemy.
```

## Troubleshooting

### "MemoryError: Unable to allocate array"
**Cause**: Dataset too large for memory.  
**Fix**: Use sampling or chunking:
```bash
data-mystic profile --input huge.csv --sample 100000  # first 100k rows
data-mystic correlate --input huge.csv --chunk-size 50000
```

### "ValueError: Cannot convert non-finite values"
**Cause**: Infinite or NaN values in numeric columns.  
**Fix**: Enable automatic imputation:
```bash
data-mystic profile --input dirty.csv --impute-strategy median
```

### "RuntimeError: Distributed scheduler failed"
**Cause**: Parallel processing with too many threads.  
**Fix**: Limit threads:
```bash
export DATA_MYSTIC_N_JOBS=2
```

### "ImportError: Prophet dependencies missing"
**Cause**: PyStan installation failure.  
**Fix**: Install via conda or use cmdstanpy backend:
```bash
pip install prophet
# OR
conda install -c conda-forge prophet
```

### "No patterns detected (all correlations < threshold)"
**Cause**: Threshold too high or truly no linear relationships.  
**Fix**: Lower threshold or switch method:
```bash
data-mystic correlate --input data.csv --threshold 0.1 --method spearman
```

### "Cluster instability detected"
**Cause**: Data not naturally clustering or features not scaled.  
**Fix**: Try different method or verify scaling:
```bash
data-mystic cluster --input data.csv --method dbscan --eps 0.5 --min-samples 10
```

## Rollback Commands

Data Mystic is read-only by design. No rollback needed. However, to undo analysis outputs:

```bash
# Remove generated reports
rm -rf analysis_output/
rm analysis_report.html

# Clear cached processed datasets
rm ~/.cache/data-mystic/*.parquet

# Reset configuration to defaults
unset DATA_MYSTIC_RANDOM_SEED
unset DATA_MYSTIC_LOG_LEVEL

# If large temporary files created, clean them
find /tmp -name "data-mystic-*" -type f -mtime +1 -delete
```

For accidental data modification (if `--overwrite` was used on source file):
```bash
# Versioned sources: git restore data.csv
# Non-versioned: Restore from backup if created via --backup flag
data-mystic restore-backup --input data.csv --backup-file data.csv.backup_20240115_142300
```

## Performance Tuning

For datasets >1GB:
```bash
# Use all CPU cores
export DATA_MYSTIC_N_JOBS=-1

# Disable plots for 40% speedup
export DATA_MYSTIC_NO_PLOT=true

# Increase chunk size for sequential reads
data-mystic profile --input massive.csv --chunk-size 100000
```

## Advanced Usage

Pipe commands together:
```bash
# Extract anomalies, then cluster them
data-mystic anomalies --input events.csv --method isolation | \
  data-mystic cluster --method kmeans --auto-k true
```

Combine with jq for JSON filtering:
```bash
data-mystic forecast --input metrics.jsonl | \
  jq '.forecast[] | select(.yhat_upper > 90)'
```

Schedule hourly anomaly detection:
```bash
cron: 0 * * * * data-mystic anomalies --input /var/log/hourly.jsonl --output /alerts/$(date +\%Y\%m\%d_\%H).json
```

---

Data Mystic operates under constraint: correlations ≠ causation. All insights require domain validation before action.
```