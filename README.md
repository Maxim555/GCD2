# GCD2
#Getting and Cleaning Data
# Import train and test sets into R
X_train <- read.table("./1/UCI HAR Dataset/train/X_train.txt")
Y_train <- read.table("./1/UCI HAR Dataset/train/y_train.txt")
X_test <- read.table("./1/UCI HAR Dataset/test/X_test.txt")
Y_test <- read.table("./1/UCI HAR Dataset/test/y_test.txt")

#Import features.txt into R
features <- read.table("./1/UCI HAR Dataset/features.txt")

#Getting a character vector with features names
features_v <- as.character(as.vector(features[,2]))

#Assigning names from features_v vector to X_train and X_test data frames
colnames(X_train) <- features_v
colnames(X_test) <- features_v

#Join X_train and X_test data frames
X_combined <- rbind(X_train,X_test)

#Import activity labels, subject_test and subject_train into R
activity_labels <- read.table("./1/UCI HAR Dataset/activity_labels.txt")
subject_test <- read.table("./1/UCI HAR Dataset/test/subject_test.txt")
subject_train <- read.table("./1/UCI HAR Dataset/train/subject_train.txt")

#Join Y_train and Y_test data frames
Y_combined <- rbind(Y_train,Y_test)

#Add Activity column name
colnames(Y_combined) <- "Activity"

#Join X and Y combined data frames
combined_df <- cbind(X_combined,Y_combined)

#Getting a vector with column names in the combined data frame
names_v <- as.character(as.vector(names(combined_df))) # column names in the combined data frame

#select mean and std from dataframe
pattern <- "mean()|std()"
logic_step2 <- grepl(pattern,names_v)
step2_df <- combined_df[,logic_step2]

#select not containing meanFreq from dataframe
step2_df2 <- select(step2_df,-contains("meanFreq"))
#step3

#Create a joint subject data frame
subject_combined <- rbind(subject_train,subject_test)

#Create one big dataset
step2_df2 <- cbind(step2_df2,subject_combined)
step2_df2 <- cbind(step2_df2,Y_combined)

#Assigning Activity values to words
step2_df2$Activity[step2_df2$Activity == 1] <- "WALKING"
step2_df2$Activity[step2_df2$Activity == 2] <- "WALKING_UPSTAIRS"
step2_df2$Activity[step2_df2$Activity == 3] <- "WALKING_DOWNSTAIRS"
step2_df2$Activity[step2_df2$Activity == 4] <- "SITTING"
step2_df2$Activity[step2_df2$Activity == 5] <- "STANDING"
step2_df2$Activity[step2_df2$Activity == 6] <- "LAYING"
#step5 
library(dplyr)
ID <- "ID"
as.character(ID)
#Rename V1 column to ID
step2_df2 <- rename(step2_df2,ID = V1)
#Create a new joint ID_Activity column
step2_df2$ID_Activity <- as.character(paste(step2_df2$ID,step2_df2$Activity,sep=""))

#Group and summarize final dataframe
df <- step2_df2 %>%group_by(ID_Activity) %>% summarise_each(funs(mean))

#write.table(df,"C:/Users/Marina/Documents/Rdata/1/UCI HAR Dataset/finalfile.txt",row.name=FALSE,sep="\t")
