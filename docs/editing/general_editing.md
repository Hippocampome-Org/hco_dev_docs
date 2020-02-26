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
    
- Currently the new page must be manually to /docs/index.rst under the appropriate folder

Example:

	.. toctree::
    :hidden:
    :caption: folder

	    folder/page1
	    folder/page2
