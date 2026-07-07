Aggregation YAML Configuration
================================

This page describes the structure and fields of the YAML file used to
configure the standalone **Aggregation** module in VCasT. This file controls
which input file is aggregated, how rows are grouped, and where the
aggregated results are written.

Example YAML File
-------------------

.. literalinclude:: ../../_static/cfg_examples/agg_config.yaml
   :language: yaml
   :caption: Sample Aggregation Configuration YAML

Configuration Sections
------------------------

Input Data
^^^^^^^^^^^

- **input_file**:
  Path to a tab-separated file of per-record statistics, such as the output
  of the :doc:`Statistics <../stat/stat>` or
  :doc:`MET Stat <../met_stat/met_stat>` components.

Grouping and Metrics
^^^^^^^^^^^^^^^^^^^^^^

- **group_by**:
  List of column names to group rows by before averaging (e.g.
  ``["model", "fcst_lead"]``). The order of columns determines the grouping
  hierarchy in the output.

- **stats**:
  List of metric column names to average within each group (e.g.
  ``["rmse", "bias"]``). Each metric produces one output column named after
  the metric.

- **ci**:
  [``true``, ``false``] Whether to compute a 95% confidence interval around
  each group mean, using a Student's t-distribution. When enabled, each
  metric column is accompanied by ``{metric}_bcl`` (lower bound) and
  ``{metric}_bcu`` (upper bound) columns.

Output
^^^^^^^

- **output_agg_file**:
  Path where the aggregated, tab-separated output file is written. The
  output includes one row per unique combination of ``group_by`` values,
  a ``count`` column with the number of records in that group, and the
  averaged (and optionally bounded) metric columns.

---

For a working example, see the following use case.
