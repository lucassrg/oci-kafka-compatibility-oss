# Setup a Local environment

## Introduction

In this lab you will prepare your environment to build the microservices using Java.

Estimated time: 10 minutes

### Objectives

In this lab, you will:

* Install SDKMAN
* Install Java GraalVM
* Install Gradle


## Task 1: Installing SDKMAN on Mac/Unix

1. Download and Install SDKMAN

    ```bash
    $ curl -s "https://get.sdkman.io" | bash

    $ source "$HOME/.sdkman/bin/sdkman-init.sh"
    ```

1. Check installed version:

    ```bash

    $ sdk version
    SDKMAN 5.10.0+617
    ```

For other operating systems, check [SDKMAN documentation](https://sdkman.io/install).


## Task 2: Using Java GraalVM

1. First list Java versions available:

    ```bash

    $ sdk list java
    ```

1. Identify Java GraalVM latest version, Release 11, for example:

    ```bash
    GraalVM       |     | 21.0.0.r11   | grl     |            | 21.0.0.r11-grl
    ```

1. Install it:

    ```bash
    $ sdk install java 21.0.0.r11-grl
    ```

1. Check installation:

    ```bash
    $ java -version
    openjdk version "11.0.10" 2021-01-19
    OpenJDK Runtime Environment GraalVM CE 21.0.0 (build 11.0.10+8-jvmci-21.0-b06)
    OpenJDK 64-Bit Server VM GraalVM CE 21.0.0 (build 11.0.10+8-jvmci-21.0-b06, mixed mode, sharing)
    ```

## Task 3: Installing Gradle

1. Install Gradle using SDKMAN:

    ```bash
    $ sdk install gradle
    ```

1. Check installation:

    ```bash
    $ gradle -version                                          

    Welcome to Gradle 6.8.2!

    Here are the highlights of this release:
    - Faster Kotlin DSL script compilation
    - Vendor selection for Java toolchains
    - Convenient execution of tasks in composite builds
    - Consistent dependency resolution

    For more details see https://docs.gradle.org/6.8.2/release-notes.html


    ------------------------------------------------------------
    Gradle 6.8.2
    ------------------------------------------------------------

    Build time:   2021-02-05 12:53:00 UTC
    Revision:     b9bd4a5c6026ac52f690eaf2829ee26563cad426

    Kotlin:       1.4.20
    Groovy:       2.5.12
    Ant:          Apache Ant(TM) version 1.10.9 compiled on September 27 2020
    JVM:          11.0.10 (GraalVM Community 11.0.10+8-jvmci-21.0-b06)
    OS:           Mac OS X 10.14.6 x86_64
    ```

You may now [proceed to the next lab](#next).

## Acknowledgements

* **Author** - Lucas Gomes
* **Contributors** -  Rishi Johari
* **Last Updated By/Date** - Lucas Gomes, August 2021