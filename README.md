# Real-time analytics with Amazon Managed Streaming for Apache Kafka and Kinesis Data Analytics for Apache Flink
In this example we will use Kinesis Data Analytics studio to process data from Apache kafka on Amazon MSK.

## Create an Amazon MSK Cluster
1. You can follow the steps mentioned here: https://docs.aws.amazon.com/msk/latest/developerguide/create-cluster.html or using AWS Management console. 
2. For this example make sure the MSK ClientBroker value (Encryption) is `PLAINTEXT` and access control method is none. See the detail here: https://docs.aws.amazon.com/kinesisanalytics/latest/java/example-notebook-msk.html

## Add a NAT Gateway to your VPC
1. Follow the steps mentioned here: https://docs.aws.amazon.com/kinesisanalytics/latest/java/example-notebook-msk.html#example-notebook-msk-nat to add a NAT Gateway to your VPC. 

## Create Studio notebook
1.


