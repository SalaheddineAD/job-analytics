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

## Conclusion
By constructing and deploying this data pipeline, we unlock vital insights into job market trends, skill demand, and salary benchmarks, aiding job seekers and recruiters alike in making informed decisions.
