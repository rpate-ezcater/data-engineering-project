# Building a simple End-to-End Data Engineering System 
This project uses different tools such as kafka, airflow, spark, postgres and docker. 

A step by step guide to run this pipeline: https://medium.com/@hamzagharbi_19502/end-to-end-data-engineering-system-on-real-data-with-kafka-spark-airflow-postgres-and-docker-a70e18df4090

## Overview

1. Data Streaming: Initially, data is streamed from the API into a Kafka topic.
  
2. Data Processing: A Spark job then takes over, consuming the data from the Kafka topic and transferring it to a PostgreSQL database.
   
3. Scheduling with Airflow: Both the streaming task and the Spark job are orchestrated using Airflow. While in a real-world scenario, the Kafka producer would constantly listen to the API, for demonstration purposes, we'll schedule the Kafka streaming task to run daily. Once the streaming is complete, the Spark job processes the data, making it ready for use by the LLM application.

All of these tools will be built and run using docker, and more specifically docker-compose.

![chatuml-diagram](https://github.com/HamzaG737/data-engineering-project/assets/71135893/ce92b731-038a-4d9c-9722-f97a6ba51153)

## Getting started
* install `asdf` versions
* run:
```
poetry install
```
* setup the network: 
```
docker network create airflow-kafka
```
* spin up kafka: 
```
docker compose up
```
* go to [kafka ui dashboard](http://localhost:8000/)
* create a PostgreSQL database
* seed it:
```
poetry run python ./scripts/create_table.py
```
* build spark docker container: 
```
docker build -f spark/Dockerfile -t rappel-conso/spark:latest --build-arg POSTGRES_PASSWORD=$POSTGRES_PASSWORD  .
```
* create some env. var. for building and running airflow:
```
echo -e "AIRFLOW_UID=$(id -u)\nAIRFLOW_PROJ_DIR=\"./airflow_resources\"" > .env
```
* go to [airflow dashboard](http://localhost:8080/)
  * login is airflow/airflow
  * run the `kafka_spark_dag`

