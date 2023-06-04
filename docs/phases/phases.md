In Vivo Recordings Oscillatory Phases
======================

## Adding data

New data should be added to the:
phases_evidence_type_rel.csv
phases_evidence_fragment_rel.csv
phases_fragment.csv
phases.csv
files then the hippocampome database should be rebuilt. The database is rebuilt using the csv2db_import_v2 [github repository](https://github.com/Hippocampome-Org/csv2db_import_v2).

## Updating neuron listings:

generate_json_params.php contains a manually sorted list of neuron names. Adding or removing neuron names must be done manually to preserve an intended order on this page.
They are also listed in /phases/gen_json/neuron_groups.json.

In generate_json.php, $neuron_classes should be updated to the number of neuron ids intended
to be displayed on the phases matrix.

Note: 
For local version development the phases page seems to only load correctly when enough php arguments are provided:
For example:
http://localhost/php_test/phases.php?species_check1=checked&age_check1=checked&<br>sex_check1=checked&method_check1=checked&behavior_check1=checked&species_check2=checked&age_check2=checked&<br>sex_check2=checked&method_check2=checked&behavior_check2=checked&age_check3=checked&sex_check3=checked&<br>method_check3=checked&behavior_check3=checked&method_check4=checked&behavior_check4=checked&<br>method_check5=checked&behavior_check5=checked&method_check6=checked&behavior_check6=checked&<br>behavior_check7=checked&behavior_check8=checked&page=main_page&row_select=

possibly a redirect rule fixes this on the site live online but this is not fixed where just going to phases.php with no arguments are provided are able to load the page.

The same seems to be true for the main page in a local version. Opening http://localhost/php_v2_dev/phases.php will not load correctly but opening the following will load correctly: http://localhost/php_v2_dev/phases.php?species_check1=checked&<br>age_check1=checked&sex_check1=checked&method_check1=checked&behavior_check1=checked&species_check2=checked&age_check2=checked&<br>sex_check2=checked&method_check2=checked&behavior_check2=checked&age_check3=checked&sex_check3=checked&method_check3=checked&behavior_check3=checked&method_check4=checked&<br>behavior_check4=checked&method_check5=checked&behavior_check5=checked&method_check6=checked&<br>behavior_check6=checked&behavior_check7=checked&behavior_check8=checked&page=main_page&row_select=
