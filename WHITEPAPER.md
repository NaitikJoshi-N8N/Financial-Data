# AI Spreadsheet Transformation Platform

## Canonical Project Whitepaper (AI-Importable)

---

## 1. Project Identity

**Working Name:** SheetOps (placeholder)

**Category:** AI-assisted data transformation platform

**Primary Function:** Convert user intent and interactive spreadsheet previews into deterministic, auditable Excel/CSV outputs via a JSON-based transformation engine.

**Design Goal:** Enable non-technical users to safely perform complex spreadsheet transformations without manual Excel operations, while maintaining deterministic execution suitable for enterprise use.

---

## 2. Core Principles (Non-Negotiable)

1. **AI never edits data directly**
2. **All transformations are defined in JSON specs**
3. **Execution is deterministic and replayable**
4. **Excel is an interface, not a logic layer**
5. **User preview edits must be first-class inputs**
6. **Every output must be explainable and auditable**

---

## 3. Supported File Types

### Input

* CSV
* XLSX (single or multiple sheets)

### Output

* XLSX (one or many files)
* CSV (optional)

---

## 4. High-Level System Architecture

```
User
 └── Upload Interface
 └── Preview & Edit Interface
 └── Transformation Execution
 └── Output & Download

AI Layer
 └── Intent Interpretation
 └── Schema Inference
 └── Transformation Spec Generation

Execution Engine
 └── JSON Spec Validation
 └── Deterministic DataFrame Operations
 └── File Generation
```

---

## 5. User Interfaces

### 5.1 Upload Interface

**Purpose:** File ingestion and initialization

**Capabilities:**

* Drag-and-drop CSV/XLSX
* Multi-file upload
* File type detection
* Upload progress feedback

---

### 5.2 Preview & Editing Interface (Critical)

**Purpose:** Allow users to control how raw data becomes structured rows and columns before transformation.

**Features:**

* Editable spreadsheet-like grid
* Drag-and-drop rows (reordering)
* Drag-and-drop columns (reordering)
* Inline cell editing
* Insert/delete rows and columns
* Column type assignment (string, number, date, currency)

**Visual Design:**

* Dotted grid background (SVG/CSS)
* Tech-forward aesthetic
* Subtle hover and drag animations

**Output:**

* Canonical table representation
* User-driven schema corrections

---

### 5.3 Transformation Interface

**Purpose:** Review and execute transformation logic

**Features:**

* Display generated JSON transformation spec
* Preview sample diffs (before/after)
* Run transformation job
* Progress indicators and logs

---

### 5.4 Output & Download Interface

**Purpose:** Deliver results

**Features:**

* List of generated files
* Per-file preview snippets
* Download links
* Execution summary and validation report

---

## 6. Transformation Specification (Core Abstraction)

### 6.1 Design Goals

* Declarative
* Deterministic
* Serializable
* Diffable
* Replayable

### 6.2 Example Spec

```json
{
  "version": "1.0",
  "inputs": [
    {"file": "input.xlsx", "sheet": "Sheet1"}
  ],
  "schema": {
    "booking_date": "date",
    "amount": "number",
    "currency": "string"
  },
  "preview_edits": {
    "column_order": ["booking_date", "currency", "amount"],
    "row_operations": []
  },
  "operations": [
    {
      "type": "currency_conversion",
      "source_column": "amount",
      "currency_column": "currency",
      "target_currency": "USD"
    }
  ],
  "outputs": {
    "format": "xlsx",
    "naming": "output.xlsx"
  }
}
```

---

## 7. Supported Operation Types (MVP)

### Structural

* Rename columns
* Reorder columns
* Add derived columns

### Filtering

* Conditional filters
* Null handling

### Aggregation

* Group by
* Sum, average, count

### File Operations

* Split files by column value
* Merge files

### Normalization

* Currency conversion
* Date normalization

### Validation

* Row count checks
* Total reconciliation
* Exception flagging

---

## 8. AI Responsibilities

AI is allowed to:

* Infer column semantics
* Map natural language intent to transformation specs
* Explain transformation plans

AI is NOT allowed to:

* Modify raw data
* Execute transformations
* Write output files

---

## 9. CSV-to-Excel Interpretation Rules

* CSV is parsed into a canonical table
* User preview edits define authoritative row/column structure
* Preview state becomes part of transformation spec
* No assumptions without user confirmation

---

## 10. Execution Engine Behavior

1. Load original data
2. Apply preview edits
3. Validate schema
4. Execute operations sequentially
5. Generate output files
6. Emit execution report

Execution must be:

* Stateless
* Deterministic
* Versioned

---

## 11. Technology Stack

### Preferred Stack (Claude Opus Optimized)

**Frontend:**

* React 18
* Vite
* TypeScript
* TailwindCSS
* TanStack Table
* @dnd-kit
* Framer Motion

**Backend:**

* Python 3.11
* FastAPI
* Pydantic v2
* pandas
* openpyxl

**Storage:**

* S3-compatible object storage

---

### Alternative Stack (Claude Sonnet Optimized)

**Frontend:**

* Next.js 14
* TypeScript
* TailwindCSS
* AG Grid
* Framer Motion

**Backend:**

* Node.js 20
* Route handlers / tRPC
* Danfo.js
* SheetJS (xlsx)

---

## 12. Automation & Reuse

* Transformation specs can be saved
* Specs can be re-run with new inputs
* API-triggered executions supported

---

## 13. Security & Compliance (Baseline)

* Encrypted storage
* No AI training on customer data
* Execution logs retained
* Signed URLs for downloads

---

## 14. Failure Safeguards

* Spec validation before execution
* Diff previews
* User approval required
* Hard execution limits

---

## 15. Positioning

This platform is a **horizontal spreadsheet data operations layer**.

It is not industry-specific. Domain-specific workflows are templates, not core logic.

---

## 16. Canonical Instruction to AI

> Treat this document as the source of truth. Do not invent architecture. Do not bypass the transformation spec. Do not allow AI-driven data mutation. All logic must be expressed via JSON transformation specifications and executed deterministically.

---

**End of Whitepaper**
