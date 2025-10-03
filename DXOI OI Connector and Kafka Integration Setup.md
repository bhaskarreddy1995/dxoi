<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# DXOI OI Connector and Kafka Integration Setup

Yes, the DXOI platform includes a built-in Kafka microservice that is specifically designed to work with the OI Connector for streaming metrics and observability data to the DXOI platform. Here's how to connect to the DXOI platform:[^1][^2]

## Built-in Kafka Infrastructure

The DXOI platform comes with an integrated Kafka microservice that serves as the messaging backbone for the OI Connector. This eliminates the need for external Kafka deployments, as the platform provides:[^1]

- **Native Kafka Integration**: The OI Connector uses the built-in Kafka service to publish metrics from NetOps Performance Management to DXOI[^2]
- **Automatic Topic Management**: The connector automatically defines the metrics to export and manages Kafka topics[^3]
- **Streamlined Data Flow**: Metrics flow from the data aggregator through Kafka to the OI Connector, which then forwards them to DXOI[^2]

![DXOI OI Connector Setup and Kafka Connection Configuration Flow](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/14919634534e9205708f0a1c9536e526/2e943aa8-a7bf-464f-8ce6-d6b26d9f140c/735e4586.png)

DXOI OI Connector Setup and Kafka Connection Configuration Flow

## Connection Configuration Process

To connect the OI Connector to the DXOI platform, you need to configure several key areas during installation:

### 1. NetOps Portal Configuration

Configure the connection between the OI Connector and NetOps Portal:[^2]

- **Protocol**: HTTP or HTTPS (default: http)
- **Hostname**: NetOps Portal server hostname (default: localhost)
- **Port**: Service port 8181 for HTTP, 8182 for HTTPS
- **Tenant**: Tenant name (default: _default_)
- **Username**: Administrator account credentials


### 2. OI Gateway Connection

Establish the connection to the DXOI platform:[^2]

- **Protocol**: HTTPS (recommended for secure communication)
- **Hostname**: Fully qualified domain name of the DXOI Gateway
- **Port**: 80 for HTTP, 443 for HTTPS
- **Tenant ID**: DX Operational Intelligence tenant identifier (UUID format)
- **Security Token**: Generated authentication token from DXOI

To obtain the DXOI Gateway hostname, use these commands:[^2]

```bash
# For OpenShift
oc get routes -n<namespace> | egrep apmservices-gateway

# For Kubernetes  
kubectl get ingress -n<namespace> | grep apmservices-gateway
```


### 3. Kafka Broker Setup

Configure the connection to the Kafka infrastructure:[^2]

- **Number of Brokers**: Specify the number of Kafka brokers (default: 1)
- **Kafka Port**: Client port for broker connections (default: 9092, or 9093 for SSL)
- **Broker Hostname**: Hostname of each Kafka broker in the cluster
- **SSL Configuration**: Enable SSL/TLS for secure communication (Y/N)
- **Keystore Location**: Path to client keystore if SSL is enabled


## Network Communication Requirements

The OI Connector requires specific network ports to be open for communication:[^2]


| Component | Port | Protocol | Purpose |
| :-- | :-- | :-- | :-- |
| Kafka | 9092/9093 | TCP/TLS | Broker communication between data aggregator and OI Connector |
| Data Aggregator | 8581/8582 | HTTP/HTTPS | REST communication for metric export configuration |
| NetOps Portal | 8181/8182 | HTTP/HTTPS | REST communication from OI Connector |
| OI Connector Status | 8782 | HTTP | Status reporting to NetOps Portal |
| DXOI Gateway | 8080 | HTTPS | Secure connection to DXOI platform |

## Data Flow Architecture

The metrics streaming follows this path:[^1][^2]

1. **Data Aggregator** harvests and publishes NetOps Performance Management metrics to the Kafka data bus
2. **Kafka Microservice** receives and manages metric streams on designated topics
3. **OI Connector** consumes metrics from Kafka topics
4. **OI Connector** forwards processed metrics to the DXOI platform via the Gateway
5. **DXOI Platform** ingests metrics for analysis and visualization

## Installation Prerequisites

Before connecting to the platform, ensure these requirements are met:[^2]

- **Java Runtime Environment**: JRE 17 (for version 23.3.8+) or JRE 11 (for 23.3.7 and lower)
- **NetOps Kafka**: Installed with adequate log retention (1-2 hours recommended)
- **Disk Space**: 60 GB per hour of log retention for Kafka broker
- **Dedicated Server**: OI Connector should run on a separate server from other NetOps components
- **Administrator Account**: Non-expiring password account specific to OI Connector


## Verification Steps

After configuration, verify the connection by:[^2]

1. **Check Service Status**: `systemctl status caperfcenter_oiconnector`
2. **Monitor OI Connector Status**: Verify "Up" status and "Normal" health in NetOps Portal
3. **Validate Metric Flow**: Confirm metrics are being queried every minute from OI Connector logs
4. **Test Kafka Topics**: Use Kafka console consumer to verify metric data is flowing:[^1]
```bash
cd <kafka_directory>/bin
./kafka-console-consumer.sh --topic metric-export --bootstrap-server <broker-host>:9092
```

The DXOI platform's integrated Kafka microservice streamlines the setup process, providing a native messaging infrastructure that requires minimal external configuration while ensuring reliable metric streaming from NetOps Performance Management to the DXOI observability platform.
<span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^21][^22][^23][^24][^25][^26][^27][^28][^29][^30][^31][^32][^33][^34][^35][^36][^37][^38][^39][^4][^40][^41][^42][^5][^6][^7][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://www.youtube.com/watch?v=ZNoH89lA8Q0

[^2]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/integrating/integrate-with-dx-operational-intelligence/install-the-oi-connector.html

[^3]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/24-3/administrating/data-extraction/configure-streaming-metric-export-to-a-kafka-cluster.html

[^4]: https://www.geeksforgeeks.org/advance-java/microservices-communication-with-apache-kafka-in-spring-boot/

[^5]: https://developer.okta.com/blog/2022/09/15/kafka-microservices

[^6]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/dx-apm-agents/SaaS/infrastructure-agent/kafka-monitoring.html

[^7]: https://docs.imply.io/polaris/kafka-connect-reference/

[^8]: https://roshancloudarchitect.me/building-resilient-microservices-architecture-with-kafka-and-zookeeper-a-real-life-example-8aa0d95d4775

[^9]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/dx-operational-observability/saas/monitoring/opentelemetry/integration/kafka.html

[^10]: https://docs.snowflake.com/en/user-guide/kafka-connector-install

[^11]: https://docs.nestjs.com/microservices/kafka

[^12]: https://academy.broadcom.com/aiops/operational-observability/overview-video

[^13]: https://docs.confluent.io/platform/current/installation/configuration/connect/index.html

[^14]: https://www.infosys.com/iki/techcompass/microservice-communication.html

[^15]: https://academy.broadcom.com/aiops/operational-intelligence/dx-operational-intelligence-get-ready-for-saas-version-22-9-01

[^16]: https://docs.confluent.io/platform/current/connect/index.html

[^17]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/22-2/integrating/integrate-with-dx-operational-intelligence/oi-connector-release-notes.html

[^18]: https://help.boomi.com/docs/Atomsphere/Integration/Connectors/int-Kafka_connector_c9d1cdde-8864-4408-882b-64520400fdbb

[^19]: https://docs.camunda.io/docs/components/connectors/out-of-the-box-connectors/kafka/

[^20]: https://www.youtube.com/watch?v=IR1NLfaq7PU

[^21]: https://kafka.apache.org/documentation/

[^22]: https://docs.oracle.com/en-us/iaas/Content/connector-hub/create-service-connector-streaming-source.htm

[^23]: https://docs.automic.com/documentation/webhelp/english/ARA/ALL/DOCU/latest/CDA Guides/Content/AWA/Variables/UC_AIOPS_SETTINGS.htm

[^24]: https://docs.oracle.com/iaas/Content/connector-hub/create-service-connector-monitoring-source.htm

[^25]: https://www.youtube.com/watch?v=ZHTIux_cGzg

[^26]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/dx-netops/23-3/dx-netops-integrations/kafka-integration.html

[^27]: https://docs.automic.com/documentation/webhelp/english/ALL/components/DOCU/24.0/Automic Automation Guides/Content/AIR/air_Integrating.htm?TocPath=Administering+and+Configuring%7CAutomic+Intelligent+Remediation%7C_____1

[^28]: https://docs.microfocus.com/itom/Operations_Bridge_Manager:2021.11/OmPtRtMetrichtml

[^29]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/dx-openexplore/saas/integrations-overview/label_integrations-list/kafka.html

[^30]: https://support.pega.com/question/kafka-externalization-setupconfiguration

[^31]: https://www.servicenow.com/community/itom-forum/servicenow-operational-intelligence-complete-setup-and-step-by/m-p/3230686

[^32]: https://help.boomi.com/docs/atomsphere/integration/connectors/int-kafka_connection_a663a4ec-4892-489e-9ab2-436d950ef826/

[^33]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/24-3/integrating/integrate-with-dx-operational-intelligence.html

[^34]: https://www.youtube.com/watch?v=5z0ZfUI3sT0

[^35]: https://jacow.org/icalepcs2019/papers/mompl010.pdf

[^36]: https://docs.pega.com/bundle/platform/page/platform/decision-management/create-data-admin-kafka.html

[^37]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/integrating/integrate-with-dx-operational-intelligence/install-the-oi-connector.html

[^38]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/integrating/integrate-with-dx-operational-intelligence/secure-connections-to-dx-operational-intelligence.html

[^39]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/integrating/integrate-with-dx-operational-intelligence/configure-the-oi-connector.html

[^40]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/installing/install-netops-kafka.html

[^41]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/administrating/data-extraction/configure-streaming-metric-export-to-a-kafka-cluster.html

[^42]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/14919634534e9205708f0a1c9536e526/1134c632-53c3-4d79-9e44-88dad6df124f/96f18c81.csv

