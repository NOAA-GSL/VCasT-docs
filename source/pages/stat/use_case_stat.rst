Statistics Use Case: HRRR Grid-to-Grid Verification
======================================================

This use case demonstrates how to run VCasT's Statistics module to compute a
mix of continuous and categorical verification metrics directly from GRIB2
forecast and reference files, without a MET pre-processing step.

Prerequisites
--------------

Before running the example, make sure VCasT is installed. Follow the
installation steps in :doc:`Quick Start Guide <../overview/quick-start>`.

Run the Example
-----------------

1. **Clone the test repository:**

   .. code-block:: bash

      git clone https://github.com/NOAA-GSL/VCasT-tests
      cd VCasT-tests/examples/Stats/all_stats-0001

2. **Run VCasT with the provided YAML file:**

   .. code-block:: bash

      vcast stat.yaml

   This locates the forecast and reference GRIB2 files under
   ``../input_files/HRRR``, optionally interpolates them, and writes the
   computed metrics to ``stat.out``.

YAML Configuration Explained
-------------------------------

Below is the content of the example ``stat.yaml``, which configures VCasT to:

- Process a single hourly time step of 2-meter temperature (``T2M``)
- Compare ``HRRR`` forecast fields against ``HRRR`` reference fields
- Compute continuous metrics (``rmse``, ``bias``, ``quantiles``, ``mae``,
  ``corr``, ``stdev``) as well as categorical metrics thresholded at
  290 K with a 2-grid-point radius of influence (``gss``, ``fbias``,
  ``pod``, ``far``, ``csi``, ``sr``)

.. code-block:: yaml
   :caption: Sample stat.yaml configuration

   start_date: "2024-04-01_02:00:00"
   end_date: "2024-04-01_02:00:00"
   interval_hours: "1"

   start_lead_time: 1
   end_lead_time: 1
   interval_lead_time: 1

   vars: "T2M"

   fcst_model: "HRRR"
   fcst_file_template: "../input_files/HRRR/hrrr.{year}{month}{day}{hour}.wrfprsf{lead_time}.T2M.grib2"

   ref_model: "HRRR"
   ref_file_template: "../input_files/HRRR/hrrr.{valid_year}{valid_month}{valid_day}{valid_hour}.wrfprsf00.T2M.grib2"

   output_dir: "."
   output_filename: "stat.out"

   stat_type: "det"
   stat_name:
     - "rmse"
     - "bias"
     - "quantiles"
     - "mae"
     - "gss:290:290:2"
     - "fbias:290:290:2"
     - "pod:290:290:2"
     - "far:290:290:2"
     - "csi:290:290:2"
     - "corr"
     - "stdev"
     - "sr:290:290:2"

   interpolation: false
   target_grid: "../input_files/REF/ref_2024040102_f001.nc"

   processes: 1

Output
-------

The output file (``stat.out``) is a tab-separated file with one row per
date/lead-time/variable/level, and one column per requested statistic (with
``quantiles`` expanding into six columns: ``25p``, ``50p``, ``75p``, ``IQR``,
``LW``, ``UW``).

To feed these results into the :doc:`Aggregation <../agg/agg>` or
:doc:`Plotting <../plot/plot>` components, point their ``input_file`` /
``vars`` settings at this output file.
