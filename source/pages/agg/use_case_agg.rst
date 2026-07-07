Aggregation Use Case: Standalone Aggregation
===============================================

This use case demonstrates running the Aggregation module directly against a
pre-computed statistics file, independent of the MET Stat or Statistics
components.

Prerequisites
--------------

Before running the example, make sure VCasT is installed. Follow the
installation steps in :doc:`Quick Start Guide <../overview/quick-start>`.

Run the Example
-----------------

1. **Clone the test repository:**

   .. code-block:: bash

      git clone https://github.com/NOAA-GSL/VCasT-tests
      cd VCasT-tests/examples/Significance/ss-0002

2. **Run VCasT with the provided YAML file:**

   .. code-block:: bash

      vcast config_agg.yaml

   This reads ``../input_files/modelb-T2M.data``, groups rows by
   ``fcst_lead``, and averages the ``rmse`` and ``bias`` columns within each
   group.

YAML Configuration Explained
-------------------------------

.. literalinclude:: ../../_static/cfg_examples/agg_config.yaml
   :language: yaml
   :caption: Sample aggregation configuration

Output
-------

The output (``agg.data``) contains one row per unique ``fcst_lead`` value,
with a ``count`` column and one column per requested statistic (``rmse``,
``bias``) holding the group mean. Set ``ci: true`` in the configuration to
additionally emit ``{stat}_bcl`` / ``{stat}_bcu`` confidence-interval bound
columns for each statistic.
