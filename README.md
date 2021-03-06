# stata_secutrial
Stata programs for handling secutrial data

Handles 
* variable label
* value labels
* dates
* datetimes

An example export options file is included as ExportOptions.html. The important thing for the program to work is that the metadata options are enabled and the store reference value option is set to seperate table

## Usage

Assuming that the file is in your working directory (assume C:/data), and that you have folders temp (for temporary files), orig (for original data) and unzip within orig (the zip file will be unzipped here), your do file could contain the following
```
global pp = "C:/data"
global orig = "$pp/orig"
global tmp = "$pp/temp"
global unzip = "$orig/unzip"

do SecuTrial_zip_data_import
```
The global macros ```orig```, ```tmp``` and ```unzip``` are all required so that the program know where to save files (the folders also have to exist). 

When running the file, a window will open and ask you to identify the zip file you downloaded from SecuTrial. Simply navigate to it, select it and press OK.

Once the program has run, the prepared datafiles are saved in the ```orig``` folder.

Three additional functions specifically for SecuTrial are also available.
* ```add_aid``` is a helper program to add the alternative ID to files.
* ```add_center``` is a helper program to add the center ID to files.
* ```scrubvars``` is a helper program to remove secutrial system variables from files.

Two additional functions (```xlabel``` and ```xvarlabel```) are also included. They are for labelling values and variables. Each has it's own helpfile. Disclaimer: They were not written by me and I take no responsibility for them.

These files could be put in ```C:/ado/personal``` or their location could be added to the adopath in stata, e.g.
```
adopath + "path/to/files"
```

### Preparing specific forms

Specific forms can be requested via a ```forms``` global. For example, if a database contains formed named ```form1```, ```form2 ``` and ```form3```, the following would prepare all three forms
```
do SecuTrial_zip_data_import
```
while the next would only prepare ```form1```
```
global forms = "form1" 

do SecuTrial_zip_data_import
```


### Additional usage details
If you have exported the audit log data as well as the main data, by setting another global called ```audit``` to 1, the audit log files will also be prepared.

Similarly, queries can also be prepared if they have been exported and a global called ```query``` is set to 1.

E.g., the following would prepare the query data and the audit log data for all files in the database.
```
global query = 1
global audit = 1

do SecuTrial_zip_data_import
```

## Notes
Dates and datetimes are only identified as such if they are exported as strings (but not all strings are interpreted as date(time), the metadata provides additional details to the program). 

The original date/datetime variable is retained as ```x_orig``` where ```x``` is the variables original name. This is to ensure that no data is lost in the conversion from string to date due to an unexpected format.

This code is known to work with SecuTrial version 5.2.0.9. Some previous versions do not have the itemtype variable in the items file that is used to determine dates.


