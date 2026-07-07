Quick Start Guide
=================

This guide walks you through setting up VCasT in a Python environment for immediate use and testing. It assumes you have Python 3.10+ installed and access to Git.

1. Set Up a Virtual Environment
-------------------------------

Using a virtual environment is recommended to avoid dependency conflicts.

.. code-block:: bash

   python -m venv venv

Activate the environment:

.. code-block:: bash

   source venv/bin/activate

2. Install VCasT
----------------

VCasT is hosted on GitHub and can be installed directly using pip.

Full version (recommended – includes all features and dependencies):

.. code-block:: bash

   pip install "git+https://github.com/NOAA-GSL/VCasT.git@develop#egg=vcast[all]"

This installs the following dependencies:

- `matplotlib>=3.8,<4`
- `pandas>=2.2,<3`
- `scipy>=1.11,<2`
- `numpy>=1.26,<3`
- `xarray>=2024.4,<2025`
- `netCDF4>=1.6,<2`
- `zarr>=2.17,<3`
- `pygrib>=2.1,<3`

Lite version (minimal dependencies for plotting and basic metrics):

.. code-block:: bash

   pip install "git+https://github.com/NOAA-GSL/VCasT.git@develop#egg=vcast[lite]"

This installs:

- `matplotlib>=3.8,<4`
- `pandas>=2.2,<3`
- `scipy>=1.11,<2`
- `numpy>=1.26,<3`

3. Confirm the Installation
---------------------------

Verify that the `vcast` CLI is available in your environment:

.. code-block:: bash

   which vcast

4. Running VCasT
-----------------

VCasT has a single entry point:

.. code-block:: bash

   vcast <config>.yaml

There is no subcommand to choose — VCasT inspects the keys present in the
YAML file and automatically picks the matching action:

.. list-table::
   :header-rows: 1

   * - Required keys present
     - Action
     - Component
   * - ``input_stat_folder``, ``line_type``, ``date_column``, ``output_file``
     - Read/filter/aggregate MET ``.stat`` files
     - :doc:`MET Stat <../met_stat/met_stat>`
   * - ``plot_type``, ``vars``, ``output_filename``
     - Generate a plot
     - :doc:`Plotting <../plot/plot>`
   * - ``stat_name``, ``fcst_file_template``, ``ref_file_template``
     - Compute statistics directly from GRIB2/NetCDF/Zarr files
     - :doc:`Statistics <../stat/stat>`
   * - ``input_file``, ``group_by``, ``output_agg_file``
     - Aggregate a statistics file
     - :doc:`Aggregation <../agg/agg>`
   * - ``input_model_A``, ``input_model_B``, ``output_file``
     - Bootstrap significance test between two models
     - :doc:`Pairwise <../pairwise/pairwise>`

Because the action is inferred from key names, every config file must
include *all* of the required keys for exactly one action — mixing keys
from two actions, or a typo in a required key name, will cause VCasT to
report the file as unrecognized.

A ``--test-mode`` flag is also available (``vcast <config>.yaml --test-mode``);
it truncates numeric output to 10 decimal places for reproducible
byte-for-byte comparisons in automated tests, and is not needed for normal
use.

5. Run the Test Suite
---------------------

To ensure your installation is working, you can run the full test suite.

First, clone the test repository:

.. code-block:: bash

   git clone https://github.com/NOAA-GSL/VCasT-tests
   mv VCasT-tests tests

Then install the `dev` extra (which includes `pytest` and `Pillow`) if needed and run:

.. code-block:: bash

   pip install "vcast[dev]"
   pytest -v tests

This will validate the core functionality of the library, including its metric calculations and I/O behavior.

6. (Optional) Clone the VCasT Source Code
-----------------------------------------

If you intend to contribute to VCasT or explore its internals:

.. code-block:: bash

   git clone https://github.com/NOAA-GSL/VCasT
   cd VCasT
   git submodule update --init --recursive

This also pulls in any optional submodules (e.g., examples and tests).

7. Next Steps
-------------

- Read the :doc:`Introduction <introduction>` to understand what VCasT can do.
