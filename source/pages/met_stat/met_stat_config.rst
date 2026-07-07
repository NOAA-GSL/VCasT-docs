MET Stat YAML Configuration
===========================

This page describes the structure and fields of the YAML file used to configure the **MET Stat** module in VCasT. This file controls how `.stat` files are read, filtered, and processed to extract and aggregate forecast verification statistics.

Example YAML File
-----------------

.. literalinclude:: ../../_static/cfg_examples/met_stat_config.yaml
   :language: yaml
   :caption: Sample MET Stat Configuration YAML


Configuration Sections
----------------------

The YAML file is divided into several sections:

Input and Line Type Settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- **input_stat_folder**:  
  Path to the directory containing MET ``.stat`` files. All files must be located within a single folder to allow the tool to scan and process them collectively. Recursive search or multiple paths are not supported.

- **line_type**:
  Specifies the MET line type to filter (e.g., ``cnt``, ``cts``, ``ecnt``, ``mode_cts``, etc.). This setting determines which records are extracted and must be compatible with the metrics listed in ``stat_vars``. Only variables valid for the chosen line type will be available.

  ``mode_cts`` is parsed differently from the other line types: since MODE
  object-verification output files contain only ``mode_cts`` records with a
  fixed column layout, rows are matched purely by column count rather than
  by an in-row ``line_type`` field.

  For a full list of supported line types and their associated statistics, see the `MET User’s Guide - Point-Stat Output Line Types <https://metplus.readthedocs.io/projects/met/en/latest/Users_Guide/point-stat.html#output-line-types>`_ and the `MODE Output Line Types <https://metplus.readthedocs.io/projects/met/en/latest/Users_Guide/mode.html#output-line-types>`_.

Date Settings
^^^^^^^^^^^^^

- **date_column**: Column used to apply the date filter (e.g., `fcst_valid_beg`).

- **start_date** / **end_date**: Time range to include data for processing (`YYYY-MM-DD_HH:MM:SS` format).

Filtering Options
^^^^^^^^^^^^^^^^^

- **string_filters**: Filter rows by allowed string values in specific columns. Examples include:

  - **model**: List of models and/or ensemble members, e.g., ``[ens0, ens1]``.
  - **fcst_var**: List of forecast variables, e.g., ``[REFC, TMP]``.
  - **fcst_lead**: List of forecast lead times in ``HH0000`` format, e.g., ``[010000, 020000]``.


Output Options
^^^^^^^^^^^^^^

- **reformat_file**: [`true`, `false`] writes the fully filtered DataFrame to file (with all columns associated to the chosen line type option).
- **output_reformat_file**: Path to save the filtered dataset.

- **stat_vars**: List of variables to extract (e.g., `fbias`, `gss`).
- **output_file**: [`true`, `false`] exports selected statistical variables.
- **output_plot_file**: Path to save the extracted metric values.

Aggregation Options
^^^^^^^^^^^^^^^^^^^

- **aggregate**: [`true`, `false`] whether to compute group statistics (mean, CI).
- **group_by**: List of column(s) to group by (e.g., `[fcst_lead, model]`).
- **output_agg_file**: Path to save aggregated output.
- **ci**: [`true`, `false`] whether to compute confidence intervals for each metric.

---

For a working example or template, see the following use cases.