# Collections Module — MVP Feature List (Detailed)

## Method

The MVP is the smallest cut of the collections module that **actually works** for an Affordable Housing Finance Company. It is not the demo, not the showcase, not the everything-bagel — it is the v1 that can collect money, stay compliant, and survive an audit.

For each of the 9 slots from the first-principles feature list (7 jobs + 2 cross-cutting), the MVP is broken down into:

- **Ships in v1** — what we build
- **Deferred** — what we explicitly do not build (yet)
- **Done when** — acceptance criteria
- **Why this cut** — the first-principles reason for the line in the sand

Anything not on the "ships" list is layer-2. The ordering at the end (sequencing) is a build plan, not a wish list.

---

## 1. Sense — MVP

**Ships in v1**
- Loan master sync from LMS (account, sanction, EMI, schedule, ROI, tenor)
- DPD calculator running at least hourly
- Bucket assignment: Current / 1-30 / 31-60 / 61-90 / 90+ / NPA / Written-off
- Borrower 360 single screen: applicant + co-applicant + guarantor with masked PAN, mobiles, email, address
- Linked-accounts view (cross-default visibility) keyed off canonical Customer ID
- Property record: address, type, last valuation, valuer, valuation date
- Behavioural history for last 12 months: bounces, cures, PTPs kept/broken
- Last-contact timestamp per channel
- Append-only event log (every state transition recorded)

**Deferred**
- ML propensity scores
- Geo-mesh dashboards
- Best-time-to-call models
- RFID title custody
- KYC-freshness auto-tasks
- Insurance-claim history feed

**Done when**
- Any account's DPD is queryable in <2 seconds
- 360 view loads on one screen, no tab-switching
- Yesterday's bucket-roll report is in the manager's inbox by 9am

**Why this cut**
Without clean state, no action is correct. ML and geo-analytics layer on top.

---

## 2. Decide — MVP

**Ships in v1**
- **Bulk allocation upload** — ops uploads a case → team mapping (CSV / Excel) for field and tele teams
- **Strategy for communication defined in the system** — static treatment trees per bucket / channel govern what is sent (SMS / IVR / call / visit) and when, once a case is allocated
- DPD-bucket segmentation — system knows each case's bucket and applies the right tree automatically
- Frequency caps in config, enforced by dialer (max calls/day, max SMS/day)
- Static priority sort within agent queue (DPD desc → ticket desc)
- PTP-on-hold suppression (active PTP pauses dunning until expiry)

**Deferred**
- Rule-based auto-allocation to channel / agent
- Multi-dimensional segmentation (ticket × geography × employment)
- ML propensity scores for ranking
- Champion-challenger
- Cost-aware action selection
- NBA recommender
- Automated re-allocation triggers

**Done when**
- Ops can upload a case → team mapping for the day's portfolio in <5 minutes; system populates queues immediately
- The right dunning tree runs for each bucket without manual intervention once allocated
- Frequency caps verifiably non-overrideable in penetration test

**Why this cut**
v1 splits the decision cleanly: **ops decides *who* handles each case** (via bulk upload) while **the system decides *how* to communicate** (strategy trees by bucket). Auto-allocation layers on once the rules are proven and ops trusts the system to choose channel and agent.

---

## 3. Reach — MVP

**Ships in v1**
- SMS via DLT-registered templates (one per bucket)
- IVR for early-bucket nudges and missed-call deflect
- Agent outbound dialer in progressive mode
- Field officer mobile app (online-first OK)
- Calling-hour enforcement (8am–7pm IST + holiday list) — non-overrideable
- DND honour at SMS and dialer
- Co-applicant cascade — call applicant first, then co-applicant after RNR×3

**Deferred**
- WhatsApp BSP (high-leverage but 6–8 weeks for templates + onboarding)
- Voice-bot
- Predictive dialer (start with progressive)
- Best-time-to-call learning
- Skip-tracing connectors
- Reference-walking workflow

**Done when**
- Agent can place a recorded outbound call from desktop in <5 seconds
- SMS sends from a template with merge fields (name, amount, due date)
- Field officer sees today's beat plan, marks dispositions, logs payments — all on phone

**Why this cut**
SMS + IVR + agent + field cover >90% of Indian collection contacts. WhatsApp is a 6-week adder, not a v0 blocker.

---

## 4. Act — MVP

**Ships in v1**
- UPI payment link (one-tap, amount-pre-filled) — triggered by tele-caller or FOS, delivered via SMS
- Tele-call with disposition capture + call recording + one-click "send payment link"
- Field visit — cheque / UPI on-spot (FOS can send the link from the app) + geo-tagged photo + structured disposition

**Deferred**
- Cash collection at field (custody, deposit, shortage-tracking overhead)
- Section 138 NI Act notice (template-driven)
- SARFAESI Section 13(2) demand notice
- SARFAESI Section 13(4) symbolic possession workflow
- OTS workflow with delegation matrix and waiver booking
- Insurance-claim filing (upload + tracker)
- E-stamp + e-sign integration
- Full e-auction module
- Restructure NPV simulator
- WhatsApp interactive pay flow
- Voice-bot intent capture
- E-NACH self-service

**Done when**
- Agent sends a payment link in <10 seconds
- Field officer issues a digital receipt on doorstep
- A delinquent borrower can be touched by digital + tele + field channels in a single day

**Why this cut**
v1 ships the operational collection core — digital + tele + field. Legal recovery (Sec 138, SARFAESI), OTS, and insurance workflows are a separate workstream that layers on once the operational engine is stable. They involve longer build cycles, lawyer dependencies, and don't move recoveries on day one.

---

## 5. Record — MVP

**Ships in v1**
- Every call recorded with consent disposition captured
- Every visit logged with timestamp + geo-tag + disposition + photo
- PTP capture as structured object (amount + date + channel)
- PTP tracking (kept / broken / partial) with auto-link of incoming payment

**Deferred**
- Document vault for notices, AD/POD, OTS letters, court orders, valuation reports
- Audit log on config changes, allocations, waivers, write-offs, PII access
- Hash-chained tamper-evidence
- OCR-indexed vault
- Customer-voice auto-tagging
- Versioning of legal documents

**Done when**
- Any call retrievable by account number in <30 seconds
- Every visit has timestamp, geo-tag, photo, and disposition stored
- PTP-performance report runs weekly

**Why this cut**
v1 records what the operational team needs day-to-day — every call, every visit, every PTP. Legal-grade document custody and full config-audit logging are governance layers that pair with the deferred legal recovery workflow; ship them together once Sec 138 / SARFAESI is on the roadmap.

---

## 6. Settle — MVP

**Ships in v1**
- Receipt: cheque, UPI, NEFT, NACH, payment link
- Provisional → final receipt on clearance / NACH success
- Configurable appropriation waterfall (charges → penal → interest → principal)
- Bank-statement to receipt match (auto + exception queue)
- NACH file reconciliation (presented / success / bounce)
- Daily GL posting with sub-ledger tie-out
- Bounce reversal — reverse EMI, restore DPD, reopen case, reverse PTP-kept

**Deferred**
- Cash receipts (branch counter + FOS doorstep)
- Cash custody reconciliation per FOS / branch / agency
- Card / wallet / BBPS receipts
- Auction-proceeds appropriation
- Insurance-proceeds appropriation
- Automated provisioning on waiver/write-off
- Borrower self-service for excess/suspense

**Done when**
- Any digital receipt reconciled to GL by next day
- Unmatched bank receipts <1% of daily volume
- Bounce reversal cascades cleanly (DPD restored, case reopened, PTP-kept reversed)

**Why this cut**
Money in the wrong account = unrecovered. Reconciliation is non-negotiable. Cash is deferred — its custody, deposit, and shortage-tracking burden isn't worth carrying when every borrower can pay via UPI / NACH / link.

---

## 7. Improve — MVP

**Ships in v1**
- Daily roll-rate report per bucket
- Weekly cure-rate report per bucket × geography
- Agent productivity: calls/day, AHT, RPC%, cure ₹
- PTP-kept% and PTP→payment lag at agent and team level
- Cost-to-collect by channel (channel-cost ÷ recoveries)
- Monthly NPA report (NHB-return-ready)
- Vintage curves at quarterly cadence

**Deferred**
- Champion-challenger framework
- What-if simulator
- Real-time dashboards (daily refresh OK)
- ML-model performance tracking
- Static-pool analysis
- Auto-promotion of winners

**Done when**
- Manager sees yesterday's roll, cure, RPC by 9am
- Agent leaderboard refreshes daily
- NPA report is one-click for NHB submission

**Why this cut**
Three numbers — roll, cure, RPC — steer 80% of decisions. The rest is layer-2.

---

## 8. Guardrails — MVP

**Ships in v1**
- Calling-hour window enforced at dialer (8am–7pm IST + holiday list)
- DND honour at SMS and dialer
- Recording-consent capture (IVR prompt for inbound; agent script for outbound)
- Per-borrower frequency cap at dialer (configurable; default 3 calls/day)
- Role-based access control (collector / supervisor / manager / compliance)
- Data retention policy — recordings 7 yrs, KYC 10 yrs post-closure

**Deferred**
- Audit log on config, allocation, waiver, write-off, PII access
- Geofenced FOS access (basic location for v1)
- Full DEPA consent ledger with revocation propagation
- Field-level PII masking by role
- IIBF expiry tracking (HR-side for v1)
- Automated Ombudsman SLA escalation

**Done when**
- 4th outbound call to same borrower in a day is impossible (penetration-tested)
- Every call has a consent disposition
- Calling-hour and DND breaches are zero in production

**Why this cut**
The regulatory floor on borrower-facing conduct. Every gap here is a complaint, a fine, or a license risk. Internal-governance audit (config / waiver / write-off / PII access trails) layers on later alongside the legal and document-vault stack.

---

## 9. Plumbing — MVP

**Ships in v1**
- LMS bidirectional integration (account state, schedule, payments, dispositions)
- Core banking / GL integration with daily tie-out
- Payment rails — NACH (NPCI), UPI, payment gateway, NEFT
- Telephony — SIP trunks + dialer + recording storage (S3, 7-yr retention)
- Messaging — SMS aggregator with DLT registration
- Credit bureau — CIBIL minimum (CRIF / Equifax / Experian optional)
- HRMS integration for agent master + skill + roster
- SSO for all users
- API-first design (every UI action has an API)

**Deferred**
- WhatsApp BSP
- Account Aggregator
- E-stamp / e-sign / DigiLocker
- Court & e-auction platform integrations
- Alternate-data feeds (EPFO, recharge proxies)
- Webhook / event-bus extensibility

**Done when**
- A loan disbursed in LMS shows up in collections by the next sync
- A receipt posts to GL within the day
- An outbound call connects via dialer and is recorded
- An SMS lands within 1 minute of trigger

**Why this cut**
Without these, the module is an island. Deferred ones are adders, not blockers.
