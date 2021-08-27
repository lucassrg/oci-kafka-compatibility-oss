# Build a Producer application

## Introduction

In this lab you will build a Producer application microservice.


Estimated time: 15 minutes

### Objectives

In this lab, you will:

* Clone the git repository
* Update Application Configuration
* Build and test the application


## Task 1: Clone Git Repository

1. First you need to clone the repository containing the Producer application (Order Service). This is a service that is responsible for placing orders.

    ```bash
    git clone https://github.com/lucassrg/order-svc-kafka.git
    ```

1. Open up the project using your favorite IDE.


## Task 2:  Update OCI Streaming Service (OSS) configuration file

1. Next, we will copy the server configuration details from OSS in order to setup the application.

1. Go to the OCI console on `> Home > Analytics > Streaming > Stream Pools > e-commerce`

1. Under Resources section, click on Kafka Connection Settings and then `Copy All`.

1. Take notes on the these settings, for example:

    ```bash
    bootstrap.servers=cell-1.streaming.us-ashburn-1.oci.oraclecloud.com:9092
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="odax/oracleidentitycloudservice/lucas.gomes@oracle.com/ocid1.streampool.oc1.iad.amaaaaaadoga" password="AUTH_TOKEN";
    ```

    Note that we need to modify the username returned as part of the `sasl.jaas.config` string since we created an aplication specific user. The username is composed of the following values:
    `username="<tenancy name>/<username>/<stream pool ocid>"`

1. Now update the application configuration file: `> src > main > resources > application-oss.yml`

    ```yaml
    micronaut:
    application:
        name: orderSvc
    server:
        port: 8080
    kafka:
    bootstrap:
        servers: streaming.us-phoenix-1.oci.oraclecloud.com:9092
    security:
        protocol: SASL_SSL
    sasl:
        mechanism: PLAIN
        jaas:
        config: ${KAFKA_SASL_JAAS_CONFIG}
    ```

    Rather than using the variable `${KAFKA_SASL_JAAS_CONFIG}` in this example we will directly store the value in the property `sasl.jaas.config` with the username change. We should also replace the `"AUTH_TOKEN"` string with the IAM Auth token created on step 3.

1. At the end you should have something similar to this:

    ```yaml
    micronaut:
    application:
        name: orderSvc
    server:
        port: 8080
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


1. Running the Order service

    ```bash
    $ ./gradlew run -Dmicronaut.environments=oss
    ```

1. Testing the Order service


    ```bash
    $ curl -s localhost:8080/order | jq
    []
    ```

1. Adding new orders to the system

    ```bash
    $ curl -s -H "Content-Type: application/json" -X POST -d '{"customerId": 1, "totalCost": 100.00}' localhost:8080/order | jq

    $ curl -s -H "Content-Type: application/json" -X POST -d '{"customerId": 2, "totalCost": 200.00}' localhost:8080/order | jq
    ```

1. Listing orders created by the service

    ```bash
    curl -s localhost:8080/order | jq                                                                                        [
    {
        "id": 0,
        "customerId": 1,
        "totalCost": 100,
        "shipmentStatus": "PENDING"
    },
    {
        "id": 1,
        "customerId": 2,
        "totalCost": 200,
        "shipmentStatus": "PENDING"
    }
    ]
    ```



You may now [proceed to the next lab](#next).

## Acknowledgements

* **Author** - Lucas Gomes
* **Contributors** -  Rishi Johari
* **Last Updated By/Date** - Lucas Gomes, August 2021