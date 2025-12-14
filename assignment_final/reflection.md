# Reflection

## Confidence and Uncertainty in the Design

The parts of the design I feel most confident about are the overall data flow and the use of serverless services for intake and processing. The end-to-end pipeline from patient data submission, to secure storage, to clinician-facing summaries follows a clear and logical structure. Using Cloud Run for the Flask API and Cloud Functions for intake and rules-based alerting links well with best practices covered in the course and feels appropriate for a workload that is event-driven rather than constant. The separation between raw data in Cloud Storage and structured data in Cloud SQL is another area of confidence, as it supports auditability, debugging, and analytics without overcomplicating the system.

The areas I am least sure about relate to the analytics and clinical logic. While the optional use of Vertex AI for risk scoring is conceptually sound, designing a clinically meaningful model would require more domain knowledge, labeled data, and validation. Similarly, choosing appropriate alert thresholds for heart failure patients is a non-trivial problem and would require close collaboration with clinicians to avoid alert fatigue.

---

## Alternative Architecture Considered

One alternative architecture considered was using a virtual machine (Compute Engine) to host both the API and background processing logic. This approach would simplify deployment by placing all components on a single VM and might be easier to reason about for a very small system. However, this option was not chosen because it does not scale as cleanly and requires more operational management, such as patching, uptime monitoring, and capacity planning. Since the course emphasized managed and serverless cloud services, the Cloud Run and Cloud Functions approach was a better fit both technically and educationally.

Another alternative was to store all data directly in Cloud SQL without using Cloud Storage for raw data. While simpler, this design was rejected because it removes the ability to retain immutable raw records, which are important for auditing, troubleshooting ingestion issues, and reprocessing data if logic changes later.

---

## Next Steps with More Time and Resources

If given an additional 4–8 weeks and unlimited cloud credits, the next steps would focus on realism, scaleability, and clinical usefulness. First, I would improve the analytics component by developing and validating a more sophisticated risk prediction model using historical patient data and clinician-labeled outcomes. This would include proper model evaluation, explainability, and monitoring for drift.

Second, I would expand the clinician dashboard with richer visualizations, role-based views, and workflow features such as alert escalation, documentation templates, and integration with scheduling systems. Third, I would strengthen security and governance by adding more detailed audit logging, automated compliance checks, and stricter separation between development and production environments.

Finally, I would explore integration with healthcare data standards such as FHIR to improve interoperability with electronic health record systems. This would move the project from a course-focused prototype toward something that more closely resembles a production-grade healthcare cloud application.

---

