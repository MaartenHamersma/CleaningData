# CleaningData
Coursera Geting and Cleaning Data assignment

1. Load the data files, features is common to both test and train (the 2nd column gives the variable names, 561 of them)

2. then load the test data
subject_test <- subjects_test.txt (not sure what this is, 2947 obs., 1 variable)
y_test <- y_test.txt (not sure what this is, 2947 obs., 1 variable)
X_test <- X_test.txt (the real data, 2947 obs and 561 variables)

3. Same for the Train data set

subject_train <- read.table("~/C4/UCI HAR Dataset/train/subject_train.txt")
names(subject_train) = "Subject"
y_train 
and make the variable name = "Lables" names(y_train) = "Lables"
X_train 

3.     rename the X_test variable names from V1..V561 to the names in features
names(X_test) <- features[,2]
names(X_train) <- features[,2]

4. Combine the Y_test (labels) and subject (people) to the test and train data sets using bind_cols
X_test2 <- bind_cols(y_test, X_test)
X_train2 <- bind_cols(y_train, X_train)
X_test3 <- bind_cols(subject_test, X_test2)
X_train3 <- bind_cols(subject_train, X_train2)

5. Now combine the test and Train datasets using bind_rows, and remove the intermediate ones
X_data <- bind_rows(X_test3, X_train3)
rm(X_train)
rm(X_test)
rm(X_train2)
rm(X_test2)
rm(X_train3)
rm(X_test3)

6. Now extract the variables Subject, Lables, and those containing "mean" or "std".
  The columns with the "means" are first, the columns with std are "to the right".
means_and_stds <- select(X_data,"Subject", "Lables", contains("mean"), contains("std") )

7. Next - make a new tidy data set with average of each variable for each activity and each subject . . .
First Have to remove the "()" and "-" from column names, so make a function using gsub and replace them in the variable name

CorrectNames <- function(x) {colnames(x) <- gsub("[()]", "", colnames(x));x}
means_and_stds <- CorrectNames(means_and_stds)
CorrectNames <- function(x) {colnames(x) <- gsub("-", "", colnames(x));x}
means_and_stds <- CorrectNames(means_and_stds)

8. Now average each variable by the |Subject and Lable

m_and_s_average <- means_and_stds %>% group_by(Subject,Lables) %>% summarize_all(.,list(mean= mean))

write.table(m_and_s_average, "m_and_s_average", row.name=FALSE)
