Pairwise Significance Testing
=============================

The **Pairwise** module in VCasT performs statistical significance testing
between two forecasts using bootstrapped comparisons.

It is primarily used to assess whether one forecast significantly
outperforms another across a set of scores (e.g., RMSE, bias, FSS), broken
down by forecast lead time.

Given two tab-separated statistics files (typically the output of the
:doc:`Statistics <../stat/stat>`, :doc:`MET Stat <../met_stat/met_stat>`, or
:doc:`Aggregation <../agg/agg>` components) and a metric column name, VCasT:

1. Groups both inputs by ``fcst_lead``.
2. For each shared lead time, resamples the metric values with replacement
   (bootstrap) to build a distribution of the difference between the two
   models.
3. Reports the observed difference, a two-sided p-value, a 95% confidence
   interval on the difference, which model performed better, and whether the
   difference is statistically significant (``p_value < 0.05``).

.. toctree::
   :maxdepth: 1

   pairwise_config
   use_case_pairwise
