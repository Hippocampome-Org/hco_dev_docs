General editing
===============

Some basic methods are:
- Create a new file in a folder under docs
- the top of the file must have a capatalized first word title and equal signs the length of the title. 
This will be used as the table of contents entry for the page.

Example:

    My new page
    ===========

    Page content
    
- Currently the new page must be manually added to /docs/index.rst including the appropriate folder name. If a page already exists no table of contents changes are needed.

Example:

	.. toctree::
    :hidden:
    :caption: folder

	    folder/page1
	    folder/page2
