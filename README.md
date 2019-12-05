# SDN Project Kafka Deployment

## Project Overview
* Deploy Kafka
* Application is exposing data using ReST services - for that any standard API available on open network can be used. weather data API is used in this project.
* Producer: Calls the REST api to read the data (in JSON format) and puts it on the kafka bus using Java api's.
* Consumers: Three consumers are there to consume the data and do specific tasks
  * Consumer1 : Prints data locally .
  * Consumer2 : Send data to REST application using Java api's.
  * Consumer3 : Stores the data in Elasticsearch DB.

## Getting Started


### Clone this repository
# Producer:
$ git clone https://github.com/ankitramtriya/kafka-producer.git

Prerequisites
Apche Kafka
Elasticsearch
Prerequisites Installation
Install Apache kafka on ubuntu
Step 1 - Pre-requisite

Apache Kafka required Java to run. You must have java installed on your system. If java is not installed, execute below command to install default OpenJDK on your system.

$ sudo apt install default-jdk
Step 2 – Download Apache Kafka

Download the Apache Kafka binary files from its official download website. You can also select any nearby mirror to download.

$ wget http://www-us.apache.org/dist/kafka/2.2.1/kafka_2.12-2.2.1.tgz
Extract the archive file and move it to the usr/local/kafka directory

$ tar xzf kafka_2.12-2.2.1.tgz
$ mv kafka_2.12-2.2.1 /usr/local/kafka
Step 3 – Start Kafka Server

Kafka uses ZooKeeper, so first, start a ZooKeeper server on your system. You can use the script available with Kafka to get start single-node ZooKeeper instance.

$ cd /usr/local/kafka
$ bin/zookeeper-server-start.sh config/zookeeper.properties
Start the Kafka server:

$ bin/kafka-server-start.sh config/server.properties

Create another file with name server1.properties
change value of log-dir to /tmp/kafka-log2
change value of port to 9093
now run below command
$ bin/kafka-server-start.sh config/server1.properties

Step 4 – Create a Topic in Kafka

$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 2 --partitions 6 --topic twitter_tweets
The replication-factor describes how many copies of data will be created. As we are running with two instance keep this value 2.



You will get output like this if the above command executes successfully.

Created topic "twitter_tweets".
See all the created topic on Kafka by running the list topic command

$ bin/kafka-topics.sh --list --zookeeper localhost:2181
Step 5 - After installing kafka come out from this directory or open new terminal with this same repository folder


Install Elasticsearch
Step 1 - Dependencies

First, update the list of available packages by running the given below command

$ apt-get update.
Step 1.1 - Test your installed java version

You can then check that Java is installed by running the command

$ java -version.
That’s all the dependencies we need for now, so let’s get started with obtaining and installing Elasticsearch.

Step 2 - Download and Install

Elasticsearch can be downloaded directly from their site in zip, tar.gz, deb, or rpm packages.

Step 2.2 - Download the archive

$ wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-0.90.7.zip
$ unzip elasticsearch-0.90.7.zip
Step 3 - Edit "elasticsearch.yml" file

Go to the config folder in elasticsearch-0.90.7 . Edit the file elasticsearch.yml

$ sudo vi /etc/elasticsearch/elasticsearch.yml
Then find the line that specifies network.bind_host, then uncomment it and change the value to localhost so it looks like the following:

network.bind_host: localhost
Then insert the following line somewhere in the file, to disable dynamic scripts:

script.disable_dynamic: true
Save and exit. Now restart Elasticsearch to put the changes into effect:

$ sudo service elasticsearch restart
Step 4 - Test your Elasticsearch install

You have now extracted the zip to a directory and have the Elasticsearch binaries available, and can start the server.Make sure you’re in the resulting directory.

Let’s ensure that everything is working. Move out of the config directory and run

$ ./bin/elasticsearch
Elasticsearch should now be running on port 9200. Do note that Elasticsearch takes some time to fully start, so running the curl command below immediately might fail. It shouldn’t take longer than ten seconds to start responding, so if the below command fails, something else is likely wrong.



Consumer application
Step 1 - Go inside the Spring-Boot-Kafka-Consumer directory and build the application using maven and before building you need to change log file path to wherever you want

$ cd Spring-Boot-Kafka-Consumer
$ vim src/main/resources/logback-spring.xml
Step 2 - Change logfile path

<file>your_log_file.log</file>
<fileNamePattern>your_log_file.%d{yyyy-MM-dd}.log</fileNamePattern>
Step 3 - Build application

$ mvm clean install
On succession of build, Kafka-Consumer-App.jar file gets generated inside the "target" folder and that .jar file is the final build for consumer application

Step 4 - Run application

$ java -jar Kafka-Consumer-App.jar
Opens the log of consumer

Producer application
Step 1 - Go inside the Spring-Boot-Kafka-Producer directory and build the application using maven and before building you need to change log file path to wherever you want

$ cd Spring-Boot-Kafka-Producer
$ vim src/main/resources/logback-spring.xml
Step 2 - Change logfile path

<file>your_log_file.log</file>
<fileNamePattern>your_log_file.%d{yyyy-MM-dd}.log</fileNamePattern>
Step 3 - Build application

$ mvm clean install
On succession of build, Kafka-Producer-App.jar file gets generated inside the "target" folder and that .jar file is the final build for consumer application

Step 4 - Run application

$ java -jar Kafka-Producer-App.jar
Open the log of producer

When producer started scheduled task of getting data from the weather and push it to the kafka broker, then all the consumers listning to this broker will recieve data and started doing their specific tasks.

Step 5 - To check that consumer3 has successfully inserted data to elasticsearch, go to consumer3 log and find the below log message

Consumed JSON Message :{ elasticsearch URL :: http://localhost:9200/weather/databus/{id}/_create } | { id : 38 } Data Sent To Elasticsearch.
Step 6 - Copy the above "id" and replace it with in below url

http://localhost:9200/weather/databus/<id>
Step 7 - Open the above url in the browser

You can see that the data is succesully inserted !!!

Thanks
Prof. Samar Shailendra, for allowing us to do this project.
