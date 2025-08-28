# HF_SVAR_Identification
The repository contains the package for implementing heterogenous VAR estimation using high-frequency data as inputs. The methodology is taken from the paper ["SVAR Identification with High-Frequency Macroeconomic Data"](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4140697). The goal of the proposed approach is to achieve the full model identification of the structural shocks by employing high-frequency macroeconomic data that allow a natural diagonalization of the contemporaneous relations.

## The Model: Heterogeous estimation
$B_0\boldsymbol{y}_{t} = B_d \boldsymbol{y}^{(d)}  + B_w \boldsymbol{y}^{(w)} + B_m \boldsymbol{y}^{(m)} + B_q \boldsymbol{y}^{(q)} + \boldsymbol{e}_t,$

with:

$\boldsymbol{y}^{(d)} = \boldsymbol{y}_{t-1},$

$\boldsymbol{y}^{(w)} = \Sigma_{i=2}^{5} \boldsymbol{y}_{t-i},$

$\boldsymbol{y}^{(m)} = \Sigma_{i=6}^{22} \boldsymbol{y}_{t-i},$

$\boldsymbol{y}^{(q)} = \Sigma_{i=23}^66 \boldsymbol{y}_{t-i}.$


## Quick start 
import pandas as pd
import hfhvar
from hfhvar.config import ModelConfig
from hfhvar.io import as_numpy
from hfhvar.bootstrap import residual_bootstrap_irf
from hfhvar.plotting import plot_irfs
import numpy as np

df = pd.read_excel('data/df_epu.xlsx', index_col=0, parse_dates=True)
Y, names = as_numpy(df)


cfg = ModelConfig(p_d=1, shock_index=0, shock_size=90.0, bootstrap_draws=10, period_display_quarters=40)
irf, lo, hi = residual_bootstrap_irf(Y, cfg)
plot_irfs(irf, lo, hi, cfg.periods_per_quarter, names)

