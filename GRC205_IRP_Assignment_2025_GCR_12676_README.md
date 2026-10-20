# **GRC205** **INCIDENT RESPONSE PLANNING ASSIGNMENT**

**Student Name:** Faith Anyachokie  
**Student ID:** 2025/GCR/12676  
**Module:** GRC205 – Incident Management & Business Continuity  
**Date:** 20/10/2026

# **Task 1 – Incident classification and initial assessment**

## **1.1 Executive assessment**

Northbridge Skills Institute should classify the event as a confirmed major cybersecurity incident involving ransomware or destructive file encryption, with suspected account compromise and possible data exfiltration. The incident is confirmed by the mass file changes, .locked extensions and ransom note reported at 08:30 (E06), alongside the learning portal becoming inaccessible at 08:40 (E07). The earlier evidence suggests a possible intrusion path through a payroll-related attachment, an unfamiliar Microsoft 365 sign-in, use of the svc\_backup account on file and backup servers, and an unusual outbound upload. These links are plausible but not yet proven to be one continuous attack.

The initial severity is SEV-1 / Critical because multiple business services are affected, backups cannot yet be trusted, a privileged service account may be compromised, the contact list and email are not reliable for incident coordination, and an examination for 100 learners is due at 11:00. The incident threatens confidentiality, integrity and availability. It also creates potential privacy, contractual, financial and reputational consequences. Severity should be reassessed as reliable evidence emerges.

## **1.2 Incident timeline**

All times are West Africa Time (WAT). “Day −1” is the day before the incident.

| Time | Evidence | What is observed | Initial interpretation and confidence |
| :---- | :---- | :---- | :---- |
| Day −1, 16:40 | E01 | A payroll attachment was opened. | Possible initial access or user-execution event. Hypothesis only: opening the attachment does not by itself prove it was malicious or caused the incident. |
| Day 0, 01:15 | E02 | Microsoft 365 records show a sign-in from an unfamiliar IP address/device. | Possible compromised identity or unauthorised session. Verify identity, MFA result, device, session activity and IP reputation. |
| Day 0, 01:35 | E03 | svc\_backup makes administrator logons to FS01 and BAK01. | Potential privilege misuse and access to file/backup infrastructure. Confirm whether this account is expected to log on interactively and whether the source host is authorised. |
| Day 0, 01:50 | E04 | FIN-PC07 sends 1.2 GB over HTTPS. | Anomalous transfer that could be legitimate, cloud sync, backup activity or exfiltration. Destination, content and process are unknown; do not label it confirmed data theft. |
| Day 0, 02:00 | E05 | Backup jobs report “success”, but restoration and integrity have not been verified. | A successful job status is not proof that backups are intact, isolated or restorable. Treat backup trust as unknown. |
| Day 0, 08:30 | E06 | Mass file changes, .locked extensions and a ransom note appear. | Strong evidence of ransomware or destructive encryption. Confirm affected hosts, file scope and note contents without interacting with the attacker. |
| Day 0, 08:40 | E07 | Learner portal is inaccessible. | Confirms material service disruption; determine whether it is encrypted, offline, overloaded or otherwise affected. |
| Day 0, 08:45 | E08 | Contact list is inaccessible and email is considered untrusted. | Incident control begins. Out-of-band communications are needed because normal coordination channels may be compromised. |

### **Working hypotheses to test**

* The payroll attachment may have delivered malware or credentials theft.  
* The unfamiliar Microsoft 365 sign-in may reflect account compromise, but could also be a legitimate user, VPN or travel-related sign-in.  
* The svc\_backup account may have been used to access file and backup infrastructure; the account's name alone does not prove malicious use.  
* The 1.2 GB HTTPS transfer may represent exfiltration, but destination, content and process evidence are required.  
* The actor may have encrypted or tampered with backups, but E05 does not establish backup compromise.

## **1.3 Severity and impact**

| Dimension | Assessment | Rationale |
| :---- | :---- | :---- |
| Availability | Critical | The learner portal is unavailable; mass file changes suggest broader disruption. The 11:00 examination may be affected. |
| Integrity | Critical | Files have been changed or encrypted. The integrity of file, finance and backup systems is uncertain. |
| Confidentiality | High / potentially critical | E04 shows a large outbound transfer, and E02–E03 suggest identity and privileged access concerns. Exfiltration is not confirmed. |
| Operational impact | Critical | Learning services, finance-related assets, file services, backup services and incident coordination may be affected. |
| Privacy/legal impact | Potentially high | Payroll material and learner information could be in scope, but the evidence supplied does not establish what data, if any, left the organisation. |
| Overall severity | SEV-1 / Critical | Multiple systems, possible privileged compromise, uncertain backups, service interruption and a time-sensitive examination require executive-led response. |

## **1.4 Immediate investigation priorities**

* Contain spread without destroying evidence. Isolate affected endpoints and servers through network controls. If several systems or subnets are affected, consider a coordinated segment-level isolation. Avoid casually powering off systems if volatile evidence can be preserved.  
* Protect identity and administrative access. Revoke suspicious Microsoft 365 sessions, disable or restrict compromised accounts, reset credentials from a clean device, and investigate svc\_backup. Do not use a potentially compromised email account for response coordination.  
* Preserve evidence. Export Microsoft 365 audit/sign-in logs, identity-provider logs, endpoint detection alerts, firewall/proxy/DNS logs, file-server logs, backup-console logs, ransom notes and relevant system images. Record who collected each item, when, from where, and its hash where practical.  
* Determine scope. Identify affected hosts, shared drives, cloud accounts, privileged identities, endpoints and services. Search for the ransom note and file extension across managed assets using approved tools.  
* Investigate the suspected outbound transfer. Establish FIN-PC07's destination IP/domain, TLS/SNI or proxy records, process responsible, user context, transfer timing and whether the data can be identified. Do not infer exfiltration from volume alone.  
* Validate backups safely. Check backup-console access and logs from a clean administrative workstation; protect backup credentials; confirm immutability/offline copies; test restoration in an isolated environment before declaring backups usable.  
* Protect the 11:00 examination. Establish whether a clean, independently verified platform or approved alternative is available. The decision must be made by the incident lead and academic operations lead using safety, fairness, integrity and availability criteria.  
* Establish reliable communications. Use telephone calls, an approved out-of-band channel or an in-person coordination point. Assume ordinary email and the contact list are untrusted until verified.  
* Determine reporting and notification duties. Engage legal/privacy advisers to assess applicable law, contracts, insurers and regulator obligations once the likely data and jurisdictional scope is understood.  
* Maintain a decision log. Record time, decision, decision-maker, evidence, rationale, action owner and review time.

## **1.5 What is known and what remains unconfirmed**

Known from the scenario: mass file changes and a ransom note; portal unavailability; an unfamiliar Microsoft 365 sign-in; administrator logons by svc\_backup; a 1.2 GB HTTPS transfer; and unverified backup integrity.

Not yet established: the initial access vector, malware family, attacker identity, whether the M365 sign-in was malicious, whether the service account was abused, whether the upload contained organisational data, whether data was exfiltrated, the complete affected-system count, and whether backups are usable.

This distinction matters because an incident response plan should support decisive containment while avoiding unsupported claims to learners, regulators or the public.

## **1.6 Assumptions**

* Northbridge is a fictional education provider with 1,800 learners and 90 staff.  
* The incident is managed in WAT, with control beginning at 08:45 on Day 0\.  
* The examination scheduled for 11:00 involves 100 learners.  
* System names and the E01–E08 exhibits are taken from the assignment scenario.  
* The organisation's country of establishment, data-hosting locations and governing contracts have not been specified. Legal notification decisions must therefore be confirmed by qualified local counsel rather than assigned an invented deadline.

# **Task 2 – Incident response plan and team responsibilities**

## **2.1 Purpose and policy statement**

Northbridge Skills Institute should maintain an approved Incident Response Policy that enables the organisation to detect, report, assess, contain, investigate, recover from and learn from cybersecurity incidents. The policy should protect learners, staff, institutional records, finances and the continuity of education. It should apply to employees, contractors, service providers, managed devices, cloud services and information handled on the institute's behalf.

The policy should be aligned with NIST SP 800-61 Rev. 3 and the NIST Cybersecurity Framework 2.0. Incident response should be integrated into the organisation's broader risk management rather than treated as a stand-alone technical activity (NIST, 2025). Ransomware-specific procedures should also reflect the CISA \#StopRansomware Guide (CISA et al., 2023).

## **2.2 Scope and principles**

The policy covers endpoints, servers, identity and email platforms, learning management systems, financial systems, file services, backups, network equipment, third-party services and relevant physical records.

The response must follow these principles:

* Protect people and essential services first. Prioritise learner welfare, examination integrity and critical operations.  
* Contain quickly, but preserve evidence. Coordinate isolation and avoid unnecessary actions that destroy forensic evidence.  
* Use trusted channels. If email, identity systems or contact lists may be compromised, use verified out-of-band communications.  
* Apply least privilege. Restrict administrative rights and use clean devices and known-good credentials for response.  
* Preserve confidentiality. Share incident details only with authorised people who need them.  
* Keep a decision and evidence trail. Record key actions, approvals, evidence handling and changes to scope.  
* Do not make unsupported claims. Distinguish confirmed facts from hypotheses and unknowns.  
* Recover only to a trusted state. Rebuild or restore from verified clean sources; do not assume that a successful backup job means a safe restore.  
* Meet applicable legal and contractual duties. Legal/privacy advisers must identify the governing jurisdiction, relevant data types and notification requirements.  
* Learn and improve. Conduct a post-incident review and track corrective actions to completion.

## **2.3 Activation and severity**

Any staff member may report a suspected incident to the service desk, security team or designated incident lead. If normal reporting channels are unavailable, staff should use the published out-of-band contact route or report to the designated physical coordination point.

The incident lead should activate the response team when there is credible evidence of ransomware, unauthorised privileged access, widespread service disruption, suspected data loss, compromise of identity or backup systems, or a threat to essential academic activity.

| Severity | Definition | Response expectation |
| :---- | :---- | :---- |
| SEV-1 Critical | Multiple critical systems affected; widespread ransomware; privileged/identity/backup compromise; major service outage; potential significant data exposure. | Immediate incident command, executive notification, coordinated containment, continuous updates. |
| SEV-2 High | Significant service or department affected, contained scope, or credible compromise with material business impact. | Urgent response, incident lead assigned, frequent updates. |
| SEV-3 Moderate | Limited scope, no confirmed critical service impact, and manageable containment. | Assigned investigation and documented remediation. |
| SEV-4 Low | Low-impact security event or policy violation with no evidence of material compromise. | Routine triage and closure with evidence. |

Severity is dynamic. The incident lead may raise or lower it when new evidence justifies the change and records the rationale.

## **2.4 CSIRT roles and responsibilities**

The Computer Security Incident Response Team (CSIRT) is a cross-functional response group. One person may hold more than one role in a small organisation, but each responsibility must have a named owner and backup.

| Role | Main responsibilities |
| :---- | :---- |
| 1\. Incident Commander / Incident Lead | Declares severity, sets priorities, coordinates workstreams, maintains decision log and briefs leadership. |
| 2\. Technical Response Lead / Security Analyst | Leads triage, log analysis, scoping, threat hunting, indicators of compromise and technical recommendations. |
| 3\. IT Infrastructure and Endpoint Lead | Isolates hosts and network segments, manages endpoint/server changes, rebuilds systems and validates technical dependencies. |
| 4\. Identity and Cloud Administrator | Reviews M365 sign-ins and audit logs, revokes sessions, secures privileged accounts, checks cloud configuration and access. |
| 5\. Backup and Recovery Lead | Protects backup infrastructure, verifies restore points, tests recovery and documents recovery validation. |
| 6\. Legal and Privacy Lead | Advises on evidence preservation, privilege, contracts, regulatory duties, notification thresholds and law-enforcement engagement. |
| 7\. Communications Lead | Prepares approved holding statements, learner/staff updates and public communications through trusted channels. |
| 8\. Academic Operations Lead | Assesses learner impact, examination continuity, accessibility, rescheduling and fairness. |
| 9\. Executive Sponsor / Senior Management | Approves exceptional business decisions, resources, risk acceptance and major service suspensions. |
| 10\. HR / People Operations | Supports staff communications, employee welfare and internal procedures where appropriate. |
| 11\. External Forensics / Managed Security Provider | Provides specialist analysis, malware/forensic support and independent recovery assurance when engaged. |

## **2.5 RACI matrix**

R \= Responsible for doing the work; A \= Accountable for the final decision; C \= Consulted; I \= Informed. Each activity has one accountable role.

| Activity | Incident Lead | Security/Technical | IT/Identity/Backup | Legal/Privacy | Communications | Academic Ops | Executive |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Declare incident and severity | A/R | C | C | C | I | C | I |
| Isolate affected systems | A | R | R | I | I | C | I |
| Revoke suspicious sessions and secure accounts | A | C | R | C | I | I | I |
| Preserve forensic evidence | A | R | R | C | I | I | I |
| Scope possible data exposure | A | R | R | C | I | I | I |
| Validate backup integrity | A | C | R | I | I | C | I |
| Decide examination continuity/rescheduling | C | C | C | C | I | A/R | I |
| Determine legal notification obligations | C | C | C | A/R | C | I | I |
| Approve external/public statement | C | C | I | C | R | C | A |
| Approve restoration to production | A | C | R | C | I | C | I |
| Close incident and accept residual risk | R | C | C | C | I | C | A |

## **2.6 Readiness improvements**

* Maintain offline contact and escalation lists. Keep printed and offline copies with primary and alternate contacts, role-based numbers, vendor details and escalation thresholds. Test the list quarterly.  
* Create an isolated recovery environment. Maintain protected backups, immutable or offline copies where feasible, separate backup credentials and documented clean-room restore steps. Test restore integrity regularly.  
* Harden identity and privileged access. Require phishing-resistant MFA where feasible, separate administrative accounts, least privilege, monitored service accounts, restricted interactive logons and rapid session revocation.  
* Improve central logging and alerting. Retain identity, endpoint, server, firewall, proxy, DNS and backup logs in a protected central system with alerts for unusual sign-ins, privilege escalation, large outbound transfers and mass file changes.  
* Maintain asset and dependency maps. Document critical services, data flows, backup dependencies, cloud services, suppliers and the systems needed to deliver examinations.  
* Prepare communication templates. Draft staff, learner, regulator and media holding statements in advance and identify who can approve them.  
* Exercise the plan. Run quarterly tabletop exercises and at least annual technical recovery tests. Record actions, owners and due dates.  
* Agree third-party support in advance. Establish contact and service arrangements for incident response, legal counsel, cloud providers, insurers and forensic specialists.  
* Train staff on suspicious attachments and reporting. Teach users how to report suspected phishing promptly without deleting messages or forwarding malicious attachments widely.  
* Define decision authority for critical services. Document who can suspend learning services, postpone examinations, isolate a network segment or authorise emergency spending.

## **2.7 Governance and review**

The executive sponsor should approve the policy. The incident lead should review it at least annually and after a major incident, major system change or exercise. The organisation should maintain version control, an approval date, owner, review date and records of staff awareness. Metrics should include detection-to-escalation time, containment time, restore-test success, overdue corrective actions and exercise completion.

# **Task 3 – Response playbook and evidence preservation**

## **3.1 Incident response playbook**

This playbook is designed for the Northbridge ransomware scenario. It follows the principle that incident response, recovery and risk management are connected activities, as described by NIST SP 800-61 Rev. 3 (NIST, 2025). The sequence may be adapted to protect life, prevent immediate harm or contain active spread, but deviations must be recorded.

| Step | Action | Owner | Target / output |
| :---- | :---- | :---- | :---- |
| 1 | Declare SEV-1, appoint incident commander, open a decision log and set a secure coordination point. | Executive sponsor / incident lead | Immediate; named lead and incident record. |
| 2 | Establish out-of-band communications because E08 says email and the contact list are untrusted. Verify participants by a second channel. | Communications lead / incident lead | Immediate; verified contact roster. |
| 3 | Isolate confirmed affected endpoints and servers, beginning with known impacted assets and any systems showing active encryption. Use network controls where possible. | IT lead | Immediate; containment log with host, time and method. |
| 4 | Preserve volatile and time-sensitive evidence where feasible before disruptive actions. Capture relevant endpoint telemetry, logs, memory or disk images through competent responders. | Technical lead / forensics | Before remediation where feasible; evidence records. |
| 5 | Secure identity systems: revoke suspicious M365 sessions, restrict affected accounts, reset credentials from a clean device and assess privileged accounts. | Identity/cloud lead | Early; account and session action log. |
| 6 | Investigate svc\_backup activity on FS01 and BAK01. Determine source, logon type, privilege use, scheduled jobs and whether the account's behaviour was expected. | Security analyst / identity lead | Early; verified account timeline. |
| 7 | Investigate FIN-PC07's 1.2 GB HTTPS transfer. Identify destination, process, user, proxy/firewall records and data category if possible. Preserve uncertainty until evidence confirms or rejects exfiltration. | Security analyst | Early; data-transfer assessment. |
| 8 | Determine scope across endpoints, FS01, LMS01, BAK01, finance systems, M365, identity infrastructure and connected services. Search for mass file changes, ransom notes, suspicious processes and lateral movement. | Technical lead / IT | Initial scope report; update as new evidence appears. |
| 9 | Protect backup systems. Restrict backup-console access, review logs and credentials, preserve backup metadata and prevent overwriting of candidate recovery points. | Backup lead | Immediate; backup trust assessment. |
| 10 | Notify executive leadership, legal/privacy, academic operations and relevant service providers using trusted channels. Issue a factual holding update with unknowns clearly stated. | Incident lead / communications | Initial update and scheduled cadence. |
| 11 | Decide the 11:00 examination plan. Test any alternative platform or site using clean devices and verified systems. If integrity, availability or fairness cannot be assured, postpone and communicate the decision. | Academic operations lead, with incident commander | Decision before exam start where practicable; rationale recorded. |
| 12 | Eradicate the cause only after sufficient scoping and evidence preservation. Remove malicious persistence, close exploited access paths, patch weaknesses and rebuild compromised systems from known-good images. | Technical lead / IT | Eradication record and validation results. |
| 13 | Test recovery in an isolated environment. Restore from a clean point, scan and validate data, rotate credentials, verify logs and monitor for reinfection before reconnecting production systems. | Backup lead / IT / security | Signed recovery checklist for each service. |
| 14 | Restore services in priority order, monitor closely and keep a rollback plan. Prioritise identity and core infrastructure needed for safe restoration, then essential file/finance and learning services based on dependency mapping. | Incident lead / IT / academic ops | Service owner acceptance and monitoring plan. |
| 15 | Conduct a post-incident review, preserve the final evidence set, assess legal and contractual duties, communicate confirmed findings and track improvements. | Incident lead / legal / executive | Review report, owners and due dates. |

### **Operational cautions**

* Do not pay a ransom or communicate with the actor without executive, legal and law-enforcement advice. Payment does not guarantee decryption or deletion of stolen data.  
* Do not assume that a backup job marked “success” means the backup is intact or safe to restore.  
* Do not state that personal data was stolen based only on the 1.2 GB transfer. Establish the destination and data involved.  
* Do not reconnect systems solely because files are readable. Confirm that the initial access path, persistence and compromised credentials have been addressed.  
* Preserve the ransom note and relevant logs. Do not upload sensitive evidence to public repositories.

## **3.2 Evidence register**

Evidence should be collected by authorised personnel, stored securely, assigned a unique ID, and protected from alteration. Hash files where appropriate, record timestamps and time zones, and document any clock differences. A forensic copy should be used for analysis where possible.

| Evidence ID | Item / source | Collection and preservation method | Why it matters | Current limitation |
| :---- | :---- | :---- | :---- | :---- |
| EV-01 | E01 payroll attachment and related email | Preserve original message with headers, attachment, mail audit logs and endpoint execution telemetry; calculate hashes for exported files. | Tests possible initial access and user-execution hypothesis. | Opening the attachment does not prove it was malicious. |
| EV-02 | E02 Microsoft 365 sign-in and audit logs | Export sign-in, MFA, device, IP, session and audit events from an authorised clean admin session; retain original export and hash. | Tests possible account compromise and post-login actions. | “Unfamiliar” IP/device is not conclusive without context. |
| EV-03 | E03 svc\_backup logons to FS01 and BAK01 | Preserve Windows Security logs, EDR telemetry, service-account configuration, source-host details and backup-console logs. | Establishes whether the account was misused and whether backup infrastructure was accessed. | Need baseline for expected service-account behaviour. |
| EV-04 | E04 FIN-PC07 outbound HTTPS transfer | Preserve firewall, proxy, DNS, endpoint process and network telemetry; document destination, bytes, time and process. | Tests potential exfiltration or legitimate transfer. | Volume alone does not reveal content or intent. |
| EV-05 | E05 backup job status and backup sets | Export backup job logs, retention/deletion events, administrative access logs and configuration; protect restore points from modification. | Determines whether usable recovery points exist and whether backup access was abused. | “Success” is not proof of integrity or restorability. |
| EV-06 | E06 encrypted files, extensions and ransom note | Preserve a sample from a safe location, metadata, file hashes, timestamps and endpoint telemetry; avoid executing the note or any payload. | Confirms ransomware behaviour and may help identify scope and variant. | Malware family and full impact remain unknown. |
| EV-07 | E07 LMS01 / learner portal outage | Preserve application, server, identity, network and hosting logs; record availability tests and affected functions. | Establishes impact and helps identify whether outage is caused by encryption or another failure. | Inaccessibility alone does not prove direct encryption of LMS01. |
| EV-08 | E08 communication channel failure | Record which systems/contact list were inaccessible, who verified the issue and when; preserve relevant service status and logs. | Supports use of out-of-band coordination and informs continuity planning. | Does not by itself prove email compromise; the scenario requires treating it as untrusted. |

### **Chain-of-custody fields**

For every collected item, record: evidence ID; description; source host/account; collector; date/time and time zone; acquisition method/tool; hash algorithm and hash; original storage location; each transfer (from/to, time, purpose and signatures); access restrictions; and analysis copies made. Store originals in access-controlled, tamper-evident storage.

## **3.3 Containment, eradication and recovery criteria**

### **Containment is sufficient when**

* Active encryption and suspicious network communication are stopped or isolated.  
* Affected network segments, endpoints, privileged identities and cloud sessions are identified and controlled.  
* Backup infrastructure is protected from further access or deletion.  
* A current scope estimate and list of unknowns exists.  
* Evidence is preserved to the extent practical and emergency actions are documented.

### **Eradication is sufficient when**

* The initial access path is identified or reasonably bounded and mitigated.  
* Malicious files, persistence mechanisms, unauthorised accounts, sessions, scheduled tasks and remote-access paths have been removed or disabled.  
* Compromised credentials and keys have been rotated from a clean environment.  
* Critical systems are rebuilt or validated against known-good baselines.  
* Security monitoring shows no continuing signs of attacker access for a defined observation period approved by the technical lead.

### **Recovery is approved only when**

* A clean restore point is identified and validated in an isolated environment.  
* Restored systems pass malware, integrity, configuration and access-control checks.  
* Critical credentials and service accounts are secured and least privilege is restored.  
* Logging, endpoint protection, backups and monitoring are functioning.  
* System owners test the service and verify data consistency.  
* The academic operations lead validates the learner portal and examination-related functions.  
* The incident commander accepts the residual risk with technical and business-owner advice.  
* A rollback plan, heightened monitoring and incident escalation path are active.

### **Suggested recovery priority**

* Trusted out-of-band coordination and clean administrative access.  
* Core identity, network and security monitoring needed to rebuild safely.  
* Backup infrastructure and clean recovery environment.  
* Essential file and finance services, based on business dependency and data-integrity needs.  
* Learner portal/LMS and examination services, with academic integrity checks.  
* Remaining user endpoints and lower-priority services.

The exact order should be validated against Northbridge's dependency map; it must not be assumed that one server can be restored independently of its identity, network or application dependencies.

# **Task 4 – Communication and escalation**

## **4.1 Communication principles**

Because E08 states that the contact list is inaccessible and email is untrusted, initial coordination must use verified out-of-band channels such as telephone calls, a pre-approved secure channel or an in-person coordination point. No one should send sensitive incident details through the suspected compromised email environment until it is assessed and approved for use.

Communications should be timely, factual, accessible and consistent. Messages must distinguish confirmed facts from active investigation, avoid speculation about attacker identity or data theft, and provide recipients with practical next steps. The communications lead coordinates wording, legal/privacy advisers review relevant notices, and the authorised executive approves public statements.

## **4.2 Communication matrix**

| Audience | Purpose / key message | Channel | Owner / approver | Timing |
| :---- | :---- | :---- | :---- | :---- |
| Executive leadership / board | Severity, business impact, decisions required, current knowns and unknowns, resource needs. | Verified phone bridge or in-person briefing; secure channel once validated. | Incident lead; executive sponsor accountable. | Immediately, then at agreed intervals (e.g. every 30–60 minutes during initial response). |
| IT, security and service owners | Containment instructions, affected assets, evidence handling, technical status and task assignments. | Out-of-band bridge and approved incident workspace. | Technical lead / incident lead. | Immediately and continuously as actions change. |
| Staff | Do not use affected systems for incident coordination; report suspicious prompts; follow continuity instructions; do not speculate publicly. | Verified SMS/phone tree or approved alternate channel. | HR and communications lead. | Initial holding message as soon as verified; updates when actions change. |
| Learners and examination candidates | Status of portal/examination, whether to attend or log in, how updates will arrive, accessibility/support arrangements. | Verified SMS, official website/status page if clean, phone or institution-controlled alternate channel. | Academic operations lead; communications lead. | Before the 11:00 examination decision where practicable; further updates on a defined schedule. |
| Parents/guardians where relevant | Practical information about learner disruption and support, without exposing private learner details. | Verified institution channels. | Student services / communications. | If affected learners require it and policy permits. |
| Legal counsel / privacy lead | Scope, data types, jurisdictions, evidence, contract and regulatory notification analysis. | Verified direct contact. | Incident lead. | Early, with updates as evidence develops. |
| Regulators / data protection authority | Required report where legal thresholds and timelines apply; verified facts, impact, response and contact point. | Official regulator channel or required reporting portal. | Legal/privacy lead; authorised executive. | Within the applicable statutory or contractual deadline after confirming duties. |
| Law enforcement / national cyber response body | Report suspected criminal activity, seek assistance and share relevant indicators/evidence through approved channels. | Official reporting channel. | Incident lead / legal counsel. | Early where appropriate, without delaying containment. |
| Cloud, ISP, security and backup providers | Request preservation of logs, session revocation, threat analysis and service recovery support. | Verified support contacts / emergency escalation. | IT lead / incident lead. | Immediately where their systems or services may be involved. |
| Media / general public | Provide one accurate holding statement and a route for further updates. | Approved spokesperson and official channel. | Communications lead drafts; executive sponsor approves, with legal review. | Only when necessary or after public impact requires a statement. |
| Insurer / contractual partners | Notice under policy/contract, preserve claims rights and obtain approved response support. | Contractually specified channel. | Legal / finance / incident lead. | As soon as required by the applicable contract or policy. |

## **4.3 Executive briefing**

At 08:45 WAT, Northbridge Skills Institute activated incident control after evidence of a serious ransomware incident. At 08:30, mass file changes, .locked extensions and a ransom note were observed. The learner portal became inaccessible at 08:40. Earlier indicators include an unfamiliar Microsoft 365 sign-in, administrator logons by the svc\_backup account to FS01 and BAK01, and a 1.2 GB HTTPS transfer from FIN-PC07. These events may be connected, but the investigation has not yet established the initial access path or confirmed data exfiltration.

The incident is rated SEV-1 / Critical because essential services are disrupted, backup integrity is unverified, privileged access may be compromised and an examination for 100 learners is scheduled for 11:00. The response team is isolating affected systems, securing identities, preserving logs and investigating the outbound transfer. Backup systems are being protected, and no restoration will be approved until a clean recovery point is tested.

A decision is required on the examination using verified service and integrity checks. Communications will use out-of-band channels because email and the contact list are currently treated as untrusted. Legal/privacy advisers will assess applicable notification duties as evidence develops.

## **4.4 Learner notice**

Northbridge Skills Institute is responding to a technical security incident that has affected access to the learner portal. Our technical team is investigating and working to restore services safely.

If you are scheduled to sit the examination at 11:00 today, please do not assume that the examination is proceeding. We will issue a confirmed update through the institution's verified alternate communication channel as soon as a decision has been made. Please check only the official channel identified in that update and avoid relying on forwarded messages or unofficial social media posts.

Do not attempt to access the affected portal repeatedly or share passwords, verification codes or screenshots containing personal information. If you need urgent assistance, use the verified student-support contact provided through the alternate channel.

We will provide further information when it has been confirmed.

## **4.5 Legal, privacy and notification considerations**

The organisation must identify the applicable jurisdiction before stating a statutory deadline. Relevant questions include: where Northbridge is established; where learners and staff are located; which data subjects and data categories are involved; where systems and service providers operate; whether contractual or sector-specific duties apply; and whether evidence supports unauthorised access, loss, alteration or disclosure of personal data.

Legal/privacy advisers should preserve evidence and document the assessment, including the reasons for notifying or not notifying a regulator or affected individuals. The 1.2 GB transfer is an investigative lead, not proof that personal data was stolen. If personal data may have been affected, the team should identify the data, people, likely consequences and protective steps, then follow the applicable legal process. Do not copy sensitive records into the public GitHub repository or include identifiable learner, payroll or staff data in status messages.

## **4.6 Communication approval and cadence**

* Internal technical instructions: incident lead or technical lead.  
* Executive updates: incident lead, with decisions recorded by the executive sponsor.  
* Learner/staff notices: communications lead drafts; academic operations/HR verifies practical instructions; legal/privacy reviews if data exposure is discussed.  
* Regulatory notifications: legal/privacy lead coordinates, with authorised sign-off.  
* Media/public statements: one authorised spokesperson; executive approval and legal review.  
* Cadence: hourly during the initial critical phase or when a material decision changes; less frequent once impact and restoration stabilise. Avoid promising a restoration time until evidence supports it.

# **Task 5 – Tabletop exercise and improvement plan**

## **5.1 Exercise overview**

Exercise name: Northbridge Ransomware: Examination Day

Duration: 60 minutes

Exercise time: 08:45–09:45 WAT, Day 0

Participants: Incident lead, security analyst, IT lead, identity/cloud administrator, backup lead, legal/privacy lead, communications lead, academic operations lead, executive sponsor and facilitator/scribe.

Purpose: Test whether Northbridge can make and record timely decisions during a ransomware incident while maintaining reliable communications, preserving evidence, protecting backups and deciding how to handle a scheduled examination.

Rules: This is a discussion exercise; no production changes are made. Participants should state what they would do, who has authority, what evidence they need and how they would communicate. The facilitator should introduce only the facts in each inject and record assumptions separately from scenario facts.

## **5.2 Objectives and success criteria**

* Assign an incident commander and confirm SEV-1 status within five minutes.  
* Establish a verified out-of-band communication channel within ten minutes.  
* Identify containment actions and evidence-preservation needs without treating hypotheses as facts.  
* Decide how to protect identity and backup systems.  
* Make a defensible examination-continuity decision before the exercise ends.  
* Identify applicable legal/privacy assessment steps without inventing a notification deadline.  
* Record at least five decisions, owners, rationale and follow-up actions.

## **5.3 Minute-by-minute injects**

| Exercise time | Inject | Information supplied | Questions for participants |
| :---- | :---- | :---- | :---- |
| 00:00–00:10 | Inject 1 – Initial activation (08:45 WAT) | E06: mass file changes, .locked extensions and ransom note at 08:30. E07: learner portal inaccessible at 08:40. E08: contact list inaccessible and email untrusted at 08:45. | Who declares severity? Who commands the response? Which systems are isolated first? What channel is trusted? What must be preserved before changes? |
| 00:10–00:20 | Inject 2 – Identity and privileged access | E02: unfamiliar M365 IP/device at 01:15. E03: svc\_backup administrator logons to FS01 and BAK01 at 01:35. | Which sessions/accounts are restricted? How is service-account activity validated? Who can reset credentials? What evidence must be preserved? |
| 00:20–00:30 | Inject 3 – Possible data transfer and backup uncertainty | E04: 1.2 GB HTTPS upload from FIN-PC07 at 01:50. E05: backup jobs show “success” at 02:00, but restore integrity is unverified. | What evidence would confirm or refute exfiltration? How are backups protected? Who can authorise a test restore? What must not be said publicly yet? |
| 00:30–00:40 | Inject 4 – Examination deadline | The examination for 100 learners begins at 11:00. The portal is unavailable and the team cannot yet confirm the scope or integrity of the LMS. | Who decides whether to proceed, switch to an alternative or postpone? What tests are mandatory? How will learners be informed through a trusted channel? |
| 00:40–00:50 | Inject 5 – Additional inject: vendor asks for an emergency admin login | A caller claiming to be the cloud/backup provider asks for an administrator password to “restore services quickly”. The caller's identity cannot be verified using the inaccessible contact list. | How is the request verified? What credentials or tokens must never be shared? Who contacts the provider through an independently verified route? How is the attempted social engineering documented? |
| 00:50–01:00 | Hotwash and decision review | Facilitator asks participants to summarise current status, decisions, unresolved questions and next actions. | Which decision was hardest? What information was missing? Which playbook steps were unclear? Who owns each improvement and by when? |

## **5.4 Decision log (exercise template with expected decisions)**

| \# | Decision | Accountable role | Rationale / evidence | Review trigger |
| :---- | :---- | :---- | :---- | :---- |
| 1 | Declare SEV-1 / Critical and appoint an incident commander. | Executive sponsor / incident lead | Confirmed ransomware indicators and portal outage; E06–E07. | Reassess as scope becomes known. |
| 2 | Use out-of-band communication and treat email/contact list as untrusted. | Incident lead | E08. | Reassess after identity and email integrity checks. |
| 3 | Isolate affected systems and protect adjacent segments while preserving evidence. | Incident lead with IT lead | Active file changes and service disruption; E06–E07. | Reassess when spread is contained. |
| 4 | Revoke suspicious sessions and investigate svc\_backup; do not assume the identity events are conclusively malicious until corroborated. | Identity/cloud lead | E02–E03. | Review after log correlation and account-owner verification. |
| 5 | Treat backup integrity as unknown; protect restore points and test in an isolated environment. | Backup lead | E03 and E05. | Review after integrity tests and access-log analysis. |
| 6 | Do not describe the 1.2 GB transfer as confirmed theft; investigate destination and content. | Incident lead / technical lead | E04 alone does not prove exfiltration. | Review when network and endpoint evidence is correlated. |
| 7 | Postpone the examination unless a clean, tested alternative meets availability, integrity, fairness and accessibility requirements. | Academic operations lead | Portal is down; LMS integrity is not yet verified. | Review when independent validation is complete. |
| 8 | Verify the vendor caller through a known, independent contact route; never share administrator passwords. | IT lead | Unverified emergency request. | Review after provider identity is confirmed. |
| 9 | Ask legal/privacy advisers to determine applicable notification and contractual duties. | Legal/privacy lead | Potential data exposure, jurisdiction not specified. | Review when affected data and jurisdictions are known. |

## **5.5 Improvement plan**

| \# | Improvement | Priority | Owner | Due date | Evidence of completion |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | Produce an offline incident contact list with primary/alternate contacts and a quarterly test. | High | Incident lead / HR | Within 30 days | Signed test record and printed copy location. |
| 2 | Run a backup recovery test in an isolated environment and document integrity checks. | Critical | Backup lead | Within 30 days | Restore-test report, results and remediation tickets. |
| 3 | Review svc\_backup privileges, interactive logon rights, credential storage and monitoring. | Critical | Identity / infrastructure lead | Within 14 days | Approved service-account review. |
| 4 | Add alerts for unusual cloud sign-ins, large outbound transfers and mass file modifications. | High | Security lead | Within 45 days | Test alerts and incident tickets. |
| 5 | Approve an examination continuity procedure, including postponement authority and accessible alternate channels. | High | Academic operations lead | Within 30 days | Approved procedure and staff briefing. |
| 6 | Create staff, learner, executive and regulator communication templates with approval routes. | Medium | Communications / legal | Within 30 days | Version-controlled templates. |
| 7 | Establish an evidence-handling form and train responders on chain of custody. | High | Security lead / legal | Within 45 days | Completed training and sample evidence record. |
| 8 | Test third-party emergency escalation using independently verified contacts. | Medium | IT procurement / IT lead | Within 60 days | Exercise record and updated supplier register. |

## **5.6 Evaluation**

The facilitator should score each objective as met, partly met or not met, with notes. The exercise is successful if participants can identify a lead, establish trusted communications, contain active spread, protect evidence and backups, avoid unsupported data-theft claims, make a reasoned examination decision, and assign improvements with owners and dates. The after-action report should be approved by the executive sponsor and reviewed at the next risk or governance meeting.

# **Task 6 – Case study analysis and critical reflection**

## **Case study: Ransomware Incident Response in a Mid-Size Healthcare Organisation**

The case study describes a ransomware attack against a healthcare organisation with 2,500 employees. An employee opened a phishing email attachment, allowing malware to establish persistence and move laterally across the network for 72 hours before Ryuk ransomware encrypted critical systems. The incident response team isolated affected network segments, verified offline backups and used forensic analysis to remove the malware. Critical systems were restored within 72 hours, while full restoration took seven days. No ransom was paid, no patient data exfiltration was confirmed, and regulatory notifications were completed on time.

### **Three strengths in the response and lessons**

First, the organisation activated its incident response team promptly after detection. Immediate containment through network segmentation and system quarantine limited the ransomware's spread and supported a coordinated response.

Second, offline backups enabled recovery without ransom payment. Their availability was particularly important because ransomware can compromise network-connected backups. However, regular restoration testing remains necessary to establish that backups are usable when needed.

Third, the organisation conducted a post-incident review, updated its incident response plan, scheduled additional training and shared threat intelligence with sector partners. These actions demonstrate a commitment to continuous improvement rather than treating recovery as the end of incident management.

### **Two limitations and weaknesses exposed**

First, the attackers remained undetected for 72 hours while establishing persistence and moving laterally. Although the organisation eventually contained the incident, this delay raises questions about the effectiveness of its earlier monitoring, phishing defences and detection capabilities.

Second, the case study states that no patient data exfiltration was confirmed but does not explain the evidence supporting that conclusion. Without details of network traffic analysis, endpoint investigations and relevant access logs, it is difficult to assess how confidently data theft was ruled out.

### **Lessons for Northbridge Skills Institute**

Northbridge should strengthen server monitoring, improve phishing awareness and maintain tested offline or immutable backups. It should also prepare alternative examination arrangements in case the learning portal cannot be restored safely before an examination.

Restoring critical systems within 72 hours means prioritising essential services to reduce immediate harm. Full restoration over seven days allows additional time to rebuild, validate and return less critical systems to operation.

Before concluding that no patient data was stolen, investigators would need to examine network and proxy logs, endpoint telemetry, access records and evidence of unauthorised data transfers.

Following the tabletop exercise, I would revise Northbridge's recovery procedure to require successful restoration testing in an isolated environment before approving the portal's return to service. This would balance operational urgency with security and data protection.

# **References**

* Cybersecurity and Infrastructure Security Agency (CISA), Federal Bureau of Investigation (FBI), National Security Agency (NSA), and Multi-State Information Sharing and Analysis Center (MS-ISAC) (2023). \#StopRansomware Guide.

[https\://www\.cisa.gov/resources-tools/resources/stopransomware-guide](https://www.cisa.gov/resources-tools/resources/stopransomware-guide) 

* National Institute of Standards and Technology (NIST) (2025). Incident Response Recommendations and Considerations for Cybersecurity Risk Management: A CSF 2.0 Community Profile (NIST Special Publication 800-61 Revision 3).

[https\://csrc.nist.gov/projects/incident-response](https://csrc.nist.gov/projects/incident-response) 

* International Cybersecurity and Digital Forensics Academy (ICDFA). GRC205: Incident Response Planning (Course teaching slides \- Slide 16).
