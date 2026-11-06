# usedby API v1: endpoints

Base URL `https://www.usedby.ai/api/v1`. Read-only, no key. OpenAPI: https://www.usedby.ai/api/v1/openapi.json. Human docs: https://www.usedby.ai/developers.

Every response below is a real production response captured on 2026-11-06, trimmed (`"…"` marks cuts). Counts change daily.

## search_tools

Tools by name, other names, topic or category. `customers.total` counts every public line; `named` = CONFIRMED+DECLARED+OBSERVED; `signals` = job posts.

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/tools \
  --data-urlencode 'q=AI coding assistant' --data-urlencode 'limit=5'
```

Params: `q` (required), `limit` 1-100. Empty `tools: []` means usedby has no public line, not that the tool has no customers.

Response (trimmed):

```json
{
  "query": "AI coding assistant",
  "tools": [
    {
      "name": "Cursor",
      "slug": "cursor",
      "tagline": "AI code editor and coding agent that turns ideas into code",
      "category": {
        "name": "AI coding assistants & agents",
        "slug": "ai-code-assistants"
      },
      "customers": {
        "total": 118,
        "named": 93,
        "signals": 25
      },
      "match": "topic",
      "url": "https://www.usedby.ai/tools/cursor"
    },
    {
      "name": "GitHub Copilot",
      "slug": "github-copilot",
      "tagline": "AI coding agent that works from an open issue to a reviewed and merged pull request",
      "category": {
        "name": "AI coding assistants & agents",
        "slug": "ai-code-assistants"
      },
      "customers": {
        "total": 52,
        "named": 36,
        "signals": 16
      },
      "match": "topic",
      "url": "https://www.usedby.ai/tools/github-copilot"
    }
  ],
  "truncated": true,
  "notes": [
    "…"
  ],
  "source": {
    "name": "usedby",
    "url": "https://www.usedby.ai/tools",
    "markdown": null,
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby (https://www.usedby.ai/tools)"
  }
}
```

## who_uses

Companies that publicly use one tool, strongest proof first, each with its source.

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/tools/harvey/customers \
  --data-urlencode 'limit=5'
```

Params: `limit` 1-100 (default 25), `size` (`STARTUP,SMALL,MEDIUM,LARGE,ENTERPRISE,MEGA`), `industry` (name, slug or plain label; 400 with candidates when unknown), `offset` + `snapshot` (follow `pagination.next`), `includeEvidence`, `includeCapabilities`, `constraint`. Anonymous access stops at 200 rows per resource.

Response (trimmed):

```json
{
  "tool": {
    "name": "Harvey",
    "slug": "harvey",
    "category": {
      "name": "Legal AI",
      "slug": "legal-compliance-ai"
    },
    "url": "https://www.usedby.ai/tools/harvey"
  },
  "customers": {
    "total": 234,
    "named": 234,
    "mix": {
      "confirmed": 20,
      "declared": 0,
      "observed": 214,
      "signal": 0
    },
    "bySize": {
      "STARTUP": 2,
      "SMALL": 16,
      "MEDIUM": 39,
      "LARGE": 32,
      "ENTERPRISE": 6,
      "MEGA": 29,
      "unknown": 110
    },
    "listed": [
      {
        "claim": {
          "id": "e01e1b7e-2ded-4e81-a068-54ef038e847e",
          "url": "https://www.usedby.ai/claims/e01e1b7e-2ded-4e81-a068-54ef038e847e",
          "relationship": {
            "type": "documented_usage",
            "label": "Usage described by a public source"
          },
          "proof": {
            "level": "CONFIRMED",
            "method": "CASE_STUDY"
          },
          "…": "freshness, dates, source, limitations, citation"
        },
        "company": "Bayer",
        "slug": "bayer",
        "industry": "Biotech & Pharma",
        "size": "MEGA",
        "proof": "CONFIRMED",
        "firstPublicProof": {
          "date": "2024-10-06",
          "kind": "on_or_before"
        },
        "source": {
          "url": "https://www.harvey.ai/customers/bayer",
          "method": "CASE_STUDY",
          "capturedAt": "2026-10-24"
        },
        "url": "https://www.usedby.ai/companies/bayer"
      },
      "… 4 more"
    ],
    "truncated": true
  },
  "latestCheck": {
    "date": "2026-11-04",
    "checkedLines": 205
  },
  "notes": [
    "…"
  ],
  "source": {
    "name": "usedby",
    "url": "https://www.usedby.ai/tools/harvey",
    "markdown": "https://www.usedby.ai/tools/harvey.md",
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby (https://www.usedby.ai/tools/harvey)"
  },
  "pagination": {
    "offset": 0,
    "nextOffset": 5,
    "next": "https://www.usedby.ai/api/v1/tools/harvey/customers?limit=5&offset=5&snapshot=2026-11-06T16%3A05%3A37.494Z",
    "snapshot": "2026-11-06T16:05:37.494Z"
  }
}
```

## company_stack

Tools one company publicly uses. The path takes a slug, a name or a domain (`notion.so` resolved to `notion-notion-so`).

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/companies/ramp/stack \
  --data-urlencode 'limit=10'
```

Params: `limit` 1-100 (default 25), `offset`, `snapshot`.

Response (trimmed):

```json
{
  "company": {
    "name": "Ramp",
    "slug": "ramp",
    "website": "https://ramp.com",
    "industry": "Fintech & Payments",
    "headquarters": null,
    "country": "US",
    "size": {
      "band": "LARGE",
      "employeesMin": 1001,
      "employeesMax": 5000
    },
    "url": "https://www.usedby.ai/companies/ramp"
  },
  "tools": {
    "total": 31,
    "named": 31,
    "mix": {
      "confirmed": 13,
      "declared": 0,
      "observed": 18,
      "signal": 0
    },
    "listed": [
      {
        "claim": {
          "url": "https://www.usedby.ai/claims/rev_usage_11b85f2f32ddd59d4ee874c5",
          "…": "trimmed"
        },
        "tool": "Claude",
        "slug": "claude",
        "vendor": "Anthropic",
        "category": "AI assistants",
        "proof": "CONFIRMED",
        "firstPublicProof": {
          "date": "2026-10-18",
          "kind": "on_or_before"
        },
        "source": {
          "url": "https://claude.com/customers/ramp",
          "method": "CASE_STUDY",
          "capturedAt": "2026-10-18"
        },
        "url": "https://www.usedby.ai/tools/claude"
      },
      "… 9 more"
    ],
    "truncated": true
  },
  "latestCheck": {
    "date": "2026-11-06",
    "checkedLines": 31
  },
  "notes": [
    "…"
  ],
  "source": {
    "name": "usedby",
    "url": "https://www.usedby.ai/companies/ramp",
    "markdown": "https://www.usedby.ai/companies/ramp.md",
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby (https://www.usedby.ai/companies/ramp)"
  },
  "pagination": {
    "offset": 0,
    "nextOffset": 10,
    "next": "https://www.usedby.ai/api/v1/companies/ramp/stack?limit=10&offset=10&snapshot=2026-11-06T21%3A08%3A18.024Z",
    "snapshot": "2026-11-06T21:08:18.024Z"
  }
}
```

## recent_changes

Companies newly listed as customers by a tool's vendor. Departures are not published. A quiet tool may not be tracked.

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/changes \
  --data-urlencode 'days=7' --data-urlencode 'limit=5'
```

Params: `days` 1-365 (default 30), `tool`, `limit` 1-100. A line can carry `source: null` and relationship `unknown`: cite its `claim.url`.

Response (trimmed):

```json
{
  "window": {
    "days": 7,
    "since": "2026-10-30"
  },
  "tool": null,
  "counts": {
    "companies": 157,
    "lines": 161
  },
  "listed": [
    {
      "claim": {
        "url": "https://www.usedby.ai/claims/cmw5alw71003jus4edavvy7nz",
        "…": "trimmed"
      },
      "company": "Engie",
      "companySlug": "engie",
      "tool": "C3 AI",
      "toolSlug": "c3-ai",
      "proof": "CONFIRMED",
      "listedBy": "2026-11-06",
      "listedAfter": null,
      "source": {
        "url": "https://c3.ai/customers/no-code-ai-for-demand-prediction",
        "kind": "page"
      }
    },
    "… 4 more"
  ],
  "truncated": true,
  "tracking": {
    "trackedTools": 464,
    "activeTools": 510
  },
  "notes": [
    "A change is a company newly listed as a customer by a tool's vendor. Departures are not published.",
    "Only tools with change tracking can show new customers; a quiet tool may simply not be tracked yet."
  ],
  "source": {
    "name": "usedby",
    "url": "https://www.usedby.ai/changes",
    "markdown": "https://www.usedby.ai/changes.md",
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby (https://www.usedby.ai/changes)"
  }
}
```

## compare

Two tools side by side: customer totals with proof mix, top industries, vendor facts (each with its source and `checkedAt`), and the companies listed on both.

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/compare \
  --data-urlencode 'a=cursor' --data-urlencode 'b=github-copilot' --data-urlencode 'limit=5'
```

Params: `a`, `b` (slug or name; check the resolved `tool.name`), `limit`, `offset`, `snapshot`, `includeEvidence`, `includeCapabilities`, `constraint`. Name only the companies in `overlap.shared`.

Response (trimmed):

```json
{
  "a": {
    "tool": {
      "name": "Cursor",
      "slug": "cursor",
      "category": {
        "name": "AI coding assistants & agents",
        "slug": "ai-code-assistants"
      },
      "url": "https://www.usedby.ai/tools/cursor"
    },
    "customers": {
      "total": 118,
      "named": 93,
      "mix": {
        "confirmed": 13,
        "declared": 0,
        "observed": 80,
        "signal": 25
      }
    },
    "topIndustries": [
      {
        "industry": "DevTools & Infrastructure",
        "companies": 14
      },
      {
        "industry": "Fintech & Payments",
        "companies": 13
      },
      {
        "industry": "Marketplace",
        "companies": 8
      }
    ],
    "facts": [
      {
        "key": "certifications",
        "value": "SOC 2 Type II, ISO 27001, ISO 42001",
        "source": "https://cursor.com/security",
        "checkedAt": "2026-10-21"
      },
      "…"
    ]
  },
  "b": {
    "tool": {
      "name": "GitHub Copilot",
      "slug": "github-copilot",
      "category": {
        "name": "AI coding assistants & agents",
        "slug": "ai-code-assistants"
      },
      "url": "https://www.usedby.ai/tools/github-copilot"
    },
    "customers": {
      "total": 52,
      "named": 36,
      "mix": {
        "confirmed": 10,
        "declared": 0,
        "observed": 26,
        "signal": 16
      }
    },
    "topIndustries": [
      {
        "industry": "Fintech & Payments",
        "companies": 6
      },
      {
        "industry": "Automotive Manufacturing",
        "companies": 4
      },
      {
        "industry": "Data & Analytics",
        "companies": 4
      }
    ],
    "facts": [
      {
        "key": "free_trial",
        "value": "Free trial or free plan available.",
        "source": "https://github.com/pricing",
        "checkedAt": "2026-10-20"
      },
      "…"
    ]
  },
  "overlap": {
    "both": 14,
    "aOnly": 104,
    "bOnly": 38,
    "withSignal": 7,
    "shared": [
      {
        "company": "Adobe",
        "slug": "adobe",
        "url": "https://www.usedby.ai/companies/adobe",
        "proof": {
          "a": "OBSERVED",
          "b": "OBSERVED"
        }
      },
      "…"
    ],
    "truncated": true
  },
  "latestCheck": {
    "date": "2026-11-06",
    "checkedLines": 161
  },
  "notes": [
    "…"
  ],
  "source": {
    "name": "usedby",
    "url": "https://www.usedby.ai/compare/cursor-vs-github-copilot",
    "markdown": "https://www.usedby.ai/compare/cursor-vs-github-copilot.md",
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby (https://www.usedby.ai/compare/cursor-vs-github-copilot)"
  },
  "pagination": {
    "offset": 0,
    "nextOffset": 5,
    "next": "https://www.usedby.ai/api/v1/compare?a=cursor&b=github-copilot&limit=5&offset=5&snapshot=2026-11-06T16%3A05%3A39.308Z",
    "snapshot": "2026-11-06T16:05:39.308Z"
  }
}
```

## shortlist_tools

Up to three pilot tools that meet exact requirements for one of three tasks: `support-resolution`, `sales-enrichment`, `workflow-automation`.

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/shortlist \
  --data-urlencode 'task=support-resolution' \
  --data-urlencode 'required=[{"criterion":"integrations","expected":"Salesforce"}]'
```

Params: `task` (one of the three IDs), `operation`, `required` and `preferred` (JSON arrays, up to six each; criteria `pricing`, `free_trial`, `integrations`, `languages`, `certifications`, `data_training`, `data_residency`, `dpa`, `api_access`, `export_availability`, `usage_limits`, `required_plan`; optional `plan`, `region`), `context` (JSON object). Cite the decision `source.url` with its full query string, and `evaluatedAt`.

Response (trimmed):

```json
{
  "version": 1,
  "status": "ok",
  "task": {
    "id": "support-resolution",
    "label": "Resolve customer support requests"
  },
  "eligible": [
    {
      "tool": {
        "slug": "decagon",
        "name": "Decagon"
      },
      "status": "eligible",
      "reasons": [],
      "required": [
        {
          "request": {
            "criterion": "integrations",
            "expected": "Salesforce"
          },
          "decision": {
            "status": "supported",
            "eligible": true,
            "reason": "matching_evidence",
            "evidence": [
              {
                "id": "cmutlhphq001hre1oa4hlgv20:item:0",
                "criterion": "integrations",
                "value": "Salesforce",
                "productId": "cmuppvzf36dced3de318c399c",
                "plan": null,
                "region": null,
                "language": null,
                "volumeBasis": null,
                "sourceUrl": "https://decagon.ai/product/integrations",
                "passage": "Sync support tickets and tap into customer data from applications like Salesforce, Intercom, Zendesk, and more.",
                "sourceType": "vendor_page",
                "checkedAt": "2026-10-21T09:18:39.837Z",
                "status": "supported",
                "conditions": []
              }
            ]
          }
        }
      ],
      "preferred": [],
      "preferredSupportedCount": 0,
      "documentedUsages": [
        {
          "id": "cmupu973n0064q41o6hq6qklx:cmuy1l24n003hpp1on3mapvhn:cmuy1l20n003gpp1oo96bdlqf:support-resolution",
          "vocabularyVersion": 1,
          "task": {
            "id": "support-resolution",
            "label": "Resolve customer support requests"
          },
          "usageId": "cmupu973n0064q41o6hq6qklx",
          "storyId": "cmuy1l24n003hpp1on3mapvhn",
          "storySlug": "faire-decagon",
          "company": {
            "name": "Faire",
            "slug": "faire"
          },
          "tool": {
            "name": "Decagon",
            "slug": "decagon"
          },
          "passage": "Decagon helped streamline common inquiries, freeing up support agents to handle more complex customer needs.",
          "source": {
            "evidenceId": "cmuy1l20n003gpp1oo96bdlqf",
            "url": "https://decagon.ai/case-studies/faire",
            "kind": "VENDOR_CASE_STUDY",
            "capturedAt": "2026-10-07T11:43:08.473Z",
            "language": "en"
          }
        }
      ],
      "peerExamples": []
    },
    "… Dialpad, Intercom"
  ],
  "eligibleOverflow": 4,
  "needsVerification": [
    "… 3 listed"
  ],
  "excluded": [],
  "coverage": {
    "considered": 25,
    "eligible": 7,
    "returnedEligible": 3,
    "eligibleOverflow": 4,
    "needsVerification": 18,
    "excluded": 0
  },
  "evaluatedAt": "2026-11-06T21:08:44.978Z",
  "notes": [
    "Scope: the accepted catalog tools with documented task examples. Absence of evidence does not establish non-use or missing product capability.",
    "…"
  ],
  "needsVerificationOverflow": 10,
  "excludedOverflow": 0,
  "source": {
    "name": "usedby",
    "url": "https://www.usedby.ai/shortlist?task=support-resolution&required=%5B%7B%22criterion%22%3A%22integrations%22%2C%22expected%22%3A%22Salesforce%22%7D%5D",
    "markdown": "https://www.usedby.ai/shortlist.md?task=support-resolution&required=%5B%7B%22criterion%22%3A%22integrations%22%2C%22expected%22%3A%22Salesforce%22%7D%5D",
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby (https://www.usedby.ai/shortlist?task=support-resolution&required=%5B%7B%22criterion%22%3A%22integrations%22%2C%22expected%22%3A%22Salesforce%22%7D%5D)"
  }
}
```

## what_peers_use

Tools used by companies like the user's: same industry and size (T1), same industry (T2), industry pillar (T3), market (T4). The first tier with enough companies answers.

```bash
curl -sS -G -A 'usedby-skill/0.1 (+https://github.com/ckrzento/usedby-skill; host=claude-code)' https://www.usedby.ai/api/v1/peers \
  --data-urlencode 'task=meeting notes' --data-urlencode 'industry=fintech' \
  --data-urlencode 'size=small' --data-urlencode 'limit=5'
```

Params (GET): `task` (free text, category slug or profession question), `profession`, `job`, `industry`, `size` (`small`, `mid`, `large`, a size band or a headcount), `country` (ISO 2, reported as a count, never a filter), `locale`, `limit` 1-8. `company` is accepted only in a POST JSON body; this skill does not send it. `status` is `answered` or `clarify`.

Response (trimmed):

```json
{
  "status": "answered",
  "category": {
    "slug": "meeting-assistants",
    "name": "Meeting assistants"
  },
  "industry": {
    "slug": "fintech-payments",
    "name": "Fintech & Payments"
  },
  "size": "small",
  "country": null,
  "answeringTier": "T2",
  "tiersTried": [
    {
      "tier": "T1",
      "label": "same industry and size",
      "companies": 3,
      "tools": 2,
      "smallSample": true,
      "skipped": true,
      "topTools": [
        "Grain",
        "Fireflies"
      ]
    },
    {
      "tier": "T2",
      "label": "same industry",
      "companies": 13,
      "tools": 7,
      "smallSample": false,
      "skipped": false,
      "topTools": [
        "tl;dv",
        "Fireflies",
        "Granola"
      ]
    },
    "…"
  ],
  "sample": {
    "companies": 13,
    "strongProofShare": 0.15384615384615385,
    "sizeKnownShare": 0.6923076923076923,
    "excludedVendorCompanies": 0
  },
  "tools": {
    "total": 7,
    "listed": [
      {
        "name": "Fireflies",
        "slug": "fireflies",
        "companies": 2,
        "mix": {
          "confirmed": 1,
          "declared": 0,
          "observed": 1,
          "signal": 0
        },
        "countryCount": null,
        "latestCheck": "2026-11-04T06:12:44.288Z",
        "alreadyUses": false,
        "strongProofPeers": {
          "total": 1,
          "listed": [
            {
              "usageId": "rev_usage_82ee812a1b1b7cde88e61a81",
              "company": "DepositFix",
              "slug": "depositfix",
              "proof": "CONFIRMED",
              "size": "STARTUP",
              "lastCheckedAt": "2026-10-18T21:57:06.972Z",
              "source": {
                "url": "https://www.usedby.ai/claims/rev_usage_82ee812a1b1b7cde88e61a81",
                "method": "CASE_STUDY",
                "original": "https://fireflies.ai/blog/how-depositfix-improved-their-business-decisions-using-fireflies"
              }
            }
          ],
          "overflow": 0
        },
        "why": [],
        "momentum": {
          "status": "available",
          "windowDays": 90,
          "companies": 0,
          "signalCompanies": 0,
          "basis": "public_proof_arrival"
        },
        "reasonStatus": "reason_not_published"
      },
      "… 4 more"
    ],
    "overflow": 2
  },
  "alreadyUses": [],
  "alreadyUsesTotal": 0,
  "alreadyUsesOverflow": 0,
  "summary": "3 companies on usedby with public proof use tl;dv (same industry). T2; 0 CONFIRMED, 0 DECLARED, 3 OBSERVED, 0 SIGNAL. https://www.usedby.ai/peers?category=meeting-assistants&limit=5&industry=fintech-payments&size=small&job=take-meeting-notes",
  "compareUrl": "https://www.usedby.ai/compare/fireflies-vs-tldv",
  "limitations": [
    "Public usage on usedby is not market share or a recommendation. Requirements are not evaluated.",
    "Unknown sizes are excluded from the largest-peer view. No line is hidden because of its age.",
    "These are public examples from this category. Use by this profession for this exact task has not been established for every tool. Your requirements still need verification."
  ],
  "source": {
    "url": "https://www.usedby.ai/peers?category=meeting-assistants&limit=5&industry=fintech-payments&size=small&job=take-meeting-notes",
    "terms": "https://www.usedby.ai/terms",
    "citation": "Source: usedby, https://www.usedby.ai/peers?category=meeting-assistants&limit=5&industry=fintech-payments&size=small&job=take-meeting-notes"
  }
}
```

## Answer examples

Built from the responses above. Each line states its proof level and its own source.

**Who uses Harvey?**

> usedby tracks 234 companies with a public line for Harvey: 20 CONFIRMED, 214 OBSERVED, 0 SIGNAL.
> - Bayer, CONFIRMED (case study): https://www.harvey.ai/customers/bayer
> - Bridgewater, CONFIRMED (case study): https://www.harvey.ai/customers/bridgewater
> Source: usedby, https://www.usedby.ai/tools/harvey. Counts are companies tracked on usedby, not market share.

**What does Notion use?** (mostly job posts)

> usedby has 5 public lines for Notion: 1 CONFIRMED, 4 SIGNAL.
> - Enterpret, CONFIRMED (case study): https://www.enterpret.com/customers/notion
> - Cursor, SIGNAL (a Notion job post mentions it; the weakest proof, not a case study): https://jobs.ashbyhq.com/notion/57b88701-42ce-4c7d-901d-bf65a6256ebc
> Source: usedby, https://www.usedby.ai/companies/notion-notion-so

**Support AI that integrates with Salesforce** (shortlist)

> usedby evaluated this on 2026-11-06T21:08:44Z: https://www.usedby.ai/shortlist?task=support-resolution&required=%5B%7B%22criterion%22%3A%22integrations%22%2C%22expected%22%3A%22Salesforce%22%7D%5D
> Eligible for the Salesforce integration requirement (vendor evidence, with the date usedby checked it):
> - Decagon: https://decagon.ai/product/integrations (checked 2026-10-21)
> - Dialpad: https://www.dialpad.com/features/contact-center-integrations/ (checked 2026-10-08)
> - Intercom: https://www.intercom.com/integrations (checked 2026-10-22)
> 4 more tools are eligible but not returned (`eligibleOverflow: 4`). 13 tools need verification (3 returned, `needsVerificationOverflow: 10`): unknown evidence, not a no.
> Your other requirements (budget, languages, security) were not sent, so they are not evaluated.

**Payroll software** (no line)

> usedby has no public line for payroll tools (search returned no match: https://www.usedby.ai/tools). Anything below is general knowledge, not usedby data.
