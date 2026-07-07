Statistics YAML Configuration
==============================

This page describes the structure and fields of the YAML file used to
configure the **Statistics** module in VCasT. This file controls how
forecast/reference file pairs are located, optionally interpolated, and
compared to compute verification metrics.

Example YAML File
------------------

.. literalinclude:: ../../_static/cfg_examples/stat_config.yaml
   :language: yaml
   :caption: Sample Statistics Configuration YAML

Configuration Sections
-----------------------

Date, Time, and Lead Time Settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **start_date** / **end_date**:
  Processing window, in ``YYYY-MM-DD_HH:MM:SS`` format.

- **interval_hours**:
  Time step (in hours) between successive processing dates.

- **start_lead_time** / **end_lead_time** / **interval_lead_time**:
  Optional. Defines a range of forecast lead times (in hours) to process.
  If any one is set, all three are required. If omitted, ``lead_times``
  must be supplied directly, or a single lead time of ``0`` is assumed.

- **lead_times**:
  Optional explicit list of lead times, used instead of the
  start/end/interval triplet above (e.g. ``[0, 1, 2]``).

- **members**:
  Optional list of ensemble member identifiers. If provided, VCasT loops
  over each member (deterministic mode) or treats them as one ensemble
  (``stat_type: ens``).

Forecast and Reference Settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **vars**:
  Variable name (or list of names) to process, e.g. ``T2M``. Names are
  resolved via the model/variable lookup table bundled with VCasT.

- **fcst_model** / **ref_model**:
  Model identifiers used to resolve variable names, levels, and units for
  the forecast and reference sources respectively.

- **fcst_file_template** / **ref_file_template**:
  Path templates for locating forecast and reference files. Templates
  support ``{year}``, ``{month}``, ``{day}``, ``{hour}``, ``{lead_time}``,
  ``{members}``, and the equivalent ``{valid_year}``/``{valid_month}``/
  ``{valid_day}``/``{valid_hour}`` fields for the observation valid time.

Output Configuration
^^^^^^^^^^^^^^^^^^^^^

- **output_dir**:
  Directory where the output file is written. Must already exist.

- **output_filename**:
  Name of the tab-separated output file.

Statistical Metrics
^^^^^^^^^^^^^^^^^^^^

- **stat_type**:
  ``det`` for deterministic (per-file) statistics, or ``ens`` for
  ensemble statistics computed across all ``members`` at once.

- **stat_name**:
  List of metrics to compute. Continuous metrics (``rmse``, ``bias``,
  ``mae``, ``stdev``, ``corr``, ``quantiles``) take no parameters.
  Categorical metrics (``gss``, ``fbias``, ``pod``, ``far``, ``sr``,
  ``csi``) and ``fss`` require a ``metric:fcst_threshold:ref_threshold:radius``
  or ``metric:fcst_threshold:ref_threshold:window_size`` specifier, e.g.
  ``"gss:290:290:2"`` or ``"fss:20:20:1"``.

Grid and Interpolation Settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **interpolation**:
  [``true``, ``false``] Whether forecast and reference fields should be
  interpolated onto a common target grid before computing statistics.

- **target_grid**:
  Path to a NetCDF file defining the target grid. Required when
  ``interpolation`` is ``true``.

Parallel Processing
^^^^^^^^^^^^^^^^^^^^

- **processes**:
  Number of worker processes used to compute statistics in parallel
  across dates, lead times, and members.

---

For a working example, see the ``all_stats`` cases in the
`VCasT-tests <https://github.com/NOAA-GSL/VCasT-tests>`_ repository under
``examples/Stats``.
