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

attachment_<matrix_name>

{matrix_name}_fragment

{matrix_name}_evidence_{prop1}_rel

Add content later
---------------------

Content to be added
