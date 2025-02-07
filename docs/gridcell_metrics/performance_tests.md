GridMet Performance Tests
=========================

## Performance Testing Criteria
<br>The criteria used to identify the correct detection of fields included that field detection would be assess through visual inspection of original autocorrelogram plots compared to GridMet’s field detection plots. Only the central seven fields in the plots are used for field detection in these tests. The software was required to detect those seven fields and not incorrectly include a field from a further distance from the plot center instead of any of those fields. Size of the detected field relative to that in the recorded cell plot was not considered in evaluating correct detection because that was considered to be too subjective. GridMet was also required to not detect more than one field as a single field in a way that merges the fields. A caveat to this is that autocorrelograms can in a variety of cases merge fields at their borders, and if it appeared difficult in visual inspection to understand the separation of such fields then the automated detection was not penalized for its detection of those fields.

<center>
<br>Plots Used to Assess Performance	
<br>Note: the plots below are in the units of pixels that have not been converted to centimeters. For instance, in some plots, 32 pixels can represent 100 cm on the y-axis and also the x-axis.
<br>Cell N1; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n1.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n1.jpg?raw=true"  width="700"></a>
Cell N2; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n2.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n2.jpg?raw=true"  width="700"></a>
Cell N3 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N4; Field Detection Incorrect
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n4.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n4.jpg?raw=true"  width="700"></a>
Notes: Cell N4 was observed to have incorrect detection because 2 of its 35 field area pixels are in the wrong field. This will have only very small effects on metric results but is still an error.
<br>Cell N5; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n5.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n5.jpg?raw=true"  width="700"></a>
Cell N6 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N7 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N8; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n8.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n8.jpg?raw=true"  width="700"></a>
<br>Cell N9 Was Omitted from Testing Due to Having a Grid Score Below 0.2.
<br>Cell N10; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n10.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n10.jpg?raw=true"  width="700"></a>
<br>Cell N11; Field Detection Incorrect
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n11.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n11.jpg?raw=true"  width="700"></a>
Note: Cell N4 was observed to have incorrect detection because it has two instances of merged fields. These instances will only have small effects on its metrics.
<br>Cell N12; Field Detection Correct
<a href='https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n12.jpg?raw=true'><img src="https://github.com/Hippocampome-Org/hco_dev_docs/blob/master/media/performance_tests/n12.jpg?raw=true"  width="700"></a>
</center>