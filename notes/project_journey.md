# SwiftUPI — Project Journey

> **SwiftUPI** — exploring offline UPI payments through Bluetooth mesh.
>
> This is a living development log. It is created **before development starts** and updated after each phase is actually completed.
>
> **Rule:** Every phase starts as `Pending`. A phase is changed to `Completed` only after the work is built, run/tested where applicable, and pushed to GitHub.

---

## Project Goal

Explore how an offline-first payment flow could work when the sender does not have direct internet connectivity.

The proposed flow:

```text
Sender
  ↓
Encrypted payment packet
  ↓
Nearby devices / mesh
  ↓
Bridge device regains internet
  ↓
Backend ingestion
  ↓
Validation
  ↓
Settlement
```

The project focuses on understanding:

- Mesh networking concepts
- Hybrid encryption
- Idempotency
- Replay protection
- Deferred settlement
- Transactional backend processing
- Concurrency

### Scope

SwiftUPI is a **prototype / teaching project**.

It is not intended to be:

- Real offline UPI
- A replacement for UPI
- A production payment system
- A real Bluetooth implementation
- An NPCI or bank-integrated payment application

The current technical direction is a **software-simulated mesh with a Spring Boot backend**.

---

# Phase 1: Business Requirements (BRD)

**Status: Completed**

## Objective

Define the business/problem context before designing the product.

## BRD should cover

- Problem statement
- Why offline payment is worth exploring
- Target problem space
- Business/product objective
- High-level scope
- Out of scope
- Success criteria
- Major assumptions
- Major risks

## Deliverable

```text
notes/
└── BRD.md
```

## Completion condition

BRD is written, reviewed, and pushed to GitHub.

---

# Phase 2: Product Requirements (PRD)

**Status: Completed**

## Objective

Translate the problem into a clear product definition.

## PRD should cover

- User/problem context
- Product goal
- User journey
- Core payment flow
- Functional requirements
- Non-functional requirements
- MVP scope
- Acceptance criteria
- User-facing limitations
- Future considerations

## Planned product flow

```text
Compose Payment
      ↓
Encrypt Payment
      ↓
Inject into Mesh
      ↓
Move Through Nearby Devices
      ↓
Bridge Regains Internet
      ↓
Send Packet to Backend
      ↓
Validate
      ↓
Settle Once
```

## Deliverable

```text
notes/
└── PRD.md
```

## Completion condition

PRD is written, reviewed, and pushed to GitHub.

---

# Phase 3: Technical Requirements (TRD)

**Status: Completed**

## Objective

Translate the product requirements into a technical design that can actually be implemented.

## TRD should cover

- System architecture
- Technology stack
- Component responsibilities
- Data model
- Encryption approach
- Mesh simulation approach
- Idempotency approach
- Replay protection
- Settlement flow
- API design
- Persistence
- Testing strategy
- Technical limitations
- Production evolution

## Planned architecture

```text
Offline Sender
     ↓
PaymentInstruction
     ↓
Hybrid Encryption
     ↓
MeshPacket
     ↓
Virtual Mesh
     ↓
Bridge Device
     ↓
Spring Boot Backend
     ↓
Idempotency
     ↓
Decryption + Validation
     ↓
Settlement
     ↓
Transaction Ledger
```

## Planned technology direction

- Java 17+
- Spring Boot
- Maven
- Spring Data JPA
- H2 in-memory database
- RSA-OAEP
- AES-256-GCM
- SHA-256
- ConcurrentHashMap
- Server-side HTML dashboard

The mesh is planned as a **software simulation**, not real Bluetooth.

## Deliverable

```text
notes/
└── TRD.md
```

## Completion condition

TRD is written, reviewed, and pushed to GitHub.

---

# Phase 4: Initial README

**Status: Completed**

## Objective

Add a short README before development begins.

It should explain:

- What SwiftUPI is
- Why it is being built
- Current project status
- Documentation available in `notes/`
- That development is in progress

This is **not the final README**.

## Deliverable

```text
README.md
```

## Completion condition

Initial README is written and pushed to GitHub.

---

# Phase 5: Spring Boot Project Foundation

**Status: Completed**

## Objective

Create the minimal runnable Spring Boot application.

## Files

```text
pom.xml
mvnw
mvnw.cmd
src/main/java/com/demo/upimesh/UpiMeshApplication.java
src/main/resources/application.properties
```

## Completion condition

The application starts successfully and the foundation is pushed to GitHub.

---

# Phase 6: Payment & Domain Model

**Status: Completed**

## Objective

Create the core objects required to represent accounts, payments, mesh packets and transactions.

## Files

```text
src/main/java/com/demo/upimesh/model/
├── Account.java
├── AccountRepository.java
├── MeshPacket.java
├── PaymentInstruction.java
├── Transaction.java
└── TransactionRepository.java
```

## Completion condition

The model layer compiles and the phase is pushed to GitHub.

---

# Phase 7: Payment Security

**Status: Pending**

## Objective

Protect payment information while it travels through intermediary devices.

## Files

```text
src/main/java/com/demo/upimesh/crypto/
├── ServerKeyHolder.java
└── HybridCryptoService.java
```

## Planned approach

```text
PaymentInstruction
      ↓
Fresh AES-256 key
      ↓
AES-256-GCM encryption
      ↓
RSA-OAEP encrypts AES key
      ↓
Encrypted payload
```

## Completion condition

Encryption/decryption behaviour is implemented and verified, then pushed to GitHub.

---

# Phase 8: Virtual Device

**Status: Pending**

## Objective

Create the simulated phone/device abstraction used by the mesh.

## File

```text
src/main/java/com/demo/upimesh/service/
└── VirtualDevice.java
```

## Planned behaviour

A virtual device should represent:

- Device identity
- Internet availability
- Packets currently held
- Mesh participation

## Completion condition

The virtual device component works with the planned model and is pushed to GitHub.

---

# Phase 9: Offline Mesh Simulation

**Status: Pending**

## Objective

Simulate packet movement between nearby devices.

## File

```text
src/main/java/com/demo/upimesh/service/
└── MeshSimulatorService.java
```

## Planned flow

```text
phone-alice
    ↓
Virtual devices
    ↓
Gossip rounds
    ↓
TTL decreases
    ↓
phone-bridge
```

## Important limitation

This phase does **not** implement real Bluetooth.

It simulates the intended mesh behaviour inside the Spring Boot application.

## Completion condition

Packets can propagate through the simulated mesh and the phase is pushed to GitHub.

---

# Phase 10: Offline Payment Injection

**Status: Pending**

## Objective

Create the demo flow that acts as the sender's device.

## File

```text
src/main/java/com/demo/upimesh/service/
└── DemoService.java
```

## Planned flow

```text
Payment details
      ↓
PaymentInstruction
      ↓
Nonce + timestamp
      ↓
Encryption
      ↓
MeshPacket
      ↓
phone-alice
```

## Completion condition

A demo payment can be created and injected into the simulated mesh, then pushed to GitHub.

---

# Phase 11: Idempotency & Replay Protection

**Status: Pending**

## Objective

Prevent the same payment packet from being settled more than once.

## Files

```text
src/main/java/com/demo/upimesh/service/
└── IdempotencyService.java

src/main/java/com/demo/upimesh/config/
└── AppConfig.java
```

## Planned idempotency flow

```text
Ciphertext
    ↓
SHA-256 hash
    ↓
Atomic claim
    ↓
First request → continue
Duplicate → reject
```

The prototype uses `ConcurrentHashMap` for JVM-local atomic claiming.

## Replay protection

The encrypted payment is planned to contain:

- Timestamp
- Nonce

The backend should reject stale packets and repeated delivery of the same packet.

## Completion condition

Duplicate/replay behaviour is implemented and verified, then pushed to GitHub.

---

# Phase 12: Transaction Settlement

**Status: Pending**

## Objective

Perform the demo ledger operation after a payment passes validation.

## File

```text
src/main/java/com/demo/upimesh/service/
└── SettlementService.java
```

## Planned flow

```text
Validated payment
       ↓
Debit sender
       ↓
Credit receiver
       ↓
Write transaction
```

The settlement operation is planned to be transactional.

## Completion condition

The settlement flow works correctly and is pushed to GitHub.

---

# Phase 13: Secure Bridge Ingestion

**Status: Pending**

## Objective

Create the backend pipeline that receives packets from bridge devices.

## File

```text
src/main/java/com/demo/upimesh/service/
└── BridgeIngestionService.java
```

## Planned pipeline

```text
Incoming MeshPacket
       ↓
SHA-256 ciphertext hash
       ↓
Idempotency claim
       ↓
Decrypt
       ↓
Freshness validation
       ↓
Settlement
```

## Completion condition

The bridge ingestion pipeline works end to end and is pushed to GitHub.

---

# Phase 14: REST API

**Status: Pending**

## Objective

Expose the backend functionality through HTTP endpoints.

## File

```text
src/main/java/com/demo/upimesh/controller/
└── ApiController.java
```

## Planned API surface

```text
GET  /api/server-key
GET  /api/accounts
GET  /api/transactions
GET  /api/mesh/state

POST /api/demo/send
POST /api/mesh/gossip
POST /api/mesh/flush
POST /api/mesh/reset
POST /api/bridge/ingest
```

## Completion condition

The required API endpoints work and are pushed to GitHub.

---

# Phase 15: Dashboard

**Status: Pending**

## Objective

Provide a simple interface for demonstrating the complete payment flow.

## Files

```text
src/main/java/com/demo/upimesh/controller/
└── DashboardController.java

src/main/resources/templates/
└── dashboard.html
```

## Planned dashboard capabilities

- Compose payment
- Inject payment into mesh
- Run gossip rounds
- Upload bridge packets
- View account balances
- View transactions
- View mesh state

## Completion condition

The dashboard can drive the demo flow and is pushed to GitHub.

---

# Phase 16: Testing

**Status: Pending**

## Objective

Verify the important security and concurrency behaviour.

## File

```text
src/test/java/com/demo/upimesh/
└── IdempotencyConcurrencyTest.java
```

## Planned tests

### Encryption round trip

```text
encryptDecryptRoundTrip
```

Verify encrypted payment data can be decrypted correctly.

### Tampered ciphertext

```text
tamperedCiphertextIsRejected
```

Verify modified ciphertext is rejected.

### Concurrent duplicate delivery

```text
singlePacketDeliveredByThreeBridgesSettlesExactlyOnce
```

Three concurrent deliveries of the same packet should result in:

```text
1 × SETTLED
2 × DUPLICATE_DROPPED
```

The sender should be debited only once.

## Completion condition

Tests pass and the test phase is pushed to GitHub.

---

# Phase 17: End-to-End Validation

**Status: Pending**

## Objective

Run the complete flow from payment creation to settlement.

## Target flow

```text
Dashboard / Sender
        ↓
PaymentInstruction
        ↓
Hybrid Encryption
        ↓
MeshPacket
        ↓
Virtual Devices
        ↓
Gossip Rounds
        ↓
Bridge Device
        ↓
/api/bridge/ingest
        ↓
Idempotency Check
        ↓
Decryption
        ↓
Freshness Check
        ↓
Settlement
        ↓
Account + Transaction
```

## Validation

Confirm that:

- Payment can be injected
- Packet moves through the simulated mesh
- Bridge can upload the packet
- Backend validates the packet
- Sender and receiver balances update correctly
- Transaction is recorded
- Duplicate delivery does not settle twice
- Tampering is rejected

## Completion condition

The complete demo is run successfully and the phase is pushed to GitHub.

---

# Phase 18: Final Documentation

**Status: Pending**

## Objective

Update the public README after the implementation is actually complete.

The final README should document the **real state of the project**, not the original plan.

It should include:

- What SwiftUPI is
- Why it was built
- How to run it
- Demo flow
- Architecture
- Security approach
- Idempotency approach
- API reference
- Tests
- Actual project structure
- What is simulated
- Honest limitations
- Future production evolution

## File

```text
README.md
```

## Completion condition

Final README reflects the actual implemented project and is pushed to GitHub.

---

# Planned Project Structure

The final target structure is:

```text
swiftupi/
├── notes/
│   ├── BRD.md
│   ├── PRD.md
│   ├── TRD.md
│   └── project_journey.md
│
├── README.md
├── pom.xml
├── mvnw
├── mvnw.cmd
│
└── src/
    ├── main/
    │   ├── java/com/demo/upimesh/
    │   │   ├── config/
    │   │   │   └── AppConfig.java
    │   │   ├── controller/
    │   │   │   ├── ApiController.java
    │   │   │   └── DashboardController.java
    │   │   ├── crypto/
    │   │   │   ├── HybridCryptoService.java
    │   │   │   └── ServerKeyHolder.java
    │   │   ├── model/
    │   │   │   ├── Account.java
    │   │   │   ├── AccountRepository.java
    │   │   │   ├── MeshPacket.java
    │   │   │   ├── PaymentInstruction.java
    │   │   │   ├── Transaction.java
    │   │   │   └── TransactionRepository.java
    │   │   ├── service/
    │   │   │   ├── BridgeIngestionService.java
    │   │   │   ├── DemoService.java
    │   │   │   ├── IdempotencyService.java
    │   │   │   ├── MeshSimulatorService.java
    │   │   │   ├── SettlementService.java
    │   │   │   └── VirtualDevice.java
    │   │   └── UpiMeshApplication.java
    │   └── resources/
    │       ├── application.properties
    │       └── templates/
    │           └── dashboard.html
    └── test/
        └── java/com/demo/upimesh/
            └── IdempotencyConcurrencyTest.java
```

**Note:** This is the planned final structure. Files should be introduced and committed only in the phase where they are actually needed.

---

# Git Workflow

Every phase follows the same rule:

```text
PLAN
  ↓
BUILD
  ↓
RUN / TEST
  ↓
UPDATE notes/project_journey.md
  ↓
STAGE ONLY THAT PHASE'S FILES
  ↓
COMMIT
  ↓
PUSH
  ↓
NEXT PHASE
```

Never use:

```bash
git add .
```

for the phase-by-phase development history.

Instead:

```bash
git add <specific phase file>
git add <specific phase file>
git add notes/project_journey.md

git commit -m "<phase-specific commit message>"
git push
```

---

# Status Board

| Phase | Status |
|---|---|
| 1. Business Requirements (BRD) | Pending |
| 2. Product Requirements (PRD) | Pending |
| 3. Technical Requirements (TRD) | Pending |
| 4. Initial README | Pending |
| 5. Spring Boot Project Foundation | Pending |
| 6. Payment & Domain Model | Pending |
| 7. Payment Security | Pending |
| 8. Virtual Device | Pending |
| 9. Offline Mesh Simulation | Pending |
| 10. Offline Payment Injection | Pending |
| 11. Idempotency & Replay Protection | Pending |
| 12. Transaction Settlement | Pending |
| 13. Secure Bridge Ingestion | Pending |
| 14. REST API | Pending |
| 15. Dashboard | Pending |
| 16. Testing | Pending |
| 17. End-to-End Validation | Pending |
| 18. Final Documentation | Pending |

---

## Project Principle

**Build honestly. Document as you go.**

No phase is considered complete because it is planned, documented, or present in another copy of the project.

A phase becomes **Completed only after the work is actually done, verified, and pushed to GitHub.**
