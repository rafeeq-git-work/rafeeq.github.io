**Detailed, implementation-ready effort estimation** for the **Post-Draw Cycle (New Enhancements Only)**.

This excludes all pre-draw and existing workflows.

```
Phase 1: Campaign Setup
├── Corporate creats campaign
├── System configures workflow
└── Chamber approves campaign

Phase 2: Draw & Winner Selection
├── Chamber executes draw
├── Winners finalized
└── For Hand-to-Hand: System generates Prize Cards with QR codes

Phase 3: Delivery Execution
│
├── ONLINE DELIVERY PATH:
│   ├── Corporate sends digital prize externally
│   ├── Corporate uploads digital proof
│   ├── Corporate triggers OTP (optional)
│   └── Winner enters OTP to confirm
│
├── SHIPMENT DELIVERY PATH:
│   ├── Corporate arranges shipping
│   ├── Corporate uploads bill of lading
│   ├── Corporate triggers OTP
│   └── Winner enters OTP upon receipt
│
└── HAND-TO-HAND DELIVERY PATH:
    ├── System provides QR code to winner
    ├── Corporate and winner meet in person
    ├── Corporate scans QR code via app
    ├── Corporate enters winner's OTP
    └── System validates and auto-confirms

Phase 4: Verification & Closure
├── System validates all deliveries complete
├── Admin generates Final Fulfillment Report
├── Report includes method-specific metrics
└── Campaign marked "Post-Draw Completed"

Phase 5: Audit & Oversight
├── Chamber reviews method compliance
├── Ministry analyzes delivery method trends
└── All records preserved for audit
```

---

# ✅ POST-DRAW CYCLE – DETAILED EFFORT ESTIMATION

**Scope: Phase 3, 4, 5 Only (New Enhancements)**
**Stack: .NET Core MVC + Dapper + SQL Server**

---

# 📌 OVERALL DELIVERY ARCHITECTURE (NEW)

After Winner Finalization:

```
Winner → Delivery Record → Delivery Mode
                        → Proof / QR / OTP
                        → Confirmation
                        → Closure
                        → Audit
```

Core New Objects:

* DeliveryRecord
* DeliveryProof
* OTPTransaction
* QRToken
* FulfillmentReport

---

# 📊 DATABASE IMPACT (NEW / MODIFIED)

---

## 1️⃣ New Tables

---

### 1. DeliveryRecords

```sql
DeliveryRecords
---------------
DeliveryId (PK)
CampaignId (FK)
WinnerId (FK)
DeliveryMode (Online/Shipment/HandToHand)
CurrentStatus
IsFinalized
CreatedAt
CreatedBy
UpdatedAt
UpdatedBy
```

Purpose: Central post-draw tracking.

⏱️ Effort: **1 Day**

---

### 2. DeliveryProofs

```sql
DeliveryProofs
--------------
ProofId (PK)
DeliveryId (FK)
FilePath
FileType
UploadedBy
UploadedAt
Checksum
```

Purpose: Stores digital and shipment proofs.

⏱️ Effort: **0.5 Day**

---

### 3. OTPTransactions

```sql
OTPTransactions
---------------
OtpId (PK)
DeliveryId (FK)
OtpCode (Encrypted)
ExpiryTime
IsUsed
Attempts
GeneratedAt
VerifiedAt
```

Purpose: Manages confirmation.

⏱️ Effort: **0.5 Day**

---

### 4. QRDeliveryTokens

```sql
QRDeliveryTokens
----------------
TokenId (PK)
DeliveryId (FK)
QrHash
IsActive
ExpiresAt
UsedAt
CorporateId

```

Purpose: Hand-to-hand security.

⏱️ Effort: **0.5 Day**

---

### 5. AuditDeliveryLogs

```sql
AuditDeliveryLogs
-----------------
LogId (PK)
DeliveryId (FK)
ActionType
UserId
CreatedAt
```

Purpose: Compliance trail.

⏱️ Effort: **0.5 Day**

---

## 2️⃣ Modified Existing Tables

### Campaigns

```sql
+ PostDrawStatus
+ PostDrawCompletedAt
```

### Winners

```sql
+ DeliveryId (FK)
```

### Users

```sql
+ DeviceFingerprint
```

⏱️ Effort: **0.5 Day**

---

### ✅ Total DB Work: **3 Days**

---

# 🖥️ UI SCREENS & ESTIMATION

---

# 1️⃣ Corporate – Delivery Management Screen

---

## Purpose

Allow Corporate to manage all deliveries.

---

## UI Elements

| Component     | Description            |
| ------------- | ---------------------- |
| Table         | Winner + Delivery List |
| Dropdown      | Delivery Mode          |
| Status Badge  | Current State          |
| Upload Button | Proof                  |
| OTP Button    | Trigger                |
| Scan Button   | QR                     |
| Error Label   | Validation             |
| Progress Bar  | Completion             |

---

## Fields

```
Winner Name
Prize
Delivery Mode
Status
Proof Status
OTP Status
Last Action
```

---

## Actions

* Upload proof
* Trigger OTP
* Open QR scanner
* Mark failed
* View history

---

## Validation

* File type
* Max size
* Mode-based rules
* Status lock

---

## Errors

* OTP expired
* Invalid proof
* Duplicate scan
* Permission denied

---

⏱️ Estimation

| Layer         | Days |
| ------------- | ---- |
| UI (Razor)    | 2    |
| JS Validation | 1    |
| Backend       | 2    |
| Integration   | 1    |

✅ **Total: 6 Days**

---

# 2️⃣ Corporate – QR Scanner Screen (Mobile/Web)

---

## Purpose

Enable camera scanning.

---

## UI

| Control      | Purpose  |
| ------------ | -------- |
| Video Tag    | Camera   |
| Scan Button  | Start    |
| OTP Input    | Enter    |
| Confirm      | Submit   |
| Status Panel | Feedback |

---

## Workflow

```
Open → Camera → Scan → Validate → OTP → Confirm
```

---

## Validations

* Camera permission
* Token active
* Status valid

---

## Errors

* Invalid QR
* Used token
* Expired OTP
* Network error

---

⏱️ Estimation

| Layer       | Days |
| ----------- | ---- |
| UI          | 1    |
| Camera API  | 1    |
| QR Library  | 1    |
| Backend API | 1    |
| Testing     | 1    |

✅ **Total: 5 Days**

---

# 3️⃣ Winner – QR + OTP Confirmation Page

---

## Purpose

Winner verification.

---

## UI

```
QR Display
OTP Input
Submit
Resend Button
Timer
```

---

## Validation

* OTP format
* Expiry check
* Rate limit

---

⏱️ Estimation

| Layer     | Days |
| --------- | ---- |
| UI        | 1    |
| Backend   | 1    |
| SMS/Email | 1    |

✅ **Total: 3 Days**

---

# 4️⃣ Admin – Closure & Reporting Screen

---

## Purpose

Finalize campaign.

---

## UI

| Component      | Purpose   |
| -------------- | --------- |
| Status Summary | Progress  |
| Export Button  | PDF       |
| Filters        | Date/Mode |
| Flag Button    | Review    |

---

⏱️ Estimation

| Layer      | Days |
| ---------- | ---- |
| UI         | 1    |
| Backend    | 1    |
| Report API | 1    |

✅ **Total: 3 Days**

---

# 5️⃣ Chamber & Ministry Oversight Dashboard

---

## Purpose

Monitoring and compliance.

---

## UI

```
Campaign List
Delivery Mode Pie
Failure Chart
Trend Graph
Filters
```

---

⏱️ Estimation

| Layer     | Days |
| --------- | ---- |
| UI        | 1.5  |
| Backend   | 1.5  |
| Analytics | 1    |

✅ **Total: 4 Days**

---

# ⚙️ BACKEND / SERVICE LAYER

---

## 1️⃣ DeliveryService

Responsibilities:

* Create records
* Validate transitions
* Lock records
* Handle failures

⏱️ 2 Days

---

## 2️⃣ OTPService

Responsibilities:

* Generate
* Encrypt
* Validate
* Rate limit

⏱️ 1.5 Days

---

## 3️⃣ QRService

Responsibilities:

* Generate tokens
* Validate scans
* Bind device

⏱️ 1.5 Days

---

## 4️⃣ ProofService

Responsibilities:

* Upload
* Hash
* Store
* Validate

⏱️ 1 Day

---

## 5️⃣ AuditService

Responsibilities:

* Log all actions
* Export
* Integrity checks

⏱️ 1 Day

---

✅ Backend Total: **7 Days**

---

# 📑 REPORTING MODULE

---

## Final Fulfillment Report

---

### Fields

| Field    | Source           |
| -------- | ---------------- |
| Campaign | Campaigns        |
| Winner   | Winners          |
| Mode     | DeliveryRecords  |
| Proof    | DeliveryProofs   |
| QR Time  | QRDeliveryTokens |
| OTP Time | OTPTransactions  |
| Failure  | Logs             |

---

### Metrics

* Online %
* Shipment %
* Hand-to-Hand %
* Avg Confirmation Time
* Failure Rate
* Reattempt Rate

---

### Export

* PDF
* Excel

---

⏱️ Estimation

| Task          | Days |
| ------------- | ---- |
| SQL Views     | 1    |
| DTOs          | 0.5  |
| Export Engine | 1    |
| UI            | 0.5  |

✅ **Total: 3 Days**

---

# 🔐 SECURITY & COMPLIANCE (MANDATORY)

| Feature          | Effort |
| ---------------- | ------ |
| Token Encryption | 0.5    |
| OTP Hashing      | 0.5    |
| Rate Limiting    | 0.5    |
| Device Binding   | 0.5    |
| Pen-Test Fix     | 1      |

✅ Total: **3 Days**

---

# 🧪 TESTING & STABILIZATION

| Activity          | Days |
| ----------------- | ---- |
| Unit Tests        | 2    |
| Integration Tests | 2    |
| UAT Support       | 2    |

✅ Total: **6 Days**

---

# 📅 FINAL CONSOLIDATED ESTIMATE

| Module              | Days |
| ------------------- | ---- |
| Database            | 3    |
| Corporate Dashboard | 6    |
| QR Scanner          | 5    |
| Winner Portal       | 3    |
| Admin Reporting     | 3    |
| Oversight Dashboard | 4    |
| Backend Services    | 7    |
| Reports             | 3    |
| Security            | 3    |
| Testing             | 6    |

---

# ✅ TOTAL PROJECT ESTIMATE

### 🎯 **43–45 Working Days**

Assuming:

* 1 Full-Stack Developer
* 1 QA
* Existing auth/SMS infra
* No major UX rework

---

# 📌 DELIVERY PLAN (RECOMMENDED)

| Sprint   | Focus              | Duration |
| -------- | ------------------ | -------- |
| Sprint 1 | DB + Core Delivery | 2 Weeks  |
| Sprint 2 | QR + OTP + UI      | 2 Weeks  |
| Sprint 3 | Reports + Audit    | 2 Weeks  |
| Sprint 4 | Hardening + UAT    | 1 Week   |

---

# 🏁 BUSINESS VALUE

After this enhancement:

✔ Fully automated hand-to-hand
✔ Legally defensible reports
✔ Zero manual confirmation
✔ Regulator-ready audits
✔ Reduced dispute risk
✔ Scalable delivery model
