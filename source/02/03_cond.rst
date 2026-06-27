3. Set calculation conditions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Launch the iRIC software and select "Rainfall-Runoff-Inundation v1.4.2.2" from "New Project."

RRI on iRIC primarily involves specifying and creating grid and grid attribute values through the "Calculation Conditions" menu.

Click on "Calculation Conditions > Settings."

- Please note that the screenshots may slightly differ from the latest interface, but the content remains the same.

3.1 Creating Grids and Grid Attributes
+++++++++++++++++++++++++++++++++++++++
Grids and grid attributes are created using the **RRI DEM and River Grid Creator**.
Go to "Grid > Select Grid Algorithm > RRI DEM and River Grid Creator" to launch it.

.. figure:: img/select_grid_algo.jpg
   :scale: 50%
   :alt:

.. note::
   In the previous version, grids were created using the "Make Geographic Condition Only" mode in the calculation conditions. In the new version, this feature is a legacy setting. For new projects, please use the RRI DEM and River Grid Creator.

   If iRIC is already installed, launch the maintenance tool, run "Update components", and in "Add or remove components", check "RRI DEM and River Grid Creator" under "Grid generators" to update.

The "Grid Generation" dialog will open. Configure the settings as follows.

**"Terrain Data" tab**

.. figure:: img/rri_demAdjust2_main.jpg
   :scale: 50%
   :alt:

- Coordinate System: Lat/Lon
- DEM file: Hydrologically corrected elevation file (.asc)
- DIR file: Surface flow direction data file (.asc)
- ACC file: Number of upstream drainage grids file (.asc)

**"River Shape" tab**

.. figure:: img/rri_demAdjust2_river.jpg
   :scale: 50%
   :alt:

- River Channel Cell ACC Threshold: ACC threshold for designating cells as river channels
- River Width: :math:`W = C_w A^{S_w}`
- River Depth: :math:`D = C_d A^{S_d}`
- Levee: Height [m], Minimum ACC for height setting

Click "Generate Grid(C)" to start processing.
When processing is complete, grids and grid attributes are automatically created.
Save the project.

-----

Once the above process is complete, you will be able to review the grids and grid attributes for the calculation.

To review the grids and grid attributes, it is necessary to designate the coordinate system. After setting your own coordinate, click 'OK'.

.. figure:: img/coordinate_en.jpg
   :scale: 40%
   :alt:

After setting the coordinate system, you will be able to review the grid shape and grid attributes.

.. figure:: img/grid_shape_en.jpg
   :scale: 50%
   :alt:

   Grid system

.. figure:: img/ini_elv_en.jpg
   :scale: 50%
   :alt:

   Cell attributes：Elevation[m]　DEM 

.. figure:: img/ini_dir_en.jpg
   :scale: 50%
   :alt:

   Cell attributes：DIR　Flow direction; East(1),South-East(2),South(4),South-West(8),West(16),North-West(32),North(64),North-East(128)

.. figure:: img/ini_acc_en.jpg
   :scale: 50%
   :alt:

   Cell attributes：ACC;　Number of upstream cells. Since cell size is set to be uniform for the entire basin, multiplying this value by the area per cell will give the upstream drainage area for that cell.

.. figure:: img/ini_width_en.jpg
   :scale: 50%
   :alt:
   
   Cell attributes：Width[m]　River width; Defined as :math:`W = C_w A^{S_w}` 

.. figure:: img/ini_dep_en.jpg
   :scale: 50%
   :alt:

   Cell attributes：Depth[m] River depth; Defined as :math:`D = C_d A^{S_d}`

.. figure:: img/ini_height_en.jpg
   :scale: 50%
   :alt:

   Cell attributes：Levees are set in the red-colored locations.  
   Levees with the height specified by the levee height [m] are uniformly set in cells where the number of upstream drainage pixels exceeds the levee cell threshold.

----

3.2 Rainfall conditions
++++++++++++++++++++++++++++++
The next step is to set the rainfall conditions.  
Prepare the rainfall data for the target region and period in the format described in "2. Preparation for a rainfall dataset".  

Set the following values for your own prepared data:

.. list-table:: Rainfall Data Settings
   :widths: 70 30
   :header-rows: 1

   * - Screen
     - Conditions
   * - .. image:: img/cond_2_en.jpg
     - | - Specify the rainfall data file
       | - Specify xllcorner_rain in longitude (radians)
       | - Specify yllcorner_rain in latitude (radians)
       | - Specify cellsize_rain_x as dx (radians)
       | - Specify cellsize_rain_y as dy (radians)


----

3.3 Time control
++++++++++++++++++++++++++++++
Set the number of hours for the simulation.  
Regardless of the duration of the rainfall data or boundary data, the calculation period will be determined by the value set here.  
Though the calculation time step is automatically controled by the adaptive Runge-Kutta method,
initial time steps for slope calculations and river channel calculations can be specified separately.  
Set the number of output times for the calculation results, excluding the initial time output.

.. list-table:: Calculation time control
   :widths: 80 20
   :header-rows: 1

   * - Screen
     - Conditions
   * - .. image:: img/cond_3_en.jpg
     - | - Simulation Time (hour)
       | - Time Step for Slope Simulation (sec)
       | - Time Step for River channel
       | Simulation (sec)
       | - Number for output times

----

3.4 River simulation parameters
++++++++++++++++++++++++++++++
The parameters for the river channel simulation are as follows:

- Threshold of flow accumulation: Cells are designated as river channels if their upstream contributing area (number of cells) exceeds this threshold
- Manning's roughness for River Region: This parameter sets the Manning's roughness coefficient for cells identified as river channels.

.. list-table:: River channel settings
   :widths: 70 30
   :header-rows: 1

   * - Screen
     - Conditions
   * - .. image:: img/cond_4_en.jpg
     - | - Manning's roughness for River Region
     - | - Threshold of flow accumulation

----

3.5 Slope simulation parameters
++++++++++++++++++++++++++++++
The parameters for the slope simulation can be defined individually for each land use category. 
Land use categories are assigned to each grid cell as attributes, with values from 1 to 5, allowing for up to five distinct categories. 
To set these up, import the land use data file (ldu_export.asc downloaded in step "1. Preparation for the Basin Terrain Dataset") by right-clicking "Land Use Type" in the Object Browser and selecting "Import". 
Once imported, the display will visually differentiate the cells by color, based on their assigned land use flags.

.. figure:: img/geo_lnd_en.jpg
   :width: 420pt
   :alt:

   Imported Land Use Data

Mapping the Imported Land Use Data to the Grid:
To map the imported land use data to the grid, go to "Grid" > "Attribute Mapping" and click "Execute". 
This will open a window where you can specify the attribute to map. Select "Land Use Type" and click the "OK" button.

.. figure:: img/select_attr_en.jpg
   :width: 210pt

   Attribute Mapping Selection Window

Once the mapping process is finished, the land use types will be associated with the grid cells. 
You can verify this by navigating to "Grid" > "Cell Attributes" in the menu and then checking the box labeled "Land Use Type". 
This will display the mapped land use type as a cell attribute.

.. figure:: img/ini_lnd_en.jpg
   :width: 420pt
   :alt:

   Cell attributes：Land Use Type

The following screen allows you to specify model parameter sets. Each set corresponds to the numerical values 1 to 5 assigned as grid attributes to each cell.

.. list-table:: Slope simulation parameters
   :widths: 70 30
   :header-rows: 1

   * - Screen
     - Conditions
   * - .. image:: img/cond_5_en.jpg
     - | - Information regarding
       | model parameters can be
       | found in the `RRI manual <https://www.pwri.go.jp/icharm/research/rri/rri_top.html>`_ .

----

3.6 Hotstart
++++++++++++++++++++++++++++++
Hot start is used when you want to start a calculation using the results of a previous calculation as the initial values. This screen allows you to set the previously calculated slope water depth (hs), river channel water depth (hr), etc., as initial conditions.

.. list-table:: Hotstart
   :widths: 70 30
   :header-rows: 1

   * - Screen
     - Conditions
   * - .. image:: img/cond_8_en.jpg
     - | - Hotstart 


3.7 Settings for Advanced Users
++++++++++++++++++++++++++++++
The RRI model uses the Adaptive Runge-Kutta method, which automatically adjusts the calculation time step (default: 600 seconds for slopes, 60 seconds for river channels, see Section 3.3) 
to ensure that the error in the convergence calculation is below a certain value (eps). 
When using a small mesh size, such as 10m, in the RSR model analysis, reducing the value of eps (for example, by one order of magnitude) can stabilize the calculation.
Similarly, ddt_min_riv is the truncation error for river channel calculations, and ddt_min_slo is the truncation error for slope calculations.
When using a small mesh size like 10m, reducing these values by about one order of magnitude can help prevent calculation failures.

.. list-table:: Settings for Advanced Users
   :widths: 70 30
   :header-rows: 1

   * - Screen
     - Conditions
   * - .. image:: img/RSR_cond83_en.jpg
     - | - Settings for Advanced Users 

