# Getting and Cleaning Data Course Project

The requirement is to create one R script called run_analysis.R that does the following.

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement.
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive activity names.
5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.


## Steps to work on this course project

* Download the compressed file and place in the working directory; Unzip the file into the same Directory, and view the list of files. Below is the R code used in this step.

<!-- -->
	fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
	download.file(fileUrl, destfile ="./dataset.zip", method = "curl")
	dateDownloaded <- date()
	unzip("./dataset.zip", exdir = './')
	list.files("./", recursive = TRUE)

* In the working directory where the data set is, run run_analysis.R script to perform the required transformations. The script is attached below.

<!-- -->
	### 1. Merges the training and the test sets to create one data set. 
	train <- read.table("./UCI HAR Dataset/train/x_train.txt", header = F)
	trainlabel <- read.table("./UCI HAR Dataset/train/y_train.txt", header = F)
	trainsubject <- read.table("./UCI HAR Dataset/train/subject_train.txt", header = F)

	test <- read.table("./UCI HAR Dataset/test/x_test.txt", header = F)
	testlabel <- read.table("./UCI HAR Dataset/test/y_test.txt", header = F)
	testsubject <- read.table("./UCI HAR Dataset/test/subject_test.txt", header = F)

	data <- rbind(train, test)
	label <- rbind(trainlabel, testlabel)
	subject <- rbind(trainsubject, testsubject)

	### 2. Extracts only the measurements on the mean and standard deviation for each measurement. 

	features <- read.table("./UCI HAR Dataset/features.txt")[,2]
	subset <- grepl("mean|std", features)
	subdata <- data[, subset]
	Data1 <- cbind(subject, label, subdata)
	colnames(Data1) <- c("Subject", "Activity", as.character(features[subset]))

	### 3. Uses descriptive activity names to name the activities in the data set.

	activity <- read.table("./UCI HAR Dataset/activity_labels.txt", header = F)
	for (i in seq_along(activity[, 1])) {
    		Data1$Activity[Data1$Activity == activity[i, 1]] <- as.character(activity[i, 2])
	}
	
	### 4. Appropriately labels the data set with descriptive variable names.

	names(Data1)<-gsub("^t", "time", names(Data1))
	names(Data1)<-gsub("^f", "frequency", names(Data1))
	names(Data1)<-gsub("Acc", "Accelerometer", names(Data1))
	names(Data1)<-gsub("Gyro", "Gyroscope", names(Data1))
	names(Data1)<-gsub("Mag", "Magnitude", names(Data1))
	names(Data1)<-gsub("BodyBody", "Body", names(Data1))

	### 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

	library(plyr)
	Data2 <- aggregate(. ~Subject + Activity, Data1, mean)
	write.table(Data2, file = "./tidy_data.txt", row.name=FALSE)

