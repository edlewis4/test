
# CodeBook.md
This code book documents the process inside the R program - run_analysis.R

## Setup:
Human Activity Recognition Using Smartphones Dataset

A group of 30 volunteers performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist.
Sensor data and other calculations were collected. The obtained dataset was randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.

This process run_analysis.R will read in the raw data and output a tidy dataset of the average of the measurements that involved `mean()` mean or `std()` standard deviation summarized by Activity and subject number

## Raw data: 
Source zip file is located here : 
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

For each record the following data was provided:
* Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
* Triaxial Angular velocity from the gyroscope. 
* A 561-feature vector with time and frequency domain variables. 
* Its activity label. 
* An identifier of the subject who carried out the experiment.

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

#### Getting Data
Create data directory inside working directory if it does not already exist <br>
Get the Zip file data from URL above<br>
Unzip the Zip archive into the workingdirectory `./data` directory

Reads in train related datasets
* `./data/UCI HAR Dataset/train/X_train.txt`        : store in `trainx` data.frame
* `./data/UCI HAR Dataset/train/y_train.txt`        : store in `train_y_activity` data.frame
* `./data/UCI HAR Dataset/train/subjec_train.txt`   : store in `train_y_activity` data.frame

Reads in test related datasets
* `./data/UCI HAR Dataset/test/X_test.txt`          : store in `testx` data.frame
* `./data/UCI HAR Dataset/test/y_test.txt`          : store in `test_y_activity` data.frame
* `./data/UCI HAR Dataset/test/subject_test.txt`    : store in `test_y_activity` data.frame

Reads in activity & feature labels datasets
* `./data/UCI HAR Dataset/activity_labels.txt`      : store in `activitylabels` data.frame
* `./data/UCI HAR Dataset/features.txt`             : store in `featurelabels` data.frame

#### Subsetting data - Choosing only variables with std() and mean()
Converts the `featurelabels` data.frame to a data.table to select necessary mean and std columns <br>
Subset that data.table to get only those variables that have `std()` or `mean()` in the feature name<br>

###### Determining which variables to keep that referenced mean() or std()
I kept any variablues that contained the mean() or std() within the name. 
`tBodyAcc-mean()-X` , `tBodyAcc-std()-Y`, `tBodyAccJerk-std()-X`, `fBodyAccJerk-mean()-Z`, ...

I did not to keep variables that just had the word mean in them that were used for other calculations.  e.g `angle(tBodyAccMean,gravity)`, `angle(tBodyAccJerkMean),gravityMean)`, `fBodyBodyGyroMag-meanFreq()`, `fBodyAccMag-meanFreq()`, ...


Created subset of training `trainx` and test subjects`testx` datasets- only keeping columns that have `std()` or `mean()` data as determined above with the data.table subsetting.

#### Merge data
Merge all of the Train dataframes into one and set column names - stores in `train` data.frame <br>
Merge all of the Test datasets into one and set column names  - stores in `test` data.frame

Merge the `test` and `train` dataframes

#### Making data descriptive
Use data pulled from the activity_labels.txt file into activitylabels table to change values from numbers to Descriptive activities 

Create final summary data.frame with tidy data <br>
Temporarily makes a long form tidy dataset with `Subject_num` and `Activiy` - with all other observations under a variable / value  - Stores in `result` data.frame

Create summary by `Subject_num` and `Activity` and the mean of all features

Convert `Activity` back to a Factor 
###### Activity is a factor with 6 levels 
* WALKING
* WALKING_UPSTAIRS
* WALKING_DOWNSTAIRS
* SITTING
* STANDING
* LAYING

## Write tidy data.frame to output file
Write final summary table `result` to output file - Sample output shown below <br>

Average of variables by 'Activity' and 'Subject_num' <br>
This is a wide tidy dataset with each column a variable and each row an observation
```{r}
> head(result[1:6,1:6])
  Subject_num           Activity tBodyAcc-mean()-X tBodyAcc-mean()-Y tBodyAcc-mean()-Z
           1             LAYING         0.2215982      -0.040513953        -0.1132036
           1            SITTING         0.2612376      -0.001308288        -0.1045442
           1           STANDING         0.2789176      -0.016137590        -0.1106018
           1            WALKING         0.2773308      -0.017383819        -0.1111481
           1 WALKING_DOWNSTAIRS         0.2891883      -0.009918505        -0.1075662
           1   WALKING_UPSTAIRS         0.2554617      -0.023953149        -0.0973020
```
