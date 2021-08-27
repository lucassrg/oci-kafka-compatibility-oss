# Running and Troubleshooting the application

## Introduction

In this lab you will put all together and will publish messages into the topic from the Producer service that will be consumed by the Consumer service.


Estimated time: 5 minutes

### Objectives

In this lab, you will:

* Create Orders
* List Shipments


## Task 1: Create new Orders

1. Create new Orders using curl

    ```bash
    curl -s -H "Content-Type: application/json" -X POST -d '{"customerId": 4, "totalCost": 400.00}' localhost:8080/order | jq
    {
    "id": 1,
    "customerId": 4,
    "totalCost": 400,
    "shipmentStatus": "PENDING"
    }
    ```

1. Observe the shipping service console to see the log receiving new orders

    ```
    18:19:08.938 [pool-1-thread-2] INFO  c.r.messaging.ShipmentProducer - Sending message with shipment 0 received!
    18:19:08.938 [pool-1-thread-2] INFO  c.recursive.service.ShippingService - Shipment message sent!
    18:19:08.938 [pool-1-thread-2] INFO  c.recursive.messaging.OrderConsumer - Shipped order 0 with shipment ID 0...
    18:20:09.914 [pool-1-thread-3] INFO  c.recursive.messaging.OrderConsumer - Order with id 1 received!
    18:20:09.914 [pool-1-thread-3] INFO  c.recursive.messaging.OrderConsumer - Creating shipment...
    18:20:24.916 [pool-1-thread-3] INFO  c.recursive.service.ShippingService - Shipment created!
    18:20:24.916 [pool-1-thread-3] INFO  c.recursive.service.ShippingService - Sending shipment message...
    18:20:24.917 [pool-1-thread-3] INFO  c.r.messaging.ShipmentProducer - Sending message with key 36d08d00-85e5-41a8-b777-38be51039d6b received!
    ```

## Task 2: List Shipments 

1. List the shipments were created.

    ```bash
    curl -s localhost:8081/shipping/shipments/recent/5 | jq
    [
    {
        "id": 0,
        "orderId": 0,
        "shippedOn": "2021-02-13T00:19:08.933Z"
    },
    {
        "id": 1,
        "orderId": 1,
        "shippedOn": "2021-02-13T00:20:24.916Z"
    }
    ]
    ```


## Task 3: Troubleshooting

In case you have the following issue with gradle at the time you are compiling/building your project, you need to re-generate gradlew file using the command `gradle wrapper`.

```
Error: Could not find or load main class org.gradle.wrapper.GradleWrapperMain
Caused by: java.lang.ClassNotFoundException: org.gradle.wrapper.GradleWrapperMain
```

## References:
* https://recursive.codes/p/easy-messaging-with-micronauts-kafka-support-and-oracle-streaming-service
* https://recursive.codes/p/message-driven-microservices-monoliths-with-micronaut-part-1:-installing-kafka-sending-your-first-message
* https://recursive.codes/blog/post/1650
* https://recursive.codes/blog/post/1648
* https://github.com/lucassrg/shipping-svc-kafka
* https://github.com/lucassrg/order-svc-kafka


You may now [proceed to the next lab](#next).

## Acknowledgements

* **Author** - Lucas Gomes
* **Contributors** -  Rishi Johari
* **Last Updated By/Date** - Lucas Gomes, August 2021