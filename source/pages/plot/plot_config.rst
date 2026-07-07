VCasT Plotting YAML Configuration
=================================

This page describes the structure and fields of the YAML file used to configure the **Plotting** module in VCasT. This file controls how statistical outputs are visualized through line plots, performance diagrams, or reliability diagrams.

Example YAML File
-----------------

.. literalinclude:: ../../_static/cfg_examples/plot_config.yaml
   :language: yaml
   :caption: Sample Plotting Configuration YAML

Configuration Sections
----------------------

The YAML file is divided into several sections:

General Plot Settings
^^^^^^^^^^^^^^^^^^^^^

- **plot_type**:  
  Type of plot to generate. Options include:
  - `line`: for standard line plots
  - `performance_diagram`: for categorical skill scores (POD vs. Success Ratio)
  - `reliability`: for probabilistic reliability diagrams

- **plot_title**:  
  Custom title to appear at the top of the plot.

- **x_label** / **y_label**:  
  Axis labels. Optional; inferred from data if omitted.

- **output_filename**:  
  Path where the generated plot will be saved. File types like `.png`, `.pdf`, or `.svg` are supported.

Data Settings
^^^^^^^^^^^^^

- **vars**:  
  A list mapping metrics to input data paths. Each item maps a `stat_name` to a file containing the associated values.

  Example:

  .. code-block:: yaml

     vars:
       - rmse: "/path/to/input_stats.data"
       - mae: "/path/to/input_stats.data"

- **start_date**, **end_date**:  
  Optional date range filter in `YYYY-MM-DD_HH:MM:SS` format.

- **interval_hours**:  
  Forecast lead interval spacing (e.g., `"1"` for hourly).

- **average**:  
  [`true`, `false`] Whether to average over the x-axis dimension (e.g., across lead times).

- **scale**:  
  Numerical factor to multiply y-axis values (e.g., `100` to convert from fractional to percent).

Grouping and Filtering
^^^^^^^^^^^^^^^^^^^^^^

- **unique**:  
  Column used to distinguish separate series on the plot (e.g., `model`, `fcst_var`).

- **fcst_var**:  
  Optionally used to restrict plotting to specific forecast variables.

- **ci**:
  List of statistical variables for which confidence intervals should be drawn (e.g., `['rmse', 'mae']`). Requires `{var}_bcl` / `{var}_bcu` columns in the input data (as produced by the :doc:`Aggregation <../agg/agg>` component with `ci: true`).

- **ci_fill_between**:
  [`true`, `false`] Controls how the confidence interval from `ci` is rendered. When `true`, it is drawn as a shaded band via `fill_between`. When omitted or `false`, it is drawn as error bars centered on each point instead.

- **significance**:  
  [`true`, `false`] Whether to highlight statistically significant differences (requires pre-computed significance column).

Appearance and Style
^^^^^^^^^^^^^^^^^^^^

- **legend_style**:  
  [`true`, `false`] Whether to draw a legend inside or outside the plotting area.

- **legend_title**:  
  Title displayed above the legend box.

- **labels**:  
  List of labels to use in the legend for each line/series.

- **line_color**:  
  Color names or codes for each series.

- **line_marker**:  
  Marker styles for each series (e.g., `o`, `x`, `^`).

- **line_type**:  
  Line styles (e.g., `-`, `--`, `-.`).

- **line_width**:  
  Line thickness values for each series.

Axis and Grid Settings
^^^^^^^^^^^^^^^^^^^^^^

- **xlim** / **ylim**:  
  Set manual axis limits. Use a 2-element list format, e.g., `[0, 36]`.

- **xticks** / **yticks**:  
  Custom tick marks along x or y axes.

- **grid**:  
  [`true`, `false`] Whether to show grid lines in the background.

---

For examples of plotting configuration files and output images, see the following use cases.
