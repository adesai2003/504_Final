# Use Case: Remote Monitoring Pipeline for Heart Failure Patients

## 1.Link to Mermaid Diagram
[![](https://mermaid.ink/img/pako:eNqVVG1r2zAQ_itC0JFA3h2ns9kKabJARtJmCXSweR9k--KIOpKR5LVZ6H_fWfJSVigkwYS7k557fU5HmsgUaEi3uXxKdkwZslhHIhKEXF2Rz-_96vOZksKASEmXjJMEtCYLdgB1DnrFDAdhfka0lshSxjwHMi6KiP6qroxXczzGf_JFpIXkwnyKVfdmljP9WB0SKcgkl2VK1qWwmDPTnsh9URpwyZLGBtRvUDmm3zzHwVxkoKvEXexZKRLDpdA2uSkzrL6BNvKBPLCcp6xS6qrWJUZ6B23PsNqMC0DsOAdluMhq5HeITzis2CImORc84UyQKdO7WDKVksYEp8LQhWpe0hWb-tnz2xipWAanhGrdlcGeyNfN_R3ZgNBSWc91DdPbV8S3BWmspDaZApSbFrpkAr2kFhIzDZcUMJ5XPBQsPxieaNK4L6rOsvysqY4rrj1gv-EZZVcGR55tEqlwBMjOFPJLsrmThm95Ykevz8FYwOHUnlUZdzdlbDOxRCBLZCg2R1881Bnu9gU7Sdrtm2q_6h20qqO0C-tka66n_sY6vX1jsLx2YEdxG2H-v-5Qr7rrh0Mh9U93IkFbNFM8paFRJbToHtSeVSo9VncjanawR-aEKKZMPUY0Ei-IKZj4IeX-H0zJMtvRcMtyjVpZ4JbClLNMsf3JqvBxAzWRpTA0HPR6I-uFhkf6TMP2x57X8YaBF_h-4PVGQ69FDzTsDwYd3-sNvX5wHfie3x--tOgfG7jfGfnB4DoIhgO_X32DFoWUYxOX7hW2j_HLX1SnqMw?type=png)](https://mermaid.live/edit#pako:eNqVVG1r2zAQ_itC0JFA3h2ns9kKabJARtJmCXSweR9k--KIOpKR5LVZ6H_fWfJSVigkwYS7k557fU5HmsgUaEi3uXxKdkwZslhHIhKEXF2Rz-_96vOZksKASEmXjJMEtCYLdgB1DnrFDAdhfka0lshSxjwHMi6KiP6qroxXczzGf_JFpIXkwnyKVfdmljP9WB0SKcgkl2VK1qWwmDPTnsh9URpwyZLGBtRvUDmm3zzHwVxkoKvEXexZKRLDpdA2uSkzrL6BNvKBPLCcp6xS6qrWJUZ6B23PsNqMC0DsOAdluMhq5HeITzis2CImORc84UyQKdO7WDKVksYEp8LQhWpe0hWb-tnz2xipWAanhGrdlcGeyNfN_R3ZgNBSWc91DdPbV8S3BWmspDaZApSbFrpkAr2kFhIzDZcUMJ5XPBQsPxieaNK4L6rOsvysqY4rrj1gv-EZZVcGR55tEqlwBMjOFPJLsrmThm95Ykevz8FYwOHUnlUZdzdlbDOxRCBLZCg2R1881Bnu9gU7Sdrtm2q_6h20qqO0C-tka66n_sY6vX1jsLx2YEdxG2H-v-5Qr7rrh0Mh9U93IkFbNFM8paFRJbToHtSeVSo9VncjanawR-aEKKZMPUY0Ei-IKZj4IeX-H0zJMtvRcMtyjVpZ4JbClLNMsf3JqvBxAzWRpTA0HPR6I-uFhkf6TMP2x57X8YaBF_h-4PVGQ69FDzTsDwYd3-sNvX5wHfie3x--tOgfG7jfGfnB4DoIhgO_X32DFoWUYxOX7hW2j_HLX1SnqMw)


- “This architecture uses a Flask API deployed on Cloud Run for access, Cloud Functions for serverless processing, Cloud Storage and Cloud SQL for data persistence, and Vertex AI for optional analytics.”

## Overview

This architecture implements a small, serverless healthcare data pipeline on Google Cloud to support remote monitoring of susceptible heart failure patients. The system intakes daily patient vitals and symptom data, stores both raw and processed data, applies simple rules and optional analytics to detect concerning trends, and presents results to clinicians through a web interface. The design emphasizes clarity, scalability, and alignment with services covered in prior coursework.

---

## Service Mapping

The table below maps each major cloud service to its role in the solution and connects it to relevant course assignments or modules.

| Layer | Service (Google Cloud) | Role in Solution | Related Assignment / Module |
|------|------------------------|------------------|-----------------------------|
| Storage | Cloud Storage | Store raw JSON sensor and symptom data exactly as received for auditing and replay | Cloud Storage / Object Storage module |
| Compute | Cloud Run | Run containerized Flask applications for API access and clinician dashboard | Containers / Cloud Run module |
| Compute | Cloud Functions | Perform serverless ingestion, validation, and rules-based alert processing | Serverless Functions module |
| Database / SQL | Cloud SQL (PostgreSQL) | Store structured patient data, recent vitals, alert states, and clinician actions | Managed SQL Databases module |
| Analytics / AI (optional) | Vertex AI | Run a simple analytics or risk scoring model based on recent patient trends | AI / ML or Analytics module |

---

## Data Flow Narrative

The system operates as an event-driven pipeline with clear separation between intake, storage, processing, and presentation.

**Step 1: Patient data submission**  
Heart failure patients use a mobile application to submit daily vitals (e.g., weight, blood pressure, heart rate) and symptom check-ins. The app sends this data as a JSON payload over HTTPS to a Flask API deployed on Cloud Run.

**Step 2: Ingestion and validation**  
The Flask API forwards incoming requests to a Cloud Function responsible for intake. This function validates the payload structure, normalizes measurement units, and ensures required fields are present.

**Step 3: Raw and structured storage**  
The ingestion function writes the original JSON payload to Cloud Storage as raw data for auditing purposes. In parallel, cleaned and normalized values are written to Cloud SQL, where they are stored in structured tables optimized for querying.

**Step 4: Rules-based processing and analytics**  
After new data is stored, a Cloud Function evaluates the latest measurements against simple clinical rules (for example, rapid weight gain over several days). Additionally, the function calls a Vertex AI model to generate a basic risk score based on recent trends.

**Step 5: Alert generation**  
If thresholds are exceeded or risk scores are elevated, the system creates or updates an alert record in Cloud SQL. These alerts include the trigger reason and timestamp for clinician review.

**Step 6: Clinician access and visualization**  
Clinicians access a web dashboard hosted on Cloud Run. The dashboard queries Cloud SQL to display active alerts, recent vitals, and trend summaries, supporting triage and follow-up actions.

---

## Security, Identity, and Governance Basics

Credentials and secrets (such as database passwords) are managed using environment variables and Google Cloud’s built-in service identity mechanisms rather than hard-coded values. Cloud Run services and Cloud Functions use dedicated service accounts with least-privilege permissions, allowing them to access only the specific resources they require.

Access controls are role-based. Patients authenticate only to submit their own data, while clinicians authenticate to view assigned patient information through the dashboard. From a governance perspective, the system avoids using real Protected Health Information (PHI) in development or student environments by relying on synthetic or anonymized data. Raw data is stored in private Cloud Storage buckets with no public access, and all services communicate over encrypted channels.

---

## Cost and Operational Considerations

The primary cost drivers in this architecture are compute usage on Cloud Run, database storage and queries in Cloud SQL, and any usage of Vertex AI for analytics. To keep costs low, the system relies heavily on serverless services such as Cloud Run and Cloud Functions, which scale to zero when idle and do not require always-on virtual machines.

Cloud Storage is used for raw data because it is inexpensive and well-suited for infrequently accessed files. Batch analytics and model execution are optional and can be limited to small datasets or scheduled runs to remain within free-tier or student budget constraints. Overall, the design prioritizes managed and serverless services to minimize operational overhead and cost while still demonstrating realistic cloud architecture patterns.

---


