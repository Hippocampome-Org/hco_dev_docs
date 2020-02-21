# HCO Dev Docs
Developer Documentation

## Editing pages
[StackEdit](https://stackedit.io/app#) allows interactive document creation with markdown and preview of the results. Documents for this site can be built using that and the code can be copied and pasted directly into Github online or pushed to Github from a local computer. Supported markdown reference is [here](https://commonmark.org/help/).

## Supported markdown
The documentation can be written in CommonMark:
https://commonmark.org/help/
It also supports MathJax:
https://www.mathjax.org/
Todo writing:
https://www.sphinx-doc.org/en/master/usage/extensions/todo.html
Inconfig:
https://www.sphinx-doc.org/en/master/usage/extensions/ifconfig.html
Viewcode:
https://www.sphinx-doc.org/en/master/usage/extensions/viewcode.html

## Local install
To install this documentation locally for work on it:
Install python packages (via pip or otherwise) sphinx, recommonmark, and sphinx-rtd-theme

Make a hco_dev_docs folder in your local webserver directory (e.g., /var/www/html/)
Clone this repo into that directory.

Run these commands to rebuild the docs once you update them:
$ cd /var/www/html/hco_dev_docs/docs/
$ make install

Command line scripts for running that rebuild are present in:
hco_dev_docs/docs/make.sh (for linux)
hco_dev_docs/docs/make.bat (for windows)

## Posting updates online
The https://hco-dev-docs.readthedocs.io/en/latest/ site should automatically
rebuild (given a few minutes) once the github repo is uploaded to.
