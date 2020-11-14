Adding a new page for evidence
=============================

This described some steps involved with creating a new evidence page for a new matrix page added to the site.

Steps involved in evidence page programming
----------------

`{matrix_name}` = name of new matrix

Create a new `property_page_{matrix_name}.php`

Determine the unique properties needed to retreive evidence fragments.
E.g. neuron_id, parcel_id.

Create new database tables for at least:

`attachment_{matrix_name}` This includes data about what attachment image to include for a fragment.

`{matrix_name}_fragment` This includes data about fragment info such as quote and article.

`{matrix_name}_evidence_{prop1}_rel` where {prop1} is the 1st property, e.g., neuron_id. Additional properties can be included as needed. This relates a unique set of properties to an evidence entry, which represents a fragment table entry.

`{matrix_name}_evidence_fragment_rel` this maps evidence_ids to fragment_ids. In the case that evidence_ids are numbered differently to fragment_ids, this will provide the conversion. The ids can be numbered the same, but in the past they were numbered differently in the first hippocampome.org tables. It can potentially avoid confusion if ids for new matrices are numbered the same. The template design for property pages uses this type of table in its code. Therefore, for legacy reasons it is included, but perhaps if the ids were numbered the same this table could be obsolete, and new property pages could avoid using this.

Code for table processing:

Create a new folder `php/{matrix_name}/class/`. This should contain files such as: class.articleevidencerel.php, class.attachment.php, class.evidencepropertyyperel.php, class.evidencetypetyperel.php, class.fragment.php

These files will manage functions for accessing the database tables.

Population of tables
---------------------

Csv files need to be created for the database tables. The same format of prior csv files should be used. All instructions in the "adding a new table" section need to be followed to process each csv file into a table. Manual or programming work is needed to create the values that the csv files contain.

Programming the evidence pages
---------------------

The `property_page_{matrix_name}.php` code needs to be redesigned to accomidate the specific properties, and new property class files, to have the devidence pages appropriately use the database tables for evidence. For example, function(s) for retrieving evidence_ids need to match the properties needed to represent unique fragments in the new matrix.
