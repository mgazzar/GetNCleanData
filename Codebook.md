# Getting-and-Cleaning-Data-Assingment

#Assumes data file is already in working directory and ready to be unzipped
unzip("getdata-projectfiles-UCI HAR Dataset.zip")

#Reads data from table and assigns variables
XTest<- read.table("UCI HAR Dataset/test/X_test.txt")
YTest<- read.table("UCI HAR Dataset/test/Y_test.txt")
SubjectTest <-read.table("UCI HAR Dataset/test/subject_test.txt")

XTrain<- read.table("UCI HAR Dataset/train/X_train.txt")
YTrain<- read.table("UCI HAR Dataset/train/Y_train.txt")
SubjectTrain <-read.table("UCI HAR Dataset/train/subject_train.txt")

features<-read.table("UCI HAR Dataset/features.txt")
activity<-read.table("UCI HAR Dataset/activity_labels.txt")

#Combines vectors and assigns to variables
X<-rbind(XTest, XTrain)
Y<-rbind(YTest, YTrain)
Subject<-rbind(SubjectTest, SubjectTrain)

#Get dimensions of vector variables
dim(X)

dim(Y)

dim(Subject)

index<-grep("mean\\(\\)|std\\(\\)", features[,2])
length(index)

X<-X[,index]
dim(X)

Y[,1]<-activity[Y[,1],2]
head(Y)

names<-features[index,2]
names(X)<-names
names(Subject)<-"SubjectID"
names(Y)<-"Activity"

CleanedData<-cbind(Subject, Y, X)
head(CleanedData[,c(1:4)])

#Sorts data
CleanedData<-data.table(CleanedData)
TidyData <- CleanedData[, lapply(.SD, mean), by = 'SubjectID,Activity']
dim(TidyData)

#Creates table
write.table(TidyData, file = "Tidy.txt", row.names = FALSE)

#Determines headings of table
head(TidyData[order(SubjectID)][,c(1:4), with = FALSE],12)