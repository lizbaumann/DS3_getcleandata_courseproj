
# Brief Synopsis of the Goal of this Study:

The goal of this study is to summarizes a subset of sensor data by subject and physical activity. The data used was collected from sensors contained in smartphones that measure a subject's position and direction. 30 subjects performed 6 activities, and 561 features (sensor data statistics) were captured on each subject-activity combination. Please see the 'License' section in the README.md file for more on the underlying data source.


## Description of the variables, the data, and transformations performed to clean up the data:
This is described below in the context of the steps used to meet the objective of summarizing the data by activity and subject.


### 1. Merge the training and the test sets to create one data set.
The main numeric data is contained in the following text files:
train/X_train.txt
test/X_test.txt

These were read into R and concatenated into the R dataset named rundata. This dataset has 10299 rows and 561 columns. The columns represent the different features tracked by the sensors, and each row represents a subject and activity. 

The rundata dataset at this point contained only the numeric variables and had no variable names. These shortcomings were next addressed so that the data could be made ready for summary and analysis.

### 2. Extract only the measurements on the mean and standard deviation for each measurement. 
    Note, I assumed we do not want the 'meanFreq()' variables, only want mean() and std()
Variable names were linked into the rundata dataset from the supplied features.txt file. The feature names were somewhat descriptive but were scrubbed to make more readable and descriptive. There were some characters in the variable names that were changed to remove potentially troublesome (to work with in R) symbolic characters such as hyphens, commas, and parentheses. The data is on time and frequency, abbreviated with a starting t or f in the frequency file, I changed these to read 'time' and 'freq' respectively to be a bit more descriptive. 

The question we are trying to answer involves only mean and standard deviation variables, so the data was reduced at this point to remove all other variables. I noted that there are some feature variables described as 'mean frequency', I assumed that these were not part of the requested data. After applying the filter, there were 66 columns that contained means and standard deviation features.

### 3. Use descriptive activity names to name the activities in the data set.
The combination of activity and subject provides context to the rows. The files train/y_train.txt and test/y_test.txt contain numeric indicators of the 6 activities that subject participants performed in the study: standing, walking downstairs, walking upstairs, walking, sitting, laying. Translation of the 6 numeric indicators with the descriptive activity names was performed by merging the numeric indicators with their names, available in the activity_labels.txt file, being careful not to undermine how data was originally sorted before the merging. The subject number (30 subjects) was also obtained from train/subject_train.txt and test/subject_test.txt. The subject number and activity name were placed into the dataset as columns to better describe the data and make it possible to summarize the data in the final step. The data set now has 68 columns: the 66 numeric mean and standard deviation variables, along with the addition of the two descriptor variables.

### 4. Appropriately label the data set with descriptive variable names.
    Note this step was actually already performed in step 1.

### 5. From the data set in step 4, create a second, independent tidy data set with the 
    average of each variable for each activity and each subject.
    Write the tidy dataset to a txt file.
The final step was creating a tidy dataset containing the average for each feature, summarized by subject and activity. The ddply() function from the plyr library was used to accomplish obtaining the averages by the two descriptor variable columns. The file was written out to a space delimited txt file. Column names were kept, row names were not though these were just default numeric values; the subject and activity are available separately in the first column of the output file.


One can use this code to read the tidy data table back in to R:
indata <- read.table("rundata_tidy.txt", header = TRUE) 


