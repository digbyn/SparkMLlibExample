# SparkMLlibExample
Running through existing scala examples calling MLib
Target = those new to scala/spark/mlib 
When you can, please review the source examples we will be using here:

https://spark.apache.org/docs/1.5.0/mllib-linear-methods.html
https://spark.apache.org/docs/1.5.0/mllib-naive-bayes.html


Here are my notes on setting up a sample Spark-Shell session and executing some scala scripts calling MLib.
SETUP:

Get access to a working cluster, either one you have installed or by using the Cloudera QuickStart VM.
From your host OS get a terminal window going and get access to the cluster usually via ssh root@machinename.
If you are connected to your cluster as 'root' user, probably best to switch to 'hdfs' user.

[root@quickstart cloudera]#

[root@quickstart cloudera]# su - hdfs
-bash-4.1$ whoami
hdfs
-bash-4.1$

In this example we have 2 datasets and 2 scala scripts to execute.
You will need to copy these unto your cluster or create the files on your cluster.

Typical scp command to move a file from your host OS to the cluster:
scp sample_naive_bayes_data.txt root@localhost:/home/cloudera

Source data files:
sample_libsvm_data.txt
sample_naive_bayes_data.txt

Source scala scripts:
naive_bayes_build.scala
svm.scala

The two txt data files will need to be moved over to HDFS for the user HDFS, for example:

sudo -u hdfs hadoop fs -put sample_libsvm_data.txt  /user/hdfs
sudo -u hdfs hadoop fs -put sample_naive_bayes_data.txt  /user/hdfs

Check they all made it!
[root@quickstart cloudera]# sudo -u hdfs hadoop fs -ls /user/hdfs
Found 4 items
drwxr-xr-x   - hdfs supergroup          0 2015-11-12 16:00 /user/hdfs/.Trash
drwxr-xr-x   - hdfs supergroup          0 2015-11-13 09:31 /user/hdfs/.sparkStaging
drwxr-xr-x   - hdfs supergroup          0 2015-11-12 17:12 /user/hdfs/myModelPath
-rw-r--r--   1 hdfs supergroup     104736 2015-11-12 17:09 /user/hdfs/sample_libsvm_data.txt
[root@quickstart cloudera]#

To invoke the spark shell type:

$ spark-shell

The spark-shell should start up without any errors, if you do get some errors they could be related to which node within a cluster you are connected and starting the spark-shell from or with user privs and hdfs. The spark-shell works best when invoked from a gateway node or from where spark is installed.

Once at the spark-shell command prompt;  This will load the svm.scala script and run it. 

scala> :load svm.scala

Review the results:

15/11/13 09:34:09 INFO DAGScheduler: Job 625 finished: treeAggregate at GradientDescent.scala:189, took 0.040879 s
15/11/13 09:34:09 INFO GradientDescent: GradientDescent.runMiniBatchSGD finished. Last 10 stochastic losses 1407.5720834443325, 1407.3036372657693, 1407.0358910784314, 1406.7688394349036, 1406.5024769580573, 1406.2367983397894, 1405.9718942069985, 1405.7084084658204, 1405.446171584704, 1405.184594420055
modelL1: org.apache.spark.mllib.classification.SVMModel = (weights=[0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,-0.0,-1.4307409320323257,-0.0,-0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,0.0,-0.0,-1.3640742653656597,-0.6474075986989937,-0.8307409320323272,-10.08074093203233,-14.564074265365662,-12.09240700046983,-11.02065555834261,-2.3963449070775447,9.234925163654113,24.067598282920112,1.6104286831070191,-0.0,0.0...


Try it again with the naive_bayes_build.scala model build.
Go read the docs on next steps on how to apply the model against additional datasets in hfs.
https://spark.apache.org/docs/1.5.0/mllib-naive-bayes.html

Congrats!
