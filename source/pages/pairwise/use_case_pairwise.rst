Pairwise Use Case: Comparing Two Models
==========================================

This use case demonstrates using the Pairwise module to test whether one
model's RMSE is significantly different from another's, across forecast
lead times.

Prerequisites
--------------

Before running the example, make sure VCasT is installed. Follow the
installation steps in :doc:`Quick Start Guide <../overview/quick-start>`.

Run the Example
-----------------

1. **Clone the test repository:**

   .. code-block:: bash

      git clone https://github.com/NOAA-GSL/VCasT-tests
      cd VCasT-tests/examples/Significance/ss-0001

2. **Run VCasT with the provided YAML file:**

   .. code-block:: bash

      vcast config_comp.yaml

   This compares the ``rmse`` column of ``../input_files/modela-T2M.data``
   against ``../input_files/modelb-T2M.data``, per forecast lead time.

YAML Configuration Explained
-------------------------------

.. literalinclude:: ../../_static/cfg_examples/pairwise_config.yaml
   :language: yaml
   :caption: Sample pairwise configuration

Output
-------

The output (``diff_agg.data``) contains one row per shared ``fcst_lead``
value, with the observed RMSE difference, a bootstrap p-value, a confidence
interval on the difference, which model had the lower RMSE, and whether the
difference is statistically significant at the 5% level.

.. note::
   Because the significance test relies on random bootstrap resampling, the
   p-value and confidence interval will vary slightly between runs even with
   identical input data. The sign of ``observed_diff`` and the
   ``better_model``/``significant`` classification are stable.
