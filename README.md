# Metropolitan Convention Center - Authentication API

This repository is 1 of 3 microservices that make up the mock MCC Event Registration Application.
It provides an authentication API for its adjacent HyperSQLDB service - [mcc-data](https://github.com/jaydajayda/mcc-data.git) and ReactJS client - [mcc-client](https://github.com/jaydajayda/mcc-client.git).

## Getting Started

`git clone git@github.com:jaydajayda/mcc-auth.git`

`cd mcc-auth`

`gradle clean build bootJar`

`cd build/libs/`

`java -jar <JAR FILENAME>`

**Example:**

`java -jar mcc-auth-0.0.1-SNAPSHOT.jar`