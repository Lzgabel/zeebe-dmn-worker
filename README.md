# zeebe-dmn-worker

A Zeebe worker to evaluate DMN decisions (i.e. business rule tasks). It uses the [Camunda DMN engine](https://docs.camunda.org/manual/7.12/reference/dmn11/) including the [FEEL-Scala engine](https://github.com/camunda/feel-scala) to evaluate DMN decisions. The DMN files are read from a local directory.

## Usage

Example BPMN with service task:

```xml
<bpmn:serviceTask id="decisionTask" name="Eval DMN decision">
  <bpmn:extensionElements>
    <zeebe:taskDefinition type="DMN" />
    <zeebe:taskHeaders>
      <zeebe:header key="decisionRef" value="dish-decision" />
    </zeebe:taskHeaders>
    <zeebe:ioMapping>
      <zeebe:output source="result" target="decisionResult" />
    </zeebe:ioMapping>
  </bpmn:extensionElements>
</bpmn:serviceTask>
```

* the worker is registered for the type `DMN`
* required custom headers:
    * `decisionRef` - the id of the decision to evaluate
* the result of the evaluation is passed as `result` variable

## Install

### Docker

The docker image for the worker is published to [DockerHub](https://hub.docker.com/r/camunda/zeebe-dmn-worker).

```
docker pull camunda/zeebe-dmn-worker:latest
```
* configure the connection to the Zeebe broker by setting `zeebe.client.broker.contactPoint` (default: `localhost:26500`) 
* configure the folder where the DMN files are located by setting `zeebe.worker.dmn.repository` (default: `dmn-repo`)

For a local setup, the repository contains a [docker-compose file](docker/docker-compose.yml). It starts a Zeebe broker and the worker. 

```
cd docker
docker-compose up
```

### Manual

1. Download the latest [worker JAR](https://github.com/zeebe-io/zeebe-dmn-worker/releases) _(zeebe-dmn-worker-%{VERSION}.jar
)_

1. Start the worker
    `java -jar zeebe-dmn-worker-{VERSION}.jar`

### Configuration

The worker is a Spring Boot application that uses the [Spring Zeebe Starter](https://github.com/zeebe-io/spring-zeebe). The configuration can be changed via environment variables or an `application.yaml` file. See also the following resources:
* [Spring Zeebe Configuration](https://github.com/zeebe-io/spring-zeebe#configuring-zeebe-connection)
* [Spring Boot Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)

By default, the DMN files are read from the folder `dmn-repo` next to the application (i.e. the working directory).

```
zeebe:
  worker:
    defaultName: camunda-dmn-worker
    defaultType: DMN
    threads: 3

    dmn.repository: dmn-repo

  client:
    job.timeout: 10000
    broker.contactPoint: 127.0.0.1:26500
    security.plaintext: true
```

## Build from Source

Build with Maven

`mvn clean install`

## Code of Conduct

This project adheres to the Contributor Covenant [Code of
Conduct](/CODE_OF_CONDUCT.md). By participating, you are expected to uphold
this code. Please report unacceptable behavior to
code-of-conduct@zeebe.io.
