this as a pre-execution project specification: scope, features, attack taxonomy, datasets/log sources, RAG workflow, evaluation, dashboard, security controls, deployment, and an end-to-end brute-force use case. I’ll also verify the recommended real datasets and current implementation choices before producing the Markdown document.# AI Security Analyst  
## An Intelligent Cybersecurity Assistant Using Machine Learning and Generative AI

**Document Type:** Pre-Execution Project Specification  
**Version:** 1.0  
**Status:** Planning and Architecture Definition  
**Primary Use Case:** Network and authentication attack detection, investigation, and response assistance

---

## 1. Project Overview

The **AI Security Analyst** is an intelligent cybersecurity assistant that combines:

- Machine learning-based attack detection
- Rule-based security analytics
- Log and network-flow analysis
- Retrieval-Augmented Generation (RAG)
- MITRE ATT&CK-based threat mapping
- Real-time alerting
- Analyst dashboards
- Role-based access control
- Explainable incident investigation

The system will ingest security logs and network-flow data, identify suspicious activity, classify attack categories, create security alerts, retrieve relevant cybersecurity knowledge, and generate human-readable investigation summaries.

The system is intended to support security analysts. It should not automatically perform destructive containment actions without explicit authorization.

---

## 2. Project Objectives

### 2.1 Primary Objectives

1. Detect common cyberattack patterns from network traffic and authentication logs.
2. Classify detected events into predefined attack categories.
3. Provide explanations for machine learning predictions.
4. Map detections to MITRE ATT&CK techniques.
5. Use RAG to provide evidence-based investigation guidance.
6. Display security events through an interactive dashboard.
7. Notify users about high-priority alerts.
8. Support multiple users with authentication and role-based permissions.
9. Evaluate detection quality using security-specific metrics.
10. Demonstrate an end-to-end brute-force detection workflow using a public dataset.

### 2.2 Non-Goals

The initial version will not:

- Replace a full enterprise SIEM.
- Perform unrestricted autonomous incident response.
- Execute commands on production systems.
- Guarantee detection of previously unknown attacks.
- Use an LLM as the only detection mechanism.
- Store raw passwords, secrets, or authentication credentials.

---

## 3. Proposed System Features

### 3.1 Data Ingestion

The system will support:

- CSV network-flow files
- JSON security events
- Syslog messages
- Linux authentication logs
- Windows security logs
- Firewall logs
- Web server access logs
- IDS alerts
- Authentication-provider logs
- Manually uploaded datasets for experiments

### 3.2 Machine Learning Detection

The ML layer will provide:

- Binary classification: benign versus malicious
- Multiclass attack classification
- Anomaly detection for previously unseen behavior
- Probability scores
- Feature importance
- Detection confidence
- Class imbalance handling
- Time-window-based behavioral features

### 3.3 Rule-Based Detection

Rules will detect known patterns such as:

- Multiple failed logins from one IP
- Password spraying across many accounts
- Successful login after repeated failures
- Unusual login time
- Login from a new country or ASN
- Excessive port scanning
- High-volume denial-of-service traffic
- Suspicious HTTP request patterns
- Repeated access to restricted resources

### 3.4 Generative AI Assistant

The assistant will:

- Summarize alerts
- Explain why an event was considered suspicious
- Retrieve relevant threat intelligence
- Map events to MITRE ATT&CK
- Suggest investigation steps
- Generate incident timelines
- Answer analyst questions using indexed evidence
- Produce draft incident reports
- Recommend containment actions for analyst approval

### 3.5 Dashboard

The dashboard will show:

- Total events
- Events by severity
- Events by attack category
- Detection confidence
- Top source IP addresses
- Targeted accounts
- Login failure trends
- Geographic source distribution
- Timeline of related events
- MITRE ATT&CK technique distribution
- Alert status
- Mean time to acknowledge
- Mean time to resolve
- Model performance metrics

### 3.6 Authentication and Authorization

The system will implement:

- User registration or administrator-created accounts
- Password hashing
- Login and logout
- Session or JWT-based authentication
- Optional multi-factor authentication
- Role-based access control
- Audit logs for administrative activity
- Account lockout or rate limiting
- Password reset workflow

---

## 4. High-Level Architecture

```text
                     +----------------------+
                     |   Security Data      |
                     |   Sources             |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     | Ingestion Layer      |
                     | Syslog/API/CSV/Agent |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     | Parsing and           |
                     | Normalization         |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     | Feature Engineering  |
                     | Time Windows          |
                     | Aggregation           |
                     +----------+-----------+
                                |
                +---------------+----------------+
                |                                |
                v                                v
      +--------------------+          +--------------------+
      | Rules and Correlation|         | ML Detection       |
      | Engine               |         | Classification     |
      +----------+-----------+         +----------+---------+
                 |                                |
                 +---------------+----------------+
                                 |
                                 v
                     +----------------------+
                     | Alert and Incident   |
                     | Management           |
                     +----------+-----------+
                                |
              +-----------------+------------------+
              |                                    |
              v                                    v
   +-------------------------+          +----------------------+
   | RAG Retrieval Pipeline   |          | Dashboard and APIs   |
   | Vector DB + Knowledge    |          | Analyst Interface    |
   +------------+------------+          +----------+-----------+
                |                                  |
                +----------------+-----------------+
                                 |
                                 v
                     +----------------------+
                     | Generative AI Analyst |
                     | Explanations/Reports  |
                     +----------------------+
```

---

## 5. Recommended Technology Stack

| Layer | Recommended Technology |
|---|---|
| Frontend | React, Next.js, or Streamlit for prototype |
| Visualization | Apache ECharts, Plotly, or Recharts |
| Backend API | Python FastAPI |
| ML | scikit-learn, XGBoost, PyTorch |
| Data processing | pandas, Polars, Apache Spark for scale |
| Log parsing | Python parsers, Logstash, Fluent Bit |
| Relational database | PostgreSQL |
| Time-series storage | TimescaleDB or OpenSearch |
| Vector database | pgvector, Qdrant, Weaviate, or Milvus |
| Authentication | OAuth2/OIDC, JWT, Argon2 or bcrypt |
| Task queue | Celery or Redis Queue |
| Caching | Redis |
| Containerization | Docker |
| Deployment | Docker Compose for development; Kubernetes for production |
| Monitoring | Prometheus and Grafana |
| Model tracking | MLflow |
| Object storage | MinIO or Amazon S3-compatible storage |
| LLM integration | Approved hosted LLM or local model |
| Threat framework | MITRE ATT&CK knowledge base |

The first implementation should use a relatively small stack:

```text
React/Streamlit
FastAPI
PostgreSQL
Redis
scikit-learn/XGBoost
pgvector
Docker Compose
```

Kubernetes and distributed processing should be introduced only if the data volume requires them.

---

# 6. Datasets and Log Sources

## 6.1 Primary Public Dataset: CIC-IDS2017

The primary end-to-end demonstration dataset should be **CIC-IDS2017**, created by the Canadian Institute for Cybersecurity.

It contains labeled network-flow records representing benign traffic and multiple attack types. The dataset includes brute-force traffic such as:

- FTP-Patator
- SSH-Patator
- Web Attack - Brute Force

The dataset is appropriate for the initial project because it contains labeled attack traffic and flow-level features suitable for supervised learning. For the brute-force demonstration, the project should use the Tuesday and Thursday files containing FTP, SSH, and web brute-force activity.

### Required CIC-IDS2017 fields

Typical fields include:

- Source IP
- Destination IP
- Source port
- Destination port
- Protocol
- Flow duration
- Number of packets
- Packet lengths
- Flow bytes per second
- Flow packets per second
- Inter-arrival times
- TCP flags
- Forward and backward packet statistics
- Label

The raw dataset must be inspected before modeling because some versions contain inconsistent column names, missing values, infinity values, duplicate rows, and class-label variations.

### Dataset limitation

CIC-IDS2017 is a controlled benchmark rather than a complete representation of enterprise traffic. It should be used for repeatable evaluation and demonstration, not as the only source of evidence for production readiness.

---

## 6.2 Secondary Dataset: UNSW-NB15

The **UNSW-NB15** dataset can be used for cross-dataset validation. It was generated using the IXIA PerfectStorm tool and includes normal traffic combined with synthetic contemporary attack behaviors. It contains nine broad attack categories:

- Fuzzers
- Analysis
- Backdoors
- DoS
- Exploits
- Generic
- Reconnaissance
- Shellcode
- Worms

The dataset provides preconfigured training and testing files, as well as flow features and attack labels. ([research.unsw.edu.au](https://research.unsw.edu.au/projects/unsw-nb15-dataset?utm_source=openai))

UNSW-NB15 should not be combined blindly with CIC-IDS2017. The two datasets use different feature definitions, labels, traffic-generation methods, and attack taxonomies. The recommended approach is to train and evaluate separate models, then compare generalization behavior.

---

## 6.3 Authentication Log Sources

### Linux SSH Logs

Example source:

```text
/var/log/auth.log
/var/log/secure
```

Example events:

```text
Failed password for invalid user admin from 192.0.2.10 port 54321 ssh2
Accepted password for alice from 192.0.2.10 port 54322 ssh2
Invalid user root from 192.0.2.10 port 54323
```

Important fields:

- Timestamp
- Source IP
- Username
- Authentication result
- Service
- Destination host
- Source port
- Authentication method

### Windows Security Logs

Important event IDs:

- 4624: Successful logon
- 4625: Failed logon
- 4648: Logon with explicit credentials
- 4672: Special privileges assigned
- 4720: User account created
- 4740: User account locked out

### Cloud Identity Logs

Potential sources:

- Microsoft Entra ID sign-in logs
- Google Workspace login audit logs
- Okta system logs
- AWS CloudTrail authentication events
- GitHub audit logs

Important fields:

- User
- Source IP
- Device
- User agent
- Geographic location
- Authentication result
- MFA result
- Application
- Risk score

---

## 6.4 Network and Infrastructure Sources

The production-oriented design should support:

- Firewall logs
- VPN logs
- DNS logs
- Proxy logs
- Web server logs
- IDS/IPS alerts
- Router and switch logs
- Endpoint detection alerts
- Cloud network-flow logs
- Container runtime logs
- Kubernetes audit logs

### Example normalized event schema

```json
{
  "event_id": "evt-001",
  "timestamp": "2026-09-09T12:15:00Z",
  "source_type": "linux_auth",
  "source_ip": "192.0.2.10",
  "destination_ip": "198.51.100.20",
  "username": "admin",
  "service": "ssh",
  "action": "login",
  "outcome": "failure",
  "source_country": "US",
  "user_agent": null,
  "raw_message": "Failed password for invalid user admin",
  "tenant_id": "tenant-001"
}
```

---

# 7. Attack Categories

The initial model should detect a limited, defensible set of attack categories. Expanding the taxonomy too early will produce weak labels and difficult-to-interpret results.

## 7.1 Initial Detection Categories

| Category | Description | Example Indicators |
|---|---|---|
| Brute Force | Repeated authentication attempts against an account or service | Many failures from one IP |
| Password Spraying | One or a few passwords tried across many accounts | Many usernames targeted by one IP |
| Credential Stuffing | Use of previously compromised username-password pairs | Distributed login failures across accounts |
| Port Scanning | Probing many ports or hosts | High destination-port diversity |
| DoS/DDoS | Traffic intended to exhaust resources | High packet rate or connection volume |
| Web Attack | Malicious requests targeting web applications | SQL injection, XSS, web brute force |
| Reconnaissance | Discovery and enumeration activity | Host scanning, service discovery |
| Exploitation | Attempts to exploit vulnerabilities | Suspicious payloads or exploit signatures |
| Malware/Command and Control | Suspicious outbound communication | Periodic connections, known indicators |
| Data Exfiltration | Unauthorized transfer of data | Large outbound transfer to unusual destination |
| Privilege Abuse | Suspicious use of privileged accounts | New admin activity, unusual privilege assignment |
| Anomalous Activity | Behavior that differs significantly from baseline | Novel login location or traffic pattern |

## 7.2 MITRE ATT&CK Mapping

The system should map detections to ATT&CK techniques rather than inventing unsupported technique names.

For brute-force detection, the primary technique is:

```text
T1110 - Brute Force
```

Sub-techniques include:

```text
T1110.001 - Password Guessing
T1110.002 - Password Cracking
T1110.003 - Password Spraying
T1110.004 - Credential Stuffing
```

MITRE describes brute force as repeated password guessing against accounts or password hashes and provides detection guidance for authentication failures, password spraying, and failed logins followed by success. ([attack.mitre.org](https://attack.mitre.org/techniques/T1110/?utm_source=openai))

---

# 8. Machine Learning Architecture

## 8.1 Detection Strategy

The project should use a hybrid model:

```text
Rule-based correlation
        +
Supervised ML classification
        +
Unsupervised anomaly detection
        +
Analyst feedback
```

A single model should not be responsible for all detection behavior.

## 8.2 Model Types

### Baseline Model

- Logistic Regression
- Decision Tree
- Random Forest

### Primary Tabular Model

- XGBoost or LightGBM
- Random Forest as an interpretable comparison model

### Anomaly Model

- Isolation Forest
- One-Class SVM
- Autoencoder, if sufficient data is available

### Sequence Model

Optional future implementation:

- LSTM
- Temporal convolutional network
- Transformer-based event sequence model

The first version should prioritize Random Forest or XGBoost because they are effective for tabular security data and provide usable feature importance.

## 8.3 Feature Engineering

### Network-flow features

- Flow duration
- Total forward packets
- Total backward packets
- Total bytes
- Average packet size
- Packet-rate statistics
- Byte-rate statistics
- TCP flag counts
- Destination-port entropy
- Number of unique destinations
- Number of unique source ports
- Flow inter-arrival time

### Authentication features

For a rolling time window:

- Failed login count
- Successful login count
- Failure-to-success ratio
- Unique usernames targeted
- Unique source IPs
- Unique destination hosts
- Login attempts per minute
- Number of invalid users
- Time since previous attempt
- Geographic distance from previous login
- New device indicator
- New country indicator
- MFA failure count
- Account lockout count

### Example brute-force feature record

```json
{
  "source_ip": "192.0.2.10",
  "destination_host": "server-01",
  "service": "ssh",
  "window_seconds": 300,
  "failed_attempts": 47,
  "successful_attempts": 0,
  "unique_usernames": 5,
  "unique_destination_ports": 1,
  "invalid_user_ratio": 0.8,
  "attempts_per_minute": 9.4,
  "is_new_source_ip": true,
  "label": "brute_force"
}
```

## 8.4 Data Preprocessing

Required steps:

1. Remove duplicate rows.
2. Normalize column names.
3. Convert timestamps to UTC.
4. Handle missing values.
5. Replace infinite numerical values.
6. Encode categorical features.
7. Scale features where required.
8. Remove leakage-prone fields.
9. Check class distribution.
10. Separate training and testing data by time or scenario.
11. Store preprocessing artifacts with the model.
12. Version datasets and feature definitions.

## 8.5 Data Leakage Prevention

The project must avoid:

- Randomly mixing records from the same attack flow into training and testing.
- Using the target label as a feature.
- Creating rolling features using future events.
- Fitting preprocessing transformers on the full dataset.
- Reporting metrics from the training data.
- Tuning thresholds on the final test set.

Time-based or scenario-based splits are preferred for security detection.

---

# 9. Complete RAG Pipeline

## 9.1 Purpose of RAG

The RAG system should not independently decide whether an event is malicious. Its purpose is to provide contextual assistance after the detection layer has identified a suspicious event.

The RAG system should answer questions such as:

- What does this attack category mean?
- What ATT&CK technique is associated with this alert?
- What evidence supports the detection?
- What investigation steps should an analyst perform?
- What containment actions are commonly recommended?
- What related events should be searched for?

## 9.2 RAG Knowledge Sources

The initial knowledge base should include:

- MITRE ATT&CK techniques
- MITRE ATT&CK software and groups, where relevant
- Internal incident response playbooks
- Security operating procedures
- Detection rule documentation
- Dataset documentation
- System architecture documentation
- Approved threat intelligence reports
- NIST incident response guidance
- Known false-positive explanations
- Organization-specific asset and identity context

NIST SP 800-61 Rev. 3 provides current incident-response recommendations aligned with the NIST Cybersecurity Framework 2.0 and supersedes Rev. 2. ([csrc.nist.gov](https://csrc.nist.gov/pubs/sp/800/61/r3/final?utm_source=openai))

## 9.3 Document Ingestion

```text
Documents
   |
   v
File collection
   |
   v
Text extraction
   |
   v
Cleaning and normalization
   |
   v
Metadata extraction
   |
   v
Chunking
   |
   v
Embedding generation
   |
   v
Vector database
```

Each chunk should contain metadata such as:

```json
{
  "document_id": "mitre-t1110",
  "source": "MITRE ATT&CK",
  "document_type": "technique",
  "technique_id": "T1110",
  "version": "current",
  "access_level": "analyst",
  "last_updated": "2026-05-12"
}
```

## 9.4 Chunking Strategy

Recommended starting values:

- Chunk size: 400 to 800 tokens
- Overlap: 50 to 100 tokens
- Preserve headings
- Keep attack descriptions and mitigation sections together
- Avoid splitting tables where possible
- Store source and section metadata

## 9.5 Embedding and Indexing

The indexing process:

1. Convert documents into normalized text.
2. Split text into chunks.
3. Generate embeddings.
4. Store embeddings in a vector database.
5. Store metadata alongside each vector.
6. Create keyword indexes for exact terms such as `T1110`, IP addresses, CVEs, and usernames.
7. Re-index when source documents change.

## 9.6 Query Pipeline

```text
Analyst question or alert
          |
          v
Query normalization
          |
          v
Entity extraction
(IP, user, host, technique, alert ID)
          |
          v
Hybrid retrieval
(vector similarity + keyword search + filters)
          |
          v
Result reranking
          |
          v
Context window construction
          |
          v
LLM prompt
          |
          v
Grounded answer
          |
          v
Citations and evidence
```

## 9.7 Alert-Aware Retrieval

For an alert, the system should automatically build a retrieval query from:

- Alert category
- Model prediction
- Top features
- Source IP
- Destination host
- Username
- Service
- Timestamp
- MITRE technique
- Related alert IDs
- Detection rule ID

Example retrieval query:

```text
Investigate suspected SSH brute force activity from source IP 192.0.2.10
targeting server-01. There were 47 failed attempts against five usernames
within five minutes, followed by no successful login. Provide relevant ATT&CK
techniques, validation steps, and recommended containment actions.
```

## 9.8 Grounding Requirements

The LLM must:

- Distinguish evidence from inference.
- Include the alert data used for its conclusion.
- Cite retrieved documents.
- State when evidence is insufficient.
- Avoid inventing IP reputation, user identity, or compromise status.
- Avoid claiming that an attack succeeded unless a successful event is present.
- Never expose secrets or sensitive credentials.

## 9.9 RAG Evaluation

The RAG component should be evaluated separately from the ML classifier.

Metrics:

- Retrieval precision at K
- Retrieval recall at K
- Mean reciprocal rank
- Citation correctness
- Faithfulness
- Answer relevance
- Unsupported-claim rate
- Analyst acceptance rate
- Time saved per investigation

---

# 10. Brute-Force Detection Use Case

## 10.1 Objective

Detect repeated authentication attempts against SSH or FTP services and generate an explainable alert.

## 10.2 Dataset

Use CIC-IDS2017 flow files containing:

- FTP-Patator
- SSH-Patator
- Web Attack - Brute Force

The detection pipeline should preserve both the original label and the normalized project label.

Example mapping:

```text
FTP-Patator                 -> brute_force
SSH-Patator                 -> brute_force
Web Attack - Brute Force   -> web_brute_force
BENIGN                      -> benign
```

## 10.3 Processing Workflow

```text
CIC-IDS2017 CSV
       |
       v
Load and validate columns
       |
       v
Clean missing and infinite values
       |
       v
Normalize labels
       |
       v
Split data by time or scenario
       |
       v
Train baseline model
       |
       v
Evaluate on untouched test set
       |
       v
Generate prediction
       |
       v
Create alert
       |
       v
Retrieve MITRE and playbook context
       |
       v
Generate analyst explanation
       |
       v
Display alert in dashboard
```

## 10.4 Example Detection Logic

A rule-based correlation can complement the model:

```python
if failed_attempts >= 20 and window_seconds <= 300:
    if unique_usernames <= 3:
        category = "brute_force"
    elif unique_usernames >= 10:
        category = "password_spraying"
```

A model-based decision may use:

```text
Alert if:
  P(brute_force) >= 0.85
  OR
  rule_score >= high-risk threshold
  OR
  model_score >= medium threshold and rule evidence confirms activity
```

The exact threshold must be selected using validation data and operational cost analysis.

## 10.5 Example Alert

```json
{
  "alert_id": "ALT-2026-0001",
  "severity": "high",
  "category": "brute_force",
  "mitre_technique": "T1110",
  "source_ip": "192.0.2.10",
  "destination_host": "server-01",
  "service": "ssh",
  "failed_attempts": 47,
  "unique_usernames": 5,
  "confidence": 0.96,
  "status": "open",
  "evidence": [
    "High failed-login volume within a five-minute window",
    "Repeated attempts from a single source",
    "Multiple targeted usernames",
    "Model classified the event as brute force"
  ],
  "recommended_actions": [
    "Validate whether the source IP belongs to an approved administrator",
    "Review successful logins from the same source",
    "Check whether any targeted accounts were locked",
    "Apply temporary rate limiting or blocking according to policy"
  ]
}
```

## 10.6 Generated Analyst Explanation

```text
The activity is classified as probable SSH brute force because 47 failed
authentication attempts originated from 192.0.2.10 within five minutes and
targeted five usernames on server-01. The model confidence is 96 percent.
No successful login was observed in the current event window. This behavior
maps to MITRE ATT&CK T1110, Brute Force.

Recommended validation:
1. Confirm whether the source IP is an authorized administrative address.
2. Search for successful SSH logins from the same IP.
3. Review account-lockout events.
4. Check whether the source attempted other hosts or services.
```

---

# 11. Alerting and Incident Management

## 11.1 Alert Severity

| Severity | Meaning | Example |
|---|---|---|
| Informational | Context only | Single failed login |
| Low | Suspicious but limited evidence | Small number of failures |
| Medium | Probable malicious activity | Repeated failures across accounts |
| High | Strong evidence or successful compromise risk | Brute force followed by success |
| Critical | Active or confirmed major incident | Privileged account compromise |

## 11.2 Alert Lifecycle

```text
New
  |
  v
Acknowledged
  |
  v
Investigating
  |
  +--> False Positive
  |
  +--> Confirmed Incident
             |
             v
        Containment
             |
             v
        Remediation
             |
             v
        Closed
```

## 11.3 Alert Channels

Support:

- In-dashboard notifications
- Email
- Slack or Microsoft Teams
- Webhooks
- PagerDuty or equivalent
- SIEM forwarding
- Optional SMS for critical alerts

## 11.4 Alert Deduplication

The system should group repeated alerts using:

- Same source IP
- Same target host
- Same account
- Same detection category
- Time window
- Related event identifiers

This prevents analysts from receiving hundreds of duplicate alerts for one attack campaign.

---

# 12. Authentication and User Roles

## 12.1 Roles

### Administrator

Permissions:

- Manage users
- Assign roles
- Configure detection thresholds
- Configure data sources
- View audit logs
- Manage system configuration

### Security Analyst

Permissions:

- View alerts
- Investigate incidents
- Query logs
- Use the AI assistant
- Change alert status
- Add analyst notes
- Export reports

### Incident Responder

Permissions:

- View high-priority incidents
- Add containment actions
- Manage response workflows
- Approve response recommendations

### Auditor

Permissions:

- Read-only access
- View audit history
- Export compliance reports
- Cannot modify alerts or users

### Viewer

Permissions:

- View dashboards
- Cannot view sensitive raw logs unless explicitly allowed
- Cannot modify system state

## 12.2 Security Requirements

- Passwords must be hashed using Argon2id or bcrypt.
- JWTs must have short expiration periods.
- Refresh tokens must be revocable.
- Sessions must be invalidated on logout.
- Rate limiting must protect login endpoints.
- Failed login attempts must be audited.
- Sensitive fields must be redacted from logs.
- Tenant boundaries must be enforced at the API and database layers.
- Administrative actions must be recorded.
- Secrets must be stored outside source code.

---

# 13. Dashboard Design

## 13.1 Main Dashboard

Recommended visualizations:

1. Alert count by severity
2. Attack category distribution
3. Detection trend over time
4. Top source IPs
5. Top targeted accounts
6. Top targeted services
7. Geographic attack map
8. Alert status funnel
9. Model confidence distribution
10. MITRE ATT&CK technique matrix

## 13.2 Investigation View

Each alert should have:

- Alert summary
- Severity
- Confidence score
- Timeline
- Source and destination information
- Raw event records
- Related alerts
- Model explanation
- Top contributing features
- MITRE technique mapping
- RAG-generated investigation guidance
- Analyst notes
- Response actions
- Evidence export

## 13.3 Brute-Force Visualization

The brute-force view should include:

- Failed attempts over time
- Source IP-to-account relationship graph
- Number of targeted accounts
- Attempts per minute
- Successful versus failed logins
- Source country and ASN
- First-seen and last-seen timestamps
- Authentication heatmap
- Account lockouts
- Follow-on activity after successful login

## 13.4 Model Monitoring View

Display:

- Precision
- Recall
- F1-score
- ROC-AUC
- PR-AUC
- False-positive rate
- False-negative rate
- Class distribution
- Prediction confidence
- Feature drift
- Data drift
- Alert volume over time
- Model version
- Last training date

---

# 14. Evaluation Metrics

## 14.1 Classification Metrics

Accuracy should not be the primary metric because security datasets are often imbalanced.

Required metrics:

- Precision
- Recall
- F1-score
- Macro F1-score
- Weighted F1-score
- ROC-AUC
- PR-AUC
- Confusion matrix
- Balanced accuracy
- Matthews correlation coefficient

### Definitions

```text
Precision = TP / (TP + FP)

Recall = TP / (TP + FN)

F1 = 2 * Precision * Recall / (Precision + Recall)

False Positive Rate = FP / (FP + TN)

False Negative Rate = FN / (FN + TP)
```

## 14.2 Security Operations Metrics

- False positives per day
- False negatives in test scenarios
- Alerts per analyst per hour
- Mean time to detect
- Mean time to acknowledge
- Mean time to investigate
- Mean time to contain
- Alert deduplication rate
- Analyst acceptance rate
- Percentage of alerts with sufficient evidence
- Percentage of alerts mapped to an ATT&CK technique

## 14.3 Threshold Evaluation

The model threshold should be selected based on operational costs:

```text
Expected Cost =
  False Positive Cost * FP
  +
  False Negative Cost * FN
```

For brute-force detection, missing a successful compromise is usually more expensive than investigating a small number of suspicious login patterns. However, overly aggressive thresholds can cause alert fatigue and account lockout denial-of-service conditions.

MITRE specifically notes that account lockout policies must be designed carefully because excessively strict policies can make accounts unusable. ([attack.mitre.org](https://attack.mitre.org/techniques/T1110/?utm_source=openai))

---

# 15. Performance Testing

## 15.1 API Performance

Test:

- Event ingestion throughput
- Alert creation latency
- Dashboard API latency
- Authentication latency
- Search latency
- RAG response latency
- Concurrent analyst sessions

Target initial values:

| Test | Initial Target |
|---|---:|
| Event ingestion | 100 events/second |
| Alert generation latency | Less than 5 seconds |
| Dashboard API p95 | Less than 500 ms |
| Log search p95 | Less than 2 seconds |
| RAG response time | Less than 10 seconds |
| Concurrent users | 25 |
| Authentication response p95 | Less than 500 ms |

These are initial engineering targets and must be validated against the selected hardware.

## 15.2 ML Performance

Measure:

- Batch prediction throughput
- Single-event prediction latency
- Feature-engineering latency
- Model memory usage
- CPU and GPU utilization
- Training duration
- Inference error rate

## 15.3 Load Testing

Use tools such as:

- Locust
- k6
- Apache JMeter
- pytest-benchmark

Test scenarios:

1. Normal traffic ingestion.
2. Sudden alert spike.
3. Many concurrent dashboard users.
4. Large historical search.
5. Simultaneous RAG requests.
6. Database restart and recovery.
7. Redis or queue backlog.
8. Duplicate event ingestion.

## 15.4 Reliability Testing

Required tests:

- Restart services during ingestion.
- Reprocess a data file.
- Verify idempotency.
- Simulate database failure.
- Simulate vector database failure.
- Verify alert persistence.
- Verify no event loss during retry.
- Verify audit logs remain available.
- Verify role restrictions after token expiration.

---

# 16. Explainability Requirements

Each ML alert must provide:

- Predicted class
- Confidence score
- Top contributing features
- Relevant threshold
- Model version
- Dataset or source type
- Supporting rule matches
- Related events
- Limitations of the prediction

Example:

```json
{
  "model_version": "bruteforce-xgb-1.0.0",
  "prediction": "brute_force",
  "confidence": 0.96,
  "top_features": [
    {
      "name": "failed_attempts",
      "value": 47,
      "importance": 0.31
    },
    {
      "name": "attempts_per_minute",
      "value": 9.4,
      "importance": 0.22
    },
    {
      "name": "unique_usernames",
      "value": 5,
      "importance": 0.15
    }
  ]
}
```

SHAP can be used for tree-based models, but explanations should be presented as contributing evidence rather than proof of causality.

---

# 17. Deployment Architecture

## 17.1 Development Deployment

```text
Docker Compose
  |
  +-- Frontend
  +-- FastAPI backend
  +-- PostgreSQL
  +-- Redis
  +-- ML service
  +-- RAG service
  +-- Worker service
  +-- Object storage
```

## 17.2 Production Deployment

```text
                         Internet
                            |
                     WAF / Load Balancer
                            |
                     API Gateway / Ingress
                            |
       +--------------------+--------------------+
       |                    |                    |
       v                    v                    v
  Frontend Pods       API Pods            Auth Service
                            |
              +-------------+-------------+
              |                           |
              v                           v
       Detection Workers             RAG Workers
              |                           |
       +------+------+              +-----+------+
       |             |              |            |
       v             v              v            v
  PostgreSQL     OpenSearch      Vector DB    Redis
       |
       v
 Object Storage / Model Registry
```

## 17.3 Security Controls

- TLS for all network communication
- Network segmentation
- Private database subnets
- Firewall rules
- Container image scanning
- Dependency scanning
- Secrets manager
- Database encryption at rest
- Audit logs
- Backup and recovery
- Least-privilege service accounts
- Read-only access to raw evidence where appropriate
- Prompt and response logging with sensitive-data redaction

## 17.4 Deployment Environments

Use separate environments:

```text
Development
    |
    v
Testing
    |
    v
Staging
    |
    v
Production
```

Models must not be promoted to production without:

- Evaluation results
- Versioned artifacts
- Approval
- Rollback plan
- Monitoring configuration

---

# 18. Database Design

## 18.1 Core Tables

### Users

```text
users
- id
- email
- password_hash
- role_id
- is_active
- created_at
- last_login_at
```

### Roles

```text
roles
- id
- name
- permissions
```

### Events

```text
events
- id
- timestamp
- source_type
- source_ip
- destination_ip
- username
- service
- action
- outcome
- raw_event
- normalized_event
- tenant_id
```

### Alerts

```text
alerts
- id
- category
- severity
- confidence
- status
- mitre_technique
- source_ip
- destination_host
- first_seen
- last_seen
- evidence
- model_version
- created_at
```

### Incidents

```text
incidents
- id
- title
- severity
- status
- owner_id
- description
- timeline
- resolution
- created_at
- closed_at
```

### Audit Logs

```text
audit_logs
- id
- user_id
- action
- resource_type
- resource_id
- ip_address
- timestamp
- metadata
```

---

# 19. API Design

## 19.1 Authentication

```text
POST /api/auth/login
POST /api/auth/logout
POST /api/auth/refresh
POST /api/auth/password-reset
GET  /api/auth/me
```

## 19.2 Events

```text
POST /api/events
POST /api/events/bulk
GET  /api/events
GET  /api/events/{event_id}
```

## 19.3 Alerts

```text
GET   /api/alerts
GET   /api/alerts/{alert_id}
PATCH /api/alerts/{alert_id}
POST  /api/alerts/{alert_id}/acknowledge
POST  /api/alerts/{alert_id}/close
```

## 19.4 AI Assistant

```text
POST /api/assistant/query
POST /api/assistant/investigate/{alert_id}
POST /api/assistant/report/{incident_id}
```

## 19.5 Models

```text
GET /api/models
GET /api/models/{model_id}/metrics
POST /api/models/{model_id}/predict
```

---

# 20. Testing Strategy

## 20.1 Unit Tests

Test:

- Log parsers
- Feature extraction
- Label normalization
- Alert severity logic
- Role checks
- RAG query construction
- Data validation
- API schemas

## 20.2 Integration Tests

Test:

- Ingestion to database
- Database to feature pipeline
- Feature pipeline to model
- Model to alert
- Alert to dashboard
- Alert to RAG explanation
- Authentication to protected endpoint

## 20.3 Security Tests

Test:

- Invalid login attempts
- Token expiration
- Privilege escalation
- IDOR vulnerabilities
- SQL injection
- Cross-site scripting
- CSRF
- SSRF
- Prompt injection
- Sensitive-data leakage
- Unauthorized alert modification

## 20.4 ML Tests

Test:

- Missing values
- Unseen categories
- Distribution shift
- Class imbalance
- Prediction stability
- Threshold changes
- Model serialization
- Feature-order mismatch
- Data leakage

---

# 21. Project Workflow

## Phase 1: Requirements and Data Validation

Deliverables:

- Final attack taxonomy
- Dataset inventory
- Log schema
- Data dictionary
- System requirements
- Threat model
- Privacy requirements

## Phase 2: Data Pipeline

Deliverables:

- Dataset download process
- Data cleaning scripts
- Normalized event schema
- Feature engineering pipeline
- Data quality report

## Phase 3: Baseline ML Model

Deliverables:

- Baseline classifier
- Train/test methodology
- Confusion matrix
- Evaluation report
- Model artifact
- Prediction API

## Phase 4: Detection and Alerting

Deliverables:

- Rule engine
- Correlation logic
- Alert schema
- Severity calculation
- Deduplication
- Alert lifecycle

## Phase 5: RAG Assistant

Deliverables:

- Knowledge ingestion pipeline
- Vector database
- Hybrid retrieval
- Alert-aware prompts
- Citation support
- Hallucination evaluation

## Phase 6: Dashboard

Deliverables:

- Login screen
- Role-aware navigation
- SOC overview
- Alert investigation page
- Brute-force analysis page
- Model monitoring page
- Incident report export

## Phase 7: Deployment

Deliverables:

- Docker Compose environment
- Production architecture
- Environment configuration
- Monitoring
- Backup strategy
- Security hardening
- Deployment documentation

## Phase 8: End-to-End Demonstration

Deliverables:

- CIC-IDS2017 brute-force experiment
- Trained model
- Generated alert
- RAG investigation response
- Dashboard screenshots
- Evaluation report
- Performance report

---

# 22. Acceptance Criteria

The project will be considered ready for demonstration when:

- The system can ingest CIC-IDS2017 data.
- The data pipeline produces validated features.
- The model detects benign and brute-force traffic.
- The model evaluation uses an untouched test set.
- Precision, recall, F1-score, PR-AUC, and confusion matrix are reported.
- A brute-force prediction creates an alert.
- The alert is mapped to MITRE ATT&CK T1110.
- The RAG pipeline retrieves relevant evidence.
- The assistant produces an explanation grounded in the alert data.
- The dashboard displays attack trends and alert details.
- Authentication is enforced.
- Roles restrict protected operations.
- Alert notifications are generated.
- The application runs with Docker Compose.
- At least one load test and one failure-recovery test are completed.
- The system records audit events.
- The project documents limitations and false-positive risks.

---

# 23. Risks and Mitigations

| Risk | Mitigation |
|---|---|
| Dataset does not represent production traffic | Use multiple datasets and later validate on local logs |
| Severe class imbalance | Use PR-AUC, class weights, threshold tuning, and resampling |
| Data leakage | Use time- or scenario-based splitting |
| High false-positive rate | Combine ML with rules and analyst feedback |
| LLM hallucination | Use RAG citations, structured prompts, and evidence checks |
| Prompt injection in logs | Treat logs as untrusted input and isolate instructions from data |
| Sensitive data exposure | Redaction, access control, encryption, and retention policy |
| Alert fatigue | Deduplication, severity thresholds, and alert grouping |
| Model drift | Monitor feature distributions and retrain on approved data |
| Account lockout abuse | Use careful response controls and human approval |
| RAG retrieval failure | Use hybrid retrieval and fallback responses |
| Unauthorized automated response | Require explicit approval for containment actions |

---

# 24. Recommended Initial Repository Structure

```text
ai-security-analyst/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── auth/
│   │   ├── alerts/
│   │   ├── events/
│   │   ├── models/
│   │   ├── rag/
│   │   └── main.py
│   └── tests/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── charts/
│   │   └── services/
│   └── tests/
├── data/
│   ├── raw/
│   ├── processed/
│   └── samples/
├── ml/
│   ├── notebooks/
│   ├── pipelines/
│   ├── training/
│   ├── evaluation/
│   └── artifacts/
├── rag/
│   ├── ingestion/
│   ├── chunking/
│   ├── retrieval/
│   └── evaluation/
├── deployment/
│   ├── docker/
│   ├── compose/
│   └── kubernetes/
├── docs/
│   ├── architecture.md
│   ├── data-sources.md
│   ├── model-evaluation.md
│   └── incident-response.md
├── scripts/
├── .env.example
├── docker-compose.yml
└── README.md
```

---

# 25. Final Recommended Scope

The first implementation should focus on a reliable, demonstrable vertical slice:

```text
CIC-IDS2017
   |
   v
Data cleaning
   |
   v
Brute-force classification
   |
   v
Alert generation
   |
   v
MITRE ATT&CK mapping
   |
   v
RAG investigation explanation
   |
   v
Dashboard visualization
   |
   v
Authenticated analyst workflow
```

After this workflow is stable, expand to:

1. Linux SSH authentication logs.
2. Password spraying detection.
3. Windows logon events.
4. UNSW-NB15 cross-dataset validation.
5. Additional attack categories.
6. Model drift monitoring.
7. Analyst feedback-based retraining.
8. Production log connectors.
9. Human-approved response automation.

The key design principle is to keep **detection, evidence retrieval, and generative explanation as separate components**. The ML model should identify suspicious behavior, the correlation engine should combine events, the RAG system should retrieve supporting knowledge, and the generative model should explain the result without becoming the sole source of truth.
