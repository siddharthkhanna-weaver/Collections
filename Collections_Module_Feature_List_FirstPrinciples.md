# Collections Module — Feature List (First-Principles Derivation)

## Method

A collections module is not a bag of vendor features. It exists because a lender has given money, the borrower has a contractual schedule to repay, and reality deviates from that schedule. The system must close the gap between "what is owed" and "what is collected" — legally, economically, and at scale.

From that single purpose, **seven atomic jobs** fall out. Every legitimate feature serves one of them. Anything that doesn't is bloat.

| # | Atomic Job | The Question It Answers |
|---|---|---|
| 1 | **Sense** | What is the current state of every account, borrower, contact, and asset? |
| 2 | **Decide** | For each case, at this moment, what is the right next action? |
| 3 | **Reach** | Can we actually contact this borrower, through which channel, when? |
| 4 | **Act** | Execute the chosen treatment — nudge, call, visit, notice, suit, auction. |
| 5 | **Record** | What happened, who did it, with what evidence? |
| 6 | **Settle** | Collect the money, reconcile it, close the case correctly. |
| 7 | **Improve** | Measure outcomes, attribute causes, tune the strategy. |

Two **cross-cutting layers** wrap all seven:
- **Guardrails** — compliance, consent, audit, security.
- **Plumbing** — integrations, identity, master data, configuration.

Features are derived below from each job. Each feature is justified by the job it serves; if no justification exists, it doesn't belong.

---

## 1. SENSE — Know the State

A system cannot collect what it cannot see. The minimum sensing surface:

### 1.1 Account & loan state
- Loan master with product, sanction, schedule, tenor, ROI, EMI, collateral linkage.
- Live ledger: principal outstanding, interest accrued, penal interest, charges, suspense, EMIs paid/due/skipped.
- DPD calculator (running, not batch-only) with bucket assignment (current, 1-30, 31-60, 61-90, 91-180, 180+, NPA, write-off, recovered).
- Roll/flow tracking: yesterday's bucket → today's bucket per account.
- Restructure / re-age / moratorium flags with original vs revised schedule preserved.

### 1.2 Borrower & relationship state
- Borrower 360: applicant, co-applicant, guarantor, references, employer, all linked accounts (cross-default visibility).
- Identity graph: PAN, Aadhaar masked, mobile, email, address — with verification status and source-of-truth per attribute.
- Behavioral history: past delinquencies, cures, PTPs (promise-to-pay) kept/broken, settlements, complaints, hardship flags.
- Demographic + KYC freshness (age of last refresh).

### 1.3 Collateral state (mortgage-secured loans)
- Property record: address, geo-coordinates, type, current valuation, last valuation date, valuer ID.
- Lien status, insurance policy, premium-paid status, claim history.
- Title document custody location + barcode/RFID.

### 1.4 Contactability state
- Per-channel reachability scores: mobile RNR/switched-off rate, email bounce rate, address PIN-validity, last-successful-contact timestamp.
- Consent registry per channel (DND, WhatsApp opt-in, voice-bot consent, recording consent).
- Working-hours profile (when this borrower actually picks up).

### 1.5 Field & operational state
- Geo-mesh of cases (which pincode/cluster/branch).
- Agent/agency capacity, in-shift status, route load.
- Notice/legal stage state machine (where each case sits in the legal pipeline).

### 1.6 Event stream
- Append-only event log of every state transition (payment received, bucket roll, contact attempt, PTP made, notice issued). This is the source of truth; reports are projections.

> **Why first-principles:** without 1.1–1.6, jobs 2–7 are guesswork. Most "missing feature" complaints in collection systems trace back to a hole in the sensing surface.

---

## 2. DECIDE — Choose the Right Action

Given state, the system must answer: *for this case, now, what is the cheapest action with the highest expected recovery?*

### 2.1 Segmentation
- Rule-based segmentation (DPD × ticket × product × geography × employment-type).
- Score-based segmentation (risk score, propensity-to-pay, propensity-to-cure, propensity-to-roll).
- Behavioral segmentation (chronic late-payer, first-time defaulter, willful, hardship, contactable-but-unwilling, uncontactable).
- Value tiering (high-balance/high-LGD vs long-tail).

### 2.2 Strategy engine
- Treatment-tree configuration: per segment, sequence of actions across days/buckets (Day-3 SMS → Day+1 IVR → Day+5 agent call → Day+10 field visit → Day+15 notice).
- Conditional branching (if PTP made → suspend escalation; if RNR ×3 → switch channel).
- Cost-aware action selection (use cheapest channel that achieves expected lift).
- Cool-off and frequency caps embedded in the tree.

### 2.3 Allocation engine
- Auto-assignment to channel (digital / tele / field / legal / agency) based on segment + capacity.
- Agent-level allocation respecting skill, language, geography, workload.
- Agency allocation with quota, vintage-mix, and exclusion rules.
- Re-allocation triggers (no contact in N days, PTP broken, escalation).

### 2.4 Prioritisation
- Within an agent's queue: ranked by expected-value (probability-of-recovery × amount × time-decay).
- Next-Best-Action surface on agent desktop (don't ask the agent to think — tell them what to do next).

### 2.5 Champion-challenger
- Ability to run two strategies on matched cohorts simultaneously and measure lift.

> **Why first-principles:** action without strategy = spam. Strategy without measurement = superstition. The decide layer is what separates a collection system from a dialer.

---

## 3. REACH — Establish Contact

Treatment fails if the borrower is unreachable. Reach is its own engineering problem.

### 3.1 Contact discovery & enrichment
- Skip-tracing connectors (bureau, telco, social, alternate-data vendors).
- Reference-walking workflow (call references with consent).
- Address verification via field visit feedback loop.
- Co-applicant/guarantor cascade (when primary unreachable).

### 3.2 Channel orchestration
- SMS, email, WhatsApp (BSP-integrated, template-managed), IVR, voice-bot, live agent, field visit, physical letter, legal notice.
- Channel waterfall logic (try cheapest, fall back).
- Per-channel suppression (DND, complaint-history, regulatory).

### 3.3 Right-time engine
- Best-time-to-call per borrower learned from connect history.
- Calling-hour guardrails enforced by jurisdiction (RBI Fair Practice: 8am-7pm).

### 3.4 Identity & deliverability
- Sender ID / DLT template / WhatsApp template registry.
- Bounce / undeliverable feedback loop closing back into 1.4.

---

## 4. ACT — Execute the Treatment

Each channel needs a real execution surface, not just a pointer.

### 4.1 Digital self-service (pre-due + soft bucket)
- Dynamic payment links (UPI, cards, netbanking, wallet) with deep-link to amount.
- WhatsApp pay-flow with inline payment.
- E-NACH / e-mandate setup, modify, cancel.
- PDC inventory + presentation calendar + bounce-charge automation.
- Self-service portal/app: view dues, download statement, raise hardship, request restructure, schedule callback, dispute charge.

### 4.2 Tele-calling
- Agent desktop: 360 view + script + disposition + NBA + payment-link send + recording control.
- Predictive / progressive / preview dialer modes.
- IVR & voice-bot with payment intent capture and warm-transfer to agent.
- Call-back scheduler honoured by the dialer.
- Live monitoring, whisper, barge for supervisors.

### 4.3 Field operations (FOS)
- Mobile app (offline-capable) with day's beat plan.
- Geo-tagged check-in / check-out, selfie + property photo.
- Route optimisation across the day's cases.
- On-spot receipt issuance (digital + printed) with cash, cheque, UPI, POS.
- Cash custody: deposit slip, deposit-to-bank reconciliation, shortage tracking.
- Visit disposition with structured outcomes (met-borrower, met-family, premises-locked, address-incorrect, refused, PTP, paid).
- Repossession/possession workflow (where applicable).

### 4.4 Legal & recovery
- Notice templates (reminder, demand, Section 138, SARFAESI 13(2), 13(4), Section 13(8) sale notice, possession notice).
- E-stamp + e-sign integration; physical dispatch with AD/POD tracking.
- Court case management: filing, hearings, orders, costs, outcomes — Sec 138 NI Act, SARFAESI Sec 17, civil suit, arbitration, Lok Adalat, DRT, NCLT.
- Auction module for mortgaged property: valuation, reserve price, e-auction platform, EMD, bidder management, sale certificate, sale-proceeds appropriation waterfall.
- Lawyer/agency assignment + cost tracking per case.

### 4.5 Settlement & restructuring
- OTS / waiver workflow with delegation matrix (who can approve what haircut).
- Restructure: revised schedule generation, NPV-impact, regulatory classification check.
- Re-aging eligibility check + audit reason code.
- Insurance-claim trigger: death, disability, property — claim filing, claim-proceeds appropriation.

---

## 5. RECORD — Capture What Happened

Without an evidentiary record, no recovery action survives challenge.

### 5.1 Interaction log
- Every call (with recording), SMS, email, WhatsApp, visit, letter, notice — timestamped, agent-stamped, channel-stamped.
- Free-text + structured disposition + standardised reason codes.
- Customer voice (complaints, hardship statements) tagged separately.

### 5.2 Promise-to-Pay (PTP)
- PTP capture with amount + date + channel.
- PTP tracking: kept, broken, partial.
- Auto-link of incoming payment to open PTP.

### 5.3 Document vault
- All notices issued, AD cards, POD, court orders, valuation reports, sale certificates, OTS letters, NOCs.
- Versioning, retention policy, legal-hold flag.

### 5.4 Audit trail
- Append-only log of who did what when (config change, allocation change, waiver, write-off, sensitive PII access).
- Tamper-evident (hash-chained or WORM).

---

## 6. SETTLE — Move and Reconcile Money

The act of collecting is incomplete until money is in the right ledger account.

### 6.1 Receipt
- Multi-mode receipt issue: cash, cheque, DD, UPI, NEFT/IMPS/RTGS, card, wallet, payment link, NACH presentation, auction proceeds, insurance claim.
- Provisional receipt (cheque-in-clearing) → final receipt on realisation.

### 6.2 Payment posting
- Appropriation waterfall (charges → penal → interest → principal, configurable per product/regulator).
- Excess / suspense handling.
- Reversal on cheque bounce / chargeback with downstream un-do (reverse PTP-kept, restore DPD, re-open case).

### 6.3 Reconciliation
- Bank-statement-to-receipt match (auto + exception queue).
- Cash custody reconciliation per FOS / branch / agency.
- PG / NACH file reconciliation.
- GL posting with sub-ledger tie-out.

### 6.4 Settlement accounting
- OTS waiver booking to P&L with approval reference.
- Write-off entry with reason code; recovery from written-off booked to other-income.
- Provisioning automation per RBI/NHB IRAC norms.

---

## 7. IMPROVE — Measure and Learn

A collection system that doesn't get smarter month-on-month is decaying.

### 7.1 Operational metrics
- Roll-rate, flow-rate, bounce-rate, resolve-rate, cure-rate, recovery-rate per bucket × segment × strategy × geography × agent × agency.
- Right-party-contact rate, PTP-kept rate, PTP-to-payment lag.
- Cost-to-collect per ₹ recovered, by channel.

### 7.2 People metrics
- Agent productivity (calls, AHT, cure $), QA score, schedule adherence.
- Agency scorecard and payout calc.

### 7.3 Strategy lab
- Champion-challenger results, statistical significance, auto-promote winning variant.
- What-if simulator on historical data before rolling a new strategy.

### 7.4 Regulatory & MIS
- NHB / RBI return automation (NPA, restructure, write-off, customer-complaints).
- Board / risk-committee dashboards.
- Vintage curves, static-pool analysis.

---

## 8. GUARDRAILS — Compliance, Consent, Security (cross-cutting)

These are not features added on top — they are constraints baked into every other feature.

### 8.1 Regulatory compliance
- RBI Fair Practices Code: calling hours, language, decency, no-third-party harassment.
- RBI Digital Lending Directions (2025): KFS, cooling-off, consent, data localisation, no dark patterns.
- NHB Directions for HFCs: NPA recognition, recovery agent code, complaint handling.
- IBA / IIBF certified-agent registry; certification expiry tracking.
- Recording consent capture before recording.
- Per-borrower frequency cap (e.g., max-N-calls-per-day) enforced at the dialer, not "in policy".

### 8.2 Privacy & security
- Field-level PII masking in agent UI based on role.
- Tokenised storage of bank account / card / PAN.
- Consent ledger (DEPA-aligned) with revocation propagation.
- Access control: role + attribute-based, time-bound, geofenced for FOS app.
- Data retention & purge per data-class.

### 8.3 Customer redressal
- Complaint capture, SLA timers, ombudsman escalation path.
- Hardship channel that bypasses normal collection treatment.
- Internal grievance redressal officer routing.

---

## 9. PLUMBING — Integrations, Identity, Configuration (cross-cutting)

### 9.1 Integrations (must-have, not nice-to-have)
- LOS / LMS — bidirectional account state.
- Core banking / GL.
- Credit bureaus (CIBIL / CRIF / Equifax / Experian) — pull and report.
- Payment rails: NACH (NPCI), UPI / collect, PG, BBPS, cards.
- Telephony: SIP trunks, dialer, recording storage.
- Messaging: SMS aggregator + DLT, WhatsApp BSP, email ESP.
- Bank statement / account-aggregator (consent-based).
- Bureau alerts / alternate-data signals (income, employment, mobile-recharge proxies).
- E-sign / e-stamp / digital-locker.
- Court / e-auction platforms.
- HRMS for agent master + skill + roster.

### 9.2 Master data & identity
- Single canonical borrower ID across products.
- Product, charge, fee, penal-rate, communication-template masters with effective dating.
- Reason-code taxonomy (single source for dispositions, write-off reasons, complaint reasons).

### 9.3 Configuration as a product capability
- Business-user configurability for: strategy trees, allocation rules, dunning calendars, templates, scorecard weights, waterfall, delegation matrix, SLA timers — without code change.
- Versioning, approval workflow, sandbox-test-then-promote on every config change.
- Config diff and rollback.

### 9.4 Platform qualities
- Multi-entity / multi-product / multi-currency from day one if the lender has more than one company or co-lending arrangement.
- API-first (every UI action available as API).
- Event-driven extensibility (webhook on bucket roll, on PTP, on receipt).
- Observability: per-feature SLOs, event-replay capability.

---

## 10. AHFC-Specific Specialisations

The seven jobs apply to any lender. An Affordable Housing Finance Company has four extras that genuinely change feature requirements (not just thresholds):

1. **Mortgage-as-collateral** → SARFAESI workflow + property valuation + auction module are core, not optional.
2. **Long tenor (15-30 yrs)** → contactability decay over time is a first-class problem; periodic KYC/contact-refresh is a feature, not a project.
3. **Informal-income borrowers** → field collections are dominant channel, FOS module quality is the single biggest lever.
4. **Co-applicant / guarantor / family liability** → the "borrower" is a household, not a person; the data model must reflect that everywhere (1.2, 3.1, 5.1).

These do not add a new job; they raise the bar on jobs 1, 3, 4, and 5.

---

## 11. The Minimum Viable Collections Module

If forced to ship the smallest thing that actually works for an AHFC, this is the cut. Everything else is layered on later.

| Job | MVP feature | Why it can't be skipped |
|---|---|---|
| Sense | Live DPD + bucket + 360 view | Without state, no action is correct |
| Decide | DPD-bucket-based rule engine + manual allocation | A rules engine beats nothing; ML can wait |
| Reach | SMS + IVR + agent dialer + field app | Covers >90% of contact attempts |
| Act | Payment link + tele-call + field visit + Sec-138 + SARFAESI 13(2)/13(4) | The legally-required acts for secured retail |
| Record | Disposition + PTP + recording + document vault | Required for audit and any legal action |
| Settle | Receipt + waterfall + bank recon + GL post | Money in wrong account = unrecovered |
| Improve | Roll-rate + agent productivity + RPC% | Three numbers steer 80% of decisions |
| Guardrails | Calling-hours + DND + recording-consent + audit log | Regulatory floor |
| Plumbing | LMS + payment rails + telephony + SMS/WhatsApp + bureau | Without these the module is an island |

---

## 12. Anti-Features (what NOT to build)

First-principles thinking is also about subtraction. Common bloat to avoid:

- **A second source of truth for the loan ledger.** The LMS owns it; collections projects from it.
- **A bespoke dialer when a best-of-breed exists.** Integrate, don't rebuild.
- **A "CRM" layer duplicating the customer 360.** One borrower record, many views.
- **Static reports the strategy team can't modify.** Reports as code = dead reports; reports from the event stream = live.
- **Hard-coded strategy trees.** Every rule must be config; otherwise every change is a release.
- **Vanity ML.** A model that doesn't change an action isn't a feature.
- **Over-modelled "case" abstraction** that hides the underlying account, borrower, and event. Cases are a projection, not a primitive.

---

## 13. How to Use This List

For vendor evaluation (Omnifin, Spocto, AbleCredit, Kriyam, etc.):
1. Treat sections 1–7 as the **must-cover** axes. Mark Y/Partial/N per feature.
2. Treat sections 8–9 as the **disqualifiers**. A gap here is rarely fixable.
3. Section 10 is the **AHFC-fit** lens. A vendor strong on unsecured PL but weak on SARFAESI/auction is the wrong shortlist.
4. Section 11 is the **negotiation anchor**. Don't pay enterprise prices for anything beyond MVP unless it passes a measurable-lift test.
5. Section 12 is the **scope discipline**. Push back on vendor "value-adds" that don't map to a job.
