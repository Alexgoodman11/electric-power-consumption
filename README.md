# electric-power-consumption
 
##declare packages
install.packages("dplyr")
library(dplyr)
install.packages("grDevices")
library("grDevices")
setwd("C:/Users/Alexandre/Desktop/R_directory")
zip_name <- "household_power_consumption.zip"


## Download and unzip the dataset:
if (!file.exists(zip_name)){
  fileURL <- "https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2Fhousehold_power_consumption.zip"
  download.file(fileURL, zip_name)
  unzip(zip_name)
}  

##read data
txt_name <- "household_power_consumption.txt"
db<-read.table(txt_name, header=TRUE, sep=";", dec=".")
names(db)
dim(db)
str(db)
head(db)


##subset_data
db_date<-as.Date(db$Date, format="%d/%m/%Y")
db_sub<-filter(db,db_date == "2007-02-01"|db_date == "2007-02-02")
head(db_sub)
summary(db_sub)
str(db_sub)
dim(db_sub)
names(db_sub)


##making plots

####plot1
db_sub_global_active_power<-as.numeric(db_sub$Global_active_power)
png("plot1.png", width = 480, height = 480)
hist(db_sub_global_active_power, breaks=15, main="Global Active Power", col="red",
     ylab= "Frequency", xlab="Global Active Power(kilowatts)")
dev.off()


####plot2
datetime <- strptime(paste(db_sub$Date, db_sub$Time, sep=" "), "%d/%m/%Y %H:%M:%S")
db_sub_global_active_power<-as.numeric(db_sub$Global_active_power)
png("plot2.png", width = 480, height = 480)
plot(datetime,db_sub_global_active_power,type="l",xlab="",ylab="Global Active Power(Kilowatts)")
dev.off()

####plot3
datetime <- strptime(paste(db_sub$Date, db_sub$Time, sep=" "), "%d/%m/%Y %H:%M:%S")
db_sub_metering_1<-as.numeric(db_sub$Sub_metering_1)
db_sub_metering_2<-as.numeric(db_sub$Sub_metering_2)
db_sub_metering_3<-as.numeric(db_sub$Sub_metering_3)
png("plot3.png", width = 480, height = 480)
plot(datetime,db_sub_metering_1,type="l",col= "black",xlab="",ylab="Energy sub metering")
lines(datetime,db_sub_metering_2,type="l",col="red")
lines(datetime,db_sub_metering_3,type="l",col="blue")
legend("topright", c("metering_1","metering_2","metering_3"),lty=1, lwd=2.5,col=c("black","blue","red"))
dev.off(which =dev.cur())


####plot4
datetime <- strptime(paste(db_sub$Date, db_sub$Time, sep=" "), "%d/%m/%Y %H:%M:%S")
db_sub_metering_1<-as.numeric(db_sub$Sub_metering_1)
db_sub_metering_2<-as.numeric(db_sub$Sub_metering_2)
db_sub_metering_3<-as.numeric(db_sub$Sub_metering_3)
db_sub_global_active_power<-as.numeric(db_sub$Global_active_power)
db_sub_global_reactive_power<-as.numeric(db_sub$Global_reactive_power)
voltage<-as.numeric(db_sub$Voltage)



png("plot4.png", width = 480, height = 480)
par(mfrow=c(2,2))
#####1
plot(datetime,db_sub_global_active_power,type="l",xlab="",ylab="Global Active Power(Kilowatts)")

#####2
plot(datetime,voltage,type="l",col="black")

#####3
plot(datetime,db_sub_metering_1,type="l",col= "black",xlab="",ylab="Energy sub metering")
lines(datetime,db_sub_metering_2,type="l",col="red")
lines(datetime,db_sub_metering_3,type="l",col="blue")
legend("topright", c("metering_1","metering_2","metering_3"),lty=, lwd=2.5,col=c("black","blue","red"),bty="o")

#####4
plot(datetime,db_sub_global_reactive_power,type="l")
dev.off(which =dev.cur())
