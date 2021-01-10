Adding on-hold neuron types
=============================

This described some steps involved with adding on-hold neuron types to the site.

Steps involved in adding on-hold neuron types
----------------
* addi material (data of the on-hold types) to at least the neuron name and neuron type (and neuron term if appropriate) searches.
* add any new article entries to the articles table.
* update tables such as EvidenceFragmentRel and EvidencePropertyTypeRel to be able to associate the neuron type with the article evidence (or create new tables).

  A caution here is that adding new data to the EvidenceFragmentRel and EvidencePropertyTypeRel tables may cause interference with the way the original neuron types data on the site works. Creating new tables such as EvidenceFragmentRelOnHold and EvidencePropertyTypeRelOnHold could avoid that issue.

  Another element of this is that the code to populate EvidenceFragmentRel and EvidencePropertyTypeRel is not known. New custom source code may need to be generated to do that. If a different method than programmatically creating the Tables is done, that should be noted here as well.
* figure out how to separate access with packets of users by the GMU firewall.
* create all packet images.
* somehow make sure on-hold types don't appear with regular types on the site.

  Note: usually db->json would need to be done so if that conversion never happens possibly they won't show up.
* create new neuron pages specifically with the on-hold descriptions.
