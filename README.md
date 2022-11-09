<hr><br> 
<p style="font-size: 1.2em; line-height: 2em">
<b>Important notes</b> <br> The official supporting/guiding documents concerning the SVS land-surface model 
are provided at this <a href="https://wiki.usask.ca/display/MESH/Soil-Vegetation-Snow+%28SVS%29+-+Version+1">address</a>. 
<br>
Read the <a href="https://wiki.usask.ca/display/MESH/How+to+configure+MESH-SVS+for+point+mode+%281D%29%2C+including+SVS2">sample configuration</a> on how to setup an SVS (version 1) run in point-mode.
    
The source code of the SVS1 was cloned from its GitHub repository at the following 
<a href="https://github.com/VVionnet/MESH_SVS/commit/627a369ead25d470810e9350f7cfca792e7d594b"> commit </a> 
</p>

<br>
<p style="font-size: 1.2em; line-height: 2em">
    <span>Necessary steps for running SVS1 in point-mode</span>
    <ol style="font-size: 1.2em; line-height: 2em">
        <li>Compile the source code and obtain the executable file</li>
        <li>Create the required input files</li>
        <ul>
            <li>basin_forcing.met</li>
            <li>MESH_parameters.txt</li>
            <li>MESH_input_soil_levels.txt</li>
            <li>MESH_input_run_options.ini</li>
            <li>An output folder</li>
        </ul>
    </ol>
</p>
<br>
<p style="font-size: 1.2em; line-height: 2em">
The Python class only takes care of the second step. In the following lines some explanations are provided
<br> regarding the information that one needs to pass to the SVSModel class, so that the class can create
<br> the needed input files, and subsequently run the model in point-mode.
<br><br>
</p>
<p style="font-size: 1.2em; line-height: 2em">
    <b>Meteorological data</b><br>
    basin_forcing.met is the file that SVS reads to access the hourly meteorological variables needed to run the model.<br>
    We need to create a dataframe (.csv file) each row of which contains values for:
    <ul style="font-size: 1.2em; line-height: 2em">
        <li>UTC datetime</li>
        <li>air temperature (°C)</li>
        <li>precipitation volume (mm)</li>
        <li>wind speed (m/s)</li>
        <li>atmospheric pressure (Pa)</li>
        <li>specific humidity (kg/kg)</li>
        <li>shortwave radiation (w/m2)</li>
        <li>longwave radiation (w/m2)</li>
    </ul>
    <br><br>
</p>
<p style="font-size: 1.2em; line-height: 2em">
    SVSModel will take care of converting the dataframe into a basin_forcing.met file. <br>
    For this process to happen, one should provide the path to the dataframe holding the meteo data
    <br> also, the label of the columns inside the said dataframe corresponding to the required forcing varaibles:
    
<pre><code>#Example
path_to_met_data = "./dir1/dir2/data.csv"
dict_met_col_labels = dict(
    utc_dtime="dt_utc", air_temperature="Air temperature (°C)", 
    precipitation="Precipitation (mm)", wind_speed="Wind speed (m.s-1)", 
    atmospheric_pressure="Atmospheric pressure (Pa)", 
    shortwave_radiation="Shortwave radiation (W.m-2)", 
    longwave_radiation="Longwave radiation (W.m-2)", 
    specific_humidity="Specific humidity (kg.kg-1)"
)</code>
</pre>
In this dictionary, we only need to change the values, not the keys. 
</p>
<br>
<p style="font-size: 1.2em; line-height: 2em">
<b>Parametric data</b><br>
To run SVS, we need to define one or more soil layers with their corresponding thickness in the 
MESH_input_soil_levels.txt. <br> For each of these layers, we need to provide several properties inside the MESH_parameters.txt file.<br>
And lastly, MESH_input_run_options.ini is the place to set options, such as start and end time steps.  <br>
SVSModel needs certain inputs to be able to take care of the above-mentioned tasks. Some of these inputs are to be provided <br>
inside the script, and some of them are expected to exist in a dataframe (.csv file). An example of this dataframe for our case <br>
(point-scale) is provided below (first three rows): <br>
    
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>layer_nr</th>
      <th>thickness_m</th>
      <th>ends_at_m</th>
      <th>sensor</th>
      <th>soil_type</th>
      <th>sand</th>
      <th>clay</th>
      <th>Wsat</th>
      <th>Wfc</th>
      <th>Wwilt</th>
      <th>Ksat</th>
      <th>Psi_sat</th>
      <th>bcoef</th>
      <th>dry_density</th>
      <th>enclosure_slope</th>
      <th>observed_forcing</th>
      <th>zusl</th>
      <th>ztsl</th>
      <th>draindens</th>
      <th>deglat</th>
      <th>deglng</th>
      <th>vf_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.025</td>
      <td>0.025</td>
      <td>No sensor</td>
      <td>s1</td>
      <td>78.48</td>
      <td>5.59</td>
      <td>0.3269</td>
      <td>0.0579</td>
      <td>0.0083</td>
      <td>0.000023</td>
      <td>0.307</td>
      <td>2.32</td>
      <td>1603.03</td>
      <td>0.02</td>
      <td>height</td>
      <td>10.0</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>45.82</td>
      <td>-72.37</td>
      <td>13</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.025</td>
      <td>0.050</td>
      <td>No sensor</td>
      <td>s1</td>
      <td>78.48</td>
      <td>5.59</td>
      <td>0.3269</td>
      <td>0.0579</td>
      <td>0.0083</td>
      <td>0.000023</td>
      <td>0.307</td>
      <td>2.32</td>
      <td>1603.03</td>
      <td>0.02</td>
      <td>height</td>
      <td>10.0</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>45.82</td>
      <td>-72.37</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>0.050</td>
      <td>0.100</td>
      <td>sensor_75mm</td>
      <td>s1</td>
      <td>78.48</td>
      <td>5.59</td>
      <td>0.3269</td>
      <td>0.0579</td>
      <td>0.0083</td>
      <td>0.000023</td>
      <td>0.307</td>
      <td>2.32</td>
      <td>1603.03</td>
      <td>0.02</td>
      <td>height</td>
      <td>10.0</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>45.82</td>
      <td>-72.37</td>
      <td>13</td>
    </tr>
  </tbody>
</table>

<br>
</p>
<p style="font-size: 1.2em; line-height: 2em">
One needs to manually create such dataframe and provide its path in the scripts. <br>
Some note about creating this dataframe: <br>
The 'layer_nr', 'sensor', 'soil_type' are not needed, they are simply additional information we wanted to store.<br>
The 'ends_at_m' column contain the depth at which the soil layer ends, in meter. <br>
'Ksat', 'Psi_sat' are the soil hydraulic conductivity and suction at saturation. <br>
'bcoef' is the coefficient in the default SWRC model used in SVS. <br>
'dry_density' is the soil dry density in kg/m3. <br>
'enclosure_slope' is the slope of the modelled soil column. <br>
'vf_type' is the integer denoting the type of the vegetation cover, please refer to the documentation for the look-up table.
<br>
The rest of the parameters, including the static properties (i.e. not changing with the soil layer), are explained in the sample configuration. <br>
Beside providing a path to this dataframe, one needs to set values for the keys of a dictionary defined in the sample code:
<br>
<pre><code># an example 
dict_param_col_labels = dict(
# parameters for each soil layer
sand="sand", clay="clay", wsat="Wsat", wfc="Wfc", wwilt="Wwilt",
psisat="Psi_sat", bcoef="bcoef", ksat="Ksat", rhosoil="dry_density",
# scalar parameters
slop="enclosure_slope", observed_forcing="observed_forcing", zusl="zusl",
ztsl="ztsl", deglat="deglat", deglng="deglng", vf_type="vf_type",
draindens="draindens"
)</code></pre>
</p>

<p style="font-size: 1.2em; line-height: 2em">
The last part of the provided sample scripts that might need further explanations, beside the extensive comments, is the <br>
dictionary that the user needs to set the values of the defined keys. These are some of the SVS internal parameters that are <br>
read from the MESH_parameters.txt by default or as part of the modified source code used in this research.
<br>
<pre><code># an example 
ddict_model_params = dict(
    SCHMSOL="SVS", lsoil_freezing_svs1=".true.", soiltext="NIL",
    read_user_parameters=1, water_ponding=0, KFICE=0, OPT_SNOW=2, OPT_FRAC=1,
    OPT_LIQWAT=1, WAT_REDIS=0, save_minutes_csv=0, tperm=283.15,
    user_wfcdp=0.32
)</code></pre><br>
</p>
<p style="font-size: 1.2em; line-height: 2em">
The parameters that are not mentioned in the official doc, will be explained below.<br>
'user_wfcdp' is the trigger moisture. The trigger moisture is the soil volumetric water content threshold value for the last layer,
<br> before which the model will not calculate any vertical drainage from the last layer. <br>
'KFICE' and 'WAT_REDIS' are explained in the 'hydro_svs.F90' subroutine, in the source code. <br>
'OPT_SNOW', 'OPT_FRAC', and 'OPT_LIQWAT' are explained in the 'soil_freezing.F90' subroutine, in the source code.<br>
</p>

<p style="font-size: 1.2em; line-height: 2em">
Any other needed data/information to instantiate the SVSModel class is mentioned in the sample codes or the docstrings.<br>
Nevertheless, you are always welcome to contact us.
</p>
<p style="font-size: 1.2em; line-height: 2em">
<b>Output variables</b><br>
The daily aggregated output variables of the SVS runs are stored as an attribute of the SVSModel instance. <br>
There are a few columns, hence variables, that we introduce here, the rest can be figured out by reading the official doc. <br>
The first few rows, and columns, of an example of a 'dfdaily_out' attribute: <br>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DRAI</th>
      <th>ET</th>
      <th>PCP</th>
      <th>OVFLW</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>3.1570</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2018-07-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>4.7517</td>
      <td>16.7</td>
      <td>0.0</td>
      <td>2018-07-02</td>
    </tr>
  </tbody>
</table>
</p>
<p style="font-size: 1.2em; line-height: 2em">
* 'DRAI': vertical drainage (percolation) from the bottom of the last layer <br>
* 'ET': evapotranspiration <br>
* 'PCP': precipitation <br>
* 'OVFLW': overlandflow <br>
</p>
<p style="font-size: 1.2em; line-height: 2em">
<b>Sample codes</b><br>
In this notebook, we have provided codes to run SVS, single or as an ensemble (based on the manuscript), for the L1 cover. <br>
The required dataframes (explained above) are included in the 'data' folder for the rest of the covers. <br>
To perform the same operations for the other covers, simply copy the sample codes and do the following modifications: <br>
- update the path to the dataframe that contains the parameter info for the respective cover.
- update the path to the working dir, which is created specifically for the cover (case-study) you are trying to run the model for.
- (For ensembles) update the lower and upper bound of the trigger moisture values. 
</p>

<p style="font-size: 1.2em; line-height: 2em">
<b>Python and compiler versions</b><br>
All of the Python codes were written and tested using:
<br>
<pre><code>
import sys
sys.version
>>> '3.10.6 | packaged by conda-forge | (main, Aug 22 2022, 20:41:54) [Clang 13.0.1 ]'
</code></pre><br>
</p>
<p style="font-size: 1.2em; line-height: 2em">
The codes are expected to run without any problem with Python 3.8 or higher. <br>
Please do consult the official doc on how to compile the source code, that being said, we have used <br>
gfortran 8.5 on Mac OS, and gfortran 9 on a Windows machine.
</p>
