GridMet Performance Tests
=========================

This documentation describes the results of GridMet field detection performance tests. Each plot includes a description of its field detection being correct or incorrect.

## Performance Testing Criteria
The criteria used to identify the correct detection of fields included that field detection would be assess through visual inspection of original autocorrelogram plots compared to GridMet’s field detection plots. Only the central seven fields in the plots are used for field detection in these tests. The software was required to detect those seven fields and not incorrectly include a field from a further distance from the plot center instead of any of those fields. Size of the detected field relative to that in the recorded cell plot was not considered in evaluating correct detection because that was considered to be too subjective. GridMet was also required to not detect more than one field as a single field in a way that merges the fields. A caveat to this is that autocorrelograms can in a variety of cases merge fields at their borders, and if it appeared difficult in visual inspection to understand the separation of such fields then the automated detection was not penalized for its detection of those fields.

## Opexebo Comparisons
Opexebo was used with its default configuration for testing. Each plot has the mean grid field spacing measurements from both GridMet and Opexebo listed beneath it. A note is included commenting on the measurements when the measurements are observed to be noticably different. Opexebo was run with Python 3.9.7 and installed by the command `pip3 install opexebo`. Cell data of correlograms was formatted in a plain text comma spaced values format for use with Opexebo. The cell data used is in the folder heat_maps_real_ac_py. The python script `opexebo_stats.py` was run by the command:
<br>`$ python3 ./opexebo_stats.py`
<br>to generate statistics from Opexebo. The value for gri field spacing was read as the one displayed after `'grid_spacing': ` in command line output. Each individual grid field spacing measurement that contributed to the mean spacing measurement reported was found in the array values reported after `'grid_spacings'` in the command line output. The analyses were performed on Ubuntu 21.10.

## Plots Used to Assess Performance
<center>
<br>The performance of GridMet was observed to be 93.3% (28/30) correct cell field detections. The cells that had incorrect detections were n4 and n35. The plots can be clicked on to open each one as its full sized image.
<br>Note: the plots below are in the units of pixels that have not been converted to centimeters. For instance, in some plots, 32 pixels can represent 100 cm on the y-axis and also the x-axis.
<br><br>Cell N1; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n1.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n1.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 36.16 cm; Opexebo: 35.94 cm. (px2cm is 100/32 for n1-n29)
<br><br>Cell N2; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n2.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n2.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 38.25 cm; Opexebo: 38.32 cm.
<br><br>Cell N3 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br><br>Cell N4; Field Detection Had Very Small Issues
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n4.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n4.jpg?raw=true"  width="700"></a>
Notes: Cell N4 was observed to have a very small detection issue in the way that 2 of its 35 field area pixels are in the wrong field. This will have only very small effects on metric results.
<br>Cell spacing: GridMet: 39.70 cm; Opexebo: 39.13 cm.
<br><br>Cell N5; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n5.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n5.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 45.61 cm; Opexebo: 46.28 cm.
<br><br>Cell N6 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N7 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br><br>Cell N8; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n8.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n8.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 57.61 cm; Opexebo: 56.95 cm.
<br><br>Cell N9 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br><br>Cell N10; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n10.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n10.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 75.63 cm; Opexebo: 70.17 cm.
<br><br>Cell N11; Field Detection Had Small Issues
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n11.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n11.jpg?raw=true"  width="700"></a>
Note: Cell N11's field detection was found to have small detection issues with merged fields. These issues have only a small effect on statistical results. The human evaluator identified fields at the center top and bottom positions of the recordings plot that appear merged in the field detection. The automated field detection is expected to be able to meet the human evaluator's performance abilities.
<br>Cell spacing: GridMet: 72.37 cm; Opexebo: 71.49 cm.
<br><br>Cell N12; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n12.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n12.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.47 cm; Opexebo: 73.27 cm.
<br>Note: Opexebo created a mean estimate of field spacings that appears less realistic then GridMet's estimate here.
<br><br>Cell N13; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n13.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n13.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.43 cm; Opexebo: 44.11 cm.
<br><br>Cell N14; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n14.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n14.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 45.40 cm; Opexebo: 46.04 cm.
<br><br>Cell N15; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n15.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n15.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 15.12 cm; Opexebo: 46.79 cm.
<br><br>Cell N16; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n16.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n16.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.23 cm; Opexebo: 44.83 cm.
<br><br>Cell N17; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n17.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n17.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 75.69 cm; Opexebo: 72.62 cm.
<br><br>Cell N18; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n18.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n18.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 45.75 cm; Opexebo: 74.48 cm.
<br>Note: Opexebo created a mean estimate of field spacings that appears less realistic then GridMet's estimate here.
<br><br>Cell N19; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n19.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n19.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.20 cm; Opexebo: 44.30 cm.
<br><br>Cell N20; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n20.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n20.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.85 cm; Opexebo: 75.13 cm.
<br>Note: Opexebo created a mean estimate of field spacings that appears less realistic then GridMet's estimate here.
<br><br>Cell N21; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n21.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n21.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 54.21 cm; Opexebo: 54.97 cm.
<br><br>Cell N22; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n22.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n22.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.21 cm; Opexebo: 43.32 cm.
<br><br>Cell N23; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n23.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n23.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 45.78 cm; Opexebo: 45.55 cm.
<br><br>Cell N24; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n24.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n24.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.63 cm; Opexebo: 74.48 cm.
<br>Note: Opexebo created a mean estimate of field spacings that appears less realistic then GridMet's estimate here.
<br><br>Cell N25; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n25.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n25.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 46.09 cm; Opexebo: 46.04 cm.
<br><br>Cell N26; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n26.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n26.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 45.07 cm; Opexebo: 46.04 cm.
<br><br>Cell N27; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n27.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n27.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 60.29 cm; Opexebo: 62.38 cm.
<br><br>Cell N28; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n28.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n28.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 65.09 cm; Opexebo: 62.47 cm.
<br><br>Cell N29; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n29.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n29.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 65.44 cm; Opexebo: 65.75 cm.
Note: Cell numbering skips N30, N31, and N32.
<br><br>Cell N33; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n33.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n33.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 36.35 cm; Opexebo: 32.27 cm. (px2cm is 45/32 for n33-n37)
<br><br>Cell N34; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n34.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n34.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 32.00 cm; Opexebo: 25.35 cm.
<br><br>Cell N35; Field Detection Had Small Issues
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n35.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n35.jpg?raw=true"  width="700"></a>
Note: Cell N35 was observed to have small issues with detection in the way it has two instances of merged fields. These instances will only have small effects on its metrics. The human evaluator could clearly distinguish the seperate field areas in each pair of fields that were merged in detection in this case. The evaluator being able to distinguish these areas caused an expectation for the automated detection to also create that.
<br>Cell spacing: GridMet: 31.58 cm; Opexebo: 29.74 cm.
<br><br>Cell N36; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n36.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n36.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 24.44 cm; Opexebo: 24.24 cm.
<br><br>Cell N37; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n37.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n37.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 29.26 cm; Opexebo: 29.50 cm.
</center>