# Digital Forensics and Incident Investigation

A Windows digital forensics and incident investigation project focused
on evidence integrity, Windows Security and Sysmon event analysis,
timeline reconstruction, process correlation, hypothesis assessment, and
forensic reporting.

This project documents the examination of selected Windows event-log
evidence for case **DF-2026-001**. The conclusions are limited to the
evidence collected and analyzed; observed events are not automatically
treated as malicious activity.

## 1. Project Objectives

-   Establish and document an evidence inventory for the collected
    Windows event logs.
-   Verify the integrity of original evidence and working copies using
    SHA-256.
-   Examine Windows Security and Sysmon Operational event records.
-   Correlate authentication, process-creation, and network-connection
    telemetry.
-   Reconstruct and validate an incident timeline using UTC and IST
    timestamps.
-   Assess investigation hypotheses and alternative explanations.
-   Document chain-of-custody records, documentation gaps, limitations,
    and findings.
-   Produce a structured forensic investigation report.

## 2. Investigation Environment

  -----------------------------------------------------------------------
  Component                           Description
  ----------------------------------- -----------------------------------
  Case identifier                     DF-2026-001

  Endpoint platform                   Windows

  Primary evidence source 1           Windows Security event log
                                      (`.evtx`)

  Primary evidence source 2           Sysmon Operational event log
                                      (`.evtx`)

  Analysis approach                   Event-log examination, correlation,
                                      hashing, timeline validation

  Time representation                 UTC and India Standard Time (IST,
                                      UTC+05:30)

  Collection date                     5 November 2026
  -----------------------------------------------------------------------

> This repository should contain only reviewed and sanitized portfolio
> material. Original forensic evidence and unredacted exports must
> remain in controlled storage and must not be committed.

## 3. Evidence Sources

  --------------------------------------------------------------------------
  Evidence ID       Source            Format            Examination purpose
  ----------------- ----------------- ----------------- --------------------
  EV-001            Windows Security  EVTX              Authentication and
                                                        security-event
                                                        examination

  EV-002            Sysmon            EVTX              Process creation and
                    Operational                         network-connection
                                                        examination
  --------------------------------------------------------------------------

The original evidence files and working copies were separately hashed.
The recorded SHA-256 verification showed matching hashes for each
original/working-copy pair.

### Evidence integrity

-   Hash algorithm: SHA-256
-   Original evidence: retained separately from working copies
-   Working copies: used for examination
-   Integrity verification: recorded in the case documentation

**Public evidence note:** Exact hashes, host/account identifiers, event
exports, and other case-specific values should be included only after a
privacy and disclosure review. The public repository may use a sanitized
integrity summary instead of exposing sensitive original-evidence
metadata.

## 4. Forensic Methodology

1.  Establish the case workspace and evidence inventory.
2.  Preserve original event-log files and create working copies.
3.  Calculate and compare SHA-256 hashes.
4.  Review chain-of-custody documentation and identify any record gaps.
5.  Examine relevant Windows Security and Sysmon event records.
6.  Build a normalized timeline with separate activity and event-record
    timestamps.
7.  Reconcile timestamp values against the original EVTX event XML.
8.  Correlate process-creation and network-connection records using
    process identifiers and ProcessGuid.
9.  Evaluate hypotheses against supporting evidence and alternative
    explanations.
10. Validate the final timeline and document conclusions and
    limitations.

## 5. Event Analysis

  Source               Event ID Analysis focus
  ------------------ ---------- --------------------------------------------
  Windows Security         4624 Successful logon events
  Windows Security         4625 Failed logon events
  Windows Security         4648 Logon attempted using explicit credentials
  Sysmon                      1 Process creation
  Sysmon                      3 Network connection

Event IDs provide context for examination. An event record by itself
does not establish malicious intent or prove compromise.

## 6. Timeline Reconstruction and Validation

The investigation produced a corrected timeline and a separate
reconciliation addendum.

The Day 21 validation recorded:

  Validation item                                           Result
  ------------------------------------------------------- --------
  Indexed EVTX records                                      80,228
  Original timeline rows                                    38,682
  Corrected timeline rows                                   38,682
  Event ID mapping errors                                        0
  Duplicate Source/RecordID keys                                 0
  Missing EventRecord timestamps                                 0
  Sysmon activity timestamps                                38,271
  Security records without separate activity timestamps        411
  UTC-to-IST offset errors                                       0

Activity timestamps and EventRecord timestamps are distinct fields and
must not be interchanged. The reconciliation addendum documents the
reviewed timestamp values and process-instance correlation.

## 7. Investigation Hypotheses and Findings

### H1. Normal Windows service activity

The examined `svchost.exe` activity was assessed using process paths,
command-line context, account context, service configuration, signature
information, and available hash information. The evidence supported
normal Windows service activity for the examined records. Current-state
checks do not establish the complete historical state of the endpoint.

### H2. Explicit-credential activity

A Windows Security Event ID 4648 record was examined and correlated with
available local logon/session context. The event establishes that
explicit credentials were used in the recorded activity; it does not, by
itself, establish successful authentication, credential theft, or
malicious misuse.

### H3. Network activity

Two Sysmon Event ID 3 records were correlated with a Sysmon Event ID 1
process-creation record using the same ProcessGuid. This supports
process-instance correlation. The destination purpose and authorization
were not established by the available event records. TCP port 443 and
ProcessGuid correlation do not independently prove malicious activity.

### Overall assessment

The examined evidence did not establish a confirmed endpoint compromise.
The findings are bounded by the available event logs and supporting
artifacts. Unresolved questions are documented rather than converted
into unsupported conclusions.

## 8. Chain of Custody and Documentation

The case documentation contains acquisition, hashing, and working-copy
records. A documentation gap was identified: the expected COC-001 record
was not present in the verified artifact inventory.

This gap must remain transparently disclosed. It must not be backdated,
reconstructed as though contemporaneous, or represented as an existing
record. Any later explanatory statement should be clearly identified by
its actual creation date and should not replace the missing historical
record.

## 9. Repository Structure

``` text
Digital-Forensics-Incident-Investigation/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── Final-Forensic-Investigation-Report.pdf
│   ├── Final-Forensic-Investigation-Report.docx
│   ├── Investigation-Methodology.md
│   ├── Findings-and-Hypotheses.md
│   ├── Timeline-Reconciliation.md
│   └── Chain-of-Custody-Gap-Statement.md
├── analysis/
│   ├── Forensic-Findings.md
│   ├── Evidence-Correlation.md
│   ├── Timeline-Validation.md
│   ├── Timestamp-Reconciliation.md
│   ├── H1-Normal-Service-Assessment.md
│   ├── H2-Credential-Assessment.md
│   ├── H3-Network-Assessment.md
│   └── Alternative-Explanations.md
├── evidence/
│   ├── README.md
│   ├── screenshots/
│   │   ├── 01-case-directory.png
│   │   ├── 02-evidence-inventory.png
│   │   ├── 03-evidence-integrity-summary.png
│   │   ├── 04-working-copy-verification.png
│   │   ├── 05-chain-of-custody-review.png
│   │   ├── 06-security-event-analysis.png
│   │   ├── 07-sysmon-process-analysis.png
│   │   ├── 08-network-event-analysis.png
│   │   ├── 09-corrected-timeline.png
│   │   ├── 10-timestamp-reconciliation.png
│   │   ├── 11-processguid-correlation.png
│   │   ├── 12-timeline-validation.png
│   │   └── 13-final-report.png
│   └── sanitized/
│       ├── Evidence-Manifest-Sanitized.csv
│       ├── Integrity-Verification-Sanitized.csv
│       └── Sanitized-Event-Samples/
└── scripts/
    └── README.md
```

Only add files that actually exist and have been reviewed. Do not create
empty or misleading artifacts merely to match this illustrative
structure.

## 10. Evidence Screenshots

Screenshots should be genuine captures from the investigation
environment. Redact personal data, usernames, machine identifiers,
private addresses, tokens, and unrelated information before publishing.

  -------------------------------------------------------------------------
  Screenshot                            What it should demonstrate
  ------------------------------------- -----------------------------------
  `01-case-directory.png`               Case workspace and its major
                                        folders

  `02-evidence-inventory.png`           Evidence IDs and file inventory

  `03-evidence-integrity-summary.png`   Sanitized SHA-256 integrity summary

  `04-working-copy-verification.png`    Original/working-copy comparison
                                        result

  `05-chain-of-custody-review.png`      Existing custody entries and
                                        disclosed gap

  `06-security-event-analysis.png`      Relevant Security event examination

  `07-sysmon-process-analysis.png`      Relevant Sysmon process-creation
                                        records

  `08-network-event-analysis.png`       Relevant Sysmon network records

  `09-corrected-timeline.png`           Corrected timeline sample

  `10-timestamp-reconciliation.png`     Activity/EventRecord timestamp
                                        distinction

  `11-processguid-correlation.png`      ProcessGuid-based event correlation

  `12-timeline-validation.png`          Timeline quality-control results

  `13-final-report.png`                 Report title page and sanitized
                                        conclusion
  -------------------------------------------------------------------------

### Screenshot gallery

```{=html}
<!-- Replace each placeholder with the corresponding image after adding it to evidence/screenshots/. -->
```
**01. Case directory**

![01-case-directory.png](evidence/screenshots/01-case-directory.png)

**02. Evidence inventory**

![02-evidence-inventory.png](evidence/screenshots/02-evidence-inventory.png)

**03. Evidence integrity summary**

![03-evidence-integrity-summary.png](evidence/screenshots/03-evidence-integrity-summary.png)

**04. Working-copy verification**

![04-working-copy-verification.png](evidence/screenshots/04-working-copy-verification.png)

**05. Chain-of-custody review**

![05-chain-of-custody-review.png](evidence/screenshots/05-chain-of-custody-review.png)

**06. Windows Security event analysis**

![06-security-event-analysis.png](evidence/screenshots/06-security-event-analysis.png)

**07. Sysmon process analysis**

![07-sysmon-process-analysis.png](evidence/screenshots/07-sysmon-process-analysis.png)

**08. Sysmon network analysis**

![08-network-event-analysis.png](evidence/screenshots/08-network-event-analysis.png)

**09. Corrected timeline**

![09-corrected-timeline.png](evidence/screenshots/09-corrected-timeline.png)

**10. Timestamp reconciliation**

![10-timestamp-reconciliation.png](evidence/screenshots/10-timestamp-reconciliation.png)

**11. ProcessGuid correlation**

![11-processguid-correlation.png](evidence/screenshots/11-processguid-correlation.png)

**12. Timeline validation**

![12-timeline-validation.png](evidence/screenshots/12-timeline-validation.png)

**13. Final report**

![13-final-report.png](evidence/screenshots/13-final-report.png)

## 11. Limitations

-   The examination was based on selected Windows Security and Sysmon
    event-log evidence and supporting artifacts.
-   No full disk image, memory acquisition, or packet capture was
    included in the described evidence set.
-   Application-layer communications and destination ownership/purpose
    were not independently established.
-   Current endpoint configuration and signature checks cannot prove all
    historical endpoint states.
-   Missing telemetry or an absence of matching events does not prove
    that an activity did not occur.
-   The findings do not attribute activity to a person or establish
    malicious intent without corroborating evidence.

## 12. Skills Demonstrated

-   Digital forensics fundamentals
-   Windows Security event-log analysis
-   Sysmon process and network event analysis
-   Evidence acquisition documentation
-   SHA-256 integrity verification
-   Chain-of-custody review
-   Timeline reconstruction and validation
-   UTC/IST timestamp reconciliation
-   ProcessGuid correlation
-   Hypothesis assessment and alternative explanations
-   Forensic reporting and evidence presentation

## 13. Reproducibility Overview

This repository describes the methodology and selected sanitized
outputs. A reviewer can follow the documented workflow using their own
authorized Windows lab and sample event logs:

1.  Establish a case directory and evidence register.
2.  Preserve source event logs and create separate working copies.
3.  Hash source and working files with SHA-256 and compare results.
4.  Examine relevant Security and Sysmon events.
5.  Normalize and correlate event records.
6.  Reconcile activity timestamps with event-record timestamps.
7.  Validate timeline quality and document exceptions.
8.  Assess hypotheses using evidence and alternative explanations.
9.  Produce a report that distinguishes observations, interpretations,
    and limitations.

This is a methodology overview, not a claim that the public repository
contains the original evidence required to reproduce the exact case.

## Author

**Mahesh Yaramala**

Cybersecurity \| SOC Operations \| Digital Forensics \| Incident
Investigation

------------------------------------------------------------------------

**Disclaimer:** This project is presented for educational and defensive
cybersecurity purposes. The case findings are limited to the evidence
and examination described in the report. No claim of confirmed
compromise, attribution, or malicious intent is made beyond what the
available evidence supports.
