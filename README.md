pip install hfhvar

## Quick start

After installation, you can estimate an HF-HVAR and plot impulse responses in just a few lines:

```python
import pandas as pd
import numpy as np
import hfhvar
from hfhvar.config import ModelConfig
from hfhvar.io import as_numpy
from hfhvar.bootstrap import residual_bootstrap_irf
from hfhvar.plotting import plot_irfs

# Load example data (your own CSV/XLSX with daily macro variables)
df = pd.read_excel("data/df_epu.xlsx", index_col=0, parse_dates=True)
Y, names = as_numpy(df)

# Configure model: 1 quarterly lag, shock on the first variable
cfg = ModelConfig(
    p_d=1,
    shock_index=0,
    shock_size=90.0,
    bootstrap_draws=10,
    period_display_quarters=40
)

# Estimate and bootstrap impulse responses
irf, lo, hi = residual_bootstrap_irf(Y, cfg)

# Plot with 68% bootstrap confidence bands
plot_irfs(irf, lo, hi, cfg.periods_per_quarter, names)
