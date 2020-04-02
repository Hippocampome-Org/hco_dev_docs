NbyN Matrices Header Height Increase
====================================

Steps to add or modify header height increase:

1. 
```
$("#nGrid_Granule").css("height","235"); in synapse_probabilities_noc.php
```

2. 
main_nbyn.css
replace main.css reference with:
```
<link rel="stylesheet" type="text/css" media="screen" href="synap_prob/css/main_nbyn.css" />
```
remove any preexisting lines that interefere with the following
new code:
```
.ui-jqgrid .ui-jqgrid-htable th div {
  overflow: hidden !important; 
  position:relative !important; 
  /*height:235px !important;*/
  text-align:center !important;
  margin-bottom:0px !important;  
  /*margin-bottom:5px;*/
}
.ui-jqgrid .ui-jqgrid-htable th div.nGrid_Neuron_type{

}
.ui-jqgrid .ui-jqgrid-htable th {
  /*height:22px !important;*/
  padding: 0px 2px !important;
}
.ui-state-default .ui-jqgrid-hdiv {
  /*height:235px !important;*/
  height:57px;
}
.ui-jqgrid .ui-jqgrid-htable th div.nGrid_Neuron_type{
/*  text-align:center !important;
  margin-bottom:0px !important;*/
}
.ui-jqgrid .ui-jqgrid-htable th {/*height:22px;*/padding: 0px 2px;}
.ui-jqgrid .ui-jqgrid-htable th div {
  overflow: hidden; 
  position:relative; 
  /*height:235px; */
  /*text-align:left;*/
  /*margin-bottom:5px;*/
}
.ui-jqgrid-hdiv {
  /*height:57px;*//* !important;*/
}
```
3.
main_nbyn.css
```
.rotate 
{
   top: 100px !important;
}
```
4.
jqgh_nGrid_Granule
```
$('#jqgh_nGrid_Granule').attr('style', 'width: 235px; left:-70px; top: 76px !important;position:relative;');
```
Reference: https://stackoverflow.com/questions/2655925/how-to-apply-important-using-css

5.
nGrid_frozen
```
$('#nGrid_frozen').attr('style', 'width: 1px;top:222px !important;position:relative !important;');

width: 1px;top:222px !important;position:relative !important;
```
6.
```
$("#ui-jqgrid-labels").css("height","235");
```
7.
main_nbyn.css
```
.frozen-div {
  position: absolute !important; 
  height: 257px !important; 
  top: 0px !important; 
  left: 0px !important;
  z-index:5 !important;
}

position: absolute; height: 257px; top: 0px; left: 0px;z-index:5;
```
8:
jqgh_nGrid_Neuron_Type_2
```
$('#jqgh_nGrid_Neuron_Type_2').attr('style', 'top:120px;');

insert style="top:120px;"
```
9.
main_nbyn.css
```
.frozen-bdiv {
  height:644px !important;
}
```
10.
```
.ui-jqgrid-hdiv {
  overflow-y:hidden;
}
```
_ _ _
In conclusion
replace main.css reference with:
```
<link rel="stylesheet" type="text/css" media="screen" href="synap_prob/css/main_nbyn.css" />
```
and

add ```<?php include('synap_prob/nbyn_css_mods.php'); ?>```

then the headers are sized as wanted.
