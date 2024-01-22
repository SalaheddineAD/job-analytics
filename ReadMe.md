# LinkedIn Job Analysis Pipeline

## Overview
This repository details a sophisticated job analysis pipeline, which includes a custom-built scraper for LinkedIn job postings. The scraper and the entire data pipeline are deployed on AWS EC2 and ECS instances, ensuring scalable and reliable data processing.

![LinkedIn Job Analysis Pipeline Architecture](https://github.com/SalaheddineAD/job-analytics/assets/93080778/b1724277-4c11-49a3-97fb-ca71ad88a7d2)


## Technologies
- **Python & Scrapy**: Crafted a scraper to extract job listings from LinkedIn.
- **Apache Kafka**: For managing real-time data streams.
- **Apache Spark**: For large-scale data processing.
- **Apache Cassandra**: For real-time and batch data storage.
- **Hadoop HDFS**: Used as a scalable data lake.
- **Docker**: For creating and managing containerized applications.
- **AWS EC2**: Hosting and deploying the pipeline in the cloud for enhanced performance.

## Features
### Speed Layer
- **Real-Time Analytics**: Analyzes the incoming data stream for immediate insights into job distribution by technology and location.

### Batch Layer
- **Skill Set Analysis**: Aggregates data to identify prevalent skill combinations in the job market.
- **Salary Prediction**: Applies predictive models to estimate salary ranges.
- **Demand Analysis**: Determines which job profiles are currently most sought after.
- **Location-Based Insights**: Compiles salary data by location to map out compensation landscapes.

## Data Source
- **LinkedIn Job Listings**: Extracted using the custom-built scraper.

## Deployment
- **AWS EC2**: The pipeline is fully deployed on AWS EC2, ensuring high availability and fault tolerance for continuous data analysis.

# Setting Up and Running a Data Pipeline on an X-Large EC2 Instance

## Step 1: Create an X-Large EC2 Instance

- Launch an X-Large EC2 instance on AWS.

## Step 2: Connect to EC2 Instance

- Use SSH to connect to your EC2 instance.

## Step 3: Copy `docker-compose.yml` to EC2

Assuming your `docker-compose.yml` file is located locally, use the following `scp` command to copy it to your EC2 instance. Replace placeholders accordingly:

```bash
scp -i /path/to/your/keypair.pem <path/to/local/docker-compose.yml> ec2-user@<your-ec2-instance-ip>:/path/on/ec2/docker-compose.yml

```

- **-i /path/to/your/keypair.pem**: Specify the path to your private key file used for SSH authentication.
- **<path/to/local/docker-compose.yml>**: Replace with the actual path to your local docker-compose.yml file.
ec2-user: The default user for Amazon Linux instances. Use the appropriate username for your AMI.
- <**your-ec2-instance-ip**> Replace with the public IP address or hostname of your EC2 instance.
- **:/path/on/ec2/docker-compose.yml**: Specify the path where you want to copy the file on the EC2 instance.

## Step4: Updates all installed packages and their dependencies
On a Linux system you can use the Yum package manager with this command: 
```bash
sudo yum update -y
```
## Step5: Install Docker
```bash
sudo yum install docker
```
## Step6: Install Docker Compose and sett permissions
```bash
sudo curl -L https://https://github.com/docker/compose/releases/Latest/download/docker-compose-$(uname -s)-$(uname -m) -o /user/local/bin/docker-compose
```
Below is an explanation of the above command. You don't need to know the details tho.
- Curl: allows you to download a file over Https.  -L : allows you to give the link
- The $(uname -s) and $(uname -m) command substitutions that allow the script to dynamically determine the OS and architecture to download the right Docker Compose binary.
Specifically:
  - **$(uname -s)** : prints the name of the operating system. So on Linux this would print "Linux", on MacOS it would print "Darwin". This allows picking the right OS binary.
  - **$(uname -m)**: prints the system architecture. For example "x86_64" on 64-bit Intel/AMD systems. This allows picking the right architecture binary.
  - **-o**: allows you to specify where you want to store the file you downloaded


Set permission with the command below:
```bash
sudo sudo chmod +x /usr/local/bin/docker-compose
```
Make sure it works with this command:
```bash
sudo sudo chmod +x /usr/local/bin/docker-compose
```

## Step 7: Use docker compose up to pull images and run containers
```bash
sudo docker-compose up -d
```
## Step 8: create kafka topic

Go to kafka bash:
```bash
Docker exec -i -t kafka bash
```
Create Kafka topic:
```bash
Kafka-topic.sh --create --topic your-topic-name –partitions 1 –replication-factor 1 –if-not-exists –zookeeper zookeeper:2181
```

When you set your producer and send messages. If you want to check if the messages are indeed sent you can use the command:
```bash
kafka-console-consumer.sh --topic your_topic_name --bootstrap-server localhost:9092 --from-beginning
```

## Step 9: Create Cassandra keyspace and table
Go to Cassandra terminal using:
```bash
docker exec -i -t cassandra bash
```

interact with Cassandra cluster using CQL by typing command  (username and password are Cassandra by default)
```bash
cqlsh -u myusername -p mypassword 
```
create keyspace
```bash
CREATE KEYSPACE IF NOT EXISTS jobs_analytics WITH replication =  {'class': 'SimpleStrategy', 'replication_factor': 1}
```

CREATE TABLE IF NOT EXIST jobs_analytics.job

```bash
CREATE TABLE IF NOT EXISTS jobs_analytics.job (title Text, company_name Text, job_date Date, job_link Text, job_location Text, job_seniority_level Text, job_employment_type Text, job_function Text, job_industries Text, number_applicants int, job_description Text, Primary key ((job_date, job_seniority_level, job_location), company_name, title)); 
```

## Step 10: Create a folder in hdfs:

Docker exec -i -t  namenode bash 

hdfs dfs -mkdir -p output1/scraped_jobs/

## Congraats!
Now your pipeline is almost ready. 

You just need to connect your producer with the kafka broker and you can process the data stored as you please. 