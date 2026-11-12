# Enterprise Azure Network Security Architecture

## 1. Purpose

This document defines the target-state network security architecture for BonganiSecure Financial Services, a fictional financial-services organisation.

The objective is to establish a segmented Azure network environment that limits unauthorised access, reduces lateral movement, protects sensitive workloads and supports centralised security monitoring.

This is an architecture design document. The network controls and traffic flows described below are proposed controls, not claims of a fully deployed production environment.

## 2. Existing Azure Foundation

The portfolio includes the following Azure resources in the `BonganiSecurity` resource group:

| Resource | Name | Evidence status |
|---|---|---|
| Virtual network | `vnet-bongani-day2` | Created and inspected |
| Network security group | `nsg-bongani-day2` | Created and inspected |

These resources provide a practical foundation for network security engineering.

The existence of these resources does not, by itself, confirm that subnet segmentation, NSG associations, specific traffic rules or production network controls have been implemented.

No additional Azure resources are required for this architecture exercise.

## 3. Network Security Objectives

The target architecture follows these principles:

- Apply network segmentation according to workload sensitivity and business function.
- Deny unnecessary inbound and outbound traffic.
- Allow only explicitly approved communication between network zones.
- Restrict administrative access to authorised management paths.
- Protect sensitive services from direct internet exposure.
- Monitor network activity and investigate suspicious traffic.
- Use private connectivity for supported Azure platform services where appropriate.
- Document and periodically review network access rules.

## 4. Proposed Network Architecture

The proposed environment separates workloads into logical security zones.

| Network zone | Purpose | Security objective |
|---|---|---|
| Management | Approved administrative operations | Restrict privileged access |
| Application | Business application components | Permit only required application traffic |
| Data | Databases and sensitive information | Prevent direct public access |
| Identity | Identity-related infrastructure | Restrict access to approved systems |
| Monitoring | Security logging and monitoring components | Protect monitoring infrastructure |
| AI workloads | AI applications and supporting services | Restrict access to models, data and APIs |

These are logical design zones. They are not evidence of existing subnets.

### Proposed traffic flow

1. An authorised administrator connects through an approved management access path.
2. Application users reach approved application entry points.
3. Application components communicate with data services only through explicitly permitted paths.
4. Sensitive data services do not accept unrestricted internet connections.
5. Workloads access Azure platform services through approved network paths.
6. Network and security events are forwarded to central monitoring services where deployed.

## 5. Virtual Network and Subnet Segmentation

A virtual network provides the network boundary for Azure resources.

The target design uses separate subnets for different workload categories, with network security controls applied according to the sensitivity and function of each workload.

Design requirements:

- Avoid placing all workloads in one unrestricted subnet.
- Separate application, data and management workloads.
- Use dedicated subnets where Azure services require them.
- Review subnet address allocation and avoid overlapping address spaces.
- Document the purpose and ownership of each subnet.
- Review routing and security controls whenever a workload is added or relocated.

Subnet names and address ranges must be confirmed during implementation. No unverified subnet configuration is asserted in this document.

## 6. Network Security Groups

Network Security Groups (NSGs) provide network traffic filtering through inbound and outbound security rules.

The existing `nsg-bongani-day2` resource is part of the portfolio's practical Azure foundation.

The target-state rule management approach is:

- Permit only required source and destination traffic.
- Specify the narrowest practical source and destination ranges.
- Restrict administrative protocols to approved management sources.
- Avoid unrestricted inbound management access from the internet.
- Review outbound rules to identify unnecessary external communication.
- Document business justification and ownership for exceptions.
- Review rule priority and effective traffic behaviour before changes.
- Remove obsolete rules through a controlled change process.

An NSG rule should not be described as implemented until its actual configuration has been inspected.

## 7. Private Connectivity and Service Protection

Where supported by the service and business requirements, Azure Private Link and private endpoints should be considered for sensitive platform services.

Proposed controls:

- Prefer private connectivity for supported data services.
- Restrict public network access where private connectivity is operationally appropriate.
- Integrate private DNS resolution with the network design.
- Validate name resolution from the relevant workload subnet.
- Review access restrictions independently of network connectivity.
- Document any service that must retain public access and the compensating controls.

A private endpoint does not replace identity-based authorisation. Applications and users must still authenticate and receive appropriate permissions.

## 8. DNS and Name Resolution

DNS is a critical dependency for identity, application connectivity and private service access.

The target design requires:

- Documented DNS ownership and resolution paths.
- Reliable resolution of internal service names.
- Appropriate private DNS zones for supported private endpoints.
- Controlled DNS configuration changes.
- Monitoring and investigation of unexpected DNS behaviour.
- Validation of name resolution after network changes.

Hybrid DNS integration would be designed alongside the proposed Active Directory and Microsoft Entra hybrid identity architecture.

## 9. Administrative Access Security

Administrative access must be restricted and monitored.

Proposed controls:

- Use named administrative identities rather than shared accounts.
- Require strong authentication for privileged access.
- Apply least-privilege permissions.
- Restrict management interfaces to approved access paths.
- Avoid exposing RDP or SSH management ports directly to the public internet.
- Consider Azure Bastion or another approved management solution where justified.
- Record administrative activity and investigate unexpected access.

Privileged Identity Management and advanced Conditional Access capabilities may require additional licensing. They are design recommendations, not confirmed deployments in this environment.

## 10. Network Monitoring and Threat Detection

The target architecture incorporates centralised monitoring.

Relevant telemetry may include:

- Azure Activity Log events relating to network resource changes.
- NSG flow logs or supported successor monitoring capabilities.
- Azure Firewall logs where Azure Firewall is deployed.
- DNS and private endpoint diagnostic information.
- Network connection and traffic analytics where configured.
- Defender for Cloud security recommendations where applicable.

Microsoft Sentinel and Log Analytics are proposed monitoring components. They have not been represented as deployed through this architecture document.

Monitoring objectives include identifying:

- Unexpected exposure of management ports.
- Unauthorised network security rule changes.
- Unusual outbound traffic patterns.
- Unexpected communication between security zones.
- Changes to virtual networks, subnets and routing.
- Attempts to reach sensitive services through unauthorised paths.

## 11. Network Threat Scenarios

| Threat scenario | Potential impact | Proposed mitigation |
|---|---|---|
| Unrestricted inbound access | Exposure of workloads | Restrictive inbound NSG rules |
| Excessive outbound access | Data exfiltration risk | Egress controls and monitoring |
| Flat network architecture | Lateral movement | Workload segmentation |
| Public exposure of sensitive services | Data compromise | Private connectivity and access restrictions |
| Unauthorised NSG modification | Security control bypass | RBAC, activity monitoring and change review |
| DNS manipulation | Traffic redirection | Controlled DNS administration and monitoring |
| Compromised administrator account | Network configuration abuse | MFA, least privilege and privileged access controls |

These are design scenarios, not incidents observed in the Azure subscription.

## 12. Governance and Change Management

Network security changes should follow a controlled process.

Each proposed change should include:

1. Business justification.
2. Source and destination identification.
3. Protocol and port requirements.
4. Security impact assessment.
5. Approval by the appropriate resource owner.
6. Implementation and verification.
7. Evidence capture.
8. Periodic review and removal of unnecessary access.

RBAC permissions should be scoped to the resources and responsibilities of the administrator.

Contributor access should not be treated as permission to assign Azure RBAC roles. Access administration should remain separately controlled.

## 13. Validation Checklist

The following checks should be completed during a future implementation review:

- [ ] Confirm the virtual network address space.
- [ ] Inspect actual subnet names and address ranges.
- [ ] Verify NSG-to-subnet or NSG-to-NIC associations.
- [ ] Review inbound NSG rules.
- [ ] Review outbound NSG rules.
- [ ] Identify publicly exposed services.
- [ ] Review routing and network paths.
- [ ] Check private endpoint and private DNS configuration where applicable.
- [ ] Review administrative access paths.
- [ ] Confirm diagnostic settings and available network telemetry.
- [ ] Review RBAC permissions for network administrators.
- [ ] Capture dated evidence of the final configuration.

## 14. Implementation and Evidence Classification

| Capability | Current portfolio status |
|---|---|
| Azure virtual network `vnet-bongani-day2` | Created and inspected |
| Azure NSG `nsg-bongani-day2` | Created and inspected |
| Detailed subnet segmentation | Proposed architecture |
| NSG traffic rule baseline | Proposed architecture |
| Private endpoint architecture | Proposed architecture |
| Centralised network monitoring | Proposed architecture |
| Azure Firewall deployment | Not deployed |
| Production network penetration testing | Not performed |

This distinction ensures the portfolio accurately represents practical Azure experience without overstating implementation.

## 15. Professional Relevance

This architecture demonstrates an understanding of Azure network security engineering within a financial-services environment.

It connects network controls to identity security, least privilege, workload protection, threat detection and governance.

For an Azure IAM or Cloud Security Engineer, network security is an important complementary discipline because identity permissions alone cannot provide complete protection against unauthorised network access or lateral movement.

The design provides a foundation for future implementation, security validation and integration with the wider BonganiSecure enterprise cybersecurity architecture.
