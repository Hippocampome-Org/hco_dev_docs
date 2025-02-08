GridMet Performance Tests
=========================

This documentation describes the results of GridMet field detection performance tests.

## Performance Testing Criteria
The criteria used to identify the correct detection of fields included that field detection would be assess through visual inspection of original autocorrelogram plots compared to GridMet’s field detection plots. Only the central seven fields in the plots are used for field detection in these tests. The software was required to detect those seven fields and not incorrectly include a field from a further distance from the plot center instead of any of those fields. Size of the detected field relative to that in the recorded cell plot was not considered in evaluating correct detection because that was considered to be too subjective. GridMet was also required to not detect more than one field as a single field in a way that merges the fields. A caveat to this is that autocorrelograms can in a variety of cases merge fields at their borders, and if it appeared difficult in visual inspection to understand the separation of such fields then the automated detection was not penalized for its detection of those fields.

## Opexebo Comparisons
Opexebo was used with its default configuration for testing. Opexebo was run with Python 3.9.7 and installed by the command `pip3 install opexebo`. Cell data of correlograms was formatted in a plain text comma spaced values format for use with Opexebo. The cell data used is in the folder heat_maps_real_ac_py. The python script `opexebo_stats.py` was run by the command:
<br>`$ python3 ./opexebo_stats.py`
<br>to generate statistics from Opexebo. The value for gri field spacing was read as the one displayed after `'grid_spacing': ` in command line output. Each individual grid field spacing measurement that contributed to the mean spacing measurement reported was found in the array values reported after `'grid_spacings'` in the command line output. The analyses were performed on Ubuntu 21.10.

<center>
<br><u>Plots Used to Assess Performance</u>
<br>The performance of GridMet was observed to be 93.3% (28/30) correct cell field detections. The cells that had incorrect detections were n4 and n35. The plots can be clicked on to open each one as its full sized image.
<br>Note: the plots below are in the units of pixels that have not been converted to centimeters. For instance, in some plots, 32 pixels can represent 100 cm on the y-axis and also the x-axis.
<br><br>Cell N1; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n1.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n1.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 36.19 cm; Opexebo: 35.94 cm.
<br>Cell N2; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n2.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n2.jpg?raw=true"  width="700"></a>
Cell N3 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N4; Field Detection Incorrect
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n4.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n4.jpg?raw=true"  width="700"></a>
Notes: Cell N4 was observed to have incorrect detection because 2 of its 35 field area pixels are in the wrong field. This will have only very small effects on metric results but is still an error.
<br>Cell spacing: GridMet: 39.53 cm; Opexebo: 39.13 cm.
<br>Cell N5; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n5.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n5.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 45.53 cm; Opexebo: 46.25 cm.
<br>Cell N6 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N7 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N8; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n8.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n8.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 57.56 cm; Opexebo: 56.94 cm.
<br>Cell N9 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N10; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n10.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n10.jpg?raw=true"  width="700"></a>
Cell spacing: GridMet: 75.41 cm; Opexebo: 70.1875 cm.
<br>Cell N11; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n11.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n11.jpg?raw=true"  width="700"></a>
Note: Cell N11's field detection was not penalized for any merged fields because how to separate two pairs of fields in the original recording plot was considered to be unclear to the human evalulator. The automated field detection was not expected to go beyond the human evaluator's performance abilities.
<br>Cell spacing: GridMet: 72.34 cm; Opexebo: 71.49 cm.
<br>Cell N12; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n12.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n12.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.75 cm; Opexebo: 73.25 cm.
<br>Note: Opexebo created a mean estimate of field spacings that appears less realistic then GridMet's estimate here.
<br>Cell N13; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n13.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n13.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.44 cm; Opexebo: 44.12 cm.
<br>Cell N14; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n14.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n14.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 45.41 cm; Opexebo: 46.04 cm.
<br>Cell N15; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n15.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n15.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 47.41 cm; Opexebo: 46.79 cm.
<br>Cell N16; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n16.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n16.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 44.19 cm; Opexebo: 44.83 cm.
<br>Cell N17; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n17.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n17.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 75.78 cm; Opexebo: 72.62 cm.
<br>Cell N18; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n18.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n18.jpg?raw=true"  width="700"></a>
<br>Cell spacing: GridMet: 45.81 cm; Opexebo: 74.48 cm.
<br>Note: Opexebo created a mean estimate of field spacings that appears less realistic then GridMet's estimate here.
<br>Cell N19; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n19.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n19.jpg?raw=true"  width="700"></a>
<br>Cell N20; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n20.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n20.jpg?raw=true"  width="700"></a>
<br>Cell N21; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n21.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n21.jpg?raw=true"  width="700"></a>
<br>Cell N22; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n22.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n22.jpg?raw=true"  width="700"></a>
<br>Cell N23; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n23.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n23.jpg?raw=true"  width="700"></a>
<br>Cell N24; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n24.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n24.jpg?raw=true"  width="700"></a>
<br>Cell N25; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n25.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n25.jpg?raw=true"  width="700"></a>
<br>Cell N26; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n26.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n26.jpg?raw=true"  width="700"></a>
<br>Cell N27; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n27.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n27.jpg?raw=true"  width="700"></a>
<br>Cell N28; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n28.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n28.jpg?raw=true"  width="700"></a>
<br>Cell N29; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n29.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n29.jpg?raw=true"  width="700"></a>
Note: Cell numbering skips N30, N31, and N32.
<br>Cell N33; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n33.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n33.jpg?raw=true"  width="700"></a>
<br>Cell N34; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n34.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n34.jpg?raw=true"  width="700"></a>
<br>Cell N35; Field Detection Incorrect
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n35.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n35.jpg?raw=true"  width="700"></a>
Note: Cell N35 was observed to have incorrect detection because it has two instances of merged fields. These instances will only have small effects on its metrics. The human evaluator could clearly distinguish the seperate field areas in each pair of fields that were merged in detection in this case. The evaluator being able to distinguish these areas caused an expectation for the automated detection to also create that if its detection is to be considered correct.
<br>Cell N36; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n36.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n36.jpg?raw=true"  width="700"></a>
<br>Cell N37; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n37.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n37.jpg?raw=true"  width="700"></a>
</center>