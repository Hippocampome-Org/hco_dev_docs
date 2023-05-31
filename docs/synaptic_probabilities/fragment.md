Synaptic Probabilities Evidence
===============================

## Fragment
The term fragment refers to a fragment of evidence that is used to support the data that is a part of hippocampome. This piece, aka fragment, of evidence is a source that has citation and other data used to explain where information came from for the data on hippocampome.

## SynproFragment
- SynproFragment is a list of evidence records to be displayed on the synaptic probabilities evidence pages. It is generated through the fragment_nbyk.csv and fragment_nbym.csv files in csv2db_import repo's iconv directory. The naming "n by k" refers to neuron_id by parcel and "n by m" refers to neuron_id_1 by neuron_id_2. In terms of statistics, the neurite_quantified table stores data relevant to n by k and number_of_contacts stores data relevant to n by m. The SynproFragment table provides the sources of evidence that those statistics are based on.
- Adding new data to the synaptic probabilities matrix should be coupled with updating the evidence records that cite the sources of where the data is from. Updating fragment_nbyk.csv and fragment_nbym.csv files creates new SynproFragment entries when csv2db_import is used to rebuild the hippocampome matrix.
- neurite_quantified includes a "reference_ID" column that links data in that table to references and citations in SynproFragment.

## SynproEvidenceFragmentRel and SynproArticleEvidenceRel
- SynproEvidenceFragmentRel and SynproArticleEvidenceRel are built from the contents of the fragment_nbyk.csv and fragment_nbym.csv files.
- Running the csv2db_import software with automatically generate these tables. They are used for evidence associations in a way that tables with similar names are used for these purposes with the hippocampome database in general.
- The synprofrag_to_synprofrag function in maps.py of the csv2db_import software builds the tables.

## SynproEvidencePropertyTypeRel
- The SynproEvidencePropertyTypeRel connects neuron ids and neurite ids (properties) to evidence ids. In this way the neuron_id by parcel (n by k) and neuron_id_1 by neuron_id_2 (n by m) values are connected to the listings of the evidence sources including citations.
- The SynproPropParcelRel table is needed for creating the SynproEvidencePropertyTypeRel table. This is based on synpro_prop_parcel_rel.csv. If any updates to neuron ids or other properties such as in the Property table are created then data in SynproPropParcelRel will need to be updated.
- The SynProPropParcelRel table is used to convert neurite names to neurite ids
- /synap_prob/gen_csv/gen_csv.php is used to generate the files needed. Ensure the $path_to_files directory in gen_csv_params.php has read and write access to php code running the processing.
- evi_pro_type_rel_nbyk.csv and evi_pro_type_rel_nbym.csv are created by gen_csv.php. These files should be placed in csv2db_import's /iconv/latin1 folder. The csv files are used to build SynproEvidencePropertyTypeRel table when the csv2db_import software is run.
- On the gen_csv.php page click the "Generate Evidence CSV Files" button when ready to generate the files.
- csv2db_import's synprodata_to_synprodata function in maps.py will process the csv files to create the SynproEvidencePropertyTypeRel table.

## property_page_synpro.php, property_page_synpro_nm.php, and property_page_synpro_pvals.php
- These file are used to display the evidence records. They perform live queries to the database based on php parameters that are supplied to them. The php parameters come from links that are clicked on in the synaptic probabilities matrix.

## Changing the neuron types used in synaptic probabilities data will probabily need non-trivial code updates
- Currently the neuron types used in the synaptic probabilities matrix are those that were listed in v1 of the site (as of 05/30/23). These list of neurons is hardcoded in different places in the synaptic probabilities code. Changing which neurons types that are present in the synaptic probabilities data is expected to need a non-trivial amount of coding. In the future the hippocampome code base can move toward a more automated and not hardcoded approach but this was not the state of the code base as of this documentation creation.