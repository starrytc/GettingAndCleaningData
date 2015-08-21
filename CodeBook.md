# CodeBook

This code book describes the variables, the data, and any transformations or work performed to clean up the data.

## Data 

* The data for the project is obtained from
 https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
* A full description of the data is available at 
 http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones   

## Dataset Info

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The dataset includes the following files that are relevant to this project:

* ’features.txt': List of all features.
* 'activity_labels.txt': Links the class labels with their activity name.
* 'train/X_train.txt': Training set.
* 'train/y_train.txt': Training labels.
* 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.
* 'test/X_test.txt': Test set.
* 'test/y_test.txt': Test labels.
* ‘test/subject_test.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.

## Transformation Instructions

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## Steps

[Step 1] Download the compressed file and place in the working directory; Unzip the file into the same Directory, and view the list of files. Below is the R code used in this step.

fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile ="./dataset.zip", method = "curl")
dateDownloaded <- date()
unzip("./dataset.zip", exdir = './')
list.files("./", recursive = TRUE)

[Step 2] Use script run_analysis.R to perform Transformations 1 to 5. 

* Merges the training and the test sets to create one data set. Variables used are:

train - data frame to store the training dataset (7352 observations of 561 variables)
trainlabel - data frame to store the training labels (7352 observations of 1 variable)
trainsubject - data frame to store the training subjects (7352 observations of 1 variable)
test - data frame to store the test dataset (2947 observations of 561 variables)
testlabel - data frame to store the test labels (2947 observations of 1 variable)
testsubject - data frame to store the test subjects (2947 observations of 1 variable)
data - combined data frame to store both the training and test datasets (10299 observations of 561 variables)
label - combined data frame to store both the training and test labels (10299 observations of 1 variable)
subject - combined data frame to store both the training and test subjects (10299 observations of 1 variable)

* Extracts only the measurements on the mean and standard deviation for each measurement. Variables used are:  

features - factor to store the variable names
subset - logical vector. “True” when variable names contain “mean” or “std”.
subdata - subset of the original data frame that keeps only the variables with names containing “mean” or “std” (10299 observations of 79 variables)
Data1 - data frame column combined subject, label and subdata (10299 observations of 81 variables), with the column names being “Subject”, “Activity”, and the 79 names extracted from features.

* Uses descriptive activity names to name the activities in the data set. Variables used are:

activity - data frame of 6 observations of 2 variables. The first variable is the activity label, the second is the descriptive activity names.
Data1 - same data frame as above except that the “Activity” column is replaced by activity names. 

* Appropriately labels the data set with descriptive variable names. Did the following character replacements in names(Data1):

initial “t” -> "time”
initial “f” -> "frequency"
"Acc" -> "Accelerometer”
"Gyro" -> "Gyroscope"
"Mag" -> "Magnitude"
"BodyBody" -“> Body"

* From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject. The “plyr” library is loaded. A file called tidy_data.txt is created to store the tidy data set. Variable used is:

Data2 - data frame with the average of each variable for each activity and each subject (180 observations with 81 variables) 

  

