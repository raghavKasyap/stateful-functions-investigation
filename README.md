# Stateful Functions Investigation
This repository contains the code for running stateful functions on an existing flink session cluster. It contains the Stateful Functions, Kafka producer & consumer to write into & read from topics, Flink Datastream SDK of stateful functions to submit the stateful functions pipeline to the flink job manager. 

## Application Architecture

The Application Architecture can be referred from the attached presentation project_review

## Project Structure

1. issuemgnt-stateful-function folder contains the golang stateful functions which handle the business logic
2. kafka-producer-consumer folder consists of the producers for config and events topic and consumer to read from alerts topic
3. datastream-job folder consists of Datastream job which binds the ingresses, egresses and function runtimes. The Jar file of this must to submited to Flink Job Manager

## Setup

1. Execute the docker-compose file. It starts up the kafka zookeeper, kafka broker, flink job-manager and flink task-manager in seperate containers.
2. Ensure that all the containers are running and none of them have failed. 
3. Build image using the Dockerfile in the issuemgnt-stateful-function folder. It will create an image of the stateful functions runtime. Run this image using the docker run command to start the function runtimes which exposes port 8000. 
4. Next we need to send data to config & events topics and also start the consumer to check on alerts generated from the pipeline. 
5. Open kafka-producer-consumer in intellij using import project from existing source and set JDK version to 18. Download the maven dependencies. Run EventProducer, ConfigProducer Classes. Also run AlertsConsumer which will poll and consume records from alerts topic as and when written by the pipeline.
6. Finally we need to build the jar file of the job and submit it to the flink jobmanager.
7. Import datastream-job folder as maven project and run `mvn package install` to create the Jar file. Submit the Jar file to the flink job-manager from the web endpoint localhost:8081/#/submit and run the job. 
