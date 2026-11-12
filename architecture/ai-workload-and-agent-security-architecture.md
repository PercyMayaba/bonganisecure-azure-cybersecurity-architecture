# AI Workload and Agent Security Architecture

## 1. Purpose

This document defines the target-state AI workload and agent security architecture for BonganiSecure Financial Services, a fictional financial-services organisation.

The objective is to establish security controls for AI applications, retrieval-augmented generation (RAG) systems, AI agents and supporting cloud infrastructure.

The architecture combines Azure identity and access management principles with AI security engineering.

It draws on the portfolio's existing AI security and Azure IAM work, while clearly distinguishing proposed integrations from implemented capabilities.

## 2. Security Objectives

The architecture aims to:

- Protect sensitive financial information processed by AI applications.
- Authenticate AI workloads through approved identity mechanisms.
- Apply least privilege to AI agents and supporting services.
- Enforce document-level access controls in RAG applications.
- Reduce prompt injection and indirect instruction risks.
- Prevent unauthorised tool execution and sensitive actions.
- Protect API credentials and application secrets.
- Monitor agent activity and investigate suspicious behaviour.
- Require human approval for high-impact operations.
- Maintain accountability and auditability across AI workflows.

## 3. AI Security Threat Landscape

AI applications introduce risks that extend beyond traditional application security.

| Threat | Potential impact |
|---|---|
| Prompt injection | Unauthorised agent behaviour |
| Indirect prompt injection | Malicious instructions embedded in retrieved content |
| Excessive agent permissions | Unauthorised resource access |
| RAG access-control failure | Exposure of confidential documents |
| Data exfiltration | Leakage of sensitive financial information |
| Credential exposure | Unauthorised API or cloud access |
| Insecure tool execution | Unapproved actions against connected systems |
| Inadequate output validation | Incorrect or unsafe information |
| Agent impersonation | Unauthorised workload activity |
| Insufficient monitoring | Delayed detection and response |

These are design threats, not claims that an attack has occurred in the portfolio environment.

## 4. Target-State AI Security Architecture

The proposed architecture separates AI users, application services, AI agents, retrieval services, data stores and connected tools.

### Main architecture components

1. Users authenticate through Microsoft Entra ID.
2. An AI application validates the user's identity and permissions.
3. The application invokes an approved AI workload.
4. The AI workload authenticates to supporting services using an approved workload identity.
5. Retrieval services enforce the user's document access permissions.
6. An agent policy engine validates proposed actions.
7. Sensitive operations require appropriate authorisation and, where necessary, human approval.
8. Security events are recorded for monitoring and investigation.

The architecture is a target-state design. It does not claim that these Azure AI integrations have been deployed.

## 5. AI Workload Identity

AI applications and supporting services should use managed identities or appropriately governed service principals.

The preferred approach is to avoid embedding long-lived credentials in application code.

Proposed controls:

- Assign a dedicated identity to each workload where practical.
- Apply permissions only to the services that workload requires.
- Scope Azure RBAC assignments narrowly.
- Separate development, testing and production identities.
- Review workload identity ownership.
- Monitor identity permission changes.
- Remove access when a workload is retired.
- Avoid sharing powerful identities between unrelated agents.

### Example identity design

| Workload | Identity approach | Access objective |
|---|---|---|
| RAG application | Managed identity | Access approved retrieval services |
| AI orchestration service | Managed identity | Invoke approved AI services |
| Document ingestion process | Dedicated workload identity | Ingest authorised documents |
| Agent execution service | Dedicated workload identity | Execute approved tools |
| Monitoring service | Dedicated monitoring identity | Read approved telemetry |

Actual permissions depend on the services and deployment architecture.

A managed identity does not automatically receive access to every Azure resource. Required permissions must be assigned explicitly.

## 6. AI Agent Authentication and Authorisation

AI agents should operate within a defined security boundary.

Each agent should have:

- A documented business purpose.
- An accountable owner.
- A defined set of permitted tools.
- A dedicated or appropriately scoped identity.
- A restricted set of accessible data sources.
- Explicit action policies.
- Monitoring and audit requirements.

The agent's ability to propose an action must not be confused with permission to execute that action.

The application or policy enforcement layer must independently validate authorisation.

## 7. RAG Data Access and Document-Level Security

Retrieval-augmented generation systems can expose sensitive information if retrieval permissions are not enforced.

The target design requires document-level access controls.

Proposed retrieval sequence:

1. Authenticate the user.
2. Establish the user's authorised groups and access context.
3. Retrieve candidate documents.
4. Apply access filters before content is provided to the language model.
5. Pass only authorised content into the model context.
6. Generate a response.
7. Record the relevant retrieval and access events.

### Financial-services examples

| Document category | Example access boundary |
|---|---|
| Finance | Approved finance personnel |
| Risk | Approved risk management personnel |
| Human Resources | Authorised HR personnel |
| Compliance | Approved compliance personnel |
| Executive | Designated executive users |
| Public policy documents | Approved general workforce access |

A retrieval filter should be enforced by trusted application logic and the underlying data access architecture, not merely by instructions in a prompt.

The model should not be treated as the security enforcement boundary.

## 8. Prompt Injection and Agent Instruction Security

Prompt injection occurs when malicious or untrusted content attempts to influence an AI model's behaviour.

Indirect prompt injection can originate from retrieved documents, webpages, emails or other external content.

Proposed controls:

- Treat retrieved content as untrusted input.
- Separate trusted system instructions from user and retrieved content.
- Never allow retrieved documents to redefine agent permissions.
- Validate tool arguments before execution.
- Restrict available tools to an explicit allowlist.
- Reject unauthorised operations.
- Apply output validation before returning sensitive results.
- Test agents against direct and indirect prompt injection scenarios.

Prompt instructions alone are not sufficient security controls.

## 9. Tool Execution and Action Governance

AI agents may connect to APIs, databases, cloud services and business applications.

These capabilities introduce additional risks.

The target architecture uses a policy enforcement layer between the agent and connected tools.

Proposed control sequence:

1. Agent proposes an action.
2. Policy engine identifies the requested tool and operation.
3. The request is validated against an allowlist.
4. Identity and authorisation are checked.
5. Parameters are validated.
6. Sensitive actions are routed for human approval.
7. Approved actions are executed.
8. Results and decisions are logged.

### Action risk classification

| Risk level | Example | Proposed control |
|---|---|---|
| Low | Read an approved public document | Automated policy validation |
| Medium | Retrieve internal financial information | Identity and document-level authorisation |
| High | Modify business records | Explicit approval and audit |
| Critical | Execute financial transfers or change privileged access | Human authorisation and independent controls |

AI-generated recommendations must not independently authorise financial transactions or privileged security changes.

## 10. Secrets and API Security

AI workloads may interact with external APIs and Azure services.

The target design should:

- Avoid hardcoded credentials.
- Use managed identities where supported.
- Store required secrets in an appropriately secured secret-management service.
- Restrict access to secrets by workload identity.
- Rotate credentials according to policy.
- Monitor credential changes and access.
- Validate API permissions and scopes.
- Apply rate limits and input validation.
- Protect sensitive information in application logs.

Azure Key Vault is a proposed component where secret storage is necessary. Its deployment is not claimed in this architecture.

## 11. AI Data Protection

Financial-services AI applications may process confidential or regulated information.

The target architecture should implement:

- Data classification before ingestion.
- Document ownership and access metadata.
- Data minimisation.
- Retention and deletion policies.
- Encryption in transit and at rest.
- Restricted retrieval access.
- Sensitive information handling.
- Output review for confidential data.
- Controlled use of external model providers.

The organisation should establish which data categories may be processed by each AI application.

## 12. AI Agent Monitoring and Threat Detection

AI workload monitoring should capture enough information to reconstruct important decisions and actions.

Relevant events include:

- User authentication.
- Agent identity authentication.
- Tool invocation.
- Policy decisions.
- Access-control failures.
- Retrieval access decisions.
- Human approval and rejection.
- API errors.
- Sensitive action attempts.
- Agent configuration changes.

Monitoring should avoid unnecessarily storing full confidential prompts, secrets or sensitive retrieved documents.

Where appropriate, logs should record identifiers, decisions and metadata rather than unrestricted copies of sensitive content.

### Proposed security alerts

| Detection | Security objective |
|---|---|
| Agent invokes an unapproved tool | Detect policy bypass attempts |
| Repeated access-denied events | Identify suspicious access attempts |
| Retrieval outside authorised document scope | Detect potential data exposure |
| Unexpected workload identity permission change | Detect privilege escalation |
| High-volume data retrieval | Identify possible exfiltration |
| Sensitive action without approval | Detect governance failure |
| Unexpected agent configuration change | Identify possible tampering |

These detections are proposed use cases, not live deployed alerts.

## 13. AI Incident Response

The organisation should maintain a dedicated response process for AI-related security events.

### Stage 1: Detection

Identify suspicious agent behaviour, policy violations or unexpected data access.

### Stage 2: Investigation

Review the user, workload identity, prompt context, retrieved data references, tool invocation and policy decisions.

### Stage 3: Containment

Depending on the risk, suspend the affected agent, revoke relevant access, disable a compromised credential or restrict a connected tool.

### Stage 4: Recovery

Validate the agent configuration, restore approved access and test relevant security policies.

### Stage 5: Evidence and Reporting

Document the event, affected data, potential business impact, actions taken and remaining risk.

### Stage 6: Lessons Learned

Improve agent policies, retrieval controls, testing and monitoring.

Containment must account for business dependencies and preserve relevant evidence.

## 14. AI Security Testing Strategy

The proposed security testing programme includes:

- Direct prompt injection testing.
- Indirect prompt injection testing.
- RAG document-level access-control testing.
- Cross-user data access testing.
- Unauthorised tool invocation testing.
- Privilege escalation testing.
- API credential exposure testing.
- Output validation testing.
- Agent policy bypass testing.
- Logging and incident response testing.

Testing should use synthetic data and controlled environments wherever possible.

## 15. Portfolio Integration

This architecture builds on the following portfolio capabilities:

| Portfolio project | Relevance |
|---|---|
| Entra Zero Trust IAM | Workforce authentication and access governance |
| Active Directory Security | Directory security and privileged access design |
| Azure Workload Identity and RBAC | Managed identity and least-privilege controls |
| Azure Identity Threat Detection | Identity monitoring, KQL and incident response |
| AI Agent Security Lab | Simulated agent policy enforcement and message validation |
| Azure Cybersecurity Architecture | Enterprise integration and governance |

The AI Agent Security Lab is a GitHub-based simulation. It is not a deployed Azure AI agent environment.

The architecture therefore combines practical identity engineering evidence with proposed AI workload security controls.

## 16. Implementation and Evidence Classification

| Capability | Portfolio status |
|---|---|
| Entra identity security foundation | Implemented and inspected |
| Azure managed identity and RBAC investigation | Performed |
| AI agent message validation simulation | Created |
| AI agent policy engine simulation | Created |
| RAG document-level access architecture | Designed |
| Azure AI managed identity integration | Proposed |
| Azure Key Vault integration | Proposed |
| Production AI agent deployment | Not deployed |
| Automated AI security monitoring | Not deployed |

## 17. Professional Relevance

This architecture demonstrates the relationship between cloud identity engineering and AI application security.

It shows how managed identities, RBAC, document-level authorisation, policy enforcement and security monitoring can be combined to protect enterprise AI systems.

For an Azure IAM Engineer or Cloud Security Engineer working with AI-enabled financial-services applications, these capabilities are increasingly relevant.

The design also connects AI and agentic AI knowledge with practical cloud security engineering principles, while maintaining clear boundaries between implemented work, simulation and future architecture.
