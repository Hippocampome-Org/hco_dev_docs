Synaptic Probabilities Json Files
=================================

## Use of Json files
Hippocampome matrices are typically displayed with json files that are static files that have stored database values. This is faster than having to execute database queries for every user visitng the site. Evidence pages do have database queries but collect much less data than is present compared to the many peices of data listed throughout a matrix.

## Creating synaptic probabilties Json files
Navigate to /synap_prob/gen_json/generate_json.php. Click on the button to create the matching json file. All synaptic probabilities related database tables should have been already created before clicking the button. The database data will be used to create the data.

## Displaying Json data on the site
Once the Json files have been created, they will automatically be used to display each synaptic probabilities related matrix. The web pages will look for the Json files in the default directory in which they are created.