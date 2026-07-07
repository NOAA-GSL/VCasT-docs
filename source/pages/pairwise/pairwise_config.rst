Pairwise YAML Configuration
=============================

This page describes the structure and fields of the YAML file used to
configure the **Pairwise** significance-testing module in VCasT.

Example YAML File
-------------------

.. literalinclude:: ../../_static/cfg_examples/pairwise_config.yaml
   :language: yaml
   :caption: Sample Pairwise Configuration YAML

Configuration Sections
------------------------

Input Data Files
^^^^^^^^^^^^^^^^^^

- **input_model_A** / **input_model_B**:
  Paths to tab-separated statistics files for the two models being
  compared. Both files must contain an ``fcst_lead`` column and the column
  named by ``metric`` below.

Output Settings
^^^^^^^^^^^^^^^^^

- **output_file**:
  Path to save the resulting significance data, one row per shared
  ``fcst_lead`` value found in both input files.

Analysis Metric
^^^^^^^^^^^^^^^^^

- **metric**:
  Name of the statistic column to compare between the two models (e.g.
  ``rmse``, ``bias``, ``fss``). Must exist as a column in both input files.

Output Columns
^^^^^^^^^^^^^^^^

The output file contains the following columns for each shared lead time:

- **fcst_lead**: the forecast lead time.
- **observed_diff**: mean(Model B) − mean(Model A) for the chosen metric.
- **p_value**: two-sided bootstrap p-value for the observed difference.
- **ci_bcl** / **ci_bcu**: lower/upper bounds of the bootstrap confidence
  interval around the difference.
- **better_model**: ``"Model A"`` or ``"Model B"``, whichever had the lower
  mean value for the metric.
- **significant**: ``True`` if ``p_value < 0.05``, else ``False``.

---

For a working example, see the following use case.
