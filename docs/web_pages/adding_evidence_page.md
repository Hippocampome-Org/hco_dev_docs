Adding a new page for evidence
=============================

This described some steps involved with creating a new evidence page for a new matrix page added to the site.

Steps involved in evidence page programming
----------------

{matrix_name} = name of new matrix

Create a new property_page_{matrix_name}.php

Determine the unique properties needed to retreive evidence fragments.
E.g. neuron_id, parcel_id.

Create new database tables for at least:

attachment_<matrix_name>. This includes data about what attachment image to include for a fragment.

{matrix_name}_fragment. This includes data about fragment info such as quote and article.

`{matrix_name}_evidence_{prop1}_rel` where {prop1} is the 1st property, e.g., neuron_id. Additional properties can be included as needed.

{matrix_name}_evidence_fragment_rel

Purpose of tables
---------------------


