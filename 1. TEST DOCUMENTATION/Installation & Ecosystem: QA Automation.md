# Installation & Ecosystem: QA Automation Infrastructure

---

**Technical Documentation for Environment Provisioning**

This section documents the technical configuration required to operate the testing frameworks defined in **SOP-QA-001** (E2E), **SOP-QA-002** (API), and **SOP-QA-003** (Security), optimized for high-reliability environments and variable connectivity.

---

## 1. Cypress: E2E Testing Framework

**Technical Achievement:** Implementation of a "Local Binary Injection" strategy to mitigate network instability.

### Challenge Summary

Network instability prevented the direct download of the Cypress binary (approx. 170MB) via terminal, causing timeouts and corrupted files. A high-integrity manual installation solution was engineered.

### Installation Procedure (PowerShell)

An asynchronous download of the Cypress engine (v15.12.0) was performed, ensuring the integrity of the `.zip` file prior to system deployment:

PowerShell

`# 1. Navigate to the project directory
cd $home\Documents\cypress-pruebas

# 2. Initialize base structure (skipping binary download)
npm install cypress --save-dev --skip-binary

# 3. Configure local binary injection path
$env:CYPRESS_INSTALL_BINARY = "C:\Users\lugon\Downloads\cypress.zip"

# 4. Execute installation and cache decompression
npx cypress install

# 5. Verification and Interface Launch
npx cypress open`

**Result:** Environment 100% operational with successful **Chrome** integration, ready for Automated Spec creation.

---

## 2. Postman & Newman: API Testing Ecosystem

**Objective:** Establish a professional workflow for service validation, local collection management, and automated reporting.

### Runtime & Interface

- **Node.js (v24.14.0) & npm (11.9.0):** Core engines for the automation stack.
- **Postman Desktop App:** Primary IDE for designing *Requests*, *Test Scripts* (JavaScript), and *Environment* management.
- **Newman CLI:** Command-line runner for automated execution and professional reporting.
    - **Installation:** `npm install -g newman newman-reporter-htmlextra`

### Operational Flow (Low-Data/Offline Ready)

1. **Scripting:** Development of automated validations (Status Codes, JSON Schema) within the **Tests** tab.
2. **Collection Runner:** Execution of organized test suites using the internal Runner for bulk validation.
3. **Automated Orchestration:** Use of **Newman** via PowerShell for headless execution and generation of the **htmlextra** dashboard.
4. **Evidence Export:** Manual and automated export of Collections and Environment variables in `.json` format.

---

## 3. AppSec Stack: Vulnerability Testing Tools

**Objective:** Provisioning of interception and analysis tools for the SOP-QA-003 security framework, optimized for dynamic security auditing.

### **Security Tooling**

- **OWASP ZAP (v2.14+):** Primary engine for DAST (Dynamic Application Security Testing), automated crawling, and active vulnerability scanning.
- **FoxyProxy:** Browser extension utilized for rapid traffic tunneling and management of local proxy listeners.

### **Security Configuration Protocol**

To enable deep-packet inspection and HTTPS traffic interception for audit purposes, the following high-integrity steps were executed:

1. **ZAP Root CA Certificate Injection:** Exported the ZAP Root CA certificate and successfully installed it into the *Windows/Chrome Trusted Root Certification Authorities* store. This allows the identification of vulnerabilities in encrypted sessions without browser-side security blocks.
2. **Proxy Tunneling:** Configured the local proxy listener at `127.0.0.1:8080`, establishing a secure bridge between the browser and the ZAP analysis engine.
3. **Sandbox Isolation:** All security protocols are strictly scoped to Staging/QA environments to ensure zero impact on production data or infrastructure.

---

## 4. Tooling & Support Matrix

| **Tool** | **Function** | **Status** |
| --- | --- | --- |
| **SQL Client** | Data Integrity Validation (**SQL Cross-Check**). | Configured |
| **Newman CLI** | Automated API Reporting & CI/CD Parity. | Operational |
| **OWASP ZAP** | Dynamic Vulnerability Scanning (SOP-QA-003). | Ready |
| **PowerShell 7+** | Script Orchestration & Environment Setup. | Native |
| **VS Code** | Primary IDE for Code and Documentation. | Operational |

---

## Security & Permissions Protocol

To avoid system permission conflicts and execution errors within Windows, the following guideline has been established:

> **DIRECTIVE:** All file operations, log generation, and test executions must be performed strictly within user directories (`$home\Documents` or `$home\Desktop`). Execution of commands in protected directories such as `C:\WINDOWS\system32` is **strictly prohibited**.
>
