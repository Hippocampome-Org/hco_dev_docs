String field files
==================

## Article ids
- Articles are uniquely identified by pmid_isbn and first_page. All new article entries 
should contain unique values for those fields to allow <matrix>_string_field.py files to
correctly find and associate articles ids with evidence ids in the ArticleEvidenceRel
creation process. ArticleEvidenceRel is use in evidence pages to find articles matching
evidence of neural properties.
