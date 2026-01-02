ROLE

IMPORTANT ARCHITECTURAL CONTEXT — NON-NEGOTIABLE

This implementation is NOT a standalone application.

You are designing the FIRST ANCHOR MODULE of a much larger unified internal platform ("Master System") that will eventually support multiple compartments, workflows, teams, and domains.

This module must be implemented as a VERTICAL SLICE that:
- Is fully production-ready on its own
- BUT is explicitly designed to be embedded into a future master system without refactoring

You MUST assume the following future realities and design accordingly:

1. MULTI-MODULE FUTURE
   - This Reports/RACI system is only ONE module
   - Additional modules (e.g. Chargebacks, Fraud QA, Automation, Analytics) will be added later
   - Do NOT hardcode assumptions that this is the only entity, view, or workflow

2. SHARED PLATFORM SPINE
   - Backend architecture must be modular and extensible
   - Service layers, repositories, validation, and routing must be reusable by future modules
   - File and function naming must assume multiple domains will coexist

3. UI AS A PLATFORM, NOT A PAGE
   - The frontend must be structured as an application shell
   - Sidebar navigation must be generic and extensible
   - Views must be pluggable
   - Do NOT design a “single-purpose UI”

4. CONFIG-FIRST MINDSET
   - Assume future behavior will be driven by configuration, not code changes
   - Avoid hardcoding business logic where a config abstraction is reasonable
   - Column definitions, filters, and actions should be structured to generalize later

5. NO THROWAWAY CODE
   - Nothing in this implementation should need to be rewritten when new modules are added
   - Duplication is acceptable ONLY if it is clearly a first-instance of a pattern
   - Prefer explicit, boring, extensible designs over clever shortcuts

6. NAMING & STRUCTURE DISCIPLINE
   - Avoid names that imply finality (e.g. “MainApp”, “OnlyView”, “SingleTable”)
   - Use neutral, platform-oriented naming
   - Treat this as the reference implementation future developers will copy

7. THIS MODULE DEFINES THE BAR
   - This Reports/RACI module is the quality, architecture, and UX benchmark
   - Future modules must be able to follow its patterns exactly

Failure to respect this context is considered a critical error.

Proceed with the implementation ONLY within this framing.

Act as a Principal Engineer and Product Owner responsible for designing and implementing a production-grade Google Apps Script (GAS) web application using HTML Service, with Google Sheets as the backend datastore.

AUTHORITATIVE CONTEXT
You are provided with an uploaded Excel file named “WorkFlow.xlsx”. This file is canonical and defines the system’s truth. It is not illustrative. It is the behavioral, structural, and semantic specification of the system.

You must treat the contents of this file as:

* The authoritative data contract
* The authoritative business logic reference
* The authoritative workflow and intent definition

Even if the data appears incomplete, messy, redundant, or informal, you must preserve its meaning and behavior unless explicitly improving robustness or safety.

You must not ask the user any questions. You must infer missing context and make reasonable, explicit assumptions.

HIGH-LEVEL INTENT (INFERRED AND MUST BE PRESERVED)
The system is intended to manage and operationalize a reporting workflow and responsibility matrix (RACI) for a set of operational, analytical, and fraud/chargeback-related reports.

Each row in the source file represents a report artifact with:

* A unique report identity
* A classification (e.g., Master, Derived)
* A frequency (e.g., Daily, Weekly)
* Clearly defined RACI ownership roles:

  * R = Responsible
  * A = Accountable
  * C = Consulted
  * I = Informed

The application must allow stakeholders to view, manage, filter, audit, and safely modify this information while preserving referential integrity and historical accountability.

SOURCE FILE STRUCTURE (MUST BE REVERSE-ENGINEERED AND PRESERVED)
From the uploaded file, infer and formalize the following primary sheet schema (names must be preserved exactly unless normalization is explicitly justified):

Primary Sheet: “Reports” (derived from Sheet1)
Columns:

* Report Name (string, required, unique logical identifier)
* Classification (string, required; examples include “Master”, “Derived”)
* Frequency (string, required; examples include “Daily”, “Weekly”)
* R Responsible (string, required)
* A Accountable (string, required)
* C Consulted (string, nullable)
* I Informed (string, nullable)

Observed invariants and rules:

* Rows with missing “Report Name” are non-data rows and must be ignored safely.
* “Report Name” uniqueness must be enforced at the application layer.
* Classification and Frequency behave as categorical fields and must be filterable.
* R and A must never be empty for a valid report.
* C and I may be empty.
* No implicit ordering may be assumed beyond what exists in the sheet.

You must preserve all existing rows and their semantics exactly.

APPLICATION REQUIREMENTS

GENERAL
Build a modern, dark-mode, production-grade Google Apps Script web application using HTML Service.

Architecture requirements:

* Strong separation of concerns between frontend (HTML/CSS/JS) and backend (Apps Script)
* All Google Sheets access must be abstracted behind a data access layer
* No frontend code may access Sheets directly
* Defensive programming throughout

DATA MODEL & STORAGE

* Use a single Google Sheets workbook as the datastore
* Multiple sheets must be used, even if only one is present initially
* At minimum, implement:

  1. Reports (canonical data, derived from the uploaded file)
  2. AuditLog (append-only)

AuditLog schema:

* Timestamp (ISO string)
* User Email
* Action (CREATE, UPDATE, DELETE)
* Entity Type (e.g., REPORT)
* Entity Identifier (Report Name)
* Before State (JSON string)
* After State (JSON string)

All write operations must:

* Use LockService to prevent concurrent corruption
* Validate inputs against the formalized schema
* Fail safely with normalized error messages
* Write to AuditLog atomically with the data change

BACKEND BEHAVIOR
You must implement:

* Full CRUD operations for Reports
* Server-side validation enforcing all inferred rules
* Schema enforcement and normalization
* Centralized error handling
* Centralized logging
* Explicit handling of empty, malformed, or duplicate rows in the source data
* Idempotent reads

UI / UX REQUIREMENTS

The frontend must be:

* Fully dark-mode by default
* Modern and polished (material-inspired or equivalent)
* Responsive

UI features:

* Sidebar navigation
* Main table view of reports
* Column-based sorting
* Filters for Classification and Frequency
* Text search on Report Name and role fields
* Create/Edit modal with form validation
* Delete confirmation modal
* Bulk actions (at minimum: bulk delete with safeguards)
* Loading states
* Empty states
* Error states with user-safe messaging

All UI actions must call backend functions via google.script.run and handle success/failure explicitly.

SECURITY & SAFETY

* Never trust client input
* Prevent accidental overwrites
* Prevent partial writes
* Ensure auditability of all changes
* Do not silently drop data

OUTPUT REQUIREMENTS

You must output, in this exact order:

1. System Overview
   Clear explanation of what the system does and how it maps to the uploaded file.

2. Architecture Explanation
   Frontend, backend, data access, locking, and audit strategy.

3. Sheet Schemas
   Explicit schemas for every sheet, including field types and constraints.

4. Full Google Apps Script Code

   * Code.gs (or equivalent)
   * All backend logic
   * No pseudocode
   * Fully runnable

5. Full HTML / CSS / JavaScript

   * Single or multiple HTML files as appropriate
   * Embedded or separated CSS/JS as needed
   * Fully runnable in GAS HTML Service
   * Dark-mode styling included

6. Deployment Instructions
   Step-by-step instructions to deploy as a web app.

7. Extensibility Notes
   How to safely add new fields, sheets, or workflows.

8. Known Limitations
   Honest, explicit limitations based on inferred constraints.

