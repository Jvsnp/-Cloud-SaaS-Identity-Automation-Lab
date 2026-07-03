# Cloud & SaaS Identity Automation Lab

## 🚀 Project Overview
An end-to-end administrative pipeline simulating real-world Cloud & SaaS Identity and Access Management (IAM) workflows. This lab uses Infrastructure as Code (IaC) to provision a secure database container, maps raw corporate identity telemetry exported from Microsoft Entra ID (Azure AD), and leverages PowerShell scripting to automate database ingestion and enterprise licensing alignment.

## 🛠️ Tech Stack & Tools
* **Infrastructure as Code (IaC):** Terraform (Docker Provider)
* **Containerization:** Docker Desktop (MS SQL Server 2022)
* **Automation Scripting:** PowerShell 7 / Core
* **Database Management:** T-SQL / SQL Server Extension for VS Code
* **Identity Source:** Microsoft Entra ID (Azure AD mock tenant data)

---

## 📋 Phased Implementation & Architecture

### Phase A: Identity Telemetry Extraction
* Isolated a subset of **11 target user accounts** from a raw enterprise Entra ID export (`exportUsers_2026-7-3.csv`).
* Evaluated standard cloud schema attributes (`userPrincipalName`, `displayName`, `accountEnabled`, `jobTitle`).

### Phase B: Staging Infrastructure & T-SQL Data Mapping
* **Infrastructure Provisioning:** Configured a local containerized database environment using **Terraform** to deploy a secure, isolated `mssql/server` engine on Docker.
* **Relational Schema Design:** Connected natively via VS Code and engineered a custom data staging table (`StagedIdentities`) with constraints mapped precisely to cloud attributes.
* **Compliance Auditing:** Executed analytical T-SQL queries to isolate and verify technical personnel profiles, identifying inactive accounts and separating engineering/IT staff from executives.

### Phase C: Automated Ingestion Pipeline
* Engineered an administrative **PowerShell automation script (`sync_identities.ps1`)** to eliminate manual data entry.
* **The Pipeline Workflow:**
  1. Programmatically loads and parses the target CSV file.
  2. Executes safe table truncation (`TRUNCATE`) via SQL connection strings to guarantee a fresh synchronization cycle.
  3. Iterates through the collection, formatting accounts into uniform SQL primitives (handling string sanitation and binary type-casting for `AccountEnabled`).
  4. Automatically updates corporate license metadata, upgrading qualified operational users (matching `%Engineer%` and `%Admin%` wildcards) to the **`SPECTRUM_E5_SECURITY`** license profile hands-free.

---

## 📊 Sample Compliance Audit Output
```sql
SELECT DisplayName, JobTitle, TargetLicense FROM StagedIdentities;
