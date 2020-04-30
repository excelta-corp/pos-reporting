# PoS &amp; PoP Reporting Tool

Thank you for using Excelta's own PoS and PoP Reporting Tool!

The purpose of this tool is to find cleaned data files created by the [Disty POS Data Cleaner](https://github.com/excelta-corp/pos-data-cleaner) along with Point of Purchase reports formatted in the same way. After loading them into memory, the tool can:

* Filter the data and perform summation and YTD averaging on per-territory and per-distributor bases.
* Post new data to Rep Sheets files.
* Output each territory and distributor (and all-territory/all-distributor tables) as Excel files.
* Output the above to a "Mosales" HTML report.

:minidisc: **About the code**
This program is written in python 3.7.7 and uses the numpy and pandas modules for working with tabular data, and openpyxl and xlrd to handle Excel files. The GUI is built with tkinter, and the code is frozen using PyInstaller's development build.

## Download and install

**For PC**: you can download the most recent program installer the [releases page](https://github.com/excelta-corp/pos-reporting/releases). (Scroll down until you find "PoS.PoP.Reporting.Tool.exe" and download that!) The installer may warn you that it can't confirm the identity of the software's creator—this is normal.

**For Mac**: Sorry, the program is not yet available for Mac.

## Setting up config.ini
While the program should work out of the box, it's possible that changes to our fileserver structure or a different relationship between your PC and the fileserver may require edits to the config.ini file, which tells the program where to look for its data files, reference files, and where to create its reports.

To find the config.ini file, start by navigating to the directory where you installed the tool:

:file_folder: PoS & PoP Reporting Tool<br />
&ensp;└─ :file_folder: src<br />
&emsp;&emsp;&ensp;└─ :file_folder: reporting<br />
&emsp;&emsp;&emsp;&emsp;&ensp;└─ :page_facing_up: config.ini<br />

You can open config.ini in Notepad and make changes to the paths in this file. 

| :exclamation: Please note that any lines beginning with a # character are comments, and lines **not** beginning with a # are evaluated as code. Be careful to only edit the text between the quotation mark characters, as shown below: |
| ---      |

``` python
# Comment explaining the variable below
variable_name = "Edit the stuff inside the quotes!"
```

### The `$year$` variable
You may notice that `$year$` is used in place of a year folder in some cases. This is treated as a variable by the program, and will be replaced by whatever year you're deciding to run. For example, if you enter '1988' as the year to run, it will replace `$year$` with `1988` when reading this file.

## Understanding the Reference Files
In addition to the local config.ini, the reporting tool relies on several Excel files to find the information it needs. These files are meant to be stored in a shared location and edited as necessary to change the behavior of the program.

| :exclamation: Please note that altering **column names** inside these files will cause the program to stop working. |
| ---      |

### :ledger: Distributor Ledger.xlsx
The Distributor Ledger spreadsheet helps the program understand what data it's looking for, and what it should do with that data. These are the columns of this spreadsheet, their data types, and an explanation of what they do:

  `Exact Disty Name` *(Text)* This field tells the program what each data source is named with its exact spelling and punctuation. When the program searches for data files, it will use this exact name to find each data source. (This is used both for distributors, and for data sources like Quotes, TEP, and Invoice Sales Report.) 
  
  `SYSPRO Code` *(Text)* This is the corresponding SYSPRO code to link the data back to our system.
  
  `Multi-Disty` *(Boolean: TRUE or FALSE)* If set to FALSE, the tool will replace the contents of Disty column of the corresponding data files with the Exact Disty Name from this row. Make sure this is set to TRUE for any data source that reports multiple distributors, such as the Invoice Sales Report.
  
  `Mosales` *(Boolean: TRUE or FALSE)* If set to FALSE will not be posted to Mosales.
  
  `Rep Sheets` *(Boolean: TRUE or FALSE)* If set to FALSE, files with this Exact Disty Name will not be posted to Rep Sheets.
  
  `DistyGroup` *(Text)* If two or more data sources should be merged into one, enter the new name for the group here. For example, if NYDistyA and NYDistyB should be combined, you can do this by putting something like NYDistyGroup into the DistyGroup column for both rows.
  
### :ledger: Rep Terr Reference.xlsx

Rep Terr Reference is an Excel spreadsheet that associates each sales territory with a rep company and a geographical location. It has three columns: `Terr` contains territory numbers as whole numbers, `Company` contains company names as text, and `Location` describes the sales territory locations as text.

### :ledger: 20XX summary.csv

Found in the *previous year's* folder, this file is used to populate previous year totals per distributor per territory in order to calculate growth over time. (e.g. '2020 summary.csv' in the /2020/ folder will be loaded if running the program for 2021.)

### :page_facing_up: Last_Run.log

This is a text file (open in Notepad) that was used in previous versions of the program to record the outcome of its last run for troubleshooting purposes. It is not used in the current version (1.0.0)

## Running the program

Start the program using the desktop shortcut. You should see a window with a "Year to run:" text box containing the current year, and a series of checkboxes.

| :exclamation: If you're using a VPN, make sure to connect to the fileserver now. |
| ---      |

* **Year to run:** determines what year folder the program will search to find its data files.
* **Drop empty distributors** If checked, the program will ignore any data source that is not represented at least once in either the last year summary file, or the current year data files.
* **Preview discovered files before loading data** If checked, will display a table showing which distributor files were found for each month. Helpful to confirm that a new data file is being detected, but can be skipped for speed.
* **Delete nonconforming data** *(Currently forced to enabled)* Allows the program to kill off any data that cannot be calculated with absolutely no review. For example, if a text value appears in the `Qty` field of a data file, the row will be dropped without fanfare. This option will be re-enabled when gentler validation methods are in place.
* **Upload to Syspro SQL** *(Currently forced to disabled)* Uploads new data to our SQL server. Will be enabled when this function is in place.
* **Post any new data to Rep Sheets** If checked, will search for the Rep Sheets files, read their Territory tabs, and determine if a currently loaded month is not yet represented in the Rep Sheets. If a new month is found, the program can add these rows to the appropriate tabs automatically.
* **Render Mosales report with new data** If checked, will recreate the Mosales .html report and delete and recreate the excel folder containing the spreadsheets for each territory and distributor.

Check or uncheck these boxes to suit your needs, then use the Continue button in the lower right to proceed through the program.

| :bug: Known bug: The program may not close when complete. You can always use the X in the upper right to close the window. |
| ---      |
