
# Quality Prediction for Weight Lifting Exercises

## Background

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: [link]http://groupware.les.inf.puc-rio.br/har (see the section on the Weight Lifting Exercise Dataset).

## DATA
The training data for this project are available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test data are available here:

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

The data for this project come from this source: http://groupware.les.inf.puc-rio.br/har. 

### I would like to thank from my side for their generous contribution in allowing this data to be used for the assignment.

## GOAL

The goal of my project is to predict the manner in which they did the exercise. This is the "classe" variable in the training set.
## STEPS I FOLLOWED ...
1. Downloaded the above file and placed in pwd(present working directory).This also could be done as 
    download.file(url,destfile='./filename')



```R
#loading files replacing blanks with NA
training=read.csv("pml-training.csv",na.strings=c("","NA","NULL"))
testing=read.csv("pml-testing.csv",na.strings=c("","NA","NULL"))
```


```R
dim(training)
```


<ol class=list-inline>
	<li>19622</li>
	<li>160</li>
</ol>




```R
head(training)
```


<table>
<thead><tr><th scope=col>X</th><th scope=col>user_name</th><th scope=col>raw_timestamp_part_1</th><th scope=col>raw_timestamp_part_2</th><th scope=col>cvtd_timestamp</th><th scope=col>new_window</th><th scope=col>num_window</th><th scope=col>roll_belt</th><th scope=col>pitch_belt</th><th scope=col>yaw_belt</th><th scope=col>⋯</th><th scope=col>gyros_forearm_x</th><th scope=col>gyros_forearm_y</th><th scope=col>gyros_forearm_z</th><th scope=col>accel_forearm_x</th><th scope=col>accel_forearm_y</th><th scope=col>accel_forearm_z</th><th scope=col>magnet_forearm_x</th><th scope=col>magnet_forearm_y</th><th scope=col>magnet_forearm_z</th><th scope=col>classe</th></tr></thead>
<tbody>
	<tr><td>1               </td><td>carlitos        </td><td>1323084231      </td><td>788290          </td><td>05/12/2011 11:23</td><td>no              </td><td>11              </td><td>1.41            </td><td>8.07            </td><td>-94.4           </td><td>⋯               </td><td>0.03            </td><td> 0.00           </td><td>-0.02           </td><td>192             </td><td>203             </td><td>-215            </td><td>-17             </td><td>654             </td><td>476             </td><td>A               </td></tr>
	<tr><td>2               </td><td>carlitos        </td><td>1323084231      </td><td>808298          </td><td>05/12/2011 11:23</td><td>no              </td><td>11              </td><td>1.41            </td><td>8.07            </td><td>-94.4           </td><td>⋯               </td><td>0.02            </td><td> 0.00           </td><td>-0.02           </td><td>192             </td><td>203             </td><td>-216            </td><td>-18             </td><td>661             </td><td>473             </td><td>A               </td></tr>
	<tr><td>3               </td><td>carlitos        </td><td>1323084231      </td><td>820366          </td><td>05/12/2011 11:23</td><td>no              </td><td>11              </td><td>1.42            </td><td>8.07            </td><td>-94.4           </td><td>⋯               </td><td>0.03            </td><td>-0.02           </td><td> 0.00           </td><td>196             </td><td>204             </td><td>-213            </td><td>-18             </td><td>658             </td><td>469             </td><td>A               </td></tr>
	<tr><td>4               </td><td>carlitos        </td><td>1323084232      </td><td>120339          </td><td>05/12/2011 11:23</td><td>no              </td><td>12              </td><td>1.48            </td><td>8.05            </td><td>-94.4           </td><td>⋯               </td><td>0.02            </td><td>-0.02           </td><td> 0.00           </td><td>189             </td><td>206             </td><td>-214            </td><td>-16             </td><td>658             </td><td>469             </td><td>A               </td></tr>
	<tr><td>5               </td><td>carlitos        </td><td>1323084232      </td><td>196328          </td><td>05/12/2011 11:23</td><td>no              </td><td>12              </td><td>1.48            </td><td>8.07            </td><td>-94.4           </td><td>⋯               </td><td>0.02            </td><td> 0.00           </td><td>-0.02           </td><td>189             </td><td>206             </td><td>-214            </td><td>-17             </td><td>655             </td><td>473             </td><td>A               </td></tr>
	<tr><td>6               </td><td>carlitos        </td><td>1323084232      </td><td>304277          </td><td>05/12/2011 11:23</td><td>no              </td><td>12              </td><td>1.45            </td><td>8.06            </td><td>-94.4           </td><td>⋯               </td><td>0.02            </td><td>-0.02           </td><td>-0.03           </td><td>193             </td><td>203             </td><td>-215            </td><td> -9             </td><td>660             </td><td>478             </td><td>A               </td></tr>
</tbody>
</table>




```R
colnames(training)
```


<ol class=list-inline>
	<li>'X'</li>
	<li>'user_name'</li>
	<li>'raw_timestamp_part_1'</li>
	<li>'raw_timestamp_part_2'</li>
	<li>'cvtd_timestamp'</li>
	<li>'new_window'</li>
	<li>'num_window'</li>
	<li>'roll_belt'</li>
	<li>'pitch_belt'</li>
	<li>'yaw_belt'</li>
	<li>'total_accel_belt'</li>
	<li>'kurtosis_roll_belt'</li>
	<li>'kurtosis_picth_belt'</li>
	<li>'kurtosis_yaw_belt'</li>
	<li>'skewness_roll_belt'</li>
	<li>'skewness_roll_belt.1'</li>
	<li>'skewness_yaw_belt'</li>
	<li>'max_roll_belt'</li>
	<li>'max_picth_belt'</li>
	<li>'max_yaw_belt'</li>
	<li>'min_roll_belt'</li>
	<li>'min_pitch_belt'</li>
	<li>'min_yaw_belt'</li>
	<li>'amplitude_roll_belt'</li>
	<li>'amplitude_pitch_belt'</li>
	<li>'amplitude_yaw_belt'</li>
	<li>'var_total_accel_belt'</li>
	<li>'avg_roll_belt'</li>
	<li>'stddev_roll_belt'</li>
	<li>'var_roll_belt'</li>
	<li>'avg_pitch_belt'</li>
	<li>'stddev_pitch_belt'</li>
	<li>'var_pitch_belt'</li>
	<li>'avg_yaw_belt'</li>
	<li>'stddev_yaw_belt'</li>
	<li>'var_yaw_belt'</li>
	<li>'gyros_belt_x'</li>
	<li>'gyros_belt_y'</li>
	<li>'gyros_belt_z'</li>
	<li>'accel_belt_x'</li>
	<li>'accel_belt_y'</li>
	<li>'accel_belt_z'</li>
	<li>'magnet_belt_x'</li>
	<li>'magnet_belt_y'</li>
	<li>'magnet_belt_z'</li>
	<li>'roll_arm'</li>
	<li>'pitch_arm'</li>
	<li>'yaw_arm'</li>
	<li>'total_accel_arm'</li>
	<li>'var_accel_arm'</li>
	<li>'avg_roll_arm'</li>
	<li>'stddev_roll_arm'</li>
	<li>'var_roll_arm'</li>
	<li>'avg_pitch_arm'</li>
	<li>'stddev_pitch_arm'</li>
	<li>'var_pitch_arm'</li>
	<li>'avg_yaw_arm'</li>
	<li>'stddev_yaw_arm'</li>
	<li>'var_yaw_arm'</li>
	<li>'gyros_arm_x'</li>
	<li>'gyros_arm_y'</li>
	<li>'gyros_arm_z'</li>
	<li>'accel_arm_x'</li>
	<li>'accel_arm_y'</li>
	<li>'accel_arm_z'</li>
	<li>'magnet_arm_x'</li>
	<li>'magnet_arm_y'</li>
	<li>'magnet_arm_z'</li>
	<li>'kurtosis_roll_arm'</li>
	<li>'kurtosis_picth_arm'</li>
	<li>'kurtosis_yaw_arm'</li>
	<li>'skewness_roll_arm'</li>
	<li>'skewness_pitch_arm'</li>
	<li>'skewness_yaw_arm'</li>
	<li>'max_roll_arm'</li>
	<li>'max_picth_arm'</li>
	<li>'max_yaw_arm'</li>
	<li>'min_roll_arm'</li>
	<li>'min_pitch_arm'</li>
	<li>'min_yaw_arm'</li>
	<li>'amplitude_roll_arm'</li>
	<li>'amplitude_pitch_arm'</li>
	<li>'amplitude_yaw_arm'</li>
	<li>'roll_dumbbell'</li>
	<li>'pitch_dumbbell'</li>
	<li>'yaw_dumbbell'</li>
	<li>'kurtosis_roll_dumbbell'</li>
	<li>'kurtosis_picth_dumbbell'</li>
	<li>'kurtosis_yaw_dumbbell'</li>
	<li>'skewness_roll_dumbbell'</li>
	<li>'skewness_pitch_dumbbell'</li>
	<li>'skewness_yaw_dumbbell'</li>
	<li>'max_roll_dumbbell'</li>
	<li>'max_picth_dumbbell'</li>
	<li>'max_yaw_dumbbell'</li>
	<li>'min_roll_dumbbell'</li>
	<li>'min_pitch_dumbbell'</li>
	<li>'min_yaw_dumbbell'</li>
	<li>'amplitude_roll_dumbbell'</li>
	<li>'amplitude_pitch_dumbbell'</li>
	<li>'amplitude_yaw_dumbbell'</li>
	<li>'total_accel_dumbbell'</li>
	<li>'var_accel_dumbbell'</li>
	<li>'avg_roll_dumbbell'</li>
	<li>'stddev_roll_dumbbell'</li>
	<li>'var_roll_dumbbell'</li>
	<li>'avg_pitch_dumbbell'</li>
	<li>'stddev_pitch_dumbbell'</li>
	<li>'var_pitch_dumbbell'</li>
	<li>'avg_yaw_dumbbell'</li>
	<li>'stddev_yaw_dumbbell'</li>
	<li>'var_yaw_dumbbell'</li>
	<li>'gyros_dumbbell_x'</li>
	<li>'gyros_dumbbell_y'</li>
	<li>'gyros_dumbbell_z'</li>
	<li>'accel_dumbbell_x'</li>
	<li>'accel_dumbbell_y'</li>
	<li>'accel_dumbbell_z'</li>
	<li>'magnet_dumbbell_x'</li>
	<li>'magnet_dumbbell_y'</li>
	<li>'magnet_dumbbell_z'</li>
	<li>'roll_forearm'</li>
	<li>'pitch_forearm'</li>
	<li>'yaw_forearm'</li>
	<li>'kurtosis_roll_forearm'</li>
	<li>'kurtosis_picth_forearm'</li>
	<li>'kurtosis_yaw_forearm'</li>
	<li>'skewness_roll_forearm'</li>
	<li>'skewness_pitch_forearm'</li>
	<li>'skewness_yaw_forearm'</li>
	<li>'max_roll_forearm'</li>
	<li>'max_picth_forearm'</li>
	<li>'max_yaw_forearm'</li>
	<li>'min_roll_forearm'</li>
	<li>'min_pitch_forearm'</li>
	<li>'min_yaw_forearm'</li>
	<li>'amplitude_roll_forearm'</li>
	<li>'amplitude_pitch_forearm'</li>
	<li>'amplitude_yaw_forearm'</li>
	<li>'total_accel_forearm'</li>
	<li>'var_accel_forearm'</li>
	<li>'avg_roll_forearm'</li>
	<li>'stddev_roll_forearm'</li>
	<li>'var_roll_forearm'</li>
	<li>'avg_pitch_forearm'</li>
	<li>'stddev_pitch_forearm'</li>
	<li>'var_pitch_forearm'</li>
	<li>'avg_yaw_forearm'</li>
	<li>'stddev_yaw_forearm'</li>
	<li>'var_yaw_forearm'</li>
	<li>'gyros_forearm_x'</li>
	<li>'gyros_forearm_y'</li>
	<li>'gyros_forearm_z'</li>
	<li>'accel_forearm_x'</li>
	<li>'accel_forearm_y'</li>
	<li>'accel_forearm_z'</li>
	<li>'magnet_forearm_x'</li>
	<li>'magnet_forearm_y'</li>
	<li>'magnet_forearm_z'</li>
	<li>'classe'</li>
</ol>




```R
#I believe that number of variables here is too large ie 160 and so I decided to remove irrevelant columns the first seven
training_1<-training[,-c(1:7)]
dim(training_1)
```


<ol class=list-inline>
	<li>19622</li>
	<li>153</li>
</ol>



No of rows 19622 is large and I want to shorten training data and also use the rest for cross validation.
    Our main motive is predicting the classe variable but still we have 153 columns which is too much.Next I am gonna remove the columns with most NA's and also I am gonna separate training and cross validation data.


```R
training_2<- training_1[,colSums(is.na(training_1))<=5000]
dim(training_2)
```


<ol class=list-inline>
	<li>19622</li>
	<li>53</li>
</ol>




```R
#split training and cross validation 
library(caret)
```


```R
#as per course standard general convention for splitting
inTrain<-createDataPartition(training_2$classe,p=0.75,list=FALSE)
train<-training_2[inTrain,]
test<-training_2[-inTrain,]
```


```R
dim(train)
```


<ol class=list-inline>
	<li>14718</li>
	<li>53</li>
</ol>




```R
#Now still I am looking for most affecting columns and to see that I am looking for corelation based upon current data
library(corrplot)
```


```R
corr_Mat<-cor(train[,-53],)
#https://cran.r-project.org/web/packages/corrplot/vignettes/corrplot-intro.html
corrplot(corr_Mat, method = "color",type="lower",order="AOE",tl.cex=0.75,tl.srt=27)
```


![png](Course_Project_files/Course_Project_13_0.png)



```R
highCorr<-train[,-findCorrelation(corr_Mat,cutoff=0.5)]
colnames(highCorr)
```


<ol class=list-inline>
	<li>'gyros_belt_x'</li>
	<li>'gyros_belt_y'</li>
	<li>'gyros_belt_z'</li>
	<li>'magnet_belt_y'</li>
	<li>'roll_arm'</li>
	<li>'yaw_arm'</li>
	<li>'total_accel_arm'</li>
	<li>'gyros_arm_y'</li>
	<li>'gyros_arm_z'</li>
	<li>'magnet_arm_z'</li>
	<li>'roll_dumbbell'</li>
	<li>'pitch_dumbbell'</li>
	<li>'roll_forearm'</li>
	<li>'pitch_forearm'</li>
	<li>'yaw_forearm'</li>
	<li>'total_accel_forearm'</li>
	<li>'gyros_forearm_x'</li>
	<li>'gyros_forearm_y'</li>
	<li>'accel_forearm_z'</li>
	<li>'magnet_forearm_x'</li>
	<li>'magnet_forearm_y'</li>
	<li>'classe'</li>
</ol>




```R
dim(highCorr)
```


<ol class=list-inline>
	<li>14718</li>
	<li>22</li>
</ol>



## Finally 22 columns lets hope We're on the path
# Prediction Model
Here I have to predict among 5 classes: 'A','B','C','D','E' so I think I am gonna give a shot to decision tree and for that I am using C4.5 algorithm(Ranked 1 in top in 2008) using C50 package(extension of C4.5)


```R
#https://cran.r-project.org/web/packages/C50/C50.pdf
library(C50)
```


```R
model_1<-C5.0(classe ~ ., data=highCorr,rules=TRUE)
#summary(model_1)
test_model_1<-predict.C5.0(model_1,test,type="class")
```


```R
ans_model_1<-as.data.frame(test_model_1)
dim(ans_model_1)
```


<ol class=list-inline>
	<li>4904</li>
	<li>1</li>
</ol>




```R
#lets check our model validity. following matrix code requires e1071
confusion_Mat<-confusionMatrix(ans_model_1$test_model_1,test$classe)
confusion_Mat$table
```


              Reference
    Prediction    A    B    C    D    E
             A 1323   31   12    7    8
             B   29  858   28   15    7
             C   26   23  784   52   17
             D   15   27   24  721   21
             E    2   10    7    9  848



```R
accuracy_model_1<-sum((ans_model_1$test_model_1==test$classe))/dim(test)[1]
accuracy_model_1
```


0.924551386623165


Thats pretty good but I want better than that so I am searching for model to best fit my data  but of course no overfitting

## Now trying randomForest 
this is i think quite slow but lets hope it would be fine for me


```R
#classification by randomforest
library(randomForest)
```

    randomForest 4.6-12
    Type rfNews() to see new features/changes/bug fixes.
    
    Attaching package: ‘randomForest’
    
    The following object is masked from ‘package:ggplot2’:
    
        margin
    



```R
set.seed(2727)
model_2<-randomForest(classe~.,data=highCorr,importance=TRUE)
#summary(model_2)

```


```R
#short info plot 
varImpPlot(model_2)
```


![png](Course_Project_files/Course_Project_26_0.png)



```R
test_model_2<-predict(model_2,test)
```


```R
ans_model_2<-as.data.frame(test_model_2)
dim(ans_model_2)
```


<ol class=list-inline>
	<li>4904</li>
	<li>1</li>
</ol>




```R
#lets check our model validity. following matrix code requires e1071
confusion_Mat<-confusionMatrix(ans_model_2$test_model_2,test$classe)
confusion_Mat$table
```


              Reference
    Prediction    A    B    C    D    E
             A 1388    6    2    1    0
             B    3  939   10    0    0
             C    3    2  835   27    1
             D    1    0    8  775    2
             E    0    2    0    1  898



```R
accuracy_model_2<-sum((ans_model_2$test_model_2==test$classe))/dim(test)[1]
accuracy_model_2
```


0.986337683523654


I think its great accuracy (98.63) so this model is best fiting.However still we can tune the random forest parameters so that we can achieve very high accuracy


```R
print(testing_ans_model_2)
```

       testing_model_2
    1                B
    2                A
    3                B
    4                A
    5                A
    6                E
    7                D
    8                B
    9                A
    10               A
    11               B
    12               C
    13               B
    14               A
    15               E
    16               E
    17               A
    18               B
    19               B
    20               B



```R

```
