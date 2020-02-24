Synaptic Probabilities
======================

## SynproFragment, SynproArticleEvidenceRel, SynproEvidenceFragmentRel
- Convert all entries in neurite_quantified into SynproFragment
- This is done by creating a xlsx file with columns that will be in the
SynproFragment table.
- Export xlsx file as csv file
- Add SynproFragment csv file to the ingest process based on instructions in [Adding a new table instructions](https://hco-dev-docs.readthedocs.io/en/latest/csv2db/add_table.html)
- SynproFragment, SynproArticleEvidenceRel, SynproEvidenceFragmentRel should 
have been created previously base on those adding table instructions
- Create the ingest process using the new synprofrag_to_synprofrag function in maps.py 
This process will also populate SynproArticleEvidenceRel and SynproEvidenceFragmentRel
- Update property_page_synpro.php and related files to use the new tables

## SynproEvidencePropertyTypeRel
- The above 3 tables must have been created previously for this process
- Use csv_gen.php to create data that can be used to generate 
SynproEvidencePropertyTypeRel csv file.
- The SynProPropParcelRel table is used to convert neurite names to neurite ids
- Use the csv file csv_gen created to copy and paste values into a 
new SynproEvidencePropertyTypeRel xlsx file
- Export xlsx file as csv file
- Add SynproFragment csv file to the ingest process through steps similar to above
- Create the ingest process using the new synprodata_to_synprodata function in maps.py
- Update property_page_synpro.php and related files to use the new tables
