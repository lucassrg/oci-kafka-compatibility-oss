# Build a Consumer application

## Introduction

In this lab you will build a Consumer application microservice. This is based on a shipping microservice that needs to consume all orders were placed to the topic to ship them to customers.


Estimated time: 15 minutes

### Objectives

In this lab, you will:

* Clone the git repository
* Update Application Configuration
* Build and test the application


## Task 1: Clone Git Repository

1. First you need to clone the repository containing the shipping service:

    ```bash
    git clone https://github.com/lucassrg/shipping-svc-kafka.git

    ```

## Task 2:  Update OCI Streaming Service (OSS) configuration file

To simplify this exercise, the consumer application is based on the same architecture of the previous lab. 

1. Following the same idea, you need to update the application configuration file to point to the OSS Stream:
> src > main > resources > application-oss.yml

```yaml
micronaut:
  application:
    name: shippingSvc
  server:
    port: 8081
kafka:
  bootstrap:
    servers: cell-1.streaming.us-ashburn-1.oci.oraclecloud.com:9092
  security:
    protocol: SASL_SSL
  sasl:
    mechanism: PLAIN
    jaas:
      config: org.apache.kafka.common.security.plain.PlainLoginModule required username="odax/streaming-app-user/ocid1.streampool.oc1.iad.amaaaaaadoga" password="d$.ab2rGa1C8J=sr8#bD";

```

## Task 3: Testing the application

1. Running the Shipping service

    ```bash
    $ ./gradlew run -Dmicronaut.environments=oss
    ```

1. Testing the Shipping service

    ```bash
    $ curl -s localhost:8081/shipping/shipments/recent/5 | jq
    []
    ```



You may now [proceed to the next lab](#next).

## Acknowledgements

* **Author** - Lucas Gomes
* **Contributors** -  Rishi Johari
* **Last Updated By/Date** - Lucas Gomes, August 2021