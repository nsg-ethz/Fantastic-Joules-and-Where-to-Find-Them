# Fantastic Joules and Where to Find Them

Software Artifacts from our IMC'25 paper:

> Jacob, Romain, Lukas Röllin, Jackie Lim, Jonathan Chung, Maurice Béhanzin, Weiran Wang, Andreas Hunziker et al. "Fantastic Joules and Where to Find Them. Modeling and Optimizing Router Energy Demand." In ACM Internet Measurement Conference (IMC 2025). 2025. https://doi.org/10.1145/3730567.3732920

The repository contains the [input data](/input) and Jupyter notebooks summarizing the data analysis presented in the paper (see `xxx-analysis_IMC25.ipynb`). 
Running the notebooks produces the exact plots and tables displayed in the final paper. Generated plots are available in the [output](/output) directory.

Updated versions the analysis are also included, together with a [change log](#changelog).

## Change log

- v2:
  - **Fixed the interpolation function**  
  The original anaylsis used the numpy interpolation function, but that one does not extrapolate outside of the input data range, which is necessary in our case, as the load of many PSUs is smaller than the first set point for the interpolation.  
  This fix **significantly impacts** the outcome of the PSU right-sizing analysis. In particular, the cost of oversizing increases from 1% to 4% if we would replace all PSUs by the largest ones.
  - **Fixed the PSU load computation**   
  The PSU load was computed using the ratio of the median power measurements with the PSU capacity. This does not match with the standard definition of the PSU load, which is `P_out/PSU_capacity`, while our power measurements measure `P_in`, not `P_out`.
  - **Added modular analysis for efficiencies >1**  
  Some PSU efficiencies would evolve to be larger than 1, which makes no physical sense. This is because of the approximation we make in the PSU efficiency estimation. To address this, we added some modular capping of the efficiency values. The options are:
    - No capping,
    - Capping to 1  _< Default_
    - Capping to the Titanium curve.