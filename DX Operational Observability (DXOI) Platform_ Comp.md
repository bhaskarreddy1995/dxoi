<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# DX Operational Observability (DXOI) Platform: Comprehensive Architecture and Data Flow

Here's a detailed breakdown of how the DXOI platform works, including data ingestion, processing, and visualization flows:

![DXOI Platform: Complete Data Ingestion, Processing, and Presentation Architecture](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/5de477cc28dfa06520c50684d2b8c341/125f25d7-296b-4a90-aff1-f20ebe7551a6/2f35676e.png)

DXOI Platform: Complete Data Ingestion, Processing, and Presentation Architecture

## Platform Architecture Overview

**DX Operational Observability (DX O2)** is built on a **microservices architecture** running on Kubernetes/OpenShift, consisting of approximately **60 services** that work together to provide comprehensive observability capabilities.[^1]

### Key Technologies Used:

- **Apache Kafka**: High-throughput message bus for inter-service communication
- **Elasticsearch**: Search engine and big data store for logs, alarms, and events
- **RocksDB**: Fast key-value store for metrics, topology, and blob data
- **PostgreSQL**: Relational database for configuration and user data
- **Hazelcast**: Distributed cache for performance optimization


## Detailed Architecture Layers

### 1. Data Sources Layer

**Purpose**: Collect observability data from various sources

**Components**:

- **DX APM Agents**: Application performance metrics, traces, and topology
- **DX NetOps (via OI Connector)**: Network performance metrics via Kafka
- **DX UIM (via Gateway)**: Infrastructure monitoring data
- **Log Collectors**: Application and system logs from various sources
- **Third-party Integrations**: External monitoring tools via REST/webhook

**Data Types**: Metrics, traces, logs, topology, events, alarms

### 2. Ingestion Layer

**Purpose**: Validate, authenticate, and route incoming data

**APIs Available**:

- **Metrics Ingestion API**: Time-series performance data
- **Logs Ingestion API**: Structured and unstructured log data
- **Topology Ingestion API**: Device relationships and dependencies
- **Events/Alarms Ingestion API**: Notifications and incidents
- **Change Events API**: Configuration and deployment changes

**Processing**: Data validation, authentication, initial filtering, format standardization

### 3. Message Bus Layer (Apache Kafka)

**Purpose**: High-throughput, distributed messaging between microservices

**Features**:

- **Topic-based routing**: Different data types use different topics
- **Scalable messaging**: Millions of messages per second capability
- **Fault tolerance**: Data replication and persistence
- **Consumer groups**: Parallel processing by multiple services


### 4. Processing Services Layer

**Purpose**: Business logic, correlation, analysis, and enrichment

**Key Microservices**:

#### Core Processing Services:

- **jarvis-apis**: Metrics processing, aggregation, and KPI calculation
- **situations-service**: Alarm correlation and root cause analysis
- **insights-service**: AI/ML anomaly detection and predictions
- **ess-service**: Enterprise security and authentication
- **tas-service**: Topology and asset relationship processing
- **nass-service**: Network alarm storage and management


#### Supporting Services:

- **dxi-adminui**: Web interface and user experience
- **apmservices-gateway**: API gateway and routing
- **dxi-readserver**: Data query and retrieval optimization

**Capabilities**:

- Real-time data processing
- AI/ML-powered anomaly detection
- Cross-domain correlation
- Data enrichment and contextualization
- Business logic application


### 5. Storage Layer

**Purpose**: Persistent data storage with optimized access patterns

**Storage Systems**:

#### **Elasticsearch**:

- **Data**: Logs, alarms, events, incidents
- **Features**: Full-text search, complex aggregations, real-time indexing
- **Retention**: Configurable (default: 30 days for logs)


#### **Metric Store (RocksDB-based)**:

- **Data**: Time-series metrics, performance counters
- **Features**: High-throughput reads/writes, compression
- **Retention**: Configurable (default: 45 days for raw metrics)


#### **Topology Store (RocksDB-based)**:

- **Data**: Configuration items, relationships, network topology
- **Features**: Graph traversal, hierarchical data
- **Retention**: Long-term with historical change tracking


#### **PostgreSQL**:

- **Data**: User accounts, configurations, audit trails
- **Features**: ACID transactions, complex queries
- **Retention**: Indefinite with periodic cleanup


#### **Hazelcast**:

- **Data**: Session data, cached queries, temporary data
- **Features**: Sub-millisecond access, distributed caching
- **Retention**: TTL-based or session-based expiration


### 6. Presentation Layer

**Purpose**: Data visualization and external access

**Components**:

- **AdminUI**: Primary web interface for monitoring and management
- **DX Dashboards**: Custom visualizations and executive reporting
- **REST APIs**: Programmatic access to platform data
- **Mobile/Web Apps**: End-user monitoring interfaces


## Data Ingestion Flow Process

### Step-by-Step Data Flow:

1. **Data Collection**: Agents and connectors collect metrics, logs, and topology from monitored systems
2. **Gateway Processing**: Southbound gateways receive and pre-process data from external sources
3. **API Validation**: Ingestion APIs validate format, authenticate sources, and perform filtering
4. **Message Publishing**: Validated data is published to appropriate Kafka topics
5. **Topic Consumption**: Microservices consume data from relevant topics
6. **Data Processing**: Services process, correlate, and analyze using business logic and ML
7. **Storage Writing**: Processed data is written to appropriate storage systems
8. **Indexing/Caching**: Data is indexed for search and cached for performance
9. **Data Enrichment**: Additional context and metadata is added
10. **Presentation**: Enriched data is available through UI, dashboards, and APIs

## Scaling and Performance

### Deployment Sizes:

- **Small**: 250K metrics, 200 agents, 150K traces/day
- **Medium**: 1M metrics, 1000 agents, 700K traces/day
- **Large**: 5M metrics, 5000 agents, 6M traces/day


### Horizontal Scaling:

- **Kubernetes orchestration**: Automatic scaling based on load
- **Microservices independence**: Individual service scaling
- **Storage distribution**: Elasticsearch clustering, RocksDB partitioning
- **Cache distribution**: Hazelcast clustering with consistent hashing


## Key Platform Capabilities

### **Application Observability**:

- Full-stack monitoring from frontend to backend
- Individual transaction tracing
- Performance baseline establishment


### **Infrastructure Observability**:

- Server, storage, cloud, and container monitoring
- Enterprise-scale deployment support
- Consistent onboarding experience


### **AIOps Integration**:

- Automated issue detection and correlation
- Root cause analysis with ML
- Cross-domain problem correlation
- Predictive insights and anomaly detection


### **Unified Data Lake**:

- Native format support for different data types
- Alarms, metrics, logs, events, topology, and traces
- Single platform for multiple personas (SRE, NOC, developers)


## Integration Points

### **Inbound Integrations**:

- **DX NetOps**: Via OI Connector and Kafka
- **DX UIM**: Via southbound gateways
- **Third-party tools**: REST APIs, webhooks, SNMP
- **Custom systems**: Open Data Connector (ODC)


### **Outbound Integrations**:

- **ITSM tools**: ServiceNow, BMC Remedy, etc.
- **Notification systems**: Email, Slack, PagerDuty
- **Automation platforms**: Automic, custom webhooks
- **External dashboards**: Grafana, custom applications

This architecture provides a **scalable, resilient, and comprehensive observability platform** that can handle enterprise-scale data volumes while providing real-time insights and AI-powered analytics for modern IT operations.

**PDF Reference**: Architecture details can be found on pages **22-23** (Architecture Overview), pages **354-422** (Services), and pages **36-37** (Deployment Architecture) in the DX Operational Observability 25.1 documentation.
<span style="display:none">[^2][^3][^4][^5]</span>

<div align="center">⁂</div>

[^1]: dx-operational-observability-25-1_compressed.pdf

[^2]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/5de477cc28dfa06520c50684d2b8c341/56863fe5-5a1e-4974-9c61-9482966dbeaa/090ac07b.csv

[^3]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/5de477cc28dfa06520c50684d2b8c341/56863fe5-5a1e-4974-9c61-9482966dbeaa/d6e9fc6f.csv

[^4]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/5de477cc28dfa06520c50684d2b8c341/56863fe5-5a1e-4974-9c61-9482966dbeaa/dd251448.csv

[^5]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/5de477cc28dfa06520c50684d2b8c341/56863fe5-5a1e-4974-9c61-9482966dbeaa/8178f3f2.csv

