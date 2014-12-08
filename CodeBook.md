
# CodeBook.md
#### This code book documents the code inside the R program - run_analysis.R

## Setup:
Human Activity Recognition Using Smartphones Dataset

A group of 30 volunteers performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist.
A collection of sensor data was collected.

This process run_analysis.R will read in the raw data and output a tidy dataset of the average of the measurements that involved `mean()` mean or `std()` standard deviation summarized by Activity and subject number

## Raw data is: 
####  Source zip located : (https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)
#### 
For each record the following data was provided:
======================================
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

Raw files used for this analysis
* `UCI HAR Dataset/train/X_train.txt`
* `UCI HAR Dataset/train/y_train.txt`
* `UCI HAR Dataset/train/subjec_train.txt`
* `UCI HAR Dataset/test/X_test.txt`
* `UCI HAR Dataset/test/y_test.txt`
* `UCI HAR Dataset/test/subject_test.txt`
* `UCI HAR Dataset/activity_labels.txt`
* `UCI HAR Dataset/features.txt`

Other files from the .zip archive were not used for data analysis.  The README.txt was consulted

## Process
#### create data directory if it does not already exit exist

#### GetZip file data from URL
#### Read in necessary datasets

#### Read in train datasets
* `./data/UCI HAR Dataset/train/X_train.txt`
* `./data/UCI HAR Dataset/train/y_train.txt`
* `./data/UCI HAR Dataset/train/subjec_train.txt`

#### Read in test datasets
* `./data/UCI HAR Dataset/test/X_test.txt`
* `./data/UCI HAR Dataset/test/y_test.txt`
* `./data/UCI HAR Dataset/test/subject_test.txt`

#### Read in activity & feature labels datasets
* `./data/UCI HAR Dataset/activity_labels.txt`
* `./data/UCI HAR Dataset/features.txt`

#### convert the featurelabels to a datatable to select necessary mean and std columns
#### subset data.table to get only those items that have std() or mean() in the feature name

#### create subset of trainx and testx - only keep columns that have std or mean data
#### as determined above with keep_column_nums

#### Merge Train datasets into one and set column names
#### Merge Test datasets into one and set column names

#### Merge test and train datasets

#### Use the descriptive Activity names from the activitylabels table instead of numbers
#### This is data pulled from the activity_labels.txt file into activitylabels table
#### uses plyr function mapvalues to change values from numbers to Descriptive activities 

#### use dplyr melt function to make long form tidy dataset with Subject_num and Activiy
#### All other observations will then be under a variable / value  - Store in result dataframe

#### use dply dcast function to create summary by Subject_num and Activity and the mean of all features
#### Covert Acitivy back to a Factor 
###### Activity is a factor with 6 levels 
* WALKING
* WALKING_UPSTAIRS
* WALKING_DOWNSTAIRS
* SITTING
* STANDING
* LAYING

#### Write final summary table to output file - 

