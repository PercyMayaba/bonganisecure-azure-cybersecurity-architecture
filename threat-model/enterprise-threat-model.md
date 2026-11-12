# Enterprise Cybersecurity Threat Model

## 1. Purpose

This threat model identifies the principal cybersecurity risks facing BonganiSecure Financial Services and establishes the security controls required to mitigate them within the proposed Azure enterprise architecture.

The model considers workforce identities, privileged accounts, cloud workloads, enterprise applications, financial data, network infrastructure and AI-enabled services.

It uses a risk-based approach inspired by established threat-modelling principles and the MITRE ATT&CK framework.

## 2. Critical Assets

| Asset | Business Importance | Security Priority |
|---|---|---|
| Microsoft Entra ID tenant | Workforce and application identity | Critical |
| Privileged administrator accounts | Control over enterprise infrastructure | Critical |
| Azure subscriptions | Cloud infrastructure and resources | Critical |
| Managed identities | Authentication for Azure workloads | High |
| Service principals | Application and service authentication | High |
| Active Directory | Enterprise directory and authentication | Critical |
| Financial databases | Confidential financial information | Critical |
| Enterprise applications | Business operations and transactions | High |
| AI knowledge assistant | Access to internal enterprise information | High |
| Security logs | Investigation and audit evidence | High |

## 3. Threat Actors

### External attackers

Potential objectives:
- Compromise workforce credentials.
- Gain unauthorised access to cloud applications.
- Exploit exposed application endpoints.
- Obtain sensitive financial information.

### Malicious insiders

Potential objectives:
- Abuse legitimate access.
- Escalate privileges.
- Access information outside their responsibilities.
- Modify or delete security records.

### Compromised application identities

A stolen service principal credential or compromised workload identity may allow an attacker to access Azure resources using application permissions.

### Compromised AI agents or applications

An AI application with excessive data access or poorly controlled tool permissions may expose confidential information or perform unauthorised actions.

## 4. Primary Attack Surfaces

1. Workforce authentication and sign-in processes.
2. Privileged role assignments.
3. Enterprise application permissions.
4. Service principal credentials.
5. Managed identity permissions.
6. Active Directory authentication and delegation.
7. Network security configurations.
8. Cloud storage and application endpoints.
9. AI retrieval systems and agent tools.
10. Security monitoring and audit infrastructure.

## 5. Threat Scenarios and Mitigations

### Threat 1: Workforce Account Compromise

**Attack scenario:** An attacker obtains a user's credentials through phishing or credential theft and attempts to access enterprise applications.

**Potential impact:**
- Unauthorised access to financial information.
- Access to internal business applications.
- Further privilege escalation.

**Mitigations:**
- Strong authentication and MFA.
- Conditional Access where licensing permits.
- Sign-in monitoring.
- Risk-based access policies.
- Session revocation and account disablement procedures.

### Threat 2: Privilege Escalation Through Azure RBAC

**Attack scenario:** An attacker or unauthorised administrator obtains excessive Azure permissions or assigns a privileged role to another identity.

**Potential impact:**
- Unauthorised resource modification.
- Access to sensitive cloud workloads.
- Security configuration changes.
- Potential control over additional identities.

**Mitigations:**
- Least-privilege RBAC.
- Restrict role assignment permissions.
- Monitor roleAssignments/write operations.
- Review privileged role membership.
- Use Privileged Identity Management where licensed.

### Threat 3: Service Principal Credential Compromise

**Attack scenario:** An application secret or certificate is exposed through source code, an insecure repository or an unauthorised user.

**Potential impact:**
- Unauthorised application authentication.
- Access to permitted APIs and resources.
- Data exposure or resource modification.

**Mitigations:**
- Prefer managed identities where possible.
- Secure credential storage.
- Rotate and revoke compromised credentials.
- Monitor credential changes.
- Review application permissions and ownership.

### Threat 4: Managed Identity Privilege Abuse

**Attack scenario:** An attacker compromises an Azure workload and attempts to use its managed identity to access additional resources.

**Potential impact:**
- Unauthorised access to Azure resources.
- Data exposure.
- Lateral movement within the cloud environment.

**Mitigations:**
- Restrict managed identity permissions.
- Use resource-group or resource-level RBAC scopes where appropriate.
- Review identity attachment and usage.
- Monitor managed identity and RBAC changes.
- Investigate unusual workload activity.

### Threat 5: Active Directory Privilege Abuse

**Attack scenario:** An attacker obtains privileged directory credentials or abuses excessive group membership and delegation.

**Potential impact:**
- Domain-level compromise.
- Unauthorised account creation.
- Access to sensitive systems.
- Persistence through privileged accounts.

**Mitigations:**
- Tiered administration.
- Privileged group reviews.
- Secure authentication configuration.
- Directory auditing.
- Controlled delegation and account lifecycle management.

### Threat 6: Network Segmentation Failure

**Attack scenario:** An attacker gains access to an exposed workload and attempts to reach internal application or data segments.

**Potential impact:**
- Lateral movement.
- Unauthorised service access.
- Exposure of sensitive systems.

**Mitigations:**
- Network segmentation.
- Network Security Groups.
- Restrictive inbound and outbound rules.
- Secure administrative access.
- Network configuration monitoring.

### Threat 7: AI Workload Data Exposure

**Attack scenario:** An AI assistant or agent retrieves information beyond the user's authorised access or invokes an excessively privileged tool.

**Potential impact:**
- Exposure of confidential financial information.
- Unauthorised application actions.
- Cross-user data leakage.
- Loss of accountability.

**Mitigations:**
- Entra ID authentication.
- Document-level authorisation.
- Least-privilege workload identities.
- Agent action policy enforcement.
- Input and output validation.
- Audit logging and monitoring.

## 6. Risk Prioritisation

| Threat | Likelihood | Impact | Priority |
|---|---|---|---|
| Workforce account compromise | High | Critical | Critical |
| Azure privilege escalation | Medium | Critical | Critical |
| Service principal compromise | Medium | High | High |
| Managed identity abuse | Medium | High | High |
| Active Directory privilege abuse | Medium | Critical | Critical |
| Network segmentation failure | Medium | High | High |
| AI data exposure | Medium | Critical | Critical |

These ratings are initial design assessments, not measured incident probabilities. They should be reviewed against actual business exposure and control effectiveness.

## 7. Detection and Response Strategy

The architecture will use centralised monitoring principles to correlate identity, application and Azure management activity.

Priority monitoring sources:

- Microsoft Entra sign-in logs.
- Entra audit logs.
- Service principal sign-in logs.
- Managed identity sign-in records.
- Azure Activity Logs.
- Defender for Cloud security recommendations.
- Application and workload audit records.

Where appropriately licensed and deployed, Azure Monitor, Log Analytics and Microsoft Sentinel can support centralised analytics and alerting.

## 8. MITRE ATT&CK Mapping

The following mappings provide starting points for detection engineering. They are illustrative and should be validated against the precise attack behaviour.

| Threat Behaviour | ATT&CK Technique |
|---|---|
| Valid account abuse | T1078 |
| Cloud account compromise | T1078.004 |
| Account manipulation | T1098 |
| Additional cloud credentials | T1098.001 |
| Cloud service discovery | T1526 |
| Data from cloud storage | T1530 |
| Steal application access tokens | T1528 |

## 9. Architecture Security Principles

The proposed architecture follows these principles:

1. Identity-first security.
2. Zero Trust verification.
3. Least-privilege authorisation.
4. Defence in depth.
5. Secure workload identity.
6. Network segmentation.
7. Continuous monitoring.
8. Evidence-based incident response.
9. Secure AI application design.
10. Clear accountability and governance.

## 10. Limitations and Assumptions

This is an architecture threat model for a fictional financial-services organisation.

The threats are design scenarios, not evidence of attacks against the user's Azure subscription. Mitigations are proposed controls and are not all deployed or independently tested.

The model will be refined as the architecture and security control designs are developed.
