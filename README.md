# Real-time analytics with Amazon Managed Streaming for Apache Kafka and Kinesis Data Analytics for Apache Flink
In this example we will use Kinesis Data Analytics studio to process data from Apache kafka on Amazon MSK.

## Create an Amazon MSK Cluster
1. You can follow the steps mentioned here: https://docs.aws.amazon.com/msk/latest/developerguide/create-cluster.html or using AWS Management console. 
2. For this example make sure the MSK ClientBroker value (Encryption) is `PLAINTEXT` and access control method is none. See the detail here: https://docs.aws.amazon.com/kinesisanalytics/latest/java/example-notebook-msk.html

## Add a NAT Gateway to your VPC
1. Follow the steps mentioned here: https://docs.aws.amazon.com/kinesisanalytics/latest/java/example-notebook-msk.html#example-notebook-msk-nat to add a NAT Gateway to your VPC. 

## Create Studio notebook
1. Go to Kinesis Data Analytics Console: console.aws.amazon.com/kinesisanalytics
2. Click on the Studio tab
3. Click on create Studio notebook
4. Choose "Create with custom settings" as create method
5. Enter a notebook name
6. For AWS Gluedatabse click on the refresh button and select Default Glue database. If the list is still empty, create a new Glue database.
![lab1](/images/lab1.png)



