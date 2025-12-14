# Use Case: Remote Monitoring Pipeline for Heart Failure Patients

## 1. Use Case Description

### Problem Statement

Heart failure (HF) is a chronic condition that frequently worsens gradually before leading to emergency department visits or hospital readmissions. Early signs of deterioration, such as sudden weight gain, changes in blood pressure, increased heart rate, or worsening symptoms can often be detected days in advance if patients are consistently monitored at home. However, many clinics lack an integrated, scalable system to collect daily patient measurements, analyze trends, and add only the most medically relevant alerts to care teams.

This use case describes a cloud-based remote monitoring mini-pipeline designed for heart failure patients enrolled in an outpatient ambulatory care program. The system enables patients to submit daily vital signs and symptom check-ins, store the relevant data securely, and processes it to generate clear summaries and alerts for clinicians. The goal of this use case is to support earlier interventions, reduce hospital readmissions, and improve quality of care.

**Primary users**
- **Patients**: Heart failure prone patients submitting daily vitals and symptom surveys via a mobile app or connected devices.
- **Nurses / Care Coordinators**: Review alerts and monitor trends for susceptable patients.
- **Clinicians (Cardiologists, PCPs)**: Review escalated cases and summarized patient status.
- **Clinic Administrators / IT Staff**: Manage patient enrollment, devices, and system oversight.

---

### Data Sources

The system ingests and stores several types of healthcare data:

#### 1. Patient Vitals and Device Data
Collected daily or near-daily from home monitoring devices.
- Weight
- Blood pressure (systolic/diastolic)
- Heart rate
- Blood oxygen saturation (SpO₂) 
- Activity metrics (e.g., step count) 

**Source**: Bluetooth-enabled scales, blood pressure cuffs, or wearable devices connected through a mobile application or vendor API.  
**Format**: JSON sensor readings sent via HTTPS API requests.

#### 2. Patient-Reported Symptoms
Collected through a simple daily questionnaire in the mobile app.
- Shortness of breath rating (1-10)
- Swelling in legs/ankles (yes/no)
- Difficulty lying flat (orthopnea)
- Medication adherence (yes/no)
- Optional free-text comments

**Source**: Mobile app form submissions.  
**Format**: JSON survey responses.

#### 3. Clinical and Operational Data
Maintained by the clinic and used for monitoring and triage.
- Patient enrollment and risk tier
- Personalized alert thresholds (e.g., weight gain limits)
- Care team assignments
- Alert status and clinician actions

**Source**: Clinician/admin dashboard.  
**Format**: Structured database records.

---

### Basic Workflow

1. **Patient Data Submission**  
   The patient records daily vitals using a connected device or enters them manually through a mobile app. The app authenticates the user and sends the data securely to a cloud API.

2. **Data Ingestion and Validation**  
   The ingestion service validates the incoming data, checks for required fields, normalizes measurement units, and ensures duplicate submissions are not stored multiple times.

3. **Data Storage**  
   - Raw incoming data is stored in secure object storage for audit and replay purposes.  
   - Cleaned and normalized measurements are written to an operational database for fast access by clinical applications.

4. **Event Triggering**  
   After successful storage, the system emits an event indicating that new patient data has arrived, enabling downstream processing without continuous polling.

5. **Rules-Based Analysis and Risk Scoring**  
   The system evaluates new measurements against predefined clinical rules, such as rapid weight gain or abnormal blood pressure. Optionally, a simple analytics or machine learning model generates a risk score based on recent trends.

6. **Alert Generation and Notification**  
   If thresholds are exceeded or risk scores are high, an alert is created with a clear explanation of the trigger. Notifications are routed to the appropriate nurse or care coordinator.

7. **Clinician Review and Triage**  
   Clinicians access a dashboard showing active alerts, recent trends, and patient summaries. They acknowledge alerts, document follow-up actions, and escalate care when necessary.

8. **Analytics and Reporting**  
   Periodic batch jobs aggregate historical data to support reporting on adherence, alert frequency, and overall program effectiveness.

---

### Privacy and Security Considerations

- All patient data is encrypted in transit and at rest.
- Access is controlled using role-based permissions.
- Only the minimum necessary patient identifiers are included in events and logs.
- Audit logs track access to patient data and changes to alert thresholds or clinical actions.

---


