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


## Getting Started with Docker

**Build Authentication API Image & Verify Creation:**

`cd mcc-auth`

`docker build --tag <IMAGE NAME>:v1.0 .`

`docker images`

**Docker instructions located in the [mcc-data README.md](https://github.com/jaydajayda/mcc-data/blob/main/README.md) must be completed PRIOR to completing Docker instructions below.**

**Updating Authentication API Configuration:**

In `./Dockerfile`:
* After `VOLUME /tmp`, add ARG variable - `ARG targethost=localhost:8080`
* Add ENV variable - `ENV <DATA API VARIABLE NAME>=$targethost`

*Example:*

```
...
VOLUME /tmp
ARG targethost=localhost:8080
ENV DATA_API_HOST=$targethost
...
```
______________________________________________________________

In `./src/main/java/com/metro/auth/api/RegisterAPI.java` & `TokenAPI.java` in the same directory:
* Ensure the environment variable being grabbed by `System.getenv("<DATA API VARIABLE NAME>");` matches that of
  which was declared and initialized in the Dockerfile.
* Ensure the `url` variable performs string concatentation using the correct environment variable.

*Example:*

```
String dataApiHost = System.getenv("DATA_API_HOST");
...
String url = "http://" + dataApiHost + "/api/customers";
...
```

**Rebuild Authentication API Image:**

`docker build --tag <IMAGE NAME>:v1.0`

**Start Authentication API Container & Add to Docker Network:**

`docker run -d --name=<CONTAINER NAME> -p 8081:8081 --env <DATA API VARIABLE NAME>=<DATA API IP ADDRESS>:8080 <IMAGE NAME>:v1.0`

`docker network connect <NETWORK NAME> <CONTAINER NAME>`

**Locating IP Address of Data API:**

*(This IP Address will be required for the configuration of the Client)*

`docker network inspect <NETWORK NAME>`

**Test Data API and Authentication API Endpoints:**

In a browser, navigate to: 

Data API:
`0.0.0.0:8080/api`

Authentication API:
`0.0.0.0:8081/account`