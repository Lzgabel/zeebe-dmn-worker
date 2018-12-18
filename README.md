# zeebe-dmn-worker
Zeebe job worker for DMN. It uses the Camunda DMN engine to evaluate decisions. The decisions are read from local directory.

* register for tasks of type 'DMN'
* required task header 'decisionRef' => id of the decision to evaluate
* completes task with payload 'result' which contains the complete decision result

```xml
<bpmn:serviceTask id="decisionTask" name="Eval DMN decision">
  <bpmn:extensionElements>
    <zeebe:taskDefinition type="DMN" />
    <zeebe:taskHeaders>
      <zeebe:header key="decisionRef" value="dish-decision" />
    </zeebe:taskHeaders>
    <zeebe:ioMapping>
      <zeebe:output source="$.result" target="$.decisionResult" />
    </zeebe:ioMapping>
  </bpmn:extensionElements>
</bpmn:serviceTask>
```

## How to build

Build with Maven

`mvn clean install`

## How to run

Execute the JAR file via

`java -jar target/zeebe-dmn-worker.jar`

## How to configure

You can set the following environment variables to configure the worker.

* `dmn.repo` - default: `dmnrepo`
* `zeebe.client.broker.contactPoint`- default: `127.0.0.1:26500`

## Code of Conduct

This project adheres to the Contributor Covenant [Code of
Conduct](/CODE_OF_CONDUCT.md). By participating, you are expected to uphold
this code. Please report unacceptable behavior to
code-of-conduct@zeebe.io.
