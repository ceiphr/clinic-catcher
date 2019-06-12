
**Table of Contents**
<div id="toc"></div>

# Importing and Displaying CDC Vaccine Data


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import math

# https://www.opendatanetwork.com/dataset/datahub.smcgov.org/dmz9-a27g
# df_clinic = pd.read_json('https://datahub.smcgov.org/resource/dmz9-a27g.json', orient='columns')

# https://dev.socrata.com/foundry/data.cdc.gov/pvgy-252u
# df_mal = pd.read_json('https://data.cdc.gov/resource/pvgy-252u.json', orient='columns')

# https://www.opendatanetwork.com/dataset/data.cityofnewyork.us/w9ei-idxz
df_fluvac = pd.read_json('https://data.cityofnewyork.us/resource/w9ei-idxz.json', orient='columns')

clinic_data = {
    'X Coordinate' : [],
    'Y Coordinate' : [],
    'Phone' : [],
    'Address' : [],
    'Name' : [],
    'ZIP Code' : [],
}

# Explanation for how to covert lon/lat to mercator coordinates
# https://gis.stackexchange.com/questions/156035/calculating-mercator-coordinates-from-lat-lon/156046
def merc(lat, lon):
    r_major = 6378137.000
    x = r_major * math.radians(lon)
    scale = x/lon
    y = 180.0/math.pi * math.log(math.tan(math.pi/4.0 + lat * (math.pi/180.0)/2.0)) * scale
    return (x, y)

# Loop goes through all given vaccine clinics and parses the following data points into a dictionary
for i in range(len(df_fluvac['latitude'])):
    
    # Takes given coordinates and coverts to mercator for OSM plotting
    cord = merc(df_fluvac['latitude'][i], df_fluvac['longitude'][i])
    
    # Clinic name
    clinic_data['Name'].append(df_fluvac['facility_name'][i])
    
    # Clinic phone number, address and ZIP code
    clinic_data['Phone'].append(df_fluvac['phone'][i])
    clinic_data['Address'].append(df_fluvac['address'][i])
    clinic_data['ZIP Code'].append(df_fluvac['zip_code'][i])
    
    # Coordinates for the given clinic
    clinic_data['X Coordinate'].append(cord[0])
    clinic_data['Y Coordinate'].append(cord[1])
    
# Stores dataset created above in a dataframe for easy interpretation
dataset = pd.DataFrame.from_dict(clinic_data)
dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X Coordinate</th>
      <th>Y Coordinate</th>
      <th>Phone</th>
      <th>Address</th>
      <th>Name</th>
      <th>ZIP Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-8.228747e+06</td>
      <td>4.978229e+06</td>
      <td>(718) 728-9100</td>
      <td>28-04 31st Street</td>
      <td>Newtown Pharmacy</td>
      <td>11102</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-8.224083e+06</td>
      <td>4.974014e+06</td>
      <td>(800) 358-9950</td>
      <td>84-20  Broadway</td>
      <td>Walgreens Drug Store</td>
      <td>11373</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-8.232768e+06</td>
      <td>4.954047e+06</td>
      <td>718-616-4594</td>
      <td>1601 AVENUE S</td>
      <td>Homecrest Clinic</td>
      <td>11229</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-8.235533e+06</td>
      <td>4.954290e+06</td>
      <td>(718) 513-3322</td>
      <td>260 Kings Highway</td>
      <td>Medcare Health Inc.</td>
      <td>11223</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-8.234034e+06</td>
      <td>4.977024e+06</td>
      <td>212-223-1765</td>
      <td>949 3RD AVE</td>
      <td>Duane Reade</td>
      <td>10022</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-8.219533e+06</td>
      <td>4.965528e+06</td>
      <td>718-659-9621</td>
      <td>10309 LIBERTY AVE</td>
      <td>Duane Reade</td>
      <td>11417</td>
    </tr>
    <tr>
      <th>6</th>
      <td>-8.234322e+06</td>
      <td>4.960363e+06</td>
      <td>(718) 856-2976</td>
      <td>1001 Church Avenue</td>
      <td>Drugs Care Pharmacy</td>
      <td>11218</td>
    </tr>
    <tr>
      <th>7</th>
      <td>-8.235353e+06</td>
      <td>4.976680e+06</td>
      <td>212-730-4914</td>
      <td>22 W 48TH ST</td>
      <td>Duane Reade</td>
      <td>10036</td>
    </tr>
    <tr>
      <th>8</th>
      <td>-8.225623e+06</td>
      <td>4.970008e+06</td>
      <td>(718) 386-0989</td>
      <td>66-26 metropolitan Avenue</td>
      <td>Kmart Pharamcy</td>
      <td>11379</td>
    </tr>
    <tr>
      <th>9</th>
      <td>-8.238659e+06</td>
      <td>4.955167e+06</td>
      <td>(800) 358-9950</td>
      <td>1511 86th Street</td>
      <td>Walgreens Drug Store</td>
      <td>11228</td>
    </tr>
    <tr>
      <th>10</th>
      <td>-8.230988e+06</td>
      <td>4.988955e+06</td>
      <td>(212) 491-1661</td>
      <td>600 West 168th Street</td>
      <td>Washington Hts HC *Special Event*</td>
      <td>10032</td>
    </tr>
    <tr>
      <th>11</th>
      <td>-8.235834e+06</td>
      <td>4.979878e+06</td>
      <td>212-580-0497</td>
      <td>253 W 72ND ST</td>
      <td>Duane Reade</td>
      <td>10023</td>
    </tr>
    <tr>
      <th>12</th>
      <td>-8.229405e+06</td>
      <td>4.977201e+06</td>
      <td>(718) 728-9080</td>
      <td>32-14  31st Street</td>
      <td>Rite Aid Pharmacy</td>
      <td>11106</td>
    </tr>
    <tr>
      <th>13</th>
      <td>-8.236162e+06</td>
      <td>4.971971e+06</td>
      <td>(212) 388-9348</td>
      <td>81 1st Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>10003</td>
    </tr>
    <tr>
      <th>14</th>
      <td>-8.216352e+06</td>
      <td>4.980973e+06</td>
      <td>(718) 767-6000</td>
      <td>153-65 Cross Island Pkwy</td>
      <td>Rite Aid Pharmacy</td>
      <td>11357</td>
    </tr>
    <tr>
      <th>15</th>
      <td>-8.232878e+06</td>
      <td>4.977908e+06</td>
      <td>(212) 628-1900</td>
      <td>1292 1st Avenue</td>
      <td>Tower Chemists</td>
      <td>10021</td>
    </tr>
    <tr>
      <th>16</th>
      <td>-8.232744e+06</td>
      <td>4.954945e+06</td>
      <td>(718) 998-3377</td>
      <td>1720 Kings Highway</td>
      <td>Rite Aid Pharmacy</td>
      <td>11229</td>
    </tr>
    <tr>
      <th>17</th>
      <td>-8.239093e+06</td>
      <td>4.969101e+06</td>
      <td>212-425-8460</td>
      <td>37 BROADWAY</td>
      <td>Duane Reade</td>
      <td>10006</td>
    </tr>
    <tr>
      <th>18</th>
      <td>-8.232506e+06</td>
      <td>4.972489e+06</td>
      <td>(718) 389-2403</td>
      <td>859 MANHATTAN AVE.</td>
      <td>CVS</td>
      <td>11222</td>
    </tr>
    <tr>
      <th>19</th>
      <td>-8.231611e+06</td>
      <td>4.956659e+06</td>
      <td>(718) 258-1812</td>
      <td>2577 Nostrand  Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>11210</td>
    </tr>
    <tr>
      <th>20</th>
      <td>-8.237916e+06</td>
      <td>4.974045e+06</td>
      <td>(212) 366-4085</td>
      <td>81 EIGHTH AVE.</td>
      <td>CVS</td>
      <td>10011</td>
    </tr>
    <tr>
      <th>21</th>
      <td>-8.215354e+06</td>
      <td>4.968705e+06</td>
      <td>(800) 358-9950</td>
      <td>159-34 Jamaica Avenue</td>
      <td>Walgreens Drug Store</td>
      <td>11432</td>
    </tr>
    <tr>
      <th>22</th>
      <td>-8.220357e+06</td>
      <td>4.989102e+06</td>
      <td>(718) 829-6808</td>
      <td>2748 East Tremont Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>10461</td>
    </tr>
    <tr>
      <th>23</th>
      <td>-8.229259e+06</td>
      <td>4.963234e+06</td>
      <td>(800) 358-9950</td>
      <td>1134 East New York Avenue</td>
      <td>Walgreens Drug Store</td>
      <td>11212</td>
    </tr>
    <tr>
      <th>24</th>
      <td>-8.232371e+06</td>
      <td>4.952098e+06</td>
      <td>(718) 648-6971</td>
      <td>1710 Avenue Y</td>
      <td>Stop and Shop Pharmacy</td>
      <td>11235</td>
    </tr>
    <tr>
      <th>25</th>
      <td>-8.237415e+06</td>
      <td>4.954173e+06</td>
      <td>(718) 373-8185</td>
      <td>2007 86th Street</td>
      <td>Rite Aid Pharmacy</td>
      <td>11214</td>
    </tr>
    <tr>
      <th>26</th>
      <td>-8.222459e+06</td>
      <td>4.972323e+06</td>
      <td>(718) 997-6900</td>
      <td>93-05 63rd Drive</td>
      <td>VNS Pharmacy (Neighboorhood Drug Store)</td>
      <td>11374</td>
    </tr>
    <tr>
      <th>27</th>
      <td>-8.228798e+06</td>
      <td>4.976566e+06</td>
      <td>(718) 278-2100</td>
      <td>32-87  Steinway Street</td>
      <td>Rite Aid Pharmacy</td>
      <td>11103</td>
    </tr>
    <tr>
      <th>28</th>
      <td>-8.234996e+06</td>
      <td>4.967496e+06</td>
      <td>718 260 7835</td>
      <td>100 NORTH PORTLAND AVENUE</td>
      <td>Cumberland Diagnostic and Treatment Center</td>
      <td>11205</td>
    </tr>
    <tr>
      <th>29</th>
      <td>-8.232598e+06</td>
      <td>4.979048e+06</td>
      <td>(212) 327-4757</td>
      <td>1535 2nd Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>10075</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>855</th>
      <td>-8.231358e+06</td>
      <td>4.981086e+06</td>
      <td>212-423-2042</td>
      <td>333 E 102ND ST</td>
      <td>Duane Reade</td>
      <td>10029</td>
    </tr>
    <tr>
      <th>856</th>
      <td>-8.232511e+06</td>
      <td>4.980220e+06</td>
      <td>212-722-0014</td>
      <td>1356 LEXINGTON AVE</td>
      <td>Duane Reade</td>
      <td>10128</td>
    </tr>
    <tr>
      <th>857</th>
      <td>-8.237520e+06</td>
      <td>4.974874e+06</td>
      <td>646-486-7430</td>
      <td>315 W 23RD ST</td>
      <td>Duane Reade</td>
      <td>10011</td>
    </tr>
    <tr>
      <th>858</th>
      <td>-8.238810e+06</td>
      <td>4.960115e+06</td>
      <td>718-439-0614</td>
      <td>5008 5TH AVE</td>
      <td>Duane Reade</td>
      <td>11220</td>
    </tr>
    <tr>
      <th>859</th>
      <td>-8.218140e+06</td>
      <td>4.987172e+06</td>
      <td>(718) 792-9258</td>
      <td>3590 East Tremont Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>10465</td>
    </tr>
    <tr>
      <th>860</th>
      <td>-8.234532e+06</td>
      <td>4.976128e+06</td>
      <td>(212) 758-2503</td>
      <td>757 3RD AVE.</td>
      <td>CVS</td>
      <td>10017</td>
    </tr>
    <tr>
      <th>861</th>
      <td>-8.225703e+06</td>
      <td>4.986460e+06</td>
      <td>(800) 358-9950</td>
      <td>1041 Westchester Avenue</td>
      <td>Walgreens Drug Store</td>
      <td>10459</td>
    </tr>
    <tr>
      <th>862</th>
      <td>-8.224213e+06</td>
      <td>4.987087e+06</td>
      <td>(718) 861-2359</td>
      <td>1516 Westchester Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>10472</td>
    </tr>
    <tr>
      <th>863</th>
      <td>-8.221703e+06</td>
      <td>4.986703e+06</td>
      <td>(718) 430-9513</td>
      <td>1998 Bruckner blvd</td>
      <td>Kmart Pharamcy</td>
      <td>10473</td>
    </tr>
    <tr>
      <th>864</th>
      <td>-8.229487e+06</td>
      <td>4.955497e+06</td>
      <td>(800) 358-9950</td>
      <td>2325 Flatbush Ave.</td>
      <td>Walgreens Drug Store</td>
      <td>11234</td>
    </tr>
    <tr>
      <th>865</th>
      <td>-8.234693e+06</td>
      <td>4.975224e+06</td>
      <td>212-599-7492</td>
      <td>300 E 39TH ST</td>
      <td>Duane Reade</td>
      <td>10016</td>
    </tr>
    <tr>
      <th>866</th>
      <td>-8.228602e+06</td>
      <td>4.976838e+06</td>
      <td>718-932-6950</td>
      <td>4002 BROADWAY</td>
      <td>Duane Reade</td>
      <td>11103</td>
    </tr>
    <tr>
      <th>867</th>
      <td>-8.228454e+06</td>
      <td>4.956649e+06</td>
      <td>(718) 209-8121</td>
      <td>2320 RALPH AVE.</td>
      <td>CVS</td>
      <td>11234</td>
    </tr>
    <tr>
      <th>868</th>
      <td>-8.232636e+06</td>
      <td>4.951704e+06</td>
      <td>(781) 332-8345</td>
      <td>1402 SHEEPSHEAD BAY ROAD</td>
      <td>CVS</td>
      <td>11235</td>
    </tr>
    <tr>
      <th>869</th>
      <td>-8.230802e+06</td>
      <td>4.961046e+06</td>
      <td>(718) 940-1579</td>
      <td>4102 Church Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>11203</td>
    </tr>
    <tr>
      <th>870</th>
      <td>-8.222911e+06</td>
      <td>4.991059e+06</td>
      <td>(917) 708-9700</td>
      <td>2171 White Plains Rd</td>
      <td>Pelham Pharmacy</td>
      <td>10462</td>
    </tr>
    <tr>
      <th>871</th>
      <td>-8.220072e+06</td>
      <td>4.966718e+06</td>
      <td>(718) 441-1120</td>
      <td>102-30 Atlantic Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>11416</td>
    </tr>
    <tr>
      <th>872</th>
      <td>-8.250285e+06</td>
      <td>4.948633e+06</td>
      <td>(718) 987-9727</td>
      <td>2690 HYLAN BOULEVARD</td>
      <td>CVS</td>
      <td>10306</td>
    </tr>
    <tr>
      <th>873</th>
      <td>-8.221310e+06</td>
      <td>4.990650e+06</td>
      <td>(718) 239-7569</td>
      <td>1916 WILLIAMSBRIDGE ROAD</td>
      <td>CVS</td>
      <td>10461</td>
    </tr>
    <tr>
      <th>874</th>
      <td>-8.232271e+06</td>
      <td>4.980219e+06</td>
      <td>(212) 534-6000</td>
      <td>1619 3rd Avenue</td>
      <td>Kings Third Avenue Pharmacy</td>
      <td>10128</td>
    </tr>
    <tr>
      <th>875</th>
      <td>-8.236786e+06</td>
      <td>4.967792e+06</td>
      <td>(718) 834-9003</td>
      <td>101 Clark Street</td>
      <td>Gristides Pharmacy</td>
      <td>11201</td>
    </tr>
    <tr>
      <th>876</th>
      <td>-8.241333e+06</td>
      <td>4.956225e+06</td>
      <td>(718) 491-0442</td>
      <td>9302 3rd Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>11209</td>
    </tr>
    <tr>
      <th>877</th>
      <td>-8.224834e+06</td>
      <td>4.975490e+06</td>
      <td>(718) 899-0200</td>
      <td>81-25 37th Avenue</td>
      <td>Nueva Vida Pharmacy</td>
      <td>11372</td>
    </tr>
    <tr>
      <th>878</th>
      <td>-8.228450e+06</td>
      <td>4.994478e+06</td>
      <td>(718) 432-3030</td>
      <td>21B Knolls Crescent</td>
      <td>Rite Aid Pharmacy</td>
      <td>10463</td>
    </tr>
    <tr>
      <th>879</th>
      <td>-8.222522e+06</td>
      <td>4.973102e+06</td>
      <td>(718) 760-6479</td>
      <td>61-35 Junction Blvd</td>
      <td>Costco Pharmacy</td>
      <td>11374</td>
    </tr>
    <tr>
      <th>880</th>
      <td>-8.213970e+06</td>
      <td>4.969919e+06</td>
      <td>(800) 358-9950</td>
      <td>175-06 Hillside Avenue</td>
      <td>Walgreens Drug Store</td>
      <td>11432</td>
    </tr>
    <tr>
      <th>881</th>
      <td>-8.255165e+06</td>
      <td>4.957353e+06</td>
      <td>718-616-4594</td>
      <td>2040 FOREST AVENUE</td>
      <td>Mariner's Harbor Houses CHC</td>
      <td>10303</td>
    </tr>
    <tr>
      <th>882</th>
      <td>-8.236596e+06</td>
      <td>4.976443e+06</td>
      <td>212-273-0889</td>
      <td>625 8TH AVE</td>
      <td>Duane Reade</td>
      <td>10018</td>
    </tr>
    <tr>
      <th>883</th>
      <td>-8.211336e+06</td>
      <td>4.953361e+06</td>
      <td>646-680-3000</td>
      <td>29-15 FAR ROCKAWAY BOULEVARD</td>
      <td>Advantagecare Physicans Medical Office</td>
      <td>11691</td>
    </tr>
    <tr>
      <th>884</th>
      <td>-8.228802e+06</td>
      <td>4.962434e+06</td>
      <td>(718) 345-6355</td>
      <td>1154 Clarkson Avenue</td>
      <td>Rite Aid Pharmacy</td>
      <td>11212</td>
    </tr>
  </tbody>
</table>
<p>885 rows × 6 columns</p>
</div>



# Graphing CDC Vaccine Data


```python
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure, output_notebook
from bokeh.models import HoverTool
from bokeh.tile_providers import get_provider, Vendors
from bokeh.io import show
output_notebook()

tile_provider = get_provider(Vendors.CARTODBPOSITRON)

source=ColumnDataSource(clinic_data)

# Determines size of map to create given the 
# most extreme longitude and latitude from all clinic locations
x_range=(min(clinic_data['X Coordinate']),max(clinic_data['X Coordinate']))
y_range=(min(clinic_data['Y Coordinate']),max(clinic_data['Y Coordinate']))

# Determines how to construct the map and tools for user interaction
plot = figure(title='Disease Map', 
              # Tools the user can control from right sidebar
              tools='pan,wheel_zoom', 
              # Range used to determine plot size
              x_range=x_range, y_range=y_range, 
              # Type of coordinates to plot
              x_axis_type="mercator", y_axis_type="mercator",
              # Size of the map
              width=600, height=600)

# Configure Open Street Maps to be our mapping provider
plot.add_tile(tile_provider)

# Plot circles at the coordinates for each given clinic
plot.circle(x="X Coordinate", y="Y Coordinate", size=5, fill_color="green", fill_alpha=0.8, source=source)

# When the cursor hovers over a given clinic, the name, phone number and address 
# will be taken from our dataframe and referenced in the plot.
plot.add_tools(HoverTool(tooltips=[
    ("", "@Name"),
    ("Phone", "@Phone"),
    ("Address", "@Address"),
]))

# Display map inside of this page
show(plot, notebook_handle=True)
```



    <div class="bk-root">
        <a href="https://bokeh.pydata.org" target="_blank" class="bk-logo bk-logo-small bk-logo-notebook"></a>
        <span id="1001">Loading BokehJS ...</span>
    </div>











  <div class="bk-root" id="9b7a4707-426f-4040-bfe8-2f019eba99b0" data-root-id="1004"></div>








<p><code>&lt;Bokeh Notebook handle for <strong>In[3]</strong>&gt;</code></p>



# Determine Clinics Nearby with Given Zip Code


```python
"""
Change this variable to any ZIP code within NYC to determine nearby clinics.
This will not work on a published page, the Jupyter Notebook server must be running
for code execution to occur.
"""
given_zip = 10016

# Dataset of all nearby clinics will be stored in this dictionary
nearby_clinics = {
    'Clinic Name' : [],
    'Address' : [],
}

# Parse all clinics displayed in the Bokeh OSM plot above to determine which ones
# are nearby according to ZIP code
for i in range(len(clinic_data['ZIP Code'])):
    if clinic_data['ZIP Code'][i] == given_zip:
        
        # Store dataframe clinic data in nearby clinic dictionary if nearby
        nearby_clinics["Clinic Name"].append(clinic_data['Name'][i])
        nearby_clinics["Address"].append(clinic_data['Address'][i])

# Produce new dataframe for our nearby clinics
dataset = pd.DataFrame.from_dict(nearby_clinics)
dataset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Clinic Name</th>
      <th>Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Duane Reade</td>
      <td>260 MADISON AVE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Duane Reade</td>
      <td>465 2ND AVE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NYU Hospital Center</td>
      <td>530 1st Avenue</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rite Aid Pharmacy</td>
      <td>542 Second Avenue</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Bellevue Hospital</td>
      <td>462 1 AVENUE</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Rite Aid Pharmacy</td>
      <td>524 Second Avenue</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Duane Reade</td>
      <td>401 PARK AVE S</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Duane Reade</td>
      <td>4 PARK AVE</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Duane Reade</td>
      <td>585 2ND AVE</td>
    </tr>
    <tr>
      <th>9</th>
      <td>CVS</td>
      <td>222 EAST 34TH STREET</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Duane Reade</td>
      <td>155 E 34TH ST</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Walgreens Drug Store</td>
      <td>545 3rd Avenue</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Duane Reade</td>
      <td>300 E 39TH ST</td>
    </tr>
  </tbody>
</table>
</div>


