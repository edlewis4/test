
# CodeBook.md
###### This code book documents the code inside the R program - run_analysis.R

## Setup:
30 test subjects - Converted to  ```"Subject_num"```
## Raw data is: 

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

