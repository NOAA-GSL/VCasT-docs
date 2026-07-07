Statistics Component
====================

The **Statistics (stat)** component is the core of VCasT. It reads forecast and
reference fields directly from GRIB2, NetCDF, or Zarr files, optionally
interpolates them onto a common target grid, and computes verification metrics
without requiring a MET pre-processing step.

Unlike the :doc:`MET Stat <../met_stat/met_stat>` component (which post-processes
existing MET ``.stat`` output), the Statistics component operates on raw model
and observation files directly, driven by a file-template configuration that
tells VCasT how to locate forecast/reference pairs across a date range.

Supported metrics (set via ``stat_name`` in the configuration file):

- **rmse**, **bias**, **mae**, **stdev**, **corr**: continuous error metrics
- **quantiles**: Q1/median/Q3, IQR, and outlier bounds
- **gss**, **fbias**, **pod**, **far**, **sr**, **csi**: categorical/contingency-table
  metrics, each requiring a forecast threshold, an observation threshold, and a
  radius of influence, e.g. ``"gss:290:290:2"``
- **fss**: Fractions Skill Score, requiring a forecast threshold, an observation
  threshold, and a neighborhood window size, e.g. ``"fss:20:20:1"``

Results are written to a single tab-separated output file with one row per
date/lead-time/variable/level (and, for ensemble runs, per member).

.. note::
   Ensemble mode (``stat_type: ens``) currently supports the ``fss`` and
   ``reliability`` metrics, computed across all listed ``members`` at once
   rather than per-member.

.. toctree::
   :maxdepth: 1

   stat_config
   use_case_stat
