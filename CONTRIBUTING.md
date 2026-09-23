# WebStack Inspector — System Architecture

**Project:** Design and Development of Framework for Determination & Categorization of Online Gaming Applications

**Module:** WebStack Inspector
**Architecture Version:** v1.0-MVP
**Status:** Baseline Architecture

---

## 1. Purpose

WebStack Inspector is a technical web application investigation platform designed to analyze web-based gaming applications.

The system performs **static and dynamic analysis** to identify web resources, JavaScript libraries, frameworks, SDKs, APIs, WebSockets, third-party services and other technical components.

The collected information is processed through fingerprinting, classification and evidence generation to produce a structured investigation report.

---

## 2. System Architecture

```text
                         ┌─────────────────────────┐
                         │     INVESTIGATOR / USER  │
                         └────────────┬────────────┘
                                      │
                                  Target URL
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      WEB UI / DASHBOARD  │
                         │        Streamlit         │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      FASTAPI BACKEND     │
                         │     Scan / Job Manager   │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      TARGET MANAGER      │
                         │                         │
                         │ • URL Validation        │
                         │ • Redirect Handling     │
                         │ • Scan Management       │
                         │ • Target Metadata       │
                         └────────────┬────────────┘
                                      │
                       ┌──────────────┴──────────────┐
                       │                             │
                       ▼                             ▼
              ┌──────────────────┐        ┌──────────────────┐
              │ STATIC ANALYZER  │        │ DYNAMIC ANALYZER │
              │                  │        │                  │
              │ • HTML           │        │ • Browser        │
              │ • JavaScript     │        │ • Runtime JS     │
              │ • CSS            │        │ • Network        │
              │ • Iframes        │        │ • XHR / Fetch    │
              │ • Resources      │        │ • APIs           │
              │ • Domains        │        │ • WebSockets     │
              └────────┬─────────┘        │ • Runtime DOM    │
                       │                  └────────┬─────────┘
                       │                           │
                       └──────────────┬────────────┘
                                      ▼
                         ┌─────────────────────────┐
                         │   RESOURCE NORMALIZER    │
                         │                         │
                         │ • URL Normalization     │
                         │ • Deduplication         │
                         │ • Static + Dynamic      │
                         │ • First / Third Party   │
                         │ • Provenance            │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │    FINGERPRINT ENGINE    │
                         │                         │
                         │ • URL Signatures        │
                         │ • JS Signatures         │
                         │ • DOM Indicators        │
                         │ • Runtime Indicators    │
                         │ • Domain Signatures     │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │  CLASSIFICATION ENGINE   │
                         │                         │
                         │ • JS Libraries          │
                         │ • Gaming Frameworks      │
                         │ • Analytics / Tracking   │
                         │ • Authentication        │
                         │ • Payment               │
                         │ • Communication         │
                         │ • External Services     │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │      EVIDENCE ENGINE     │
                         │                         │
                         │ • Findings              │
                         │ • Evidence              │
                         │ • Detection Method      │
                         │ • Provenance            │
                         │ • Confidence             │
                         │ • Timestamp             │
                         └────────────┬────────────┘
                                      │
                         ┌────────────┴────────────┐
                         │                         │
                         ▼                         ▼
                ┌──────────────────┐      ┌──────────────────┐
                │     DATABASE     │      │  REPORT ENGINE   │
                │                  │      │                  │
                │                  │      │ • JSON           │
                │ • Scans          │      │ • HTML           │
                │ • Resources      │      │ • PDF            │
                │ • Findings       │      │ • Evidence       │
                │ • API Requests   │      │ • Summary        │
                │ • WebSockets     │      └────────┬─────────┘
                │ • Evidence       │               │
                └──────────────────┘               │
                                                  ▼
                                      ┌────────────────────────┐
                                      │   INVESTIGATION REPORT  │
                                      └────────────────────────┘
```

---

## 3. End-to-End Data Flow

```text
Target URL
    ↓
Target Manager
    ↓
┌───────────────────┐
│                   │
▼                   ▼
Static Analysis   Dynamic Analysis
│                   │
│                   ├── Runtime JavaScript
│                   ├── API / XHR / Fetch
│                   ├── WebSocket
│                   ├── Runtime DOM
│                   └── Dynamic Resources
│
└──────────┬────────┘
           ↓
Resource Normalization
           ↓
Technology Fingerprinting
           ↓
Technology Classification
           ↓
Evidence & Confidence
           ↓
Database + Report
           ↓
Dashboard / Investigation Output
```

---

## 4. Major Modules

| Module                | Responsibility                                    |
| --------------------- | ------------------------------------------------- |
| Target Manager        | Target validation, redirects and scan management  |
| Static Analyzer       | HTML, JavaScript and resource extraction          |
| Dynamic Analyzer      | Runtime browser analysis                          |
| Network Analyzer      | API, XHR/Fetch and WebSocket detection            |
| Resource Normalizer   | Resource merging, normalization and deduplication |
| Fingerprint Engine    | Technology and SDK identification                 |
| Classification Engine | Technology/service categorization                 |
| Evidence Engine       | Evidence, provenance and confidence               |
| Database              | Persistent storage of scan results                |
| Report Engine         | Structured investigation reports                  |
| Web UI                | Scan initiation and result visualization          |

---

## 5. Analysis Approach

### Static Analysis

Static analysis examines the web application's available source and resources without requiring full runtime execution.

It focuses on:

* HTML
* JavaScript
* CSS
* Iframes
* External resources
* Domains
* First-party / third-party resources

### Dynamic Analysis

Dynamic analysis executes the application in a browser environment and observes runtime behavior.

It focuses on:

* Dynamically loaded JavaScript
* Network requests
* API endpoints
* XHR / Fetch
* WebSockets
* Runtime DOM changes
* Dynamic resources

---

## 6. Technology Identification

The Fingerprint Engine uses multiple technical signals to identify technologies.

```text
URL Signature
      +
JavaScript Signature
      +
DOM Indicator
      +
Runtime Indicator
      +
Domain / Provider Signature
      ↓
Technology Identification
```

Initial technology categories include:

* JavaScript Libraries
* Web Frameworks
* Gaming Frameworks
* Analytics / Tracking
* Authentication
* Payment
* Communication
* External Services

---

## 7. Evidence Model

Every important finding should maintain supporting evidence.

```text
Finding
 ├── Technology / Service
 ├── Resource
 ├── Detection Method
 ├── Evidence
 ├── Provenance
 ├── Confidence
 └── Timestamp
```

Detection methods may include:

* Static
* Dynamic
* Both

The system is designed to make findings **traceable and explainable**.

---

## 8. Team Responsibilities

| Team Member | Responsibility                                       |
| ----------- | ---------------------------------------------------- |
| Renuka      | Static Analysis & Resource Extraction                |
| Shivam      | Dynamic Analysis & Network Intelligence              |
| Kartikeya   | Fingerprinting and Classification

 Common for all three - Evidence & Reporting 

---

## 9. Technology Stack

| Layer            | Technology              |
| ---------------- | ----------------------- |
| Programming      | Python                  |
| Backend API      | FastAPI                 |
| Static Analysis  | HTTPX / BeautifulSoup   |
| Dynamic Analysis | Playwright + Chromium   |
| Database         | MariaDB                 |
| UI               | Streamlit               |
| Fingerprinting   | Python-based signatures |
| Testing          | Pytest                  |
| Version Control  | Git / GitHub            |

---

## 10. MVP Development Sequence

```text
Research
   ↓
Requirements
   ↓
Architecture
   ↓
Data Model
   ↓
Project Structure
   ↓
Static Analyzer
   ↓
Dynamic Analyzer
   ↓
Fingerprint Engine
   ↓
Classification Engine
   ↓
Evidence Engine
   ↓
Report Generation
   ↓
UI Integration
   ↓
Testing
   ↓
MVP Validation
```

---

## 11. Current Status

### Completed

* Project problem understanding
* Requirement study
* Static analysis research
* Dynamic analysis research
* Fingerprinting research
* Classification approach
* Evidence model
* Initial system architecture
* Team module division
* MVP scope definition

### Current Phase

**Technical Design & Data Model**

### Next Steps

1. Finalize database schema
2. Define module interfaces
3. Finalize Dynamic Analyzer design
4. Finalize browser automation approach
5. Begin MVP implementation

---

## 12. Architecture Principle

> **WebStack Inspector is designed as an evidence-collection and technology-identification platform, not merely as a website scanner.**

The system separates:

**Observation → Identification → Classification → Evidence → Reporting**

This separation allows the analysis to remain modular, traceable and extensible.

---

## 13. Architecture Status

**Version:** v1.0-MVP
**Status:** Baseline Architecture
**Last Updated:** September 2026

Future architectural changes should be documented through versioned technical decisions rather than modifying the baseline without explanation.










# WebStack Inspector — Data Model & Database Architecture

**Project:** Design and Development of Framework for Determination & Categorization of Online Gaming Applications
**Module:** WebStack Inspector
**Version:** v1.0-MVP
**Status:** Initial Data Model

---

## 1. Purpose

The database is responsible for storing the information generated during web application analysis.

The data model is designed to maintain the relationship between:

**Scan → Target → Resources → Network Activity → Technologies → Findings → Evidence**

The model should support both static and dynamic analysis results.

---

# 2. High-Level Data Model

```text
                         ┌──────────────┐
                         │     SCAN     │
                         └──────┬───────┘
                                │
                                │ 1:N
                                ▼
                         ┌──────────────┐
                         │   RESOURCE   │
                         └──────┬───────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
       ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
       │ API REQUEST  │  │  WEBSOCKET   │  │ TECHNOLOGY   │
       └──────────────┘  └──────────────┘  └──────┬───────┘
                                                   │
                                                   │
                                                   ▼
                                           ┌──────────────┐
                                           │   FINDING    │
                                           └──────┬───────┘
                                                  │
                                                  ▼
                                           ┌──────────────┐
                                           │   EVIDENCE   │
                                           └──────────────┘
```

---

# 3. Core Entities

The MVP will initially use the following core entities:

1. `scans`
2. `resources`
3. `api_requests`
4. `websockets`
5. `technologies`
6. `findings`
7. `evidence`

---

# 4. Scan

The `scans` entity represents one complete investigation of a target.

### Purpose

Stores:

* Target URL
* Final URL
* Scan status
* Start/end time
* Analysis metadata

### Conceptual Structure

```text
scans
-----
id
target_url
final_url
status
started_at
completed_at
error_message
created_atCONTRIBUTING.md
```

### Possible Status Values

```text
QUEUED
RUNNING
COMPLETED
FAILED
CANCELLED
```

---

# 5. Resources

The `resources` entity represents resources discovered through static or dynamic analysis.

Examples:

```text
JavaScript
CSS
Image
Iframe
Font
Media
Document
Other
```

### Conceptual Structure

```text
resources
---------
id
scan_id
url
domain
resource_type
mime_type
method
status_code
first_party
found_static
found_dynamic
parent_url
initiator
content_hash
created_at
```

### Important Principle

A resource discovered by both analyzers should be stored as **one normalized resource**, rather than creating duplicate records.

Example:

```text
Static Analyzer
      │
      └── app.js
             │
             ▼
       Resource Normalizer
             ▲
             │
      Dynamic Analyzer
      └── app.js

             ↓

       ONE RESOURCE
       found_static = true
       found_dynamic = true
```

---

# 6. API Requests

The `api_requests` entity stores HTTP requests identified as relevant API/XHR/Fetch activity.

### Conceptual Structure

```text
api_requests
------------
id
scan_id
resource_id
url
method
domain
request_type
status_code
content_type
initiator
timestamp
created_at
```

### Example

```json
{
  "method": "POST",
  "url": "https://example.com/api/login",
  "request_type": "xhr",
  "status_code": 200
}
```

---

# 7. WebSockets

The `websockets` entity stores WebSocket connection information.

### Conceptual Structure

```text
websockets
----------
id
scan_id
url
domain
protocol
timestamp
created_at
```

### Example

```text
wss://game.example.com/socket
```

For the MVP, the focus is on identifying and recording WebSocket connections. Deep payload analysis is outside the initial scope.

---

# 8. Technologies

The `technologies` entity represents known technologies identified by the fingerprinting engine.

Examples:

```text
React
Vue
Angular
jQuery
Phaser
PixiJS
Google Analytics
Razorpay
Stripe
```

### Conceptual Structure

```text
technologies
------------
id
name
category
provider
version
description
created_at
```

### Initial Categories

```text
javascript_library
web_framework
gaming_framework
analytics
authentication
payment
communication
external_service
```

---

# 9. Findings

A `finding` represents a technology or service identified during a particular scan.

This is different from the `technology` entity.

### Technology

Represents the known technology:

```text
Phaser
```

### Finding

Represents:

```text
Phaser detected
in Scan #102
through app.js
using static + dynamic evidence
with high confidence
```

### Conceptual Structure

```text
findings
--------
id
scan_id
technology_id
resource_id
detection_method
confidence
status
created_at
```

### Detection Methods

```text
STATIC
DYNAMIC
BOTH
```

---

# 10. Evidence

The `evidence` entity stores the information supporting a finding.

### Conceptual Structure

```text
evidence
--------
id
finding_id
evidence_type
source
description
value
provenance
timestamp
created_at
```

### Possible Evidence Types

```text
URL_SIGNATURE
JS_SIGNATURE
DOM_INDICATOR
RUNTIME_INDICATOR
DOMAIN_SIGNATURE
NETWORK_ACTIVITY
RESOURCE
```

---

# 11. Entity Relationships

```text
SCANS
  │
  ├──────────────< RESOURCES
  │
  ├──────────────< API_REQUESTS
  │
  ├──────────────< WEBSOCKETS
  │
  └──────────────< FINDINGS
                         │
                         ├──────> TECHNOLOGIES
                         │
                         └──────< EVIDENCE
```

### Relationship Summary

| Relationship          | Type |
| --------------------- | ---- |
| Scan → Resources      | 1    |
| Scan → API Requests   | 1    |
| Scan → WebSockets     | 1    |
| Scan → Findings       | 1    |
| Technology → Findings | 1    |
| Resource → Findings   | 1    |
| Finding → Evidence    | 1    |

---

# 12. Data Flow Through the Database

```text
Target URL
    ↓
Scan Created
    ↓
Static Analysis ──────┐
                      │
Dynamic Analysis ─────┤
                      ▼
                Resources
                      │
             Resource Normalization
                      │
                      ▼
              Fingerprint Engine
                      │
                      ▼
                  Findings
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
    Technologies              Evidence
          │                       │
          └───────────┬───────────┘
                      ▼
                  Database
```

---

# 13. Module-to-Database Responsibility

| Module                | Main Data Produced                   |
| --------------------- | ------------------------------------ |
| Target Manager        | Scan                                 |
| Static Analyzer       | Resources                            |
| Dynamic Analyzer      | Resources                            |
| Network Analyzer      | API Requests / WebSockets            |
| Resource Normalizer   | Normalized Resources                 |
| Fingerprint Engine    | Technology Candidates                |
| Classification Engine | Technology Categories                |
| Evidence Engine       | Findings / Evidence                  |
| Report Engine         | Read-only consumption of stored data |

---

# 14. Data Ownership Principle

Each module should produce structured data rather than directly modifying unrelated modules.

Example:

```text
Static Analyzer
       ↓
Resource Record

Dynamic Analyzer
       ↓
Resource + Network Records

Normalizer
       ↓
Normalized Resource

Fingerprint Engine
       ↓
Technology Candidate

Classification Engine
       ↓
Technology Category

Evidence Engine
       ↓
Finding + Evidence
```

This keeps the architecture modular and makes individual components easier to test.

---

# 15. MVP Data Principles

The database should follow these principles:

### 1. Traceability

Every finding should be linked back to its scan.

### 2. Provenance

The system should record whether information came from static or dynamic analysis.

### 3. Deduplication

The same resource should not be unnecessarily stored multiple times.

### 4. Extensibility

New technology categories and evidence types should be addable.

### 5. Reproducibility

Previous scan results should remain available for later review.

### 6. Separation

Resources, technologies, findings and evidence should remain separate concepts.

---

# 16. Example Investigation Record

Conceptually:

```text
SCAN
│
├── Target:
│   https://example-game.com
│
├── Resources:
│   ├── /js/app.js
│   ├── /js/game.js
│   └── /js/analytics.js
│
├── API Requests:
│   ├── POST /api/login
│   ├── POST /api/game
│   └── GET /api/user
│
├── WebSockets:
│   └── wss://example-game.com/socket
│
└── Findings:
    │
    ├── Phaser
    │   ├── Category: Gaming Framework
    │   ├── Method: STATIC + DYNAMIC
    │   └── Confidence: HIGH
    │
    └── Analytics Technology
        ├── Category: Analytics
        ├── Method: STATIC
        └── Confidence: MEDIUM
```

---

# 17. Current Database Decision

**Database:** MariaDB

The database will initially support the MVP and can be extended as the broader framework develops.

The current model is intentionally kept focused on the technical web-analysis requirements rather than attempting to model the complete future regulatory/determination framework.

---

# 18. Current Status

**Version:** v1.0-MVP

**Status:** Initial Data Model

### Completed

* Core entities identified
* Entity relationships defined
* Static/dynamic data flow defined
* Finding/evidence separation defined
* Module data responsibilities defined

### Next

1. Convert conceptual model into SQL schema
2. Define primary/foreign keys
3. Define indexes and constraints
4. Define JSON/API data contracts
5. Implement database layer
6. Connect Static Analyzer
7. Connect Dynamic Analyzer

---

## Design Principle

> **Store what was observed, where it was observed, how it was detected, and what finding it supports.**

This principle will be used throughout the WebStack Inspector data architecture.

