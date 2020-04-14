# Getting and Cleaning Data Course Project

The aim of this project is to demonstrate the ability to collect and clean a set of data prior to its analysis. To this end, a set of data collected from the accelerometers of a Samsung Galaxy S Smartphone was collected from a link provided by Coursera. Subsequently, the task entailed 


* The merger of the training and the test sets to create a single data set; 
* The extraction of the measurements on the mean and standard deviation for each measurement; 
* The recourse to descriptive activity names to name the activities in the data set; 
* The labeling of the data set with descriptive variable names; and 
* The creation of a second data set from the one created from the previous step with the average of each activity and each subject.

These steps are further discussed below.

### 1. Download the data set

The data set was downloaded from a zip folder.

### 2. The assignment of data to variables

The variables assigned included:
* features
* activities
* subject_test
* x_test
* y_test
* subject_train
* x_train
* y_train

 Further details related to this assignment ar available from in the following script:
 
```
 
  features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
  activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
  subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
  x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
  y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
  subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
  x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
  y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")
```


### 3. The merger of the training and test sets to a single data se

This step entailed the creating of a single data set from the training and the test sets. The script that was used for this is provided as follows:

```
  Further details related to this assignment ar available from in the script
  features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))
  activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))
  subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject")
  x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)
  y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")
  subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject")
  x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)
  y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")
```


### 4. The extraction of the mean and standard deviation measurements

This was implemented with the following script:

```
TidyData <- Merged_Data %>% select(subject, code, contains("mean"), contains("std"))
```

### 5. Recourse to descriptive activity to name the activities in the data set

This was done using the following script:

```
TidyData$code <- activities[TidyData$code, 2]
```

### 6. Appropriately label the data set with descriptive variable names

This was executed with the following lines of codes:

```
names(TidyData)[2] = "activity"
names(TidyData)<-gsub("Acc", "Accelerometer", names(TidyData))
names(TidyData)<-gsub("Gyro", "Gyroscope", names(TidyData))
names(TidyData)<-gsub("BodyBody", "Body", names(TidyData))
names(TidyData)<-gsub("Mag", "Magnitude", names(TidyData))
names(TidyData)<-gsub("^t", "Time", names(TidyData))
names(TidyData)<-gsub("^f", "Frequency", names(TidyData))
names(TidyData)<-gsub("tBody", "TimeBody", names(TidyData))
names(TidyData)<-gsub("-mean()", "Mean", names(TidyData), ignore.case = TRUE)
names(TidyData)<-gsub("-std()", "STD", names(TidyData), ignore.case = TRUE)
names(TidyData)<-gsub("-freq()", "Frequency", names(TidyData), ignore.case = TRUE)
names(TidyData)<-gsub("angle", "Angle", names(TidyData))
names(TidyData)<-gsub("gravity", "Gravity", names(TidyData))
```

### 7. The creation of a second data set with the average of each variable for each activity and each subject

Finally, the new data set ready to be analysed is created as follows:

```
CleanData <- TidyData %>%
  group_by(subject, activity) %>%
  summarise_all(funs(mean))
write.table(CleanData, "CleanData.txt", row.name=FALSE)
```
