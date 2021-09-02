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
![lab2](/images/lab2.png)

7. For scaling configuration change it as - Parallelism: 4, Parallelism per KPU: 1
![lab3](/images/lab3.png)

8. For Logging and monitoring - leave it as default
9. On the networking section, select "VPC configuration based on Amazon MSK cluster". This will automatically select the all the subnets and the security groupattached to  your Amazon MSK cluster.

![lab4](/images/lab4.png)

10. In this example we have added a security group rule which allows all inbound traffic from the same security group source.
![lab5](/images/lab5.png)

11. Ensure AWS supported connectors are selected
![lab6](/images/lab6.png)

12. Select next and create the notebook

## Working with Kinesis Data Analytics Studio
1. Go to Kinesis Data Analytics Console: http://console.aws.amazon.com/kinesisanalytics
2. Click on the Studio tab and select the notebook you have created in the previous step
3. Click on Run and Click on Open in Apache Zeppelin once the statue of the Notebook is running

![lab7](/images/lab7.png)

## Working with Kinesis Data Analytics Studio - create a prerequisite notebook
1. In the notebook console create a new note
2. enter the name of your notebook- "prerequisite"
3. Select Default Interprete as Flink and create the notebook
![lab8](/images/lab8.png)

** We are going to use the Kafka python client (https://kafka-python.readthedocs.io/en/master/) to ingest data to Amazon MSK.
4. execute the following code to install kafka-python

```
%flink.ipyflink

pip install kafka-python
```
 

