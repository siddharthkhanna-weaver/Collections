# Modules Required by an Affordable Housing Finance Company (AHFC) in India for Debt Recovery / Collections

> A research note to inform a product/feature comparison across vendors such as Omnifin, Spocto (Yubi), AbleCredit, Kriyam, Credgenics, Mobicule, Indus Net Kratos, Lentra, Newgen, Nucleus FinnOne, etc. The framing is built around the actual collections lifecycle of an AHFC — long-tenor, secured, mortgage-backed loans extended largely to informal/self-employed borrowers.

---

## 0. Why AHFC Collections Are Structurally Different

Affordable HFCs (Aavas, Aadhar, Aptus, Home First, India Shelter, Repco, Shubham, Agrim, Aviom, DMI Housing, etc.) sit at the intersection of three uncomfortable facts:

1. **Long tenor, low ticket** — typical loans of INR 5-25L over 15-25 years to households earning INR 15-50k/month, often self-employed with informal cash flows. Borrowers are visited at home/place of work, with loan officers building personal relationships and conducting on-ground verifications, a methodology borrowed from MFIs ([ImpactAlpha — Affordable home lending market](https://impactalpha.com/behind-the-success-of-indias-affordable-home-lending-market-patient-impact-capital/)).
2. **Secured by mortgage** — recovery rights flow through SARFAESI, 2002 (Sec 13(2), 13(4), 13(8), 14, 17), Section 138 of the NI Act on bouncing of post-dated cheques, civil suits and Lok Adalat. HFCs with net worth > INR 100 Cr have no pecuniary threshold to invoke SARFAESI ([Lexology — HFC SARFAESI for loans < INR 20L](https://www.lexology.com/library/detail.aspx?g=0da30551-ef27-4dcd-a683-a4457809ff8c)).
3. **Heavy regulation** — Master Direction (Non-Banking Financial Company – Housing Finance Company) Directions, 2021 (consolidated), the new RBI HFC Directions 2025, NHB's legacy 2010 Directions, the Fair Practices Code, the Digital Lending Directions 2025 (effective 8 May 2025) and the proposed harmonised recovery framework slated for 1 July 2026 all bear directly on collections operations ([RBI HFC Directions 2025 PDF](https://www.nhb.org.in/wp-content/uploads/2026/01/Reserve-Bank-of-India-Housing-Finance-Companies-Directions-2025.pdf), [Vinod Kothari — Uniform Recovery Norms](https://vinodkothari.com/2026/02/rbi-proposes-uniform-recovery-norms-across-all-lenders/)).

These three forces dictate the module set below.

---

## 1. Pre-Due Process (T-15 to T-0)

The objective in this window is not "recovery" — it is **EMI hygiene**: making sure the mandate hits successfully, the customer has been nudged, and contactability is intact. Vendors compete heavily here because most cure happens before delinquency starts.

### 1.1 Customer Segmentation & Risk Scoring
- **Propensity-to-default (PTD) and propensity-to-pay (PTP) models** — typically gradient-boosted models on bureau, repayment, NACH-bounce history, demographic, geographic and behavioural features. Used to triage cases into "no-touch / low-touch / high-touch" treatments.
- **Early Warning System (EWS)** — flags customers showing leading indicators (sudden bureau enquiries, salary drop, new unsecured borrowing, partial pre-payment behaviour, repeated mandate amendments).
- **Behavioural segmentation** — chronic-late vs. one-off-late vs. always-on-time; informal-income borrower seasonality (harvest cycles, festive months).
- **Champion-challenger framework** — A/B testing collections strategies on small sample segments before rolling them across the portfolio ([Experian — Identifying optimum collections strategy](https://www.experian.ie/content/dam/marketing/uki/ireland/ie/assets/decision-analytics/white-papers/experian_champion_challenger.pdf), [Indebted — Champion challenger](https://www.indebted.co/blog/guides/putting-your-collections-strategy-to-the-test-with-a-champion-challenger-model/)).

### 1.2 Multichannel Communication & Nudging
- Bulk + triggered **SMS / WhatsApp / email / IVR / voice-bot** for T-7, T-3, T-1, T-0 reminders.
- **AI-driven conversational engines** with multilingual (20+) support — Spocto X claims human-like conversational AI with real-time adaptability ([Spocto — About](https://spocto.com/about/)).
- **Dynamic content** — EMI amount, due date, NACH bank, last-bounce reason, payment-link, branch contact.
- **Throttling and quiet-hours engine** to remain compliant with the Fair Practices Code (no calls before 0800 / after 1900) and the Digital Lending Directions 2025 ([Leegality — Digital Lending Directions 2025](https://www.leegality.com/blog/digital-lending-directions-2025)).

### 1.3 Payment Facilitation
- **Payment links** delivered by SMS/WhatsApp/email — UPI, net-banking, debit card, BBPS, wallet.
- **NACH / e-NACH / e-Mandate setup, swap, amend, cancel** workflows; auto-prompted when customer changes salary account ([HDFC — Loan e-NACH and e-Mandate](https://www.hdfc.bank.in/payments/loan-e-nach-and-e-mandate)).
- **PDC (Post-Dated Cheque) inventory and presentation calendar** — many AHFC borrowers still issue PDCs alongside NACH; tracking issuance, custody, presentation date, status.
- **Prepayment / part-payment / EMI-advance** acceptance.

### 1.4 NACH / PDC Presentation Management
- Presentation file generation in NPCI format, sponsor-bank handoff, return file ingestion, **bounce reason coding** ("insufficient funds", "account closed", "signature mismatch", "mandate not registered").
- **Re-presentation rules engine** — second/third presentation cadence per product policy.
- **ECS / NACH bounce charge** auto-levy + GST and accounting entry. Standard market charges are INR 200–500 per bounce ([BasicHomeLoan — ECS Return Charges on Home Loans](https://www.basichomeloan.com/blog/home-loans/ecs-return-charges-on-home-loans)).

### 1.5 Channel & Contactability Hygiene
- **Periodic KYC / contact refresh** workflow — phone, email, address, employer, alternate contacts of co-applicant/guarantor.
- **Address verification** — physical FOS visit, geo-tagged photo, utility bill capture.
- **Number-validity and DND scrubbing**; integration with TRAI DLT for SMS templates.

### 1.6 Self-Service / Customer App
- Mobile app + web portal for statement, EMI calendar, NACH amend, payment, query log.
- **Self-cure flows** — view overdue, regenerate payment link, raise hardship request, request restructuring, upload bank statement.

---

## 2. Post-Due Process (Bucket-Wise)

Indian HFC practice splits the book by Days Past Due (DPD): **Bucket 0 (current), X-bucket (1-30 DPD), Bucket 1 (31-60 DPD), Bucket 2 (61-90 DPD), NPA / 90+ DPD, Sub-standard, Doubtful, Loss / Written-off**. NHB Directions classify an asset as NPA at 90 DPD; loss assets must be written off entirely or carry 100% provisioning ([NHB Master Circular — HFC Directions 2010](https://www.nhb.org.in/wp-content/uploads/2019/07/MC01-Master-Circular-The-Housing-Finance-Companies-NHB-Directions-2010.pdf)).

### 2.1 Bucket / DPD-Based Strategy & Queue Engine
- Configurable rules: which channel, which intensity, which agent skill, which language, which legal action triggers at which DPD.
- **Roll-rate / flow-rate computation** — what % of accounts in Bucket 1 last month rolled to Bucket 2 this month ([TrueAccord — Collections Economics](https://blog.trueaccord.com/2022/02/collections-economics-101-for-digital-lenders/)).
- **Treatment ladder** — soft → hard → legal → settlement.

### 2.2 Soft-Bucket Digital Collections (1-30, 31-60 DPD)
- AI voice-bots, WhatsApp bots with payment links, automated email cadences.
- **Predictive / progressive / preview / auto / click-to-call dialer** with real-time pacing, abandon-rate management, AMD (answering-machine detection) ([Credgenics — DialNext Dialer](https://www.credgenics.com/dialnext-dialer)).
- 2-way WhatsApp during live calls; payment-link delivery while on call.

### 2.3 Tele-calling / Call-Centre (typically 31-90 DPD)
- **Agent desktop** with customer 360, dynamic scripts, last-disposition, PTP capture, recommended best action.
- **Disposition tree** — RTP (refused to pay), PTP (promise-to-pay), NC (no contact), WN (wrong number), DN (dispute), partial-pay, settled.
- **100% call recording**, AI-based **speech analytics / QA** (compliance phrases, abuse detection, mandatory disclosures), barge-in/whisper for supervisors.
- **Compliance monitoring** — Spocto X transcribes calls and flags compliance breaches in real time ([Spocto — Public Sector Banks article](https://www.business-standard.com/content/press-releases-ani/public-sector-banks-embrace-ai-driven-debt-collection-with-spocto-x-boosting-recoveries-by-60-percent-125040301306_1.html)).

### 2.4 Field Collections / FOS Module
This is critical for AHFC because affordable-segment customers are largely thin-file, often unreachable on phone.
- **Mobile app** with offline mode, navigation to property/work address, geo-tracking and geo-fencing of agent live location.
- **Visit logging** — selfie at location, photo of premise, recorded reason for non-payment, customer signature.
- **Receipt issuance** — instant digital + printable receipt over SMS/WhatsApp/email.
- **Cash deposit & reconciliation** — daily cash limits, single/bulk deposit to lender bank, automatic reconciliation against bank statement ([Credgenics — CG Collect FOS](https://www.credgenics.com/cg-collect-field-debt-collections), [Mobicule — mCollect](https://www.mobicule.com/debt-collection/field-collection.html)).
- **Route optimisation** for FOS productivity, cluster-based allocation.

### 2.5 Legal & Recovery Workflow
The legal stack for an AHFC mortgage is **uniquely deep** versus an unsecured lender:
- **Section 138 NI Act** for cheque dishonour — 30-day demand notice → 15-day grace → criminal complaint within 30 days ([SCC Times — Section 138 procedure](https://www.scconline.com/blog/post/2025/11/15/section-138-ni-act-cheque-bounce-notice/)).
- **SARFAESI Act, 2002**:
  - **Sec 13(2)** — 60-day demand notice to discharge full liability ([Bajaj — Sec 13(2)](https://www.bajajfinserv.in/understanding-sec-13-2-of-sarfaesi-act)).
  - **Sec 13(4)** — symbolic / physical possession of secured asset, takeover of management, appointment of manager ([Bajaj — Sec 13(4)](https://www.bajajfinserv.in/understanding-sec-13-4-of-sarfaesi-act)).
  - **Sec 14** — DM/CMM assistance for physical possession.
  - **Sec 17** — borrower's right to approach DRT.
  - **Sec 13(8)** — borrower's right to redeem before sale.
  - **Public auction** — valuation, reserve price, e-auction publication, bidder management, sale certificate, surplus refund.
- **Arbitration** clause invocation where present in loan agreement.
- **Lok Adalat** for pre-litigation amicable settlement; awards are deemed civil-court decrees and binding ([NALSA — Lok Adalat](https://nalsa.gov.in/lok-adalat), [Credgenics — Lok Adalats for pre-litigation](https://blog.credgenics.com/lok-adalats-for-pre-litigation-debt-recovery/)).
- **Civil suit / DRT / NCLT** for high-value or contested cases.
- The collections platform must hold **legal case status, hearing diary, notice templates with auto-merge of borrower data, advocate panel allocation, fee management, document repository** for each notice/order/affidavit.

### 2.6 Settlement / OTS / Restructuring / Re-aging
- **OTS calculator** with NPV of recoverable amount, hair-cut ceilings, deviation matrix and approval workflow.
- **Restructuring / re-aging** — moratorium, EMI step-up, tenor extension, interest-rate change; re-amortisation schedule and bureau reporting tags.
- **Hardship workflow** for income-loss, death of borrower (insurance trigger), property damage.

### 2.7 Repossession / Property Auction
Unlike vehicle finance, an AHFC does not "repossess" in the ordinary sense — but the **property auction module** under SARFAESI is its functional equivalent:
- Valuation engine + empanelled valuer workflow, two-valuer rule for high-value, IBBI-registered valuer integration.
- Reserve price calculation; pre-auction publication (English + vernacular daily); EMD collection; e-auction integration (eBKray / MSTC / IBAPI).
- Sale certificate issuance, registration, title transfer, surplus accounting.

### 2.8 Skip Tracing
- **Internal skip-tracing** — alternate numbers from bureau enquiry trail, social-network hits, employer records, neighbour references.
- **External data partners** — Experian, CRIF, Equifax, TransUnion CIBIL, Perfios, Karza/Defmacro, Bureau, Signzy. Modern platforms simultaneously query public records, credit data, utility connections, vehicle registration, employment and digital footprint ([Credgenics blog — Skip tracing tools](https://blog.credgenics.com/skip-tracing-tools-in-debt-collections/)).
- Compliance — RBI has laid out the framework within which lenders must operate ([Datacultr — Skip Tracing in Debt Collection](https://datacultr.com/blogs/skip-tracing-in-debt-collection/)).

### 2.9 NPA / Provisioning / Write-off / Recovery from Written-off
- Auto-classification at 90 DPD; sub-standard / doubtful (D1/D2/D3) / loss bucket transitions.
- Provisioning engine — 0.4% standard, 15% sub-standard, 25-100% doubtful (secured), 100% unsecured/loss ([NHB HFC Directions 2010 — Master Circular](https://www.nhb.org.in/wp-content/uploads/2019/07/MC01-Master-Circular-The-Housing-Finance-Companies-NHB-Directions-2010.pdf)).
- **Write-off workflow** with board/management committee approval, GL impact, IT Section 36(1)(vii) tagging.
- **Recovery from written-off** — separate ledger, "TWO" (technically written-off) book, agency assignment, ARC sale tracking under SARFAESI.

### 2.10 Vendor / Agency Management
- Empanelment, KYC, IIBF certification capture for individual recovery agents (mandatory per HFC Master Direction Annex XI on engaging recovery agents) ([CredSettle — RBI Rules for Recovery Agents 2025](https://www.credsettle.com/rbi-rules-for-recovery-agents)).
- **Allocation engine** (rule-based + ML) with case mix, geography, performance loading.
- **SLA tracking** — connect rate, PTP-kept rate, resolution rate, INR collected per case.
- **Payout engine** — slab-based, milestone-based, recovery-percentage-based; TDS, GST, dispute handling.

### 2.11 Receipt, Payment Posting, GL Reconciliation
- Receipt master with serial control, online + cash + cheque + DD + UPI + payment-gateway sources.
- **EMI appropriation hierarchy** — charges → penal interest → interest → principal — per loan agreement and RBI penal-charges circular.
- Suspense and unidentified-credit handling, customer-wise + GL-wise reconciliation, cross-verification with sponsor-bank statement.

### 2.12 Dashboards / MIS / Regulatory Reporting
- Daily tactical (collection-vs-target, agent productivity, FOS productivity, PTP-kept rate, channel-wise resolution), weekly (roll/flow), monthly (vintage curves).
- **Regulatory** — NHB / RBI returns: ALM-1/2/3, DNBS-13 / RNBC, NPA returns, frauds, complaints, large-exposure, CRILC; CIBIL/Experian/CRIF/Equifax monthly bureau submission.

---

## 3. Cross-Cutting / Platform Modules

### 3.1 Allocation Engine
- Rule-based (DPD × geography × ticket-size × language × channel) plus ML-based propensity allocation; supports auto-de-allocation when cured, re-allocation on bounce.

### 3.2 Workflow / Case Management
- BPM-style configurable workflows with SLAs, escalations, maker-checker, dual-control on critical actions (waiver, settlement, possession).

### 3.3 Customer 360 View
- Single pane: KYC, co-applicant/guarantor, property, valuation history, insurance, all loans, payment ledger, bureau snapshot, all interactions across SMS/IVR/Bot/Call/Field/Legal.

### 3.4 Document Management
- Versioned repository for sanction letter, agreement, ROC mortgage, title docs, insurance policies, valuation reports, all notices (Sec 138, Sec 13(2), 13(4), Sec 14 order, possession notice, sale notice), legal orders, settlement letters.
- **Digital signing / e-stamp** integration (NeSL, Leegality, Signzy, Digio).

### 3.5 Audit Trail & Compliance
- Immutable logs for every action, especially **legal triggers** and **agent calls**.
- Compliance to:
  - **RBI Fair Practices Code** — peaceful, professional, private recovery; no calls before 0800 / after 1900; no harassment; designated grievance officer ([RBI Master Circular FPC](https://www.rbi.org.in/commonman/english/Scripts/Notification.aspx?Id=1572)).
  - **Recovery-agent code of conduct** — IIBF certification, ID card display, no abusive language, no contact with non-relevant third parties.
  - **RBI Digital Lending Directions, 2025** (effective 8 May 2025) — borrower must be notified of authorised recovery agent before contact; **cash recovery permitted only for delinquent accounts and must be reflected same-day**; **DLA recovery may not access contacts/photos/location** purely for collections ([Leegality — Digital Lending Directions 2025](https://www.leegality.com/blog/digital-lending-directions-2025), [Lawrbit — RBI Digital Lending 2025](https://www.lawrbit.com/article/reserve-bank-of-india-digital-lending-directions-2025/)).
  - **Proposed Uniform Recovery Norms** (effective 1 July 2026) — consolidating recovery conduct across all REs, materially strengthening the regime ([Vinod Kothari — Uniform Recovery Norms](https://vinodkothari.com/2026/02/rbi-proposes-uniform-recovery-norms-across-all-lenders/)).

### 3.6 Integrations
- **LMS / LOS** (FinnOne, Nucleus, Lentra, Newgen, Omnifin, Indus Net Kratos, Pennant) — for daily DPD, EMI demand, repayment posting.
- **Core banking / GL** for receipt posting and reconciliation.
- **Credit bureaus** — CIBIL, CRIF Highmark (dominant for HFC affordable segment), Experian, Equifax — bureau pull for skip-trace, monthly submission of repayment data.
- **Payment gateways / UPI / BBPS / NACH sponsor banks**.
- **Telephony / cloud contact centre** — Knowlarity, Exotel, MyOperator, Ozonetel, Five9, Genesys.
- **WhatsApp BSP** — Gupshup, Karix, Meta Cloud API; **DLT-registered SMS**.
- **Property / valuation** — empanelled valuers, RERA, sub-registrar / e-stamp.

### 3.7 Analytics
- Roll-rate, flow-rate, vintage, recovery rate by bucket × geography × product × officer.
- Champion-challenger experiment tracker.
- Agent productivity, dialer pacing analytics, voice-bot containment, WhatsApp click-through.
- "Best time to call", "best channel" model outputs feeding back into the allocation engine.

---

## 4. AHFC-Specific Nuances

### 4.1 Long Tenor, Low Ticket, Informal Income
- Collection engagements often span **20–25 years**; the same borrower may move geographies, change phone numbers and bank accounts multiple times — making **periodic contactability refresh** and **skip-tracing** structurally more important than for personal-loan books.
- Self-employed, cash-flow-driven repayment behaviour means **seasonality models** (festive, harvest, school-fees season) outperform DPD-only triage.
- Field-heavy operating model — Aavas, Aadhar, Aptus, Home First operate large branch + FOS networks; the FOS app is the primary collections tool, not the dialer.

### 4.2 Property as Collateral
- **SARFAESI is the recovery centre of gravity.** Even when HFCs rarely auction in volume, the *threat* of Sec 13(2) / 13(4) drives cure rates — the platform must produce notices, possession orders, panchnamas, valuation packs and auction notices at scale.
- **Valuation refresh** policy — annual or trigger-based revaluation in NPA accounts to keep LTV honest.
- **Title and lien tracking** — equitable mortgage / registered mortgage; CERSAI charge creation and maintenance.

### 4.3 Co-applicant / Guarantor Handling
- All communications, demand notices and possession notices must be issued to **all obligants** — primary borrower, co-applicants, guarantors. The platform must natively model multi-obligant cases, not "primary + extras".

### 4.4 Insurance Claims
- **Loan-cover term insurance** on borrower; **property insurance** on collateral — both should auto-trigger workflows on death / disability / property damage events. Death cases need **claim-filing module** (with hospital records, FIR, post-mortem, succession docs) feeding into restructuring / closure.

### 4.5 Regulatory Calendar
- **NHB / RBI HFC Master Direction 2021 (consolidated) and Directions, 2025** — asset classification, provisioning, recovery agents (Annex XI) ([RBI HFC Directions 2025](https://www.nhb.org.in/wp-content/uploads/2026/01/Reserve-Bank-of-India-Housing-Finance-Companies-Directions-2025.pdf)).
- **Fair Practices Code Master Circular** — recovery conduct ([RBI](https://www.rbi.org.in/commonman/english/Scripts/Notification.aspx?Id=1572)).
- **Digital Lending Directions, 2025** — DLA reporting on CIMS portal (June 2025), LSP oversight, multi-lender norms (Nov 2025) ([Lawrbit](https://www.lawrbit.com/article/reserve-bank-of-india-digital-lending-directions-2025/), [Legal500](https://www.legal500.com/developments/thought-leadership/the-rbis-digital-lending-directions-2025-a-unified-code-for-a-fragmented-sector/)).
- **Proposed Uniform Recovery Framework** — effective 1 July 2026, consolidating norms across all REs ([Vinod Kothari](https://vinodkothari.com/2026/02/rbi-proposes-uniform-recovery-norms-across-all-lenders/)).
- **FACE — Self-regulatory guidelines on debt recovery** (Aug 2025) — reasonable industry baseline ([FACE PDF](https://faceofindia.org/wp-content/uploads/2025/08/FACE-Guidelines-on-debt-recovery_29-Aug-2025_website.pdf)).

---

## 5. Summary Table — Module → Why an AHFC Needs It → Typical Sub-Features

| # | Module | Why an AHFC needs it | Typical sub-features |
|---|---|---|---|
| 1 | **Risk Scoring & EWS** | Long tenor + thin-file customers make early signals decisive | PTD / PTP models, bureau triggers, bounce-history scoring, seasonality features |
| 2 | **Pre-due Multichannel Comm** | Most cures happen before DPD-1 | SMS, WhatsApp, IVR, voice-bot, email, multilingual content, throttling, quiet hours |
| 3 | **Payment Facilitation** | UPI-first, low-ticket borrowers expect 1-tap pay | Payment links, UPI, BBPS, e-NACH setup/swap/cancel, part-pay, advance-EMI |
| 4 | **NACH / PDC Mgmt** | Mandate is the EMI lifeline | NPCI file generation, return processing, re-presentation rules, bounce charge auto-levy, PDC inventory |
| 5 | **Contactability Hygiene** | 20-year relationship, customer changes phones | KYC refresh, address verification, alternate-contact capture, DND scrubbing |
| 6 | **Self-Service App** | Cuts cost-to-serve; allows self-cure | Statement, EMI calendar, NACH amend, payment, hardship request |
| 7 | **Bucket / Strategy Engine** | Different DPD needs different intensity | Rule engine, treatment ladder, roll-rate / flow-rate, champion-challenger |
| 8 | **Soft-bucket Digital** | Scale, low cost in 1-30 / 31-60 buckets | Voice-bot, WhatsApp-bot, predictive dialer, AMD, on-call payment links |
| 9 | **Tele-calling Module** | Human touch for 31-90 DPD | Agent desktop, scripts, dispositions, recording, AI QA, supervisor barge-in |
| 10 | **FOS / Field App** | Affordable segment is field-driven | Geo-tag, geo-fence, route-opt, visit photo, receipt, cash deposit, offline mode |
| 11 | **Legal Workflow** | Mortgage = SARFAESI is core | Sec 138, Sec 13(2)/13(4)/14/17, arbitration, Lok Adalat, civil suit, DRT, NCLT |
| 12 | **Property Auction** | Functional repossession for HFC | Valuation, reserve price, e-auction, EMD, sale certificate, surplus refund |
| 13 | **OTS / Restructuring / Re-aging** | Long tenor → frequent hardship | NPV calc, hair-cut matrix, approvals, moratorium, step-up, re-amortisation |
| 14 | **Skip Tracing** | 20-year contactability decay | Bureau, Karza/Perfios, employer trace, alternate-number scoring |
| 15 | **NPA / Provisioning / Write-off** | NHB / RBI mandate | Auto-classification, provisioning %, write-off approvals, TWO ledger, ARC sale |
| 16 | **Vendor / Agency Mgmt** | Outsourced last-mile | Empanelment, IIBF cert, allocation, SLA, tiered payouts, TDS/GST |
| 17 | **Receipt & GL Recon** | Cash + digital + sponsor-bank reconciliation | Receipt master, appropriation hierarchy, suspense, sponsor-bank match |
| 18 | **Dashboards & MIS** | Daily ops + monthly NHB/RBI returns | Tactical, vintage, regulatory returns, bureau submission |
| 19 | **Allocation Engine** | Routes case to right channel/agent | Rule-based + ML, auto-realloc on cure/bounce |
| 20 | **Workflow / Case Mgmt** | Multi-step legal + settlement processes | BPM, SLA, maker-checker, escalations |
| 21 | **Customer 360** | One pane across loans, property, obligants | KYC, co-applicant, valuation, insurance, all interactions |
| 22 | **Document Mgmt** | Mortgage book is paper-heavy | Versioned repo, e-sign, e-stamp, NeSL/Leegality |
| 23 | **Audit Trail & Compliance** | FPC, IIBF, Digital Lending 2025 | Immutable logs, call recording, consent capture, grievance redressal |
| 24 | **Integrations** | LMS, CB, GW, telephony, BSP | API-first, eventing, retries, idempotency |
| 25 | **Analytics** | Continuous improvement | Roll-rate, flow-rate, vintage, agent productivity, champion-challenger |

---

## Sources

- [Credgenics — Debt Collections Platform](https://www.credgenics.com/)
- [Credgenics — DialNext Dialer](https://www.credgenics.com/dialnext-dialer)
- [Credgenics — CG Collect FOS](https://www.credgenics.com/cg-collect-field-debt-collections)
- [Credgenics — Lok Adalats for pre-litigation debt recovery](https://blog.credgenics.com/lok-adalats-for-pre-litigation-debt-recovery/)
- [Credgenics — Skip tracing tools in debt collections](https://blog.credgenics.com/skip-tracing-tools-in-debt-collections/)
- [Spocto — About](https://spocto.com/about/)
- [Spocto X / PSB AI-driven recoveries](https://www.business-standard.com/content/press-releases-ani/public-sector-banks-embrace-ai-driven-debt-collection-with-spocto-x-boosting-recoveries-by-60-percent-125040301306_1.html)
- [Mobicule — mCollect field collection](https://www.mobicule.com/debt-collection/field-collection.html)
- [Bajaj — Section 13(2) SARFAESI](https://www.bajajfinserv.in/understanding-sec-13-2-of-sarfaesi-act)
- [Bajaj — Section 13(4) SARFAESI](https://www.bajajfinserv.in/understanding-sec-13-4-of-sarfaesi-act)
- [Lexology — HFC SARFAESI for loans below INR 20L](https://www.lexology.com/library/detail.aspx?g=0da30551-ef27-4dcd-a683-a4457809ff8c)
- [SCC Times — Section 138 NI Act procedure](https://www.scconline.com/blog/post/2025/11/15/section-138-ni-act-cheque-bounce-notice/)
- [NALSA — Lok Adalat](https://nalsa.gov.in/lok-adalat)
- [NHB — HFC Master Circular Directions 2010](https://www.nhb.org.in/wp-content/uploads/2019/07/MC01-Master-Circular-The-Housing-Finance-Companies-NHB-Directions-2010.pdf)
- [RBI HFC Directions 2025 (NHB hosted)](https://www.nhb.org.in/wp-content/uploads/2026/01/Reserve-Bank-of-India-Housing-Finance-Companies-Directions-2025.pdf)
- [RBI Master Circular — Fair Practices Code](https://www.rbi.org.in/commonman/english/Scripts/Notification.aspx?Id=1572)
- [Vinod Kothari — RBI Proposes Uniform Recovery Norms (2026)](https://vinodkothari.com/2026/02/rbi-proposes-uniform-recovery-norms-across-all-lenders/)
- [Leegality — RBI Digital Lending Directions 2025](https://www.leegality.com/blog/digital-lending-directions-2025)
- [Lawrbit — RBI Digital Lending Directions 2025](https://www.lawrbit.com/article/reserve-bank-of-india-digital-lending-directions-2025/)
- [Legal500 — RBI Digital Lending Directions 2025](https://www.legal500.com/developments/thought-leadership/the-rbis-digital-lending-directions-2025-a-unified-code-for-a-fragmented-sector/)
- [CredSettle — RBI Rules for Recovery Agents 2025](https://www.credsettle.com/rbi-rules-for-recovery-agents)
- [FACE — Guidelines on Debt Recovery (Aug 2025)](https://faceofindia.org/wp-content/uploads/2025/08/FACE-Guidelines-on-debt-recovery_29-Aug-2025_website.pdf)
- [Experian — Champion / Challenger collections strategy](https://www.experian.ie/content/dam/marketing/uki/ireland/ie/assets/decision-analytics/white-papers/experian_champion_challenger.pdf)
- [Indebted — Champion challenger collections](https://www.indebted.co/blog/guides/putting-your-collections-strategy-to-the-test-with-a-champion-challenger-model/)
- [TrueAccord — Collections Economics 101](https://blog.trueaccord.com/2022/02/collections-economics-101-for-digital-lenders/)
- [HDFC — Loan e-NACH and e-Mandate](https://www.hdfc.bank.in/payments/loan-e-nach-and-e-mandate)
- [BasicHomeLoan — ECS Return Charges on Home Loans](https://www.basichomeloan.com/blog/home-loans/ecs-return-charges-on-home-loans)
- [ImpactAlpha — Affordable home lending market in India](https://impactalpha.com/behind-the-success-of-indias-affordable-home-lending-market-patient-impact-capital/)
- [Datacultr — Skip Tracing in Debt Collection](https://datacultr.com/blogs/skip-tracing-in-debt-collection/)
