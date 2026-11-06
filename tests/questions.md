# Test questions

20 realistic questions run against production on 2026-11-06, sequentially, one call every 6 seconds,
with `User-Agent: usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill)` and `x-usedby-internal: 1`
(these were the owner's own test calls, so they are labelled internal and do not count as third-party usage).
The runs used pre-encoded URLs; the curl lines below are the `--data-urlencode` equivalent that the skill writes.

Total calls this session: 26 (4 exploration, 20 questions, 2 retries). Quota: 300 an hour per client, so 26 is under 9%. No 429.

Pass criterion: the answer the skill can write is **usable** (it answers the question) and **citable** (each line has a source URL).

**Result: 17 / 20 pass on the first call. 18 / 20 after one guided retry (q18).**

| # | Question | Endpoint | Verdict |
|---|---|---|---|
| q01 | We are a 40-person fintech. Which AI meeting note-taker should we use? | `what_peers_use` | PASS |
| q02 | What do law firms use for AI legal research? | `what_peers_use` | PASS |
| q03 | Who uses Harvey? | `who_uses` | PASS |
| q04 | Which companies use Cursor? | `who_uses` | PASS |
| q05 | What AI tools does Ramp use? | `company_stack` | PASS |
| q06 | What does Notion (notion.so) use? | `company_stack` | PASS |
| q07 | Cursor or GitHub Copilot for our engineering team? | `compare` | PASS |
| q08 | Intercom vs Zendesk for support? | `compare` | PASS |
| q09 | We need an AI support agent that integrates with Salesforce. | `shortlist_tools` | PASS |
| q10 | Which tool should we use to enrich and prioritise sales prospects? | `shortlist_tools` | PASS |
| q11 | We need a workflow automation tool, SOC 2 is mandatory. | `shortlist_tools` | FAIL |
| q12 | What AI coding assistants are there? | `search_tools` | PASS |
| q13 | Alternatives to Gong for call recording and analysis? | `search_tools` | PASS |
| q14 | Which companies started using Claude recently? | `recent_changes` | FAIL |
| q15 | What changed on usedby this week? | `recent_changes` | PASS |
| q16 | Do hospitals use Abridge? | `who_uses` | PASS |
| q17 | Which large enterprises use Glean? | `who_uses` | PASS |
| q18 | We are a small German software startup. What do companies like us use for AI translation? | `what_peers_use` | FAIL |
| q19 | Which payroll software should my 10-person bakery use? | `search_tools` | PASS |
| q20 | What is the best Python web framework? | `none (out of scope); one search_tools call confirmed` | PASS |

## q01. We are a 40-person fintech. Which AI meeting note-taker should we use?

Endpoint: `what_peers_use`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/peers \
  --data-urlencode 'task=meeting notes' --data-urlencode 'industry=fintech' --data-urlencode 'size=small' --data-urlencode 'limit=5'
```

Result (trimmed): status `answered`, tier T2, sample 13 companies. tl;dv 3 (3 OBSERVED); Fireflies 2 (1 CONFIRMED, 1 OBSERVED); Granola 2 (1 CONFIRMED, 1 OBSERVED); Grain 2 (2 OBSERVED). Source: https://www.usedby.ai/peers?category=meeting-assistants&limit=5&industry=fintech-payments&size=small&job=take-meeting-notes

Verdict: **PASS**. Tier T2 (same industry) answered; T1 had 3 companies only. Mostly OBSERVED lines: the answer must say so.

## q02. What do law firms use for AI legal research?

Endpoint: `what_peers_use`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/peers \
  --data-urlencode 'task=legal research' --data-urlencode 'industry=law firms' --data-urlencode 'limit=5'
```

Result (trimmed): status `answered`, tier T2, sample 289 companies. Harvey 143 (13 CONFIRMED, 130 OBSERVED); Legora 72 (18 CONFIRMED, 54 OBSERVED); Supio 23 (14 CONFIRMED, 9 OBSERVED); EvenUp 21 (18 CONFIRMED, 3 OBSERVED). Source: https://www.usedby.ai/peers?category=legal-compliance-ai&limit=5&industry=legal-compliance-legaltech&job=research-law

Verdict: **PASS**

## q03. Who uses Harvey?

Endpoint: `who_uses`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools/harvey/customers \
  --data-urlencode 'limit=5'
```

Result (trimmed): Harvey: 234 companies, mix {'confirmed': 20, 'declared': 0, 'observed': 214, 'signal': 0}. First lines: Bayer CONFIRMED https://www.harvey.ai/customers/bayer; Bridgewater CONFIRMED https://www.harvey.ai/customers/bridgewater; Deutsche Telekom CONFIRMED https://www.harvey.ai/customers/deutsche-telekom. Source: https://www.usedby.ai/tools/harvey

Verdict: **PASS**

## q04. Which companies use Cursor?

Endpoint: `who_uses`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools/cursor/customers \
  --data-urlencode 'limit=5'
```

Result (trimmed): Cursor: 118 companies, mix {'confirmed': 13, 'declared': 0, 'observed': 80, 'signal': 25}. First lines: Nvidia CONFIRMED https://cursor.com/blog/nvidia; PayPal CONFIRMED https://cursor.com/blog/paypal; Salesforce CONFIRMED https://cursor.com/blog/salesforce. Source: https://www.usedby.ai/tools/cursor

Verdict: **PASS**. 25 of 118 lines are SIGNAL (job posts): the answer must not call them case studies.

## q05. What AI tools does Ramp use?

Endpoint: `company_stack`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/companies/ramp/stack \
  --data-urlencode 'limit=10'
```

Result (trimmed): Ramp: 31 tools, mix {'confirmed': 13, 'declared': 0, 'observed': 18, 'signal': 0}. Assembled CONFIRMED https://www.assembled.com/case-study/ramp; Claude CONFIRMED https://claude.com/customers/ramp; Gumloop CONFIRMED https://ramp.com/leading-indicators/how-to-build-your-team-to-capitalize-on-ai; Hex CONFIRMED https://hex.tech/customers/ramp/. Source: https://www.usedby.ai/companies/ramp

Verdict: **PASS**

## q06. What does Notion (notion.so) use?

Endpoint: `company_stack`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/companies/notion.so/stack \
  --data-urlencode 'limit=10'
```

Result (trimmed): Notion: 5 tools, mix {'confirmed': 1, 'declared': 0, 'observed': 0, 'signal': 4}. Enterpret CONFIRMED https://www.enterpret.com/customers/notion; Cursor SIGNAL https://jobs.ashbyhq.com/notion/57b88701-42ce-4c7d-901d-bf65a6256ebc; Notion SIGNAL https://jobs.ashbyhq.com/notion/d27acf46-d250-4c23-a95d-1480eab74b0a; Snowflake SIGNAL https://jobs.ashbyhq.com/notion/6597b600-7781-46e6-b70a-a9af4fb2b61b. Source: https://www.usedby.ai/companies/notion-notion-so

Verdict: **PASS**. Domain resolved. 4 of 5 lines are SIGNAL, and one line says Notion uses Notion (a data oddity to report to usedby).

## q07. Cursor or GitHub Copilot for our engineering team?

Endpoint: `compare`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/compare \
  --data-urlencode 'a=cursor' --data-urlencode 'b=github-copilot' --data-urlencode 'limit=5'
```

Result (trimmed): Cursor 118 {'confirmed': 13, 'declared': 0, 'observed': 80, 'signal': 25} vs GitHub Copilot 52 {'confirmed': 10, 'declared': 0, 'observed': 26, 'signal': 16}; both 14 (e.g. Adobe). Source: https://www.usedby.ai/compare/cursor-vs-github-copilot

Verdict: **PASS**. Facts (certifications, free trial) come with their own source and checkedAt.

## q08. Intercom vs Zendesk for support?

Endpoint: `compare`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/compare \
  --data-urlencode 'a=intercom' --data-urlencode 'b=zendesk' --data-urlencode 'limit=5'
```

Result (trimmed): Intercom 126 {'confirmed': 37, 'declared': 8, 'observed': 77, 'signal': 4} vs Zendesk AI 28 {'confirmed': 9, 'declared': 0, 'observed': 17, 'signal': 2}; both 0. Source: https://www.usedby.ai/compare/intercom-vs-zendesk-ai

Verdict: **PASS**. `zendesk` resolved to **Zendesk AI**, not the Zendesk help desk. The answer must name the tool actually compared.

## q09. We need an AI support agent that integrates with Salesforce.

Endpoint: `shortlist_tools`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/shortlist \
  --data-urlencode 'task=support-resolution' --data-urlencode 'required=[{"criterion":"integrations","expected":"Salesforce"}]'
```

Result (trimmed): status `ok`, eligible ['Decagon', 'Dialpad', 'Intercom'], eligibleOverflow 4, needsVerification 18, evaluatedAt 2026-11-06T21:08:44.978Z. Decision source: https://www.usedby.ai/shortlist?task=support-resolution&required=%5B%7B%22criterion%22%3A%22integrations%22%2C%22expected%22%3A%22Salesforce%22%7D%5D

Verdict: **PASS**. Each eligible tool carries its vendor sourceUrl and checkedAt.

## q10. Which tool should we use to enrich and prioritise sales prospects?

Endpoint: `shortlist_tools`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/shortlist \
  --data-urlencode 'task=sales-enrichment'
```

Result (trimmed): status `ok`, eligible ['Actively', 'Amplemarket', 'Common Room'], eligibleOverflow 6, needsVerification 3, evaluatedAt 2026-11-06T21:08:51.257Z. Decision source: https://www.usedby.ai/shortlist?task=sales-enrichment

Verdict: **PASS**. No requirement sent: the answer must say the user's own requirements are unevaluated.

## q11. We need a workflow automation tool, SOC 2 is mandatory.

Endpoint: `shortlist_tools`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/shortlist \
  --data-urlencode 'task=workflow-automation' --data-urlencode 'required=[{"criterion":"certifications","expected":"SOC 2"}]'
```

Result (trimmed): status `ok`, eligible [], eligibleOverflow 0, needsVerification 63, evaluatedAt 2026-11-06T21:08:57.573Z. Decision source: https://www.usedby.ai/shortlist?task=workflow-automation&required=%5B%7B%22criterion%22%3A%22certifications%22%2C%22expected%22%3A%22SOC+2%22%7D%5D

Verdict: **FAIL**. Honest abstention but not useful: all 63 candidates have `no_evidence` for certifications. Retried with `SOC 2 Type II` (same result). Also one candidate returns `tool.name: null` (slug `airtable`). Data gap on usedby's side.

## q12. What AI coding assistants are there?

Endpoint: `search_tools`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools \
  --data-urlencode 'q=AI coding assistant' --data-urlencode 'limit=5'
```

Result (trimmed): Cursor (cursor, 118); GitHub Copilot (github-copilot, 52); Augment Code (augment-code, 29); Sourcegraph (sourcegraph, 29); Cognition (cognition, 28). Source: https://www.usedby.ai/tools

Verdict: **PASS**. Follow with who_uses on 1 to 3 slugs for proof lines.

## q13. Alternatives to Gong for call recording and analysis?

Endpoint: `search_tools`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools \
  --data-urlencode 'q=conversation intelligence' --data-urlencode 'limit=5'
```

Result (trimmed): Gong (gong, 81); Dialpad (dialpad, 43); CallMiner (callminer, 40); Invoca (invoca, 37); Aviso (aviso, 26). Source: https://www.usedby.ai/tools

Verdict: **PASS**. Invoca (advertising category) is a loose match; the answer should drop it or say why.

## q14. Which companies started using Claude recently?

Endpoint: `recent_changes`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/changes \
  --data-urlencode 'days=30' --data-urlencode 'tool=claude' --data-urlencode 'limit=5'
```

Result (trimmed): window 30 days, 0 companies, 0 lines. Source: https://www.usedby.ai/changes

Verdict: **FAIL**. Zero lines, and the response cannot say whether Claude is change-tracked. The user's question stays unanswered. Better route: who_uses claude and read `firstPublicProof` (an upper bound, not an adoption date).

## q15. What changed on usedby this week?

Endpoint: `recent_changes`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/changes \
  --data-urlencode 'days=7' --data-urlencode 'limit=5'
```

Result (trimmed): window 7 days, 157 companies, 161 lines. Altice USA → ASAPP OBSERVED https://www.asapp.com/; Engie → C3 AI CONFIRMED https://c3.ai/customers/no-code-ai-for-demand-prediction; Nubank → Traversal OBSERVED https://www.traversal.com/. Source: https://www.usedby.ai/changes

Verdict: **PASS**. The first returned line has `source: null` and relationship `unknown`: cite its claim.url only.

## q16. Do hospitals use Abridge?

Endpoint: `who_uses`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools/abridge/customers \
  --data-urlencode 'industry=healthcare' --data-urlencode 'limit=5'
```

Result (trimmed): Abridge: 36 companies, mix {'confirmed': 11, 'declared': 0, 'observed': 25, 'signal': 0}, filter matched 36. First lines: Akron Children's CONFIRMED https://www.abridge.com/case-study/akron-childrens; Cambridge Health Alliance CONFIRMED https://www.abridge.com/press-release/cambridge-health-alliance-announcement; CHRISTUS Health CONFIRMED https://www.abridge.com/case-study/christus-health. Source: https://www.usedby.ai/tools/abridge

Verdict: **PASS**

## q17. Which large enterprises use Glean?

Endpoint: `who_uses`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools/glean/customers \
  --data-urlencode 'size=ENTERPRISE,MEGA' --data-urlencode 'limit=5'
```

Result (trimmed): Glean: 58 companies, mix {'confirmed': 30, 'declared': 1, 'observed': 23, 'signal': 4}, filter matched 11. First lines: DBS CONFIRMED https://www.glean.com/resources/customer-stories/dbs; Ericsson CONFIRMED https://www.glean.com/resources/customer-stories/ericsson; Zillow CONFIRMED https://www.glean.com/resources/customer-stories/zillow. Source: https://www.usedby.ai/tools/glean

Verdict: **PASS**. 11 of 58 customers match the size bands.

## q18. We are a small German software startup. What do companies like us use for AI translation?

Endpoint: `what_peers_use`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/peers \
  --data-urlencode 'task=translation' --data-urlencode 'industry=software' --data-urlencode 'size=small' --data-urlencode 'country=DE' --data-urlencode 'limit=5'
```

Result (trimmed): status `clarify`, field `industry`, candidates ['accounting-audit', 'advertising-adtech', 'aerospace-defense', 'agency-studio', 'agriculture-agtech']

Verdict: **FAIL**. `industry=software` is ambiguous and the 5 candidates are the first 5 industries alphabetically (accounting to agriculture), not near matches. Retry with `industry=devtools` (call 25) answered at T2: status `answered`, tier T2, sample 8 companies. Grammarly 4 (4 OBSERVED); DeepL 3 (1 CONFIRMED, 1 DECLARED, 1 OBSERVED); Wordtune 1 (1 OBSERVED). Source: https://www.usedby.ai/peers?category=general-writing-assistants&limit=5&industry=devtools-infrastructure&size=small&country=DE&job=translate-content. Usable after the retry, but the top tool (Grammarly) is a writing tool: the category is "Writing & translation".

## q19. Which payroll software should my 10-person bakery use?

Endpoint: `search_tools`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools \
  --data-urlencode 'q=payroll' --data-urlencode 'limit=5'
```

Result (trimmed): `tools: []`. Note: "No tracked tool matches "payroll". That means usedby has no public proof for it, not that it has no customers. Try a category word (for example "customer servic…" Source: https://www.usedby.ai/tools

Verdict: **PASS**. Correct no-line answer: the skill says usedby has no line, then may answer from general knowledge, labelled as such.

## q20. What is the best Python web framework?

Endpoint: `none (out of scope); one search_tools call confirmed`

```bash
curl -sS -G -A "$UA" https://www.usedby.ai/api/v1/tools \
  --data-urlencode 'q=python web framework' --data-urlencode 'limit=5'
```

Result (trimmed): `tools: []`. Note: "No tracked tool matches "python web framework". That means usedby has no public proof for it, not that it has no customers. Try a category word (for example "cu…" Source: https://www.usedby.ai/tools

Verdict: **PASS**. Out of scope by design (code dependency, not a tool companies adopt). The SKILL.md section 1 tells the agent not to call; this call confirms the empty answer.

## Failures and what would fix them

- **q11**: usedby holds no `certifications` evidence for the workflow-automation pilot tools. Fix on usedby's side (capture security pages for those tools). Not a skill bug.
- **q14**: `recent_changes` cannot tell "no new customer" from "not tracked". The SKILL.md now tells the agent to say both are possible; a `tracked: true|false` field per tool on usedby's side would settle it.
- **q18**: the peers `clarify` answer lists the first 5 industries in alphabetical order, not the closest ones. The SKILL.md now tells the agent to retry once with a more specific term. Fix on usedby's side: rank candidates by similarity to the input.
