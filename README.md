# Market Map & Wedge — Human Delta

## TL;DR

- **The market isn't "us vs. agent builders."** Agent builders are the platform we grade. Our real competition is (1) the builder's QA roadmap, (2) the CX QA tool that already owns the QA budget, and (3) KM vendors moving up from below into "answer quality."
- **We sit in the one seat nobody else can structurally occupy:** independent of the agent, independent of the CMS, and grading live answers *against the source* — then fixing the source.
- **The wedge:** sell Audit + Evals as one loop, *scoped to the slice of the corpus behind a dated event* (a launch, an incident, a regulator), to a sponsor above CX ops. 

---

## 1. The map

The stack an enterprise customer-facing agent runs on, bottom to top:

```
  CUSTOMERS / EMPLOYEES / OTHER AGENTS
              ▲
  ┌───────────┴────────────────────────────────────────────┐
  │ 4. CHANNEL      BPOs (Ubiquity, Sutherland), SIs       │  who's in the room
  ├────────────────────────────────────────────────────────┤
  │ 3. AGENT        Decagon · Sierra · Parloa · Intercom   │  says the answer
  │                 Fin · Agentforce · Google CCAI ·       │
  │                 Amazon Connect · Writer · in-house     │
  ├───────────┬────────────────────────────┬───────────────┤
  │ 3a. QA    │  ★ HUMAN DELTA ★           │ 3b. LLM EVAL  │  judges the answer
  │ MaestroQA │  independent groundedness  │ Braintrust ·  │
  │ Level AI  │  grade → root-cause        │ Arize ·       │
  │ Observe.AI│  article → remediate       │ Galileo ·     │
  │ vendor QA │  ◄──── the loop ────►      │ LangSmith     │
  ├───────────┴────────────────────────────┴───────────────┤
  │ 2. KNOWLEDGE    Guru · Shelf · Stonly · Zendesk/SFDC   │  stores the truth
  │                 Knowledge · Glean (internal search)    │
  ├────────────────────────────────────────────────────────┤
  │ 1. SYSTEMS OF RECORD   policy docs, CMS, contracts     │
  └────────────────────────────────────────────────────────┘
  Side surface: GEO / AI-visibility (ChatGPT, Gemini, Claude, Perplexity)
```

### Who's there and what each one structurally can't do

| Layer | Players | What they do | What they **structurally can't** do | Relationship to us |
|---|---|---|---|---|
| **3. Agent platforms** | Decagon, Sierra, Parloa, Intercom Fin, Agentforce, Google CCAI, Amazon Connect, Writer (its Agent Builder; e.g. Uber drafts support responses on it) | Build and run the agent; increasingly ship built-in QA (Decagon Watchtower, Sierra simulations, Agentforce Testing Center) | **Grade themselves credibly.** Their QA scores against rubrics the customer writes; none of the public material describes tracing a bad answer to the article that caused it, and none of them will fix a corpus they don't own. Every quarter their QA finds problems is a quarter their deflection number looks worse. | **Host, not rival.** In 15 of 24 prospects a third-party agent is already live. The threat is their *roadmap promise* (lost Kingsway, D-005; Decagon just told Northline "Q4"). |
| **3a. CX QA** | MaestroQA, Level AI, Observe.AI, Zendesk QA | Owns the QA budget line and the QA manager. Now auto-scores 100% of conversations, bot or human. | **Know if the answer was true.** Scorecards measure tone, process, required disclosures. Nothing in the rubric connects an answer to a source document. They grade the conversation, not the corpus. | **Incumbent for the budget.** Also a plausible partner: we feed a groundedness score into their scorecard. |
| **3b. LLM eval / observability** | Braintrust, Arize, Galileo, LangSmith | Faithfulness/hallucination metrics, LLM-as-judge, CI gates | **Sell to CX, or fix content.** Bought by 5–15 person engineering teams. Flagged outputs become test cases, not article fixes. Assumes you control the agent's code — useless when the agent is a Decagon black box. | **Real competitor only for in-house builds** (Axiom PR-113, Ardent loss D-008). Elsewhere, different buyer. |
| **2. Knowledge management** | Guru, Shelf, Stonly, native Zendesk/SFDC Knowledge | Store and govern articles. Moving up: Guru auto-verifies/unverifies content across connected systems; Shelf claims to identify the source of bad RAG answers. | **Be neutral about their own content, or see the live conversation.** They grade the library they sell. They don't sit on the agent's production transcripts, and a corpus they've been "governing" for years that still has a 2019 article live is their failure, not their finding. | **Closest structural competitor — and best write-back target.** Watch Shelf hardest. |
| **2. Enterprise search** | Glean | Employee-facing search and assistants over internal knowledge. Does sell into support, but as an assistant for *human reps* working tickets (e.g. Glean in Service Cloud). | **Grade the customer-facing bot, or fix the corpus.** Reading the help center isn't auditing it; Glean helps an employee find an answer, it doesn't check whether a bot's answer to a customer was true. Usually bought by CIO / IT for productivity, not by the owner of bot accuracy. | **Adjacent, not direct.** Could even index our cleaned corpus. Borrow its simple pitch, not its fight. |
| **4. Channel** | BPOs (Ubiquity, Sutherland), SIs | Run tier-1, own the client relationship, sit in renewals | **Build this.** They need a reliability story to keep the account as bots replace their seats. | **Distribution.** 2/2 BPO-sourced deals won, fastest cycles (45 and 51 days). SIs are the opposite — they're paid to say the stack is complete (Halcyon PR-109). |
| **Hyperscalers** | AWS, Microsoft, Google, Salesforce | Platform primitives | **Do per-customer, cross-vendor work.** They won't grade a rival's agent or write back into a rival's CMS. | Not a near-term competitor; a "won't they build it" investor question. |
| **GEO** | AI-visibility trackers | Measure brand share in AI assistants | **Fix the content that drives the share.** | Expansion surface, sold to marketing. Not a wedge. |

### Where we go

**The independent verification layer between knowledge and agent.** Not "a control plane" on the first call — the auditor, not the accountant. Three properties together put us there and nobody else has all three:

1. **Independent** of the agent vendor (unlike vendor QA) and of the CMS (unlike Guru/Shelf).
2. **Grounded** — grades against the source document, not a tone rubric (unlike CX QA).
3. **Closed loop** — the failed answer points to the article, and we fix the article (unlike LLM eval tools).

---

## 2. Redrawing `market_notes.md`

Read on the *product* is mostly right: the loop, the BPOs, the hyperscalers. Where the notes go wrong is *who the competition is* and two sales instincts that our own deal history contradicts. Below: what's right, what's half right, what's wrong, and what's missing.

### What's right (keep)

- **Draw the loop before you name it.** The loop (bad answer → bad article → fix) is the thing nobody else does. "Control plane" means nothing until the buyer has seen it.
- **Move fast with BPOs.** The strongest signal in the data. Both BPO-introduced deals were won, in 45 and 51 days, against an average of about 104 days for lost deals. Northline, Orchard and Solace all came through BPOs.
- **"Our vendor already does this" is the objection to solve.** It's the main objection in two closed deals, and Dana raised it on the Northline call.
- **The hyperscaler answer.** AWS, Google and Microsoft won't grade a rival's bot or write fixes into a rival's article system.

### What's half right (sharpen)

- **"We're the only company closing the loop."** True today, but not for long. Shelf now publicly claims it can find the source of bad AI answers. What's still unique: we're independent of *both* the bot vendor and the article system, and we grade the bot's real conversations.
- **"KB tools are where our pressure comes from."** Right companies, wrong reason. The threat isn't that Guru and Shelf own the budget. It's that they're adding answer-checking features. They're also where our fixes get saved, so they're as much a partner as a rival.
- **"Buyers don't trust vendor QA."** Some don't, but that isn't what decides deals. Fairmark (D-001) heard "our vendor's QA is enough" and still bought; Kingsway (D-005) heard the same thing and didn't. The difference was that Fairmark had a launch date and an exec on call 2. Independence matters most to risk buyers (Chief Risk Officers, compliance), so lead with it there.

### What's wrong (redraw)

- **"Agent builders are our two biggest competitors."** They're the platform we grade, not our rival. The brief says we sit beside them, and every Northline failure happened inside Decagon. What actually beats us is their *promise*: Kingsway waited for a feature Decagon said was coming, and Decagon just told Northline the same thing ("Q4"). Our answer is "your deadline comes before their roadmap."
- **"Glean and Writer are direct competitors."** The test is: would a buyer choose them *instead of* us for the same problem? When Northline's bot said something false on local TV, Glean would have helped human reps search faster, but it wouldn't have graded the bot or found the bad article. Writer builds bots, so it belongs with Decagon and Sierra; if a customer uses it, we grade it too. Neither appears in any of our 16 closed deals.
- **"Every enterprise is on a clock."** Most aren't, and that's the point. All 8 wins had a committed deadline; 7 of 8 losses didn't. The deadline isn't a universal truth to assume, it's the test for which prospects to chase.
- **"Lead with GEO on every first call."** GEO demos well, but the brief says it's only for existing customers, and the people who care about it are in marketing, not the customer-experience team that owns the bot. Calder & Roe is all GEO interest and no budget until 2029. Use GEO later, to get an exec's attention ("your competitor shows up in ChatGPT twice as often as you"), and as an add-on sale.
- **"Be flexible on unbundling evals."** Corvin (D-010) asked to buy only the grading, we said no, and they bought nothing. After a second incident they came back and bought everything at full price (D-014). Grading without fixing just hands the customer a list of problems nobody owns. The better "smaller yes" is what Fairmark bought: the full loop, on just the articles behind their launch.
---

## 3. The wedge

### In one line

**We sell independent groundedness grading plus remediation of the source — scoped to the knowledge behind your next dated event — to the exec who owns that date, against the alternative of trusting your agent vendor's QA and a 4% tone sample.**

### Who (qualification, from 16 closed deals)

| Must have | Evidence |
|---|---|
| A **dated trigger**: agent launch, public incident, regulator/audit, contract renewal | 8/8 wins had one; 7/8 losses didn't |
| An **exec sponsor by call 3** — CAIO, CRO, VP CX, not a QA manager | Every win had the economic buyer on call 1–3. Crestline (D-002) died with a QA-manager champion who never introduced the buyer. |
| A **live or dated customer-facing agent on a third-party platform** | No agent = nothing to grade (Wexler D-013, Lantern PR-121). In-house + engineering-led = they build evals themselves (Ardent D-008). |
| A **corpus big and old enough to be wrong** — thousands of articles, no full review in 2+ years | Bay Ridge (D-004) lost: 700 recently-reviewed articles, couldn't show decay. |
| **Read access to source content** | Marchmont (D-011) and Tessera (PR-102): infosec won't allow it. Wrong door. |

Best fits in today's pipeline: **Northline (PR-101)**, **Sable Insurance (PR-111)**, **Vantage Mobile (PR-106)**, **Peregrine Cruise (PR-123)**, **Pemberton Retail (PR-114)**. Wrong doors: Tessera, Ironwood (internal-only; wants us to build an agent), Calder & Roe (GEO only, no budget till FY29), Brightwell/Bowery/Alder (small, fresh corpora).

### Which wedge goes in first

- **Agent is live, failures are happening → lead with Evals.** Grade a sample before the second meeting, trace the worst failure to the article. (Northline, Vantage, Peregrine.)
- **Agent is paused or not yet launched → lead with Audit.** Nothing to grade yet; the corpus is the risk. (Sable — paused pending "a control story"; Meridian — RFP in progress.)
- Either way the contract is the loop. The wedge is just which half you show first.

### Why this year instead of next

Because the date is theirs, not ours. Their launch, their regulator, their renewal is on the calendar, and the vendor's groundedness feature is on a roadmap. Every week the agent runs without grading is another week of answers nobody checked, and the liability is already priced — Northline paid out on camera.

### Defending against each alternative

| They say | We say |
|---|---|
| **"Our vendor already does this."** (Decagon, Sierra, Agentforce) | "Their QA tells you which conversation went wrong. Ask it which *article* caused it — that's the question that fixes the next ten thousand. And when your regulator or your board asks who checked the agent, 'the agent vendor' isn't an independent answer." |
| **"It's on their roadmap for Q4."** | "Then it'll ship after your launch. We're grading now, and we'll be the second opinion when theirs arrives." Lost Kingsway by not having this answer — hold a date-based line. |
| **"Our QA team / MaestroQA already scores 100%."** | "Scores it for what? Tone and disclosures. Neither one checks the answer against the policy. We plug a groundedness score into the scorecard you already have." |
| **"Guru / Shelf governs our knowledge."** | "Then let's check its work. They grade the library they sell you; we grade what the agent actually said to customers. When we find the article, we write the fix back into your CMS." |
| **"We'll build it internally."** | Usually a sign of the wrong door — engineering-led, no CX sponsor (Ardent). Ask who owns the cost of a wrong answer to a customer. If nobody outside engineering does, disqualify. |
| **"Can we just do the eval part?"** | "We can do less corpus, not half the loop. Grading without fixing gives you a longer list of problems with no owner. We'll scope it to what the November launch reads from." |

---

## 4. Open questions I'd chase

- **Priya Raghunathan (VP AI CX, hired June).** "Doing her own thing with international." She's either the real buyer for this wedge or a competing internal build. Find out before going to Marcus.
- **What Decagon's Q4 groundedness feature actually is.** If it traces to source articles, the vendor-QA rebuttal narrows to independence + remediation.
- **Shelf.** Closest to closing the loop from the KM side. Need a real head-to-head.
- **`audit_findings.csv`** wasn't in my working set; the Northline narrative above uses only the root-cause IDs referenced in `eval_results.csv`. Full audit numbers would strengthen the call script.

---

## Sources

- [Decagon — Watchtower](https://decagon.ai/product/watchtower)
- [Sierra — Simulations](https://sierra.ai/blog/simulations-the-secret-behind-every-great-agent)
- [CX Today — Agentforce Testing Center](https://www.cxtoday.com/crm/salesforces-agentforce-testing-center-ensures-accurate-agent-responses/)
- [Guru — Automated knowledge quality](https://www.getguru.com/features/automated-knowledge-quality)
- [Shelf — RAG solution](https://shelf.io/rag-solution/)
- [Glean in Service Cloud](https://glean.com/blog/glean-service-cloud-launch)
- [Glean for customer support](https://www.glean.com/blog/glean-support-apps-actions)
- [Sacra — Writer profile](https://sacra.com/c/writer)
- [MaestroQA overview (Macha)](https://www.getmacha.com/blog/maestroqa-complete-guide)
- [Production AI eval tools compared (SoftwareSeni)](https://www.softwareseni.com/production-ai-evaluation-tools-compared-braintrust-arize-maxim-galileo-and-fiddler/)
- Internal: `deal_history.csv`, `prospects.csv`, `eval
