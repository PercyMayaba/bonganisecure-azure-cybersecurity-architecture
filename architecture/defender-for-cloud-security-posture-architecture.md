# Defender for Cloud Security Posture Architecture

## 1. Purpose

This document defines the target-state Microsoft Defender for Cloud architecture for BonganiSecure Financial Services, a fictional financial-services organisation.

The objective is to establish a consistent approach to cloud security posture management, workload protection, vulnerability reduction, regulatory alignment and continuous security improvement across Azure resources.

This document combines practical Azure portal observations from the portfolio with proposed enterprise security controls. It does not claim that all Defender for Cloud plans or capabilities have been deployed.

## 2. Security Objectives

The architecture aims to:

- Continuously assess Azure resource security configurations.
- Identify misconfigurations and excessive exposure.
- Prioritise security recommendations according to risk.
- Protect supported cloud workloads against threats.
- Improve visibility into identity, network and workload risks.
- Support financial-services security governance.
- Establish a repeatable remediation and exception process.
- Integrate cloud security findings into central security operations.

## 3. Existing Azure Environment and Evidence

The portfolio includes practical inspection of Microsoft Defender for Cloud in the Azure subscription.

The following observations were recorded:

| Area | Observed status |
|---|---|
| Azure subscription | Active |
| Defender for Cloud environment settings | Inspected |
| Defender plan configuration | Inspected |
| CloudPosture plan | Displayed as Free |
| Foundational CSPM | Displayed as Standard but greyed out in the inspected view |
| Discovery | Displayed as Standard |
| VM, SQL, App Services, Storage, AKS and Key Vault plans | Displayed as Free in the inspected plan table |
| Security posture investigation | Performed through portal inspection |

These are historical observations from the portfolio exercise. They should not be interpreted as proof that every advanced security feature is currently active or producing alerts.

No paid Defender plan activation is claimed.

## 4. Target-State Architecture

The proposed Defender for Cloud architecture has five main functions:

1. Cloud security posture management.
2. Workload protection.
3. Security recommendations and remediation.
4. Regulatory compliance assessment.
5. Security operations integration.

### Architecture flow

Azure resources and subscriptions are assessed by Defender for Cloud.

Security recommendations identify configuration weaknesses and potential risks.

Security teams review and prioritise findings according to business impact.

Approved remediation actions are implemented through controlled change management.

Relevant security alerts and findings are forwarded to the organisation's monitoring and incident-response processes.

## 5. Cloud Security Posture Management

Cloud Security Posture Management (CSPM) provides visibility into cloud configuration risks and security posture.

The target design should assess:

- Publicly exposed resources.
- Unnecessary network access.
- Weak resource configurations.
- Missing diagnostic settings.
- Excessive resource permissions.
- Insecure storage configurations.
- Unprotected workloads.
- Misconfigured security services.

Security posture findings should be assigned to accountable owners.

Each finding should include:

- Affected resource.
- Description of the weakness.
- Business impact.
- Severity and priority.
- Recommended remediation.
- Responsible owner.
- Target remediation date.
- Evidence of closure.

Advanced CSPM capabilities may depend on subscription eligibility, plan configuration and licensing. They must be verified before being represented as available.

## 6. Workload Protection

The target architecture considers Defender workload protection plans according to the organisation's actual Azure services.

| Workload | Security objective |
|---|---|
| Virtual machines | Detect threats and identify workload vulnerabilities |
| SQL databases | Monitor database security risks |
| Storage accounts | Identify suspicious access and insecure configuration |
| App Services | Improve application workload protection |
| Azure Kubernetes Service | Identify container and cluster security risks |
| Key Vault | Protect sensitive secrets and cryptographic material |
| AI workloads | Protect connected identities, data and application services |

Not every organisation needs every plan.

Plan selection should be based on workload inventory, risk, licensing and expected security value. Additional charges must be assessed before enabling paid capabilities.

## 7. Security Recommendations and Remediation

Security recommendations should be managed as an operational risk register.

A proposed remediation lifecycle is:

1. Identify a recommendation.
2. Confirm the affected resource and configuration.
3. Determine whether the finding is valid.
4. Assess business and technical risk.
5. Assign an owner.
6. Approve remediation or document an exception.
7. Implement the change.
8. Reassess the resource.
9. Record closure evidence.

### Example financial-services findings

| Example finding | Potential risk | Proposed response |
|---|---|---|
| Publicly accessible storage | Data exposure | Review public access and restrict where appropriate |
| Excessive network access | Unauthorised connectivity | Review NSG and firewall rules |
| Excessive resource permissions | Privilege abuse | Reduce RBAC scope and permissions |
| Missing diagnostic configuration | Reduced investigation visibility | Configure appropriate logging |
| Unprotected workload | Increased threat exposure | Assess suitable workload protection |
| Weak secrets management | Credential compromise | Review Key Vault access and secret lifecycle |

These are architecture examples, not claims that each weakness was found in the subscription.

## 8. Regulatory Compliance and Governance

Financial-services organisations require consistent evidence of security governance.

The target design should use relevant Defender for Cloud regulatory compliance assessments where supported.

Potential reference frameworks include:

- ISO 27001.
- NIST Cybersecurity Framework.
- CIS Microsoft Azure Foundations Benchmark.
- Applicable financial-services regulatory and internal control requirements.

A compliance dashboard should help management identify:

- Applicable control requirements.
- Current assessment results.
- Outstanding recommendations.
- Responsible control owners.
- Accepted exceptions.
- Remediation progress.

A compliance score is an indicator of assessed technical controls. It is not proof of full regulatory compliance or certification.

## 9. Identity and Access Integration

Defender for Cloud should be considered alongside Microsoft Entra ID and Azure RBAC.

Proposed controls include:

- Restrict access to security configuration and administration.
- Apply least privilege to security operators.
- Separate security monitoring from security administration where practical.
- Review permissions to modify Defender plans and policies.
- Monitor privileged configuration changes.
- Protect workload identities and service principals.

The portfolio's workload identity project provides practical evidence of managed identity inspection and resource-group-scoped Reader access.

Defender for Cloud does not replace identity governance or access control.

## 10. Network and Workload Integration

Defender for Cloud findings should be interpreted alongside network architecture.

Relevant areas include:

- Public IP exposure.
- Network security group rules.
- Unnecessary inbound access.
- Sensitive service exposure.
- Workload segmentation.
- Private connectivity.
- Vulnerability and workload protection.

The existing portfolio resources `vnet-bongani-day2` and `nsg-bongani-day2` provide a practical foundation for connecting network security engineering with cloud posture management.

Their existence does not establish that every recommended network control has been configured.

## 11. Security Operations Integration

The target architecture proposes integrating Defender for Cloud findings with centralised security monitoring.

Potential components include:

- Azure Monitor.
- Log Analytics workspace.
- Microsoft Sentinel.
- Security incident and alert workflows.
- Azure Activity Log.

The proposed operational process is:

1. Receive a security alert or posture finding.
2. Validate the affected resource.
3. Assess severity and business impact.
4. Correlate with identity, network and activity evidence.
5. Escalate confirmed incidents.
6. Remediate or contain the risk.
7. Document the investigation.
8. Verify closure.

Microsoft Sentinel and Log Analytics are proposed integration components. Their deployment is not claimed in this project.

## 12. Risk Prioritisation

The organisation should prioritise findings using both technical severity and business context.

| Priority | Example | Response |
|---|---|---|
| Critical | Exposed sensitive workload with credible exploitation risk | Immediate investigation and containment |
| High | Excessive privileges or publicly exposed sensitive service | Urgent remediation |
| Medium | Missing security configuration or monitoring | Planned remediation |
| Low | Minor hardening opportunity | Scheduled improvement |

The security team should consider data sensitivity, workload criticality, internet exposure, exploitability and compensating controls.

## 13. Governance and Exception Management

Security recommendations should not be closed without appropriate evidence.

Exceptions should include:

- Business justification.
- Risk owner.
- Compensating controls.
- Approval.
- Expiry or review date.
- Documented residual risk.

Recurring findings should be analysed to identify weaknesses in architecture, deployment standards or operational processes.

## 14. Validation Checklist

The following checks should be completed during future implementation and review:

- [ ] Confirm Defender for Cloud subscription configuration.
- [ ] Verify which CSPM capabilities are available.
- [ ] Review workload protection plan status.
- [ ] Inspect available security recommendations.
- [ ] Identify affected Azure resources.
- [ ] Review regulatory compliance assessments where available.
- [ ] Check security administration permissions.
- [ ] Review network-related recommendations.
- [ ] Confirm diagnostic and alert integration.
- [ ] Document paid-plan costs before activation.
- [ ] Capture evidence of remediation and reassessment.

## 15. Implementation and Evidence Classification

| Capability | Portfolio status |
|---|---|
| Defender for Cloud portal inspection | Performed |
| Defender plan configuration inspection | Performed |
| CloudPosture plan observation | Recorded as Free in inspected view |
| Foundational CSPM observation | Standard displayed, greyed out in inspected view |
| Workload protection configuration | Not claimed as deployed |
| Advanced CSPM capabilities | Not claimed as enabled |
| Regulatory compliance implementation | Proposed |
| Sentinel integration | Proposed, not deployed |
| Centralised remediation workflow | Proposed |

## 16. Professional Relevance

This architecture demonstrates an understanding of cloud security posture management and the relationship between configuration security, workload protection, identity governance, network security and operational risk.

For an Azure Cloud Security or IAM Engineer, Defender for Cloud is an important platform for identifying weaknesses across cloud environments and translating technical findings into prioritised remediation.

The design connects practical Azure portal investigation with enterprise security architecture, governance and financial-services risk management.
