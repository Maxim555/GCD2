#Variables
X_train, Y_train, X_test, Y_test - data sets we import into R
>features - data set from features.txt we import into R
>features_v - a character vector with features names
colnames(X_train), colnames(X_test) - Assigning names from features_v vector to X_train and X_test data frames
X_combined <- rbind(X_train,X_test) - joined X_train and X_test data frames
activity_labels, subject_test, subject_train - imported activity labels, subject_test and subject_train into R
Y_combined - joined Y_train and Y_test data frames
colnames(Y_combined) <- "Activity" - Add Activity column name
combined_df <- cbind(X_combined,Y_combined) - joined X and Y combined data frames
names_v <- as.character(as.vector(names(combined_df))) - vector of column names in the combined data frame
pattern - pattern contained in column names we want columns to get with
logic_step2 - logical vector, containing TRUE and FALSE with columns we are interested in
step2_df - data frame, containing columns we are interested in
step2_df2 <- select(step2_df,-contains("meanFreq")) - select not containing meanFreq from dataframe
subject_combined - joint subject data frame
step2_df2 - one big data set

#Assigning Activity values to words
step2_df2$Activity[step2_df2$Activity == 1] <- "WALKING"
step2_df2$Activity[step2_df2$Activity == 2] <- "WALKING_UPSTAIRS"
step2_df2$Activity[step2_df2$Activity == 3] <- "WALKING_DOWNSTAIRS"
step2_df2$Activity[step2_df2$Activity == 4] <- "SITTING"
step2_df2$Activity[step2_df2$Activity == 5] <- "STANDING"
step2_df2$Activity[step2_df2$Activity == 6] <- "LAYING"

#Rename V1 column to ID
step2_df2 <- rename(step2_df2,ID = V1)
#Create a new joint ID_Activity column
step2_df2$ID_Activity <- as.character(paste(step2_df2$ID,step2_df2$Activity,sep=""))

#Group and summarize final dataframe
df <- step2_df2 %>%group_by(ID_Activity) %>% summarise_each(funs(mean))
df - final data frame we have been looking for
