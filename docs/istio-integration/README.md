# Citrix ADC integration with Istio

A service mesh is an infrastructure layer that handles communication between microservices and provides capabilities like service discovery, load balancing, security, and monitoring. [Istio](https://istio.io) is an open source and platform-independent service mesh that connects, monitors, and secures microservices. Citrix ADC has advanced traffic management capabilities for enhancing application performance and provides comprehensive security. Citrix ADC integrations with Istio allow you to secure and optimize traffic for applications in the service mesh using Citrix ADC features.

Citrix ADC can be integrated with Istio in three ways:

- Citrix ADC CPX, MPX, or VPX as an Ingress Gateway to the service mesh.
- Citrix ADC CPX as a sidecar proxy with application containers in the service mesh.
- Citrix ADC CPX as an Egress Gateway for the service mesh.

## Citrix ADC as an Ingress Gateway for Istio

An Istio ingress gateway acts as an entry point for the incoming traffic and secures and controls access to the service mesh from outside. It also performs routing and load balancing. Citrix ADC CPX, MPX, or VPX can be deployed as an ingress gateway to the Istio service mesh.

## Citrix ADC CPX as a sidecar proxy for Istio

In Istio service mesh, a sidecar proxy runs alongside application pods and it intercepts and manage incoming and outgoing traffic for applications. Citrix ADC CPX can be deployed as the sidecar proxy in the application pods. A sidecar proxy applies the configured routing policies or rules to the ingress and egress traffic from the pod.

## Citrix ADC CPX as an Egress Gateway for Istio

An Egress Gateway defines the traffic exit point from a service mesh. The Citrix ADC CPX as an Egress Gateway performs load balancing, monitoring at the edge of the service mesh, and provides routing rules to exit the Istio service mesh.

## <a name="architecture">Architecture</a>

For more information on the deployment architecture for integrating Citrix ADC with Istio, see [Architecture](../istio-integration/architecture.md).

## <a name="deployment-options">Deployment Options</a>

In Istio service mesh, Citrix ADC can act as an Ingress Gateway, Egress Gateway and/or sidecar proxy in the data plane. Citrix ADC can act as an Ingress and/or Egress Gateway for services deployed with or without sidecar (sidecar can be Citrix CPX or Envoy). Below table gives a glimpse about working combinations of Citrix ADC and Envoy proxy.

| Ingress Gateway | Sidecar Proxy | Egress Gateway| Supported |
|-----------------|---------------|---------------|-----------|
| Citrix ADC | Citrix ADC CPX | Citrix ADC CPX | Yes |
| Citrix ADC | Citrix ADC CPX | Envoyproxy | Yes |
| Citrix ADC | Envoyproxy | Citrix ADC CPX | Yes |
| Citrix ADC | Envoyproxy | Envoyproxy | Yes |
| Envoyproxy | Citrix ADC CPX| Citrix ADC CPX | Yes |
| Envoyproxy | Citrix ADC CPX| Envoyproxy | Yes |

To deploy Citrix ADC with Istio using Helm charts, see [Deployment](../istio-integration/deploy-istio-adaptor-helm-chart.md).

## Features

The features which are supported on a Citrix ADC in an Istio service mesh can be broadly categorized into the following:

- Traffic management
- Security
- Observability

### Traffic management

Citrix ADC supports the following traffic management features in a service mesh.

-  Service discovery
-  Load balancing
-  Secure ingress
-  Weighted clusters
-  HTTP rewrite
-  HTTP redirect
-  HTTP mirroring
-  HTTP outlier detection
-  Weighted service entry

### Security

Some important security features, which are supported on the Citrix ADC, are the following:

-  Authentication policy
-  Monitoring of service mesh certificates and keys

#### Authentication policy

-  End user authentication or origin authentication using JWT authentication
-  Transport authentication or service-to-service authentication using mutual TLS.

#### Monitoring of service mesh certificates and keys

The `xDS-adaptor` monitors the folder where a service mesh deploys certificates and keys for mutual TLS authentication between Citrix ADC proxies. After an update of certificate and key, the `xDS-adaptor` loads the new certificate and key to the Citrix ADC.

### Observability

When a service is deployed in a service mesh, users may be interested to get insights about the service behavior. Citrix ADC proxy provides a rich set of in-built metrics to provide insights about the service behavior. When Citrix ADC CPX is deployed as a sidecar, these metrics represent the telemetry data for an application. It helps to reduce the burden of application developers by eliminating the need to include a lot of instrumentation code in the application. Instead, the developer can focus on the core application logic.

Citrix has built a couple of auxiliary tools such as [Citrix ADC Metrics Exporter](https://github.com/citrix/citrix-adc-metrics-exporter) and [Citrix Observability Exporter](https://github.com/citrix/citrix-observability-exporter) which help to export metrics and transactional data to observability tools such as Prometheus, Zipkin, Kafka, and so on.

The statistical data of a Citrix ADC ingress device can be exported to the Prometheus using [Citrix ADC Metrics Exporter](https://github.com/citrix/citrix-adc-metrics-exporter).

[Citrix Observability Exporter](https://github.com/citrix/citrix-observability-exporter) (COE) is a microservice designed to collect metrics from Citrix ADCs, and export them to observability tools such as Zipkin, Kafka, and Prometheus.
For more information about COE, see this [link](https://github.com/citrix/citrix-observability-exporter).

#### Telemetry in an Ingress or Egress Gateway

[Prometheus](https://prometheus.io) is already installed as a part of a service mesh package. By default, Citrix ADC Metrics Exporter is also deployed along with Citrix ADC acting as an Ingress or Egress Gateway. Citrix ADC Metrics Exporter fetches statistical data from Citrix ADC and exports it to Prometheus running in a service mesh. When you add Prometheus as a data source in Grafana, you can view this statistical data in the Grafana dashboard.

#### Telemetry and distributed tracing in sidecar proxies

Citrix ADC CPX with Citrix Observability Exporter (COE) can export metrics to Prometheus that is deployed in a service mesh. You can visualize this data in Grafana.

Citrix ADC CPX sends transactional data to COE which, eventually, exports these traces spans to [Zipkin](https://zipkin.io). This distributed tracing enables you to track a service-to-service communication within a service mesh. It helps to get deeper understanding about request latency, serialization, and parallelism.

The list of fields supported on Citrix ADC as per the service mesh CRDs (Destination Rule, Virtual Service, Policy, Gateway, and Service Entry) can be found [here](features.md).
