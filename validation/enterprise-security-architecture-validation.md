# Enterprise Security Architecture Validation

## 1. Purpose

This document validates the proposed enterprise cybersecurity architecture for BonganiSecure Financial Services, a fictional financial-services organisation.

The purpose is to assess whether the architecture addresses the organisation's major security requirements across:

- Microsoft Entra ID and identity governance.
- Zero Trust security.
- Active Directory and hybrid identity.
- Azure workload identity and RBAC.
- Network security and segmentation.
- Microsoft Defender for Cloud.
- Identity threat detection and security operations.
- AI workload and agent security.
- Security governance and incident response.

This is a portfolio architecture review, not an independent production security audit or certification assessment.

## 2. Validation Approach

The review uses three evidence classifications.

| Classification | Meaning |
|---|---|
| Implemented and verified | Configuration or activity directly inspected and recorded during the portfolio projects |
| Designed | A proposed architecture or security control documented but not deployed |
| Simulated | A scenario, dataset, script or workflow created for engineering practice without execution against a live production environment |

A control should not be marked implemented merely because it appears in an architecture diagram or documentation.

## 3. Enterprise Architecture Validation

| Security domain | Validation focus | Portfolio evidence | Status |
|---|---|---|---|
| Entra ID | Users, groups, authentication and identity lifecycle | Project 1 | Implemented and inspected |
| Zero Trust | Identity verification and least privilege | Project 1 and Project 5 | Partially implemented; broader design documented |
| Active Directory | Directory design and security auditing | Project 2 | Simulated |
| Workload identity | Managed identity and RBAC | Project 3 | Implemented and inspected |
| Network security | Virtual network and NSG foundation | Project 3 and Project 5 | Resources created and inspected; segmentation designed |
| Defender for Cloud | Security posture and plan inspection | Azure security lab and Project 5 | Inspected; advanced capabilities not confirmed |
| Identity detection | Sign-in, privileged role and workload identity investigations | Project 4 | Investigations performed |
| Security operations | KQL and incident response | Project 4 and Project 5 | Detection templates and procedures created |
| AI security | Agent policies and workload protection | AI Agent Security Lab and Project 5 | Simulation and architecture design |

## 4. Identity and Zero Trust Validation

### Implemented and inspected

- Microsoft Entra ID Free tenant inspected.
- Security Defaults enabled.
- Authentication Methods policy configured for Microsoft Authenticator.
- Standard user and Finance test identities created.
- Finance test identity disabled during lifecycle testing.
- Finance application assignment removed during offboarding.
- Sessions revoked during offboarding.
- Sign-in records inspected.
- Enterprise application configuration reviewed.

### Design limitations

The following capabilities are not claimed as implemented:

- Custom Conditional Access policies.
- Privileged Identity Management.
- Automated access reviews.
- Enterprise-wide device compliance enforcement.
- Automated identity lifecycle provisioning.

Some capabilities require additional licensing.

### Validation conclusion

The portfolio demonstrates practical identity administration, authentication inspection, access lifecycle management and security investigation.

The next engineering improvement is to expand identity governance and privileged access controls as licensing and environment capabilities permit.

## 5. Active Directory Validation

### Completed portfolio work

The Active Directory project includes:

- Domain and OU architecture.
- Security group auditing.
- Password policy baseline.
- Privileged access auditing.
- Kerberos and LDAP security review.
- Authentication event investigation.
- PowerShell audit scripts and synthetic datasets.

### Validation limitation

The project was completed as a GitHub-based simulation.

No live Active Directory domain controller, production domain, Group Policy configuration or actual directory users were deployed.

The PowerShell scripts are portfolio artefacts and should be executed and tested in a suitable environment before production use.

### Validation conclusion

The project demonstrates knowledge of directory architecture and Active Directory security assessment, but does not establish live Windows Server administration experience.

## 6. Azure Workload Identity and RBAC Validation

### Implemented and inspected

The portfolio includes:

- User-assigned managed identity `mi-bongani-day1`.
- Azure virtual network `vnet-bongani-day2`.
- Network security group `nsg-bongani-day2`.
- Inspection of managed identity metadata.
- Managed identity usage investigation.
- Azure RBAC role and scope review.
- Reader assignment verified at the `BonganiSecurity` resource-group scope.
- Finance enterprise application service principal investigation.

### Validation limitations

- No production workload was deployed to consume the managed identity.
- No successful workload token acquisition or data-plane access test is claimed.
- No managed identity sign-in results were returned in the inspected 30-day period.
- The Finance service principal review did not establish that the application was being used.
- No actual application functionality was tested.

### Validation conclusion

The project demonstrates practical managed identity inspection and RBAC scope administration.

A future validation step should test an actual workload's access to a non-sensitive resource using the intended identity and permissions.

## 7. Network Security Validation

### Existing foundation

The portfolio contains an Azure virtual network and network security group.

### Designed controls

- Workload segmentation.
- Management network restrictions.
- Inbound and outbound traffic governance.
- Private endpoint architecture.
- DNS security.
- Administrative access restrictions.
- Network monitoring.

### Validation limitations

The portfolio does not claim that all proposed subnets, traffic rules, NSG associations, private endpoints or network monitoring integrations have been configured.

### Validation conclusion

The architecture establishes a defensible target-state network security design.

Future implementation should verify actual subnet configuration, NSG associations, effective security rules and permitted network paths.

## 8. Defender for Cloud Validation

### Inspected

The Azure portal was used to inspect Defender for Cloud environment settings and plan configuration.

Recorded observations included:

- CloudPosture displayed as Free.
- Foundational CSPM displayed as Standard but greyed out in the inspected view.
- Discovery displayed as Standard.
- Several workload protection plans displayed as Free.

### Limitations

- Advanced CSPM functionality is not claimed as active.
- Paid workload protection plans were not activated as part of this portfolio.
- No production Sentinel or Log Analytics integration is claimed.
- No complete regulatory compliance assessment is claimed.

### Validation conclusion

The project demonstrates practical inspection of Defender for Cloud configuration and an understanding of how posture management and workload protection fit into enterprise security architecture.

## 9. Identity Threat Detection Validation

### Investigations performed

The portfolio includes:

- Entra ID sign-in investigation.
- Privileged role assignment investigation.
- Service principal security investigation.
- Managed identity threat investigation.
- Identity security monitoring architecture.
- Advanced identity threat scenarios.
- Incident reporting and response procedures.

### Detection engineering

KQL templates were created for:

- Sign-in anomalies.
- Privileged role changes.
- Service principal sign-ins.
- Service principal credential changes.
- Managed identity activity.
- RBAC privilege escalation.
- Identity lifecycle anomalies.

### Limitations

The KQL templates have not been validated in a live Sentinel or Log Analytics workspace.

The advanced threat scenarios are simulations, not attacks executed against the Azure subscription.

### Validation conclusion

The portfolio demonstrates practical identity investigation and detection engineering design, with clear boundaries around the monitoring capabilities actually deployed.

## 10. AI Workload and Agent Security Validation

### Completed portfolio work

- AI agent message validation simulation.
- Agent policy engine simulation.
- AI workload identity architecture.
- RAG document-level access architecture.
- Prompt injection threat modelling.
- Tool execution governance.
- AI security monitoring and response design.

### Limitations

The AI agent security lab is a GitHub-based simulation.

No production Azure AI workload, Azure AI Search integration, live document-level authorisation enforcement or deployed automated agent response is claimed.

### Validation conclusion

The architecture demonstrates an understanding of how cloud identity, access control and AI-specific safeguards work together to protect enterprise AI applications.

## 11. Cross-Domain Security Gaps

The following gaps remain important future engineering priorities.

| Gap | Risk | Recommended improvement |
|---|---|---|
| No deployed hybrid AD environment | Limited live hybrid identity experience | Implement a suitable test environment |
| No custom Conditional Access policies | Limited granular access enforcement | Evaluate licensing and implement approved policies |
| No PIM deployment | Privileged access exposure | Introduce just-in-time privileged access where supported |
| No production workload identity test | Data-plane access not verified | Test least-privilege workload access |
| Network segmentation not fully implemented | Potential lateral movement | Implement and validate workload subnet controls |
| Advanced CSPM not confirmed | Reduced posture visibility | Resolve plan eligibility and configuration |
| No centralised Sentinel deployment | Limited centralised detection execution | Design and cost an appropriate monitoring environment |
| KQL templates not executed | Detection accuracy unverified | Validate against actual telemetry schemas |
| AI controls not deployed | Model and agent risks remain | Implement controlled AI security testing |

These gaps are not evidence of a compromised environment. They are implementation and validation opportunities.

## 12. Enterprise Risk Assessment

The following matrix is a proposed design-level risk assessment.

| Risk | Likelihood | Impact | Priority |
|---|---|---|---|
| Compromised privileged identity | Medium | Critical | High |
| Excessive workload identity permissions | Medium | High | High |
| Inadequate network segmentation | Medium | High | High |
| Unauthorised application identity access | Medium | High | High |
| Limited centralised security monitoring | High | High | High |
| AI agent prompt injection | Medium | High | High |
| Sensitive document exposure through RAG | Medium | Critical | High |
| Inadequate identity lifecycle management | Medium | High | High |

These ratings are qualitative architecture assessments, not measured incident probabilities.

## 13. Recommended Implementation Roadmap

### Phase 1: Identity foundations

- Review privileged identity assignments.
- Maintain strong authentication.
- Improve identity lifecycle governance.
- Review application identity ownership.
- Validate least-privilege RBAC.

### Phase 2: Workload and network protection

- Validate managed identity workload access.
- Implement appropriate network segmentation.
- Review NSG rules and effective traffic paths.
- Evaluate private connectivity.
- Review Defender for Cloud posture capabilities.

### Phase 3: Security monitoring

- Establish centralised logging requirements.
- Validate KQL detection templates.
- Design appropriate Sentinel integration.
- Establish security incident escalation.
- Test identity response procedures.

### Phase 4: AI security engineering

- Implement document-level retrieval access controls.
- Establish dedicated AI workload identities.
- Test prompt injection protections.
- Enforce tool allowlists and action policies.
- Require human approval for sensitive operations.

### Phase 5: Continuous assurance

- Review security recommendations.
- Test security controls periodically.
- Maintain evidence registers.
- Reassess risks after architectural changes.
- Track remediation ownership and closure.

## 14. Final Architecture Assessment

The BonganiSecure enterprise cybersecurity architecture establishes an integrated target-state design across identity, infrastructure, network security, workload protection, security operations and AI security.

The five-project portfolio provides evidence of:

- Practical Microsoft Entra ID administration and identity lifecycle work.
- Active Directory security engineering simulations.
- Azure managed identity and RBAC inspection.
- Azure security posture investigation.
- Identity threat investigation and detection engineering.
- AI agent security simulation.
- Enterprise cloud security architecture design.

The architecture is not presented as a fully deployed enterprise environment.

Its value lies in combining practical cloud engineering evidence, structured security analysis, technical documentation and a coherent enterprise security design.

## 15. Overall Professional Assessment

The portfolio demonstrates a developing engineering capability aligned with Azure IAM and Cloud Security Engineering.

The strongest practical evidence is in Microsoft Entra identity administration, Azure RBAC scope review, managed identity inspection and identity security investigations.

The architecture work extends this foundation into network security, Defender for Cloud, security operations and AI workload protection.

The next stage is to expand live implementation and validation experience while preserving the distinction between deployed controls, inspected configurations, design proposals and simulations.

This provides a credible foundation for an Azure IAM Engineer or Cloud Security Engineer career transition.
