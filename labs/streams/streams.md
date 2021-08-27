# Create Streams (Topics) in OCI

## Introduction

In this lab you will create our Streams (topics) in OCI. The easiest way to do that is by using the OCI CLI using the [Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm).

Estimated time: 10 minutes

### Objectives

In this lab, you will:

* Create a Stream Pool
* Create Streams
* Create IAM resources to allow the application to consume and publish messages


## Task 1: Save Compartment OCID into a Environment Variable

We recommend using a dedicated IAM Compartment to store your Streams. If you don't have one yet, you can create it on:
> `Home > Identity > Compartments > Create Compartment`

Take notes on the Compartment OCID and open up Cloud Shell.

## Task 2: Create a Stream Pool

A stream pool is a grouping that you can use to organize and manage streams. Stream pools provide operational ease by providing an ability to share configuration settings across multiple streams. For customers using Streaming's Kafka compatibility feature, the stream pool serves as the root of a virtual Kafka cluster, thereby enabling every action on that virtual cluster to be scoped to that stream pool.

> Create the `e-commerce` stream pool:

```bash
oci streaming admin stream-pool create --name 'e-commerce' --compartment-id 'ocid1.compartment.oc1..aaaaaaaabgk'
```

Take notes on the `id` property returned by the command line:

```json
{
  "data": {
    "compartment-id": "ocid1.compartment.oc1..aaaaaaaabgk",
    "custom-encryption-key": {
      "key-state": "NONE",
      "kms-key-id": null
    },
    "defined-tags": {
      "Oracle-Tags": {
        "CreatedBy": "oracleidentitycloudservice/lucas.gomes@oracle.com",
        "CreatedOn": "2021-02-11T06:56:25.632Z"
      }
    },
    "endpoint-fqdn": null,
    "freeform-tags": {},
    "id": "ocid1.streampool.oc1.iad.amaaaaaadoga",
    "is-private": false,
    "kafka-settings": {
      "auto-create-topics-enable": false,
      "bootstrap-servers": null,
      "log-retention-hours": 24,
      "num-partitions": 1
    },
    "lifecycle-state": "CREATING",
    "lifecycle-state-details": null,
    "name": "e-commerce",
    "private-endpoint-settings": {
      "nsg-ids": null,
      "private-endpoint-ip": null,
      "subnet-id": null
    },
    "time-created": "2021-02-11T06:56:25.711000+00:00"
  },
  "etag": "\"91b256a3-d25d-4182-a8af-218ac4216220-36231994-6061-441c-b810-12312313\""
}
```

## Task 3: Create Streams

> Create `order-topic` stream

```bash
oci streaming admin stream create --name 'order-topic' --stream-pool-id 'ocid1.streampool.oc1.iad.amaaaaaadoga' --partitions 1
```

> Create `shipment-topic` stream

```bash
oci streaming admin stream create --name 'shipment-topic' --stream-pool-id 'ocid1.streampool.oc1.iad.amaaaaaadoga' --partitions 1
```

Take notes on the id.

## Task 4: Create IAM resources for your application

In this exercise we will create policies based on least privilege access. It is important to take notes on the resulting ids of all resources created through the next steps.

1. Create a "Service" user for your application

    ```bash
    oci iam user create --name "streaming-app-user" --description "application user to use Streams"
    ```

1. Create a Group

    ```bash
    oci iam group create --name "streaming-apps" --description "Group for Streaming Applications"
    ```

1.  Associate the user with the Group

    ```bash
    oci iam group add-user --user-id "ocid1.user.oc1..aaaaaaaaxld" --group-id "ocid1.group.oc1..aaaaaaaal5ed6lu"
    ```

1. Create a Policy to allow the application to use the Stream for publishing/subscribing

    First you need to generate a json file using the command:

    ```bash
    oci iam policy create --generate-full-command-json-input > create-stream-iam-policy.json
    ```

    Update the `create-stream-iam-policy.json` file:

    ```json
    {
    "compartmentId": "<compartment-ocid>",
    "description": "Policy to allow publishers/consumers to use a Stream",
    "name": "use-streams",
    "statements": [
        "Allow group streaming-apps to use stream-push in compartment id <compartment-ocid> where target.stream.id='<order-topic-stream-ocid>'",
        "Allow group streaming-apps to use stream-pull in compartment id <compartment-ocid> where target.stream.id='<order-topic-stream-ocid>'"
    ]
    }
    ```

    Run the command:

    ```bash
    oci iam policy create --from-json file://create-stream-iam-policy.json
    ```

1. Generate Auth Token for your Streaming Application user

    ```bash
    oci iam auth-token create --description "Stream App user auth token" --user-id  "ocid1.user.oc1..aaaaaaaaxld"
    ```

    Take notes on the token as it is not displayed anymore, for example:

    ```json
    {
    "data": {
        "description": "Stream App user auth token",
        "id": "ocid1.credential.oc1..aaaaaaaacmnlc4",
        "inactive-status": null,
        "lifecycle-state": "ACTIVE",
        "time-created": "2021-02-11T07:40:06.565000+00:00",
        "time-expires": null,
        "token": "d$.ab2rGa1C8J=sr8#bD",
        "user-id": "ocid1.user.oc1..aaaaaaaaxld"
    },
    "etag": "648e5e87e61ef6d7ae6706331bbe976d9f81b3f3"
    }
    ```

You may now [proceed to the next lab](#next).

## Acknowledgements

* **Author** - Lucas Gomes
* **Contributors** -  Rishi Johari
* **Last Updated By/Date** - Lucas Gomes, August 2021