Merges the training and the test sets to create one data set.
Reading table into R and Forming a large dataset.


x_train<-read.table("X_train.txt")
y_train<-read.table("y_train.txt")
subject_train<-read.table("subject_train.txt")

x_test<-read.table("X_test.txt")
y_test<-read.table("y_test.txt")
subject_test<-read.table("subject_test.txt")

merge_train<-cbind(y_train,subject_train,x_train)
merge_test<-cbind(y_test,subject_test,x_test)

data<-rbind(merge_train,merge_test)


Appropriately labels the data set with descriptive activity names. 

features<-read.table("features.txt")
col.f<-as.vector(features[,2])
colname<-c("activity","subject",col.f)
colnames(data)<-colname


Uses descriptive activity names to name the activities in the data set.


activity_labels<-read.table("activity_labels.txt")
act_label<-rep(NA,10299)
for (i in 1:10299){
    j=1
    while (j<=6){
        if (as.numeric(data[i,1])==as.numeric(activity_labels[j,1])){
            act_label[i]<-as.vector(activity_labels[j,2])
            break}
        else{
            j=j+1}
    }
}
act_label

data_final_1<-cbind(as.data.frame(act_label),data)


Extracts only the measurements on the mean and standard deviation for each measurement. 


a<-grep("(mean|std)\\(\\)",features[,2])
a
length(a)
features[a,]
b<-a+3
data_extra<-data_final_1[,c(1,2,3,b)]
write.table(data_extra,"data_extra.txt")

Creates a second, independent tidy data set with the average of 
each variable for each activity and each subject. 


data_s<-split(data_final_1,list(data_final_1[,3],data_final_1[,2]))
data_final_2<-as.data.frame(t(sapply(data_s, function(x) colMeans(x[,col.f]))))

write.table(data_final_2,"data_final_2.txt")
