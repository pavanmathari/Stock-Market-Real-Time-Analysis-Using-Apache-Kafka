# README.md

## Stock Market Real-Time Analysis Using Apache Kafka

This project outlines a real-time analysis system for stock market data using Apache Kafka and various AWS services to simulate a stock market application, process data streams, and analyze the data.

### Technologies Used:

- **Programming Language**: Python
- **Amazon Web Services (AWS)**:
  - S3 (Simple Storage Service)
  - Athena
  - Glue Crawler
  - Glue Catalog
  - EC2 (Elastic Compute Cloud)
- **Apache Kafka**

### Architecture Overview:
![Architecture](https://github.com/pavanmathari/Stock-Market-Real-Time-Analysis-Using-Apache-Kafka/assets/83055565/7bbd0a7c-723f-4fbe-a7eb-8fcb10c0f420)


### Data File:

The data file `indexprocessed.csv` is used as the source for the stock market simulation data in this project.

### Setup Steps:

#### Step 1: Create and Setup EC2 Instance

- Launch an EC2 instance on AWS using a Linux AMI.
- Connect to your instance using SSH.
- Install Apache Kafka by downloading it with the following command:
  ```
  wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz or wget https://archive.apache.org/dist/kafka/3.0.0/kafka_2.13-3.0.0.tgz 
  ```
- Extract Kafka:
  ```
  tar -xvf kafka_2.12-3.3.1.tgz
  ```

#### Step 2: Install Java (Kafka Dependency)

- Since Kafka runs on JVM, install Java using:
  ```
  sudo yum install java-1.8.0-openjdk or sudo apt-get install openjdk-8-jre
  ```
- Check Java version to ensure it's installed:
  ```
  java -version
  ```

#### Step 3: Start Zookeeper

- Start Zookeeper with:
  ```
  bin/zookeeper-server-start.sh config/zookeeper.properties
  ```

#### Step 4: Start Kafka Server

- Open another terminal and SSH into your EC2 machine.
- Set Kafka heap options:
  ```
  export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
  ```
- Navigate to Kafka directory and start the Kafka server:
  ```
  cd kafka_2.12-3.3.1
  bin/kafka-server-start.sh config/server.properties
  ```

#### Step 5: Configure Kafka for Public Access

- Modify `server.properties` to bind Kafka to the public IP of your EC2 instance. This can be done by editing the `ADVERTISED_LISTENERS` setting.

#### Step 6: Create Kafka Topic

- Create a topic named `demo_testing2`:
  ```
  bin/kafka-topics.sh --create --topic demo_test --bootstrap-server {EC2_Public_IP:9092} --replication-factor 1 --partitions 1
  ```

#### Step 7: Start Kafka Producer

- Start a Kafka producer to send messages to the topic:
  ```
  bin/kafka-console-producer.sh --topic demo_test --bootstrap-server {EC2_Public_IP:9092}
  ```

#### Step 8: Start Kafka Consumer

- Start a Kafka consumer to read messages from the topic:
  ```
  bin/kafka-console-consumer.sh --topic demo_test --bootstrap-server {EC2_Public_IP:9092}
  ```

#### Step 9: Store Data in S3

- Configure system to save the data to S3 using 
  for count, i in enumerate(consumer):
      with s3.open("s3://{YOUR BUCKET NAME}/stock_market_{}.json".format(count), 'w') as file:
          json.dump(i.value, file)  

#### Step 10: Crawl Data Using Glue Crawler

- Set up an AWS Glue Crawler to crawl over the data in S3.

#### Step 11: Catalog Data Using AWS Glue

- Use AWS Glue Data Catalog to catalog the data for querying.

#### Step 12: Analyze Data with Athena

- Finally, used Amazon Athena to perform queries and analyze the data.

### Important Notes:

- Replace `{EC2_Public_IP}` with the actual public IP address of your EC2 instance.
- Ensure all AWS resources are in the same region and have the proper permissions.
