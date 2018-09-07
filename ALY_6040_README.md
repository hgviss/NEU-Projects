# NEU-Projects
# ALY_6040_PythonProject
# This is the final project from Northeastern ALY 6040 Data Mining Class Spring 2018
# We are looking at blood samples that have been sent through a mass spec 
# Once we create a single data base (first version) we will look at using ETL and EDA to find insights in the data. 
# This is practice for learning python and a learning process. 
# Working with Python 3 and the Anaconda Navigator / Juypter notebook 

# I will look at applying the same code to individual data sheets depending on sample. 
# This will wait until after I had a chance to talk with DR.M. VISSER who will be the subject matter expert guide.

# H.G.Visser
# visser.h@husky.neu.edu

import pandas as pd
import numpy as np

import os
print(os.getcwd()) # checking the work directory all the files are localized in a single folder

import glob # glob look for repeating patterns
for file in glob.glob("*.xls"): # find all files that end in '.xls'
    print(file) # print list
    
# if you want to read multiple sheets into a file you have to read each sheet by name. 
# we use a function to to read every sheet regardless of name and append them all together
all_data = pd.DataFrame() #creating an empty data frame
rows = 0 
# this loop opens each xls file
for file in glob.glob("*.xls"): #Using glob to grab all files that end in '.xls'
    xls = pd.ExcelFile(file) #ExcelFile to read in workbooks
    sheets = xls.sheet_names # To get names of all the sheets
    sheet_to_df_map = {}
    # this loop reads each sheet from the xls file that was opened
    # this will take some time to run because we open every xls and then...
    # ... process each sheet in that xls file
    # when finished the loop moves on to the next xls in the directory 
    for sheet_name in sheets: # takes the data that we brought in 
        df = pd.read_excel(file, sheetname = sheet_name) #build the new data frame
        rows += df.shape[0]   ## += adds another value with the variable's value and assigns the new value to the variable.
    all_data = all_data.append(df, ignore_index = True)

print(all_data.shape[0]) # this gives us the number of data rows in the new df
print(rows) # this gives us the total number of rows that were in the xls files    

print(all_data.shape[0]) # there are 7303 rows in the df all_data
print(rows) # we crunched through 334,562 rows of data
# we need to figure out why we lost these rows


# quick count of all components
all_data['Component name'].value_counts()
