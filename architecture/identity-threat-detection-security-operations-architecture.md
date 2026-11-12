# Identity Threat Detection and Security Operations Architecture

## 1. Purpose

This document defines the target-state identity threat detection and security operations architecture for BonganiSecure Financial Services, a fictional financial-services organisation.

The objective is to establish a structured approach to detecting, investigating and responding to identity-related threats across Microsoft Entra ID and Azure.

The architecture builds on practical investigations completed in the BonganiSecure Azure Identity Threat Detection and Response portfolio project.

It distinguishes between verified Azure observations, detection engineering artefacts and proposed enterprise monitoring capabilities.

## 2. Security Objectives

The identity security operations architecture aims to:

- Detect suspicious workforce identity sign-ins.
- Monitor privileged role assignments and changes.
- Identify suspicious service principal activity.
- Monitor managed identity usage and permissions.
- Detect potential privilege escalation.
- Investigate authentication failures and anomalies.
- Correlate identity events with Azure resource activity.
- Establish consistent incident response procedures.
- Maintain evidence for security governance and audit.

## 3. Identity Threat Landscape

Identity is a critical security boundary in cloud environments.

Potential threats include:

| Threat | Potential impact |
|---|---|
| Compromised user credentials | Unauthorised account access |
| MFA fatigue or authentication abuse | Account compromise |
| Privilege escalation | Unauthorised administrative control |
| Malicious service principal activity | Application-level access abuse |
| Managed identity misuse | Unauthorised workload access |
| Excessive RBAC permissions | Unauthorised resource modification |
| Credential theft | Persistent unauthorised access |
| Identity lifecycle failures | Access retained after business need ends |

These scenarios are architecture threats, not claims that the portfolio subscription experienced confirmed compromise.

## 4. Target-State Monitoring Architecture

The proposed monitoring architecture uses the following components:

1. Microsoft Entra ID provides identity and authentication telemetry.
2. Azure Activity Log records relevant Azure management-plane operations.
3. Azure RBAC inventories support privilege and access investigations.
4. Workload identity sign-in telemetry supports service principal and managed identity investigations.
5. Azure Monitor and Log Analytics provide a centralised data platform where configured.
6. Microsoft Sentinel provides proposed security analytics, incident management and automation.
7. Security analysts investigate alerts, document evidence and coordinate remediation.

### Proposed detection workflow

Identity event → Telemetry collection → Detection rule → Alert triage → Investigation → Containment → Recovery → Lessons learned.

Microsoft Sentinel and Log Analytics are proposed components. Their deployment is not claimed in this portfolio.

## 5. Workforce Identity Threat Detection

The monitoring programme should assess user authentication activity.

Important investigation signals include:

- Repeated failed sign-ins.
- Unusual authentication patterns.
- Unexpected application access.
- Authentication failures followed by successful sign-ins.
- MFA-related authentication details.
- Changes to authentication methods.
- Unusual account activity.
- Disabled accounts attempting authentication.

### Portfolio investigation evidence

Project 4 included a live inspection of Microsoft Entra sign-in records.

Observed evidence included:

- A successful sign-in with Conditional Access status Success.
- Security Defaults shown as the applicable policy.
- Authentication method recorded as Previously satisfied.
- Additional details stating that the MFA requirement was satisfied by a claim in the token.
- A failed sign-in with error code 50089, associated with an expired flow token.
- A subsequent successful sign-in by the same user to the same application.

The evidence supports documenting an authentication interruption followed by recovery. It does not establish an attack or account compromise.

## 6. Privileged Role Assignment Monitoring

Privileged Azure roles create significant risk if assigned incorrectly or modified without authorisation.

The target architecture monitors:

- Owner assignments.
- Contributor assignments.
- Security Administrator assignments.
- Privileged role additions and removals.
- Changes to role assignment scope.
- Unexpected role assignment callers.
- Privilege escalation activity.

### Portfolio investigation evidence

Project 4 included inspection of Azure role assignments and Activity Log events.

Observed evidence included:

- The user's account held Owner at subscription scope.
- The user's account held Security Administrator at management-group scope.
- Successful role assignment write events were observed in the inspected 90-day Activity Log window.
- The caller query identified the user's own account for the inspected successful write events.
- No matching role assignment delete events were returned in the inspected query window.

These observations are limited to the inspected scope and retention window. They do not establish that no other privileged activity occurred.

## 7. Service Principal Threat Detection

Service principals allow applications and automation processes to authenticate to Microsoft Entra ID.

They must be governed as identities with access privileges.

The monitoring architecture should detect:

- Unexpected service principal sign-ins.
- New credential creation.
- Credential expiration changes.
- Unusual permission grants.
- Application role assignments.
- Changes to application ownership.
- Unexpected resource access.
- Dormant or unowned application identities requiring review.

### Portfolio investigation evidence

The Finance application service principal was inspected in Project 4.

Observed findings:

- Service principal sign-in logs returned No results for the selected period and filters.
- The enterprise application was enabled for users to sign in.
- Assignment required was enabled.
- No app assignments were found on the inspected Users and groups page.
- No admin-consented permissions were listed on the inspected Permissions page.
- The Certificates and secrets view showed zero certificates and zero client secrets.
- No owners were listed in the inspected owner view.

These observations do not prove the identity was unused or incapable of authentication through every possible mechanism.

The recommended governance response is to establish an accountable application owner and review the application's business purpose and permissions.

## 8. Managed Identity Threat Detection

Managed identities allow Azure workloads to obtain tokens without embedding application credentials in code.

They still require permission governance and monitoring.

The architecture should monitor:

- Managed identity sign-ins.
- Identity creation and modification.
- Resource attachment changes.
- RBAC assignment changes.
- Unexpected privilege increases.
- Workload access patterns.
- Identity lifecycle and ownership.

### Portfolio investigation evidence

Project 4 investigated `mi-bongani-day1`.

Observed evidence included:

- No managed identity sign-in results in the inspected 30-day period.
- A Reader role assignment at the `BonganiSecurity` resource-group scope.
- No attached resource references returned by the inspected resource-group query.
- An empty tags object in the inspected identity metadata.
- Activity Log records for managed identity write operations.
- Two successful, two started and one failed write operation in the inspected 90-day results.
- The failed operation reported `MissingSubscriptionRegistration`.
- The subscription's current Managed Identity resource provider status was subsequently verified as Registered.

The historical failure was an operational configuration issue. It is not evidence of malicious activity.

The absence of sign-in results and resource attachment references in the inspected scope does not prove that the identity is orphaned.

## 9. Detection Engineering and KQL

The portfolio includes KQL detection templates for identity security investigations.

Detection categories include:

| Detection category | Security objective |
|---|---|
| Entra sign-in monitoring | Identify authentication anomalies |
| Privileged role assignments | Detect unexpected privilege changes |
| Service principal sign-ins | Identify unusual application identity activity |
| Service principal credential changes | Monitor application credential lifecycle |
| Managed identity activity | Investigate workload identity behaviour |
| RBAC privilege escalation | Identify potentially excessive permissions |
| Identity lifecycle anomalies | Detect suspicious identity changes |

KQL templates are maintained in the Project 4 repository.

They are detection engineering artefacts and have not been represented as executed or validated in a live Sentinel or Log Analytics environment.

Before production use, each query must be validated against the actual table schema, available telemetry, permissions and retention configuration.

## 10. Incident Response Lifecycle

The proposed identity incident response lifecycle consists of six stages.

### Stage 1: Detection

Receive an alert or identify suspicious identity activity through monitoring.

### Stage 2: Triage

Confirm the affected identity, resource, time period, severity and available evidence.

### Stage 3: Investigation

Correlate relevant sign-in records, Activity Log events, RBAC assignments, application permissions and workload identity telemetry.

### Stage 4: Containment

Where compromise is established or sufficiently suspected, apply approved containment actions. Depending on the incident, these may include disabling an account, revoking sessions, removing inappropriate permissions or disabling an affected credential.

### Stage 5: Recovery

Restore authorised access, rotate credentials where required, correct configuration weaknesses and validate security controls.

### Stage 6: Lessons Learned

Document root cause, impact, timeline, evidence, remediation and preventive recommendations.

Containment actions must be authorised and appropriate to the affected workload. Disabling a production identity without dependency analysis can disrupt business services.

## 11. Financial-Services Incident Scenarios

| Scenario | Investigation priority | Potential response |
|---|---|---|
| Unexpected Owner assignment | Critical | Validate caller, scope and business approval |
| Suspicious Finance user sign-in | High | Investigate authentication and application access |
| Unusual service principal activity | High | Review credentials, permissions and ownership |
| Unexpected managed identity permission increase | High | Review RBAC scope and workload dependencies |
| Repeated failed authentication | Medium to high | Investigate source, target and subsequent success |
| Identity remains active after offboarding | High | Review access removal and session revocation |

Severity should be determined using evidence, business impact and the organisation's incident classification policy.

## 12. Roles and Responsibilities

| Role | Responsibility |
|---|---|
| IAM Engineer | Identity configuration and access governance |
| Cloud Security Engineer | Azure security controls and investigation |
| Security Analyst | Alert triage and evidence collection |
| Security Operations Lead | Incident coordination and escalation |
| Application Owner | Service principal accountability and access justification |
| Business Risk Owner | Business impact and residual risk acceptance |

The organisation should maintain clear separation between identity administration, security monitoring and risk acceptance.

## 13. Governance and Evidence Management

Each investigation should record:

- Incident or investigation reference.
- Affected identity.
- Detection source.
- Relevant timestamps.
- Query or investigation method.
- Observed facts.
- Evidence limitations.
- Risk assessment.
- Containment and remediation actions.
- Approval and ownership.
- Closure evidence.

An absence of logs or detection results must not automatically be interpreted as proof that an identity is safe or unused.

## 14. Validation Checklist

The following activities should be completed during future implementation:

- [ ] Confirm Entra ID sign-in log availability and retention.
- [ ] Review privileged Azure role assignments.
- [ ] Verify Activity Log collection and retention.
- [ ] Review service principal sign-in visibility.
- [ ] Review managed identity sign-in visibility.
- [ ] Validate KQL queries against actual schemas.
- [ ] Confirm Log Analytics integration where deployed.
- [ ] Confirm Sentinel analytics and incident configuration where deployed.
- [ ] Test incident escalation and response procedures.
- [ ] Document evidence and investigation limitations.
- [ ] Review privileged access and application ownership periodically.

## 15. Implementation and Evidence Classification

| Capability | Portfolio status |
|---|---|
| Entra ID sign-in investigation | Performed |
| Privileged role assignment investigation | Performed |
| Service principal security review | Performed |
| Managed identity security investigation | Performed |
| Identity KQL detection templates | Created |
| Identity incident response playbook | Created |
| Enterprise monitoring architecture | Designed |
| Microsoft Sentinel deployment | Not deployed |
| Log Analytics integration | Not deployed |
| Automated identity response | Not deployed |

## 16. Professional Relevance

This architecture demonstrates practical understanding of identity security operations across workforce identities, privileged Azure roles, application identities and managed identities.

It connects IAM engineering with cloud threat detection, KQL development, incident response and security governance.

For an Azure IAM Engineer or Cloud Security Engineer, the ability to investigate identity activity, understand RBAC exposure and translate evidence into defensible security decisions is an important professional capability.

The architecture also demonstrates how identity security operations can support the protection of sensitive financial-services workloads.
