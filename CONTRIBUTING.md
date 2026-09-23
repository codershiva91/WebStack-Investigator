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
                │      MariaDB     │      │                  │
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
| Kartikeya   | Fingerprinting, Classification, Evidence & Reporting |

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
