Aggregation Component
=====================

The **Aggregation (agg)** component in VCasT is responsible for grouping and
summarizing verification statistics across dimensions such as forecast lead
time, model, or thresholds.

It reads a tab-separated file of per-record statistics (typically the output
of the :doc:`Statistics <../stat/stat>` or :doc:`MET Stat <../met_stat/met_stat>`
components), groups rows by one or more columns, and computes the mean value
of each requested metric per group. When enabled, it also computes
Student's t-distribution confidence intervals around each mean.

The Aggregation component can be invoked two ways:

- **Standalone**, via a dedicated YAML configuration file (``input_file``,
  ``group_by``, ``output_agg_file``) — see the configuration reference below.
- **Inline**, as part of a MET Stat conversion when ``aggregate: true`` is
  set in a MET Stat configuration file (see
  :doc:`MET Stat YAML Configuration <../met_stat/met_stat_config>`).

.. toctree::
   :maxdepth: 1

   agg_config
   use_case_agg
