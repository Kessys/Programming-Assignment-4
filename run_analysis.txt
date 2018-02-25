# You should create one R script called run_analysis.R that does the following.
# 1. Merges the training and the test sets to create one data set.
# 2. Extracts only the measurements on the mean and standard deviation for each measurement.
# 3. Uses descriptive activity names to name the activities in the data set
# 4. Appropriately labels the data set with descriptive variable names.
# 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## 0: Set working directory and load the reshape2 package (will be used in step 5).
setwd("C:/Users/Kessys/Desktop/Cursos/Data Science Specialization/Getting e Cleaning Data/UCI HAR Dataset")
library(reshape2)


## 1: Merges the training and the test sets to create one data set.

# Read data into data frames
subject_train <- read.table("./train/subject_train.txt", header = FALSE)
subject_test <- read.table("./test/subject_test.txt", header = FALSE)
x_train <- read.table("./train/X_train.txt", header = FALSE)
x_test <- read.table("./test/X_test.txt", header = FALSE)
y_train <- read.table("./train/y_train.txt", header = FALSE)
y_test <- read.table("./test/y_test.txt", header = FALSE)

# Add column name for subject files
names(subject_train) <- "subject"
names(subject_test) <- "subject"

# Add column names for measurement files
features <- read.table("./features.txt", header = FALSE)
names(x_train) <- features$V2
names(x_test) <- features$V2

# Add column name for label files
names(y_train) <- "activity"
names(y_test) <- "activity"

# Combine files into one dataset
train <- cbind(subject_train, y_train, x_train)
test <- cbind(subject_test, y_test, x_test)
combined <- rbind(train, test)


## 2: Extracts only the measurements on the mean and standard deviation for each measurement.

# Determine which columns contain "mean()" or "std()"
meanstdcols <- grepl("mean\\(\\)", names(combined)) | grepl("std\\(\\)", names(combined))

# Ensure that we also keep the subject and activity columns
meanstdcols[1:2] <- TRUE

# Remove unnecessary columns
combined <- combined[, meanstdcols]


## 3: Uses descriptive activity names to name the activities in the data set.
## 4: Appropriately labels the data set with descriptive activity names. 

# Convert the activity column from integer to factor
combined$activity <- factor(combined$activity, labels = c("Walking", "Walking Upstairs", "Walking Downstairs", "Sitting", "Standing", "Laying"))


## 5: Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

# Create the tidy data set
melted <- melt(combined, id = c("subject", "activity"))
tidy <- dcast(melted, subject + activity ~ variable, mean)

# Write the tidy data set to a file
write.csv(tidy, "tidy.txt", row.names = FALSE)
