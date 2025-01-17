# IBM CloudPak for Watson AIOps Connectors
- A connector is an integration between IBM Cloud Pak for Watson AIOps and a particular data source.
- Connectors are packaged as a container image that runs a gRPC client inside and communicates back to the Watson AIOps main cluster via a gRPC channel.
- The preferred programming model is via Open Liberty. However, any other gRPC-enabled programming model (such as Node.js) can also be used - provided it is a supported runtime.

## Connector composition
A Connector is composed of below components

- A connector container
  - Container image that runs a gRPC client, running either in-cluster for local deployment or remotely for micro-edge deployment.

- Processor containers(0..* ) (backend processors running in-cluster)
  - Each connector must have at least one component that interacts with its collected data, which we call a processor. There are many types of processors, depending on their purpose.

- A UI tile (ConnectorSchema YAML)
   - If you are creating a connector for a data source that is not yet supported in Watson AIOps you first need to create a ConnectorSchema.
   - The ConnectorSchema CR servers a few purposes:
      - It provides content for the UI to render the corresponding form in the Data and tool integrations section of the product
      - It validates completed UI forms

- Kafka topics (0..* YAMLs)

## CP4WAIOps Bundle Manifests
An AIOps Bundle Manifest is a Custom Resource (YAML) that can contribute different types of artifacts into Watson AIOps like connectors, processors etc which can grow in future to include other types.

## High level steps to create a new connector and add bundleManifest
- Create a new connector repo using the example template located [here](https://github.com/IBM/cp4waiops-connectors-java-template)
- Rename the connector name from `java-grpc-connector-template` to your unique connector name in all the applicable places.
- Connector-sdk grpc-java-sdk is published as maven artifact which can be used by 3rd party connector contributors.
- The example template is implemented using open-liberty/grpc-java-sdk will cover below structured data types.
    - Event: Connector template to push Kafka events to Event Lifecycle
    - Metric: Connector template to push Kafka events to Metrics Manager
    - Topology: Connector template for topology bridge which will push kafka events to ASM
- Update the logic according to connector requirements. For example in the example connect [path](https://github.com/IBM/cp4waiops-connectors-java-template/tree/main/src/main/java/com/ibm/aiops/connectors/template)
- Publish the connector image to a docker registry.
- The example template also includes bundle-manifest artifacts for the connector integration and deployment on cp4waiops cluster. Create the bundleManifest artifacts for the connector using the example artifacts.Reference the published image and tag.
