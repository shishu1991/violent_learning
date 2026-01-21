# Explore Open search

**Table Of Contents**

1. [Objective](#objective)
2. [Installation](#Installation)

## Objective
This document is maintained to explore and find out the usage of **OpenSearch** NoSql database as a persistence storage for the services.

## Installation on docker

Run the following commands to install the opensearch in standalone mode.

- To search the image on docker hub
    > docker search opensearch
- To pull teh image of opensearch
    > docker pull opensearchproject/opensearch:latest
- To containerize the image on docker
    > docker run -d -p 9200:9200 -p 9600:9600 -e "discovery.type=single-node" opensearchproject/opensearch:latest

- Incase you get the error in windows 
    > Remove-item alias:curl 
- To verify OpenSearch is running
    > curl https://localhost:9200 -ku 'admin:admin'

