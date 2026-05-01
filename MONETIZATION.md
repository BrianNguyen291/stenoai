# StenoAI Monetization Plan

Vertical focus: **dental / medical clinics in Taiwan and broader Asia**. Privacy-first local AI is the differentiator vs US cloud tools (Heidi, DeepScribe, Abridge) that have PIPA / data-sovereignty concerns.

> Disclaimer: numbers are estimates from public comparables and Taiwan dental market size (~7,000 clinics, ~14,000 dentists). Validate with 5–10 user interviews before committing prices.

---

## 1. Pricing benchmarks (USD/month/seat)

| Segment | Examples | Range |
|---|---|---|
| Generic AI meeting tools | Otter, Fireflies | $15-30 |
| Privacy-first AI notes | Granola, Limitless | $25-100 |
| Medical AI scribe (US) | Heidi, DeepScribe, Abridge | $200-500 |
| Dental-specific (US/EU) | Saoirse, dentaltap | $150-400 |
| Hospital-grade scribe | Suki, Augmedix | $300-800 |

**Recommended landing zone for Asia dental:** $50-150 USD/month per dentist (~NT$1,500-4,500).

---

## 2. Pricing models — pick one

### Option A: Per-seat SaaS (recommended)
- Solo dentist: $80/mo
- Clinic (2-5 docs): $60/mo per seat + $20/mo admin seat
- Hospital / chain: custom enterprise contract

### Option B: Per-clinic flat + usage add-ons
- Base $200/mo unlimited doctors
- Usage: $0.05/min cloud transcription (free if using local Ollama)
- Usage: $0.02 per LINE / SMS / Email reminder sent

### Option C: One-time license (NOT recommended)
- $1,500-3,000 per clinic, perpetual + paid major upgrades
- Hard to fund ongoing development; predictable revenue suffers

**Pick A.** Simplest to sell, predictable MRR, easy to upsell features.

---

## 3. Add-on revenue streams

| Feature | Pricing | Margin | Build effort |
|---|---|---|---|
| **Medical report templates** (insurance claims, referrals, dental records, follow-up letters) | $5-10 / report or $30/mo unlimited | High | Medium |
| **LINE / SMS reminders** | $0.02-0.05 per message + $20/mo platform | Med | Medium |
| **Post-visit follow-up automation** (auto-send care instructions, schedule recall) | $50/mo per clinic | High | Medium |
| **Multi-language reports** (zh-Hant + en simultaneously) | $20/mo upgrade | High | Low |
| **Image attachments** (X-rays, intra-oral photos in summary) | $30/mo per clinic | Med | Medium |
| **Patient portal** (patient sees their summary, signs consent) | $40/mo | Med | High |
| **Insurance claim auto-fill (健保 codes)** ⭐ | $80/mo per clinic | Very high | High |
| **CRM/EHR integration** (Dentrix, Open Dental, Taiwan-specific systems) | $100/mo | High | Very high |
| **Voice-controlled charting** ("mark tooth 16 cavity") | $150/mo | Very high | High |

⭐ = Strongest add-on. Insurance claim auto-fill is the biggest unsolved pain point in Taiwan dental. If you nail this, you can price the bundle at $250+ /mo per dentist.

---

## 4. Revenue projection (Taiwan only)

Assumptions:
- ~7,000 active dental clinics
- ~14,000 practising dentists
- ARPU $80/mo base (no add-ons)
- Add-ons typically 30-50% of base ARPU once mature

| Adoption | Dentists paying | Base MRR | + Add-ons | Year-1 ARR |
|---|---|---|---|---|
| 0.5% | 70 | $5.6K | $7.5K | ~$90K |
| 1.0% | 140 | $11.2K | $15K | ~$180K |
| 3.0% | 420 | $33.6K | $45K | ~$540K |
| 5.0% | 700 | $56K | $75K | ~$900K |

5% of Taiwan dental market in 2-3 years is achievable for a privacy-first vertical product if you nail insurance claim integration.

---

## 5. Levers that 5-10x the ceiling

1. **Insurance claim auto-fill (健保 codes)** — biggest Taiwan-specific pain point. Premium add-on or bundled in higher tier.
2. **HIPAA / PIPA compliance certification** — unlocks enterprise / hospital sales. 5-10x ARPU possible.
3. **Multi-vertical** — same engine for legal, therapy, financial advice, sales calls. 5x TAM, but dilutes brand. Better to verticalize first.
4. **API for EHR integration** — Dentrix, Open Dental, Taiwan-local systems. Per-call fees + monthly platform.
5. **Regional expansion** — Japan (HIPAA-equivalent: APPI), Hong Kong, Singapore, Thailand. Translation effort modest given existing multi-language.
6. **White-label** — sell engine to dental software vendors as their AI feature. B2B contract: $5K-50K/mo.

---

## 6. Compliance & legal — must-do before commercial sale

| Item | Why | Cost |
|---|---|---|
| Taiwan PIPA (個人資料保護法) compliance review | Audio + transcript = sensitive personal data | Lawyer ~$2-5K |
| Encryption at rest (Keychain for keys, encrypted recordings folder) | Required under PIPA | Dev time |
| Audit log + retention controls | Configurable delete-after-X-days | Dev time |
| Liability disclaimer + human-review workflow | LLM hallucinations on medical content = malpractice exposure | Legal review |
| Data Processing Agreement template | Clinics need this for staff/patient | Lawyer |
| Optional: 衛福部 medical device classification check | If reports are presented as medical decisions, you may need certification | $50K-200K, 6-18 months — AVOID by framing as "AI assistant, not medical device" |
| Cyber insurance | Recommended | $200-500/mo |
| ISO 27001 (later, for enterprise) | Hospital sales | $20K-50K |

**Crucial:** position the product as **assistive note-taking** with required human review. Do NOT claim it generates medical decisions or diagnoses. This keeps you out of medical-device classification.

---

## 7. Sale / acquisition outlook

If you build to ~100 paying Taiwan clinics:
- 100 × $200 ARPU × 12 = $240K ARR
- Asia SaaS multiple ~3-5x ARR = **$720K - $1.2M USD acquisition**
- Strategic buyer (dental software vendor adding AI) = 5-8x = **$1.2M - $2M**

If you scale to **$1M ARR** (~500 clinics or vertical expansion):
- Standard multiple: $5M - $15M
- Strategic premium for verticals or insurance integration: $15M - $30M

If you hit **$10M ARR** (regional expansion + add-ons):
- Likely target for Japan/US dental SaaS or EHR vendor: $50M - $150M

Realistic 3-5 year window for Asia-only path: **$1M-5M exit if executed well**, $10M+ if you crack insurance integration and multi-country.

---

## 8. Build-priority order to reach commercial readiness

**Must-have for v1.0 commercial:**
1. Auto-update + crash reporting (Tier 1 in `ROADMAP.md`)
2. Cloud LLM key in Keychain (security)
3. Audit log + retention config (compliance)
4. Liability disclaimer + edit-before-share workflow
5. Person tagging + cross-meeting chat (Tier 1 of person memory)
6. Medical report templates (custom prompt per type)
7. Export to Word/PDF (clinics need this)
8. Customer Memory feature (already shipped — strong rapport differentiator)

**v1.5 — strong differentiation:**
9. LINE Bot reminders + post-visit care
10. Insurance claim auto-fill (基本 codes first; full 健保 integration phase 2)
11. Multi-language report (zh-Hant + en simultaneously)
12. Patient portal (read-only summary)

**v2.0 — enterprise / scale:**
13. EHR integration (start with one Taiwan system)
14. Multi-tenant admin (clinic owner sees all dentists)
15. White-label option
16. HIPAA/PIPA full audit

---

## 9. Go-to-market sketch

**Beachhead:** solo dentists who already do video consultations / online charting (early adopters).

1. Free 14-day trial of Pro tier (cloud LLM + add-ons enabled)
2. Direct outreach: dental association events, FB groups for Taiwan dentists, LINE communities
3. Reference customers (3-5 clinics) for testimonials before broader push
4. Content marketing: blog posts on "AI for dental clinics", "PIPA-safe AI notes"
5. Partner channel: dental supply vendors, dental association sponsorships
6. Avoid Mac App Store initially — commission cut hurts margins; direct DMG is fine

---

## 10. Risk register

| Risk | Mitigation |
|---|---|
| Whisper hallucinations on medical terms | Domain-specific glossary, post-process correction, mandatory human edit before share |
| Patient consent not captured | Add "consent recorded" toggle + auto-stop on patient exit detected |
| Local model accuracy too low for $80/mo perception | Default Pro tier to cloud LLM; local stays free tier |
| Clinic IT staff resistance | Offer install service ($200) for first deploy |
| Competing US tools localize to Taiwan | Move fast; lock in 健保 / LINE integration as moat |
| Liability for missed action items | Show prominent "AI-generated, review before use" banner |
| LINE/Meta API policy changes | Have SMS / email fallback ready |
| macOS-only limits market | Windows port within first 12 months (~$15K dev cost) |

---

## 11. Quick decisions to make before pricing

- [ ] Free tier: local Ollama only? Or always paid?
- [ ] Trial length: 7, 14, or 30 days?
- [ ] Currency: USD globally, or NTD for Taiwan?
- [ ] Annual discount: 2 months free (16.7%)?
- [ ] Volume discount tier: 5+ seats, 10+ seats?
- [ ] Cancellation: monthly easy-cancel, or annual lock-in?
- [ ] Refund policy: 30-day no-questions, or pro-rated?

Decide before launching pricing page; changing later annoys early customers.
