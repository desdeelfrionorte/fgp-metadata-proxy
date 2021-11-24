Custom Transformer: LOOKUP_TABLES_READER_NG
===========================================

**Description**

This custom transformer reads and manipulates the content of a lookup table. The lookup table must be a coma separated value (CSV) file.  The manipulation directives are contained in a YAML file located in the same directory as the CSV file. The YAML file must bear the same name that the CSV file. (ex.: Province.csv and Province.yaml). See section below for a description of the YAML file.


**Input Ports**

* This custom transformer has no Input Port


**Output Ports**

* OUTPUT_LOCAL: The content of the lookup table (one record by row in the CSV file)


**Parameters**

* FEATURE_TYPE: File name of the lookup table without the .csv extension

* CSV_PATH: Name of the root directory containing the lookup table.  The custom transformer will try to find the lookup table in two directories: firstly into CSV_PATH\shared when it is a lookup table shared between PT and secondly in CSV_PATH\PT when the lookup table is specific to a PT

* PT: Two letter code of the province or territory to process (ex.: ON, QC, AB...)

* CSV_ENCODING: Type of encoding of the lookup tale


**Content of the YAML file**

The YAML file is used by the custom transformer for manipulating (editing, cloning lines, create key/value attributes and validate unicity) the CSV file.  Below is an example of a YAML file::

 CSV_COLUMNS:
   original_value: [trim, lower, no_null, explode]
   real_value_english: [trim, upper, no_null, explode]
   real_value_french: [no_null_err, explode]
   code_value: [upper, explode]
 #
 CREATE_KEY_VALUE:
   code_value: original_value
 #
 NO_DUPLICATE:
   DUP_1: [code_value]
   DUP_2: [original_value, real_value_english]
 #
 CHECK_DOMAIN
   original_value: [year, yearly]

**Description of the three sections of a YAML file**

* The first section CSV_COLUMNS defines the actions to perform on the column of the CSV file.  Following the "CSV_COLUMNS:" you list on separate line the name of the column as defined in the header (first line) of the CSV file (an error is raised if the column name is mispelled); Followed by a colon ":"; followed by the list of actions separated by a coma "," and enclosed between brackets "[]".  Here's the list of possible actions:

  * trim: Remove front and trailing whitespcaes; it also remove extra white spces in the interior of the column value (ex.: "   a   test" ==­> "a test");
  * lower: Put the column value in lower case;
  * upper: Put the column value in upper case;
  * no_null: Check that the column is not empty or null; it terminates execution if not null;
  * explode: Duplicate the row if the column contains values separated by semicolumn ";".  The line is exploded by the number of values in the column (ex: "QC;ON;MB", will duplicate all the content of the line 2 times, the first line will contain "QC"; the second "ON" and the third "MB".

* The second section CREATE_KEY_VALUE creates of FME key/value attributes.  Following the "CREATE_KEY_VALUE:" you list on separate line all key: value pair that will create an FME attribute.  The key is the name of the column containing the attribute name to create; followed by a colon ":"; followed by the name of the column containing the value of the attribute;

* The third section NO_DUPLICATE defines the column that are used for checking the unicity of one or more columns. Following "NO_DUPLICATE:" you list on separate line the duplication to check.  You write the name of the duplication (name is not important but must be unique); follwed by colon ":"; followed by the list of name of the column to check for unicity separated by a coma and enclosed between bracket. 

* The fourth section CHECK_DOMAIN defines the domain of values to check for a specific column(s). Following "CHECK_DUPLICATE:" you list on separate line the domain validation to check.  You write the name of the column to check; followed by a colon ":"; followed by the domain values separated by a coma "," and enclosed between brackets "[]". If you want to check for empty value "", you place an extra coma at the end of the list: ex: [A,B,].  Important: do not place extra whitespaces in the list.
   
Sommaire des classes et modules
###############################
   
.. autosummary::
   LOOKUP_TABLES_READER_NG.check_file_present
   LOOKUP_TABLES_READER_NG.LoadValidateYaml
 
Description détaillée des classes et modules
############################################ 
 
.. automodule:: LOOKUP_TABLES_READER_NG
   :members:
   :special-members: __init__
   :show-inheritance:
    
