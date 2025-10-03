<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Connecting OI Connector to DXOI Platform After AIOps Installation

Since your DXOI platform AIOps installation is complete, you now need to establish the connection between the OI Connector and your DXOI platform. Here's the step-by-step process:

![Post-AIOps Installation: OI Connector to DXOI Platform Connection Process](https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/0cd7ef18e8530f95a6017a101d72acf3/ccefa036-4462-446f-9cef-ad9a2defb3c5/df3b95fa.png)

Post-AIOps Installation: OI Connector to DXOI Platform Connection Process

## 1. Platform Verification and Tenant Setup

### Verify DXOI Platform Status

First, confirm your DXOI platform is fully operational:[^1][^2]

```bash
# Check platform services are running
kubectl get pods -n <dxoi-namespace>

# Verify platform accessibility
curl -k https://<dxoi-platform-url>/health
```


### Obtain Critical Connection Parameters

You'll need these key parameters from your DXOI platform:[^1][^2]

**Retrieve Gateway Information**:[^1]

```bash
# For OpenShift
oc get routes -n <namespace> | egrep apmservices-gateway

# For Kubernetes  
kubectl get ingress -n <namespace> | grep apmservices-gateway
```

**Access Platform Settings**:

1. Navigate to your DXOI platform web interface
2. Go to **Settings** → **Connector Parameters** to get your **Cohort ID**
3. Go to **Settings** → **Tokens Management** to generate a **Tenant Security Token**
4. Note your **Tenant ID** (UUID format)

## 2. OI Connector Installation and Configuration

### Download and Install OI Connector

Download the OI Connector package from Broadcom Support:[^1]

```bash
# Download NetOps-OIConnector-<version>-Linux-RELEASE.tar.gz
tar xpvfz NetOps-OIConnector-<version>-Linux-RELEASE.tar.gz

# Launch installer
./OIConnector.bin
```


### Configure Connection Parameters During Installation

**NetOps Portal Configuration**:[^1]

- **Protocol**: http or https (https recommended)
- **Hostname**: Your NetOps Portal hostname
- **Port**: 8181 (HTTP) or 8182 (HTTPS)
- **Tenant**: _default_ or your specific tenant name
- **Username**: Administrator account for OI Connector

**OI Gateway Connection** (Critical Step):[^1]

- **Protocol**: https (recommended for DXOI)
- **Hostname**: DXOI Gateway FQDN (from route/ingress discovery above)
- **Port**: 443 (HTTPS) or 80 (HTTP)
- **Tenant ID**: Your DXOI tenant UUID
- **Security Token**: Generated tenant token from DXOI platform

**Kafka Configuration**:[^1]

- **Number of Brokers**: 1 (default, or match your Kafka cluster)
- **Kafka Port**: 9092 (standard) or 9093 (SSL)
- **Broker Hostname**: Kafka service hostname
- **SSL Configuration**: Y/N based on your setup


### OI Connector Service Configuration

Configure the OI Connector status webservice:[^1]

- **Status Port**: 8782 (default, configurable)


## 3. Authentication and Network Setup

### Security Token Configuration

Ensure you have the correct authentication setup:[^2][^3]

- **Tenant Token**: Generated from DXOI platform for Gateway authentication
- **User Token**: For API authentication
- **Service Account**: Create dedicated account with non-expiring password


### Network Connectivity Requirements

Verify these ports are accessible:[^1]


| Component | Ports | Purpose |
| :-- | :-- | :-- |
| Kafka | TCP 9092/9093 | Metrics streaming |
| Data Aggregator | HTTP 8581/HTTPS 8582 | REST communication |
| NetOps Portal | HTTP 8181/HTTPS 8182 | Portal communication |
| OI Connector Status | HTTP 8782 | Health reporting |
| DXOI Gateway | HTTPS 8080/443 | Platform connection |

## 4. Connection Validation and Verification

### Service Status Check

Verify the OI Connector service is running:[^1][^2]

```bash
# Check service status
systemctl status caperfcenter_oiconnector

# Verify both services are active
service caperfcenter_oi status
service caperfcenter_oiconnector status
```


### Connection Testing

Test the connection to your DXOI platform:[^1]

```bash
# Test Gateway connectivity
curl -k -H "Authorization: Bearer <your-token>" \
  https://<gateway-hostname>:<port>/health

# Check OI Connector logs
tail -f /opt/CA/OIConnector/logs/oiconnector.log
```


### Data Flow Verification

Confirm metrics are flowing to DXOI:[^2][^3]

1. Check OI Connector logs for successful metric transmission
2. Verify in DXOI platform that metrics are being received
3. Access DXOI dashboards to see incoming data
4. Monitor connector health status in NetOps Portal

## 5. Post-Connection Configuration

### Dashboard Monitoring

Set up monitoring for the OI Connector:[^2]

- Monitor connector health status in NetOps Portal
- Check **Administration** → **Configuration Settings** → **OI Connector Status**
- Verify "Up" status and "Normal" health indicators


### Metric Family Configuration

Customize which metrics are sent to DXOI:[^3]

```bash
# Edit configuration file
vi /opt/CA/OIConnector/conf/config.xml

# Default metric families include:
# - Availability
# - Reachability  
# - Port
# - CPU
# - Memory
```


### Troubleshooting Common Issues

**Authentication Issues**:[^2]

- Verify tenant token is correctly configured
- Ensure service account has proper permissions
- Check token expiration and renewal

**Network Connectivity Issues**:[^1]

- Verify DNS resolution between components
- Check firewall rules for required ports
- Test network connectivity with telnet/curl

**Gateway Connection Issues**:[^2]

- Confirm Gateway hostname resolution
- Verify SSL certificates if using HTTPS
- Check Gateway endpoint availability


## 6. Final Validation

Once connected, verify the complete data pipeline:

1. **OI Connector** → Collecting NetOps metrics ✓
2. **Kafka** → Streaming metric data ✓
3. **DXOI Gateway** → Receiving and processing data ✓
4. **DXOI Platform** → Ingesting metrics for analysis ✓
5. **Dashboards** → Displaying metrics and insights ✓

Your OI Connector should now be successfully connected to your DXOI AIOps platform, enabling comprehensive observability and intelligent operations capabilities across your NetOps infrastructure.
<span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^21][^22][^23][^24][^25][^26][^27][^28][^29][^30][^31][^32][^33][^34][^35][^36][^37][^4][^5][^6][^7][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/23-3/integrating/integrate-with-dx-operational-intelligence/install-the-oi-connector.html

[^2]: https://www.youtube.com/watch?v=HA3MI8k0dBs

[^3]: https://www.youtube.com/watch?v=VUlv1tGjkgg

[^4]: https://www.ibm.com/docs/en/SSJGDOB_4.6.1/pdf/IBM_Cloud_Pak_for_AIOps_4-Best_Practices_v2.pdf

[^5]: https://www.youtube.com/watch?v=On0SHuB-K-Y

[^6]: https://www.youtube.com/watch?v=5z0ZfUI3sT0

[^7]: https://www.ibm.com/docs/en/cloud-paks/cloud-pak-aiops/4.10.0?topic=offline-portable-device

[^8]: https://docs.automic.com/documentation/webhelp/english/AIR/12.6/DOCU/12.6/AIR Guides/Content/AIR/air_Integrating.htm

[^9]: https://knowledge.broadcom.com/external/article/190815/dx-aiops-troubleshooting-common-issues.html

[^10]: https://www.youtube.com/playlist?list=PLynEdQRJawmyTGCbQOCXxpp3umLjJHtAL

[^11]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/performance-management/24-3/integrating/integrate-with-dx-operational-intelligence/configure-the-oi-connector.html

[^12]: https://www.hatica.io/blog/what-is-aiops/

[^13]: https://www.microsoft.com/en-us/research/wp-content/uploads/2024/10/AIOpsLab-6705feab5dcdb.pdf

[^14]: https://www.youtube.com/watch?v=0r4JiMFbHQU

[^15]: https://sciencelogic.com/product/resources/what-is-aiops

[^16]: https://netman.aiops.org/~peidan/ANM2022/1.Introduction/ReadingList/2021TIST_A%20Survey%20of%20AIOps%20Methods%20for%20Failure%20Management.pdf

[^17]: https://www.youtube.com/watch?v=JVPcFnvt0To

[^18]: https://docs.microfocus.com/doc/2309/25.3/dyntrinstln

[^19]: https://dl.acm.org/doi/fullHtml/10.1145/3551349.3559503

[^20]: https://academy.broadcom.com/aiops/operational-intelligence/dx-operational-intelligence-get-ready-for-saas-version-22-9-01

[^21]: https://community.ibm.com/community/user/aiops/blogs/zane-bray1/2024/10/03/cloud-pak-for-aiops-4-tips-netcool-connectors

[^22]: https://learn.microsoft.com/en-us/azure/active-directory-b2c/configure-tokens

[^23]: https://learn.microsoft.com/en-us/answers/questions/2155500/on-the-application-sidebar-i-cannot-see-the-token

[^24]: https://docs.automic.com/documentation/webhelp/english/ALL/components/DOCU/24.0/Automic Automation Guides/Content/AIR/air_Integrating.htm?TocPath=Administering+and+Configuring%7CAutomic+Intelligent+Remediation%7C_____1

[^25]: https://ibm.github.io/waiops-tech-jam/labs/cloud-pak-aiops/migration-from-netcool/impact-connector/

[^26]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/dx-operational-intelligence-saas/SaaS/Integration/DX-OI-Outbound-Integration/DX-Gateway/DX-Gatewa-2.html

[^27]: https://docs.bmc.com/xwiki/bin/view/IT-Operations-Management/Operations-Management/BMC-Helix-AIOps/aiops253/Setting-up-and-going-live/Configuring-settings-to-use-the-AI-powered-capabilities-in-BMC-Helix-AIOps/

[^28]: https://www.youtube.com/watch?v=AaJUKPiLp7c

[^29]: https://techdocs.broadcom.com/us/en/ca-enterprise-software/it-operations-management/digital-operational-intelligence/23-3/Configure-and-Deploy-DX-Gateway.html

[^30]: https://www.ibm.com/docs/en/SSJGDOB_4.9.1/pdf/IBM_Cloud_Pak_AIOps_4.5.0_pt1.pdf

[^31]: https://openid.net/specs/openid-connect-federation-1_0-29.html

[^32]: https://www.youtube.com/watch?v=E_VPBxXDYdQ

[^33]: https://www.cisco.com/c/en/us/td/docs/security/cdo/cloud-delivered-firewall-management-center-in-cdo/managing-firewall-threat-defense-services-with-cisco-defense-orchestrator/m-optimize-firewall-performance-with-aiops.html

[^34]: https://guides.dataverse.org/en/latest/installation/config.html

[^35]: https://www.youtube.com/watch?v=J-B_JUYl0gI

[^36]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/0cd7ef18e8530f95a6017a101d72acf3/e45797c9-c30c-4a8a-a691-8e17a9d9f321/8b211e77.csv

[^37]: https://ppl-ai-code-interpreter-files.s3.amazonaws.com/web/direct-files/0cd7ef18e8530f95a6017a101d72acf3/e45797c9-c30c-4a8a-a691-8e17a9d9f321/04238403.csv

