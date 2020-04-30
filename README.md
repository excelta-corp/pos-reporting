# PoS &amp; PoP Reporting Tool

:construction: To be added. :construction:

## Getting Started

:construction: To be added. :construction:

### Download and install

:construction: To be added. :construction:

### Setting up config.ini
While the program should work out of the box, it's possible that changes to our fileserver structure or a different relationship between your PC and the fileserver may require edits to the config.ini file, which tells the program where to look for its data files, reference files, and where to create its reports.

To find the config.ini file, start by navigating to the directory where you installed the tool:

:file_folder: PoS & PoP Reporting Tool<br />
&ensp;└─ :file_folder: src<br />
&emsp;&emsp;&ensp;└─ :file_folder: reporting<br />
&emsp;&emsp;&emsp;&emsp;&ensp;└─ :page_facing_up: config.ini<br />

You can open config.ini in Notepad and make changes to the paths in this file. 

:warning: Please note that any lines beginning with a # character are comments, and lines **not** beginning with a # are evaluated as code. Be careful to only edit the text between the quotation mark characters.

''' python
# Comment explaining the variable below
variable_name = "Edit the stuff inside the quotes!"
'''

##### The $year$ variable
You may notice that `$year$` is used in place of a year folder in some cases. This is treated as a variable by the program, and will be replaced by whatever year you're deciding to run. For example, if you enter '1988' as the year to run, it will replace `$year$` with `1988` when reading this file.

### Reference Files
In addition to the local config.ini, the reporting tool relies on several Excel files to find the information it needs. These files are meant to be stored in a shared location and edited as necessary to change the behavior of the program.
:warning: Please note that altering **column names** inside these files will cause the program to stop working.
#### :ledger: Distributor Ledger.xlsx
The Distributor Ledger spreadsheet helps the program understand what data it's looking for, and what it should do with that data. These are the columns of this spreadsheet, their data types, and an explanation of what they do:

  `Exact Disty Name` *(Text)* This field tells the program what each data source is named with its exact spelling and punctuation. When the program searches for data files, it will use this exact name to find each data source. (This is used both for distributors, and for data sources like Quotes, TEP, and Invoice Sales Report.) 
  
  `SYSPRO Code` *(Text)* This is the corresponding SYSPRO code to link the data back to our system.
  
  `Multi-Disty` *(Boolean: TRUE or FALSE)* If set to FALSE, the tool will replace the contents of Disty column of the corresponding data files with the Exact Disty Name from this row. Make sure this is set to TRUE for any data source that reports multiple distributors, such as the Invoice Sales Report.
  
  `Mosales` *(Boolean: TRUE or FALSE)* If set to FALSE will not be posted to Mosales.
  
  `Rep Sheets` *(Boolean: TRUE or FALSE)* If set to FALSE, files with this Exact Disty Name will not be posted to Rep Sheets.
  
  `DistyGroup` *(Text)* If two or more data sources should be merged into one, enter the new name for the group here. For example, if NYDistyA and NYDistyB should be combined, you can do this by putting something like NYDistyGroup into the DistyGroup column for both rows.
  
#### :ledger: Rep Terr Reference.xlsx
Rep Terr Reference is an Excel spreadsheet that associates each sales territory with a rep company and a geographical location. It has three columns: `Terr` contains territory numbers as whole numbers, `Company` contains company names as text, and `Location` describes the sales territory locations as text.
#### :page_facing_up: Last_Run.log
This is a text file (open in Notepad) that was used in previous versions of the program to record the outcome of its last run for troubleshooting purposes. It is not used in the current version (1.0.0)
  
