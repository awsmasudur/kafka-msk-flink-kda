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

## Generating random data to Amazon MSK

Create a new paragraph and execute the below code. Replace the broker details (BROKERS). To get the MSK broker information, got to Amazon MSK console, Select the Cluster that you have created earlier and copy the bootstrap server name.

![lab9](/images/lab9.png)

```
%flink.ipyflink
from kafka import KafkaProducer
import json
import random
from datetime import datetime
topicname='kafkadevicestatus'

BROKERS = "ADD_YOUR_BROKER_NAME-1:9092,ADD_YOUR_BROKER_NAME-2:9092, ADD_YOUR_BROKER_NAME-3:9092"
producer = KafkaProducer(
    bootstrap_servers=BROKERS,
    value_serializer=lambda v: json.dumps(v).encode('utf-8'),
    retry_backoff_ms=500,
    request_timeout_ms=20000,
    security_protocol='PLAINTEXT')


#random location in AU
def getlanlon(): 
    randomnum = random.randint(0, 98)
    a=["40.7518353890324,-73.8808908305844","40.775198767451,-73.9537058517705","40.8923845153913,-73.8592161325675","40.6787276522968,-73.9136169588798",
        "40.698714735759,-73.9291922785739","40.8923845153913,-73.8592161325675","40.7235486791852,-74.0106103232247","40.8461730422632,-73.8848712214929", 
        "40.7125931529659,-73.8010844588638","40.8923845153913,-73.8592161325675","40.6816231883917,-73.8696806661599",
        "40.8923845153913,-73.8592161325675","40.8455736116659,-73.9153086719575v","40.7132768601426,-73.76482663745","40.8587638107519,-73.8973575470578",
        "40.8587638107519,-73.8973575470578","40.6848698470809,-74.0046909294697","40.7666960929776,-73.8309297851476","40.8923845153913,-73.8592161325675",
        "40.8923845153913,-73.8592161325675","40.7184624813724,-73.9877779558559","40.8549353413757,-73.8610341478372","40.8923845153913,-73.8592161325675",
        "40.8923845153913,-73.8592161325675","40.5984554477103,-73.973017951956","40.700843849553,-73.8869254474369","40.8923845153913,-73.8592161325675",
        "40.8923845153913,-73.8592161325675","40.8923845153913,-73.8592161325675","40.8472260173933,-73.9082511892843","40.6791862319457,-73.9138867644863",
        "40.8923845153913,-73.8592161325675","40.8461403842737,-73.934232825199","40.8459552316971,-73.9154527607596","40.8923845153913,-73.8592161325675",
        "40.7295464582261,-74.0008587161537","40.7316872604435,-74.0050333929424","40.6985999117196,-73.9346019665725","40.8594360234234,-73.8970889921955",
        "40.8923845153913,-73.8592161325675","40.6951077104142,-73.9433395799656","40.8637874511569,-73.9014676616923","40.8923845153913,-73.8592161325675",
        "40.652572662656,-73.950925851581","40.8070060574389,-73.9571876118584","40.6918861602162,-73.843462886304","40.8923845153913,-73.8592161325675",
        "40.7875499743226,-73.8502942535412","40.7542578482028,-73.8908051099242","40.7566122859202,-73.8846037042988","40.8923845153913,-73.8592161325675",
        "40.8241122127035,-73.9528624702588","40.8923845153913,-73.8592161325675","40.8155146182071,-73.8877024363141","40.8365565664016,-73.9039401169488",
        "40.7522695734553,-73.9874541363821","40.8923845153913,-73.8592161325675","40.8923845153913,-73.8592161325675","40.8013271246571,-73.965040014598",
        "40.8781189395912,-73.882472661198","40.8923845153913,-73.8592161325675","40.8923845153913,-73.8592161325675","40.7623040918185,-73.9116350647402",
        "40.7402560133096,-73.7863904837366","40.7273113152272,-73.9854852973404","40.8923845153913,-73.8592161325675","40.8251701858232,-73.9163571869964",
        "40.8468034696427,-73.8925182304226","40.8896096580399,-73.8505020406496","40.8923845153913,-73.8592161325675","40.7378068112698,-73.781534065117",
        "40.6059531507952,-73.758354858204","40.8923845153913,-73.8592161325675","40.7411022042417,-73.7824218228985","40.6845308258689,-73.8213055774791",
        "40.8923845153913,-73.8592161325675","40.6728245807747,-73.9226550896945","40.7730484450396,-73.9252970392453","40.729650759574,-74.00076851608",
        "40.8587638107519,-73.8973575470578","40.8923845153913,-73.8592161325675","40.7416259095131,-74.0069720184859","40.8382648423732,-73.9052278302826",
        "40.8923845153913,-73.8592161325675","40.8923845153913,-73.8592161325675","40.767508434396,-73.9120397120535","40.8680964442142,-73.9228618373219",
        "40.5924278329831,-73.9728619592552","40.6778536206197,-73.8486873983378","40.8923845153913,-73.8592161325675","40.6363610672489,-73.9554703641128",
        "40.8923845153913,-73.8592161325675","40.6942573045093,-73.9760299779354","40.8923845153913,-73.8592161325675","40.8923845153913,-73.8592161325675",
        "40.8923845153913,-73.8592161325675","40.8411545247279,-73.9046237719753","40.8923845153913,-73.8592161325675","40.7181953856582,-73.9041143166198"
        ]
    b=a[randomnum]
    return b


def getModel():
    product=["Ultra WiFi Modem", "Ultra WiFi Booster", "EVG2000", "Sagemcom 5366 TN", "ASUS AX5400"]
    randomnum = random.randint(0, 4)
    return (product[randomnum])

def getInterfaceStatus():
    status=["connected", "connected", "connected", "connected", "connected", "connected", "connected", "connected", "connected", "connected", "connected", "connected", "down", "down"]
    randomnum = random.randint(0, 13)
    return (status[randomnum])


def getCPU():
    i = random.randint(50, 100)
    return (str(i))

def getMemory():
    i = random.randint(1000, 1500)
    return (str(i))
    
def genData():
	
    model=getModel()
    deviceid='dvc' + str(random.randint(1000, 10000))
    interface='eth4.1'
    interfacestatus=getInterfaceStatus()
    cpuusage=getCPU()
    memoryusage=getMemory()
    now = datetime.now()
    event_time = now.strftime("%Y-%m-%d %H:%M:%S")
    location=getlanlon()
    
    new_dict={}
    new_dict["model"]=model
    new_dict["deviceid"]=deviceid
    new_dict["interface"]=interface
    new_dict["interfacestatus"]=interfacestatus
    new_dict["cpuusage"]=cpuusage
    new_dict["memoryusage"]=memoryusage
    new_dict["event_time"]=event_time
    new_dict["location"]=location
    #new_dict["state"]=state
    #new_dict["postcode"]=postcode
    #new_dict["suburb"]=suburb
    return new_dict

while True:
    data =genData()
    print(data)
    try:
        future = producer.send(topicname, value=data)
        producer.flush()
        record_metadata = future.get(timeout=10)
        #print("sent event to Kafka! topic {} partition {} offset {}".format(record_metadata.topic, record_metadata.partition, record_metadata.offset))
    except Exception as e:
        print(e.with_traceback())
        
```

# Real-time analytics with Flink SQL
1. In the notebook console create a new note
2. enter the name of your notebook- "flinkSQLExample"
3. Select Default Interprete as Flink and create the notebook
4. Execute the below code. Change the bootstrap server name as you have done earlier to ingest data to your Kafka cluster.

```

%flink.ssql

CREATE TABLE lab3 (
    model VARCHAR(50),
    deviceid VARCHAR(50),
    interface VARCHAR(50),
    interfacestatus VARCHAR(50),
    cpuusage DOUBLE,
    memoryusage DOUBLE,
    location VARCHAR(100),
    event_time TIMESTAMP(3),
    WATERMARK FOR event_time AS event_time - INTERVAL '5' SECOND
)
PARTITIONED BY (deviceid)
WITH  (
 'connector' = 'kafka',
 'topic' = 'kafkadevicestatus',
 'properties.bootstrap.servers' = 'b-3.mskkda.atgger.c3.kafka.ap-southeast-2.amazonaws.com:9092,b-2.mskkda.atgger.c3.kafka.ap-southeast-2.amazonaws.com:9092,b-1.mskkda.atgger.c3.kafka.ap-southeast-2.amazonaws.com:9092',
 'properties.group.id' = 'kafkadevicestatus',
 'format' = 'json',
 'scan.startup.mode' = 'latest-offset'
)

```

5. Add a new paragraph and start analyzing data in real-time

```

%flink.ssql(type=update)
select * from lab3

```

![lab10](/images/lab10.png)

6. Add a new paragraph and execute the below code. 

```
%flink.ssql(type=update)
--device model wise up time
SELECT lab3.interfacestatus, COUNT(*) AS totalstatus, lab3.model,
       TUMBLE_END(event_time, INTERVAL '10' second) as tum_time
  FROM lab3
GROUP BY TUMBLE(event_time, INTERVAL '10' second), lab3.interfacestatus,lab3.model;


```
7. Stop the job once you see the data. Change the visualization as below and start the job again.

![lab11](/images/lab11.png)
