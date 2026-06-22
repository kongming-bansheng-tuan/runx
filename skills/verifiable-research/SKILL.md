---
name: verifiable-research
description: Produce a verifiable research brief from web sources, with every claim bound to a specific, citable source.
runx:
  category: research
---

# Verifiable Research

Produce a research brief where every factual claim is traceable to a specific
source and digest. Unlike `deep-research-brief` which synthesizes with less
rigor around citation, this skill enforces a strict evidence chain: claim →
source → digest, so a reviewer can independently verify every assertion without
re-running the research.

## What this skill does

1. Takes a research question and a set of sources
2. Extracts factual claims from each source
3. Links every claim to the exact source URL and content digest
4. Produces a structured brief with a citation index
5. Flags claims that lack sufficient evidence

The output is a "brief" in the same spirit as `deep-research-brief`, but with
an additional `evidence_map` that ties each claim to one or more `(url,
content_digest, excerpt)` tuples, making the brief auditable.

## When to use this skill

- A decision needs to be defended with traceable evidence, not just synthesis.
- A reviewer or auditor must be able to confirm claims without re-fetching.
- The research touches on contested facts where provenance matters.
- An external party (client, regulator, community) will read the output and
  needs to trust it.

## When not to use this skill

- For quick-turn synthesis where the audience trusts the researcher. Use
  `deep-research-brief`.
- For a single source. Use `web-fetch` directly.
- When no source can be named (internal knowledge, speculation). This skill
  will return `needs_more_evidence` for unsourced claims.

## Procedure

1. Receive `question`, `sources` (list of URLs), and optional `depth`.
2. Fetch each source via `web-fetch`, collecting `(url, status, content_digest,
   extracted_text)` tuples. Sources that fail to fetch are recorded in
   `sources_unavailable`.
3. Extract factual claims from the combined corpus. A claim is: a single,
   falsifiable statement of fact. Avoid opinions, predictions, or
   interpretations tagged as fact.
4. For each claim, record every source that supports it with a verbatim excerpt
   (up to 200 chars) and the line/paragraph anchor if available.
5. Score each claim by evidence weight:
   - `strong`: multiple independent sources with matching digests
   - `moderate`: single source, verified fetch
   - `weak`: single source, partial or truncated fetch
6. Build the brief body: executive answer, supporting evidence organized by
   theme, open questions, and recommended posture.
7. Produce the `evidence_map` as a separate structured block.

## Edge cases and stop conditions

- **No sources provided:** return `needs_agent` with a request for at least
  one URL.
- **All sources unavailable:** return `sources_unavailable` listing each URL
  and the fetch error.
- **Claim contradicts another claim:** flag both in `contradictions`, keep
  both, and downgrade evidence weight to `weak` for each.
- **Truncated source:** mark any claim drawn from a truncated fetch as
  `evidence_weight: weak` with `truncated: true` in the evidence tuple.
- **Hallucination guard:** any claim without a corresponding excerpt in the
  source text must be removed before output. If this would empty the brief,
  return `needs_more_evidence`.

## Output schema

```yaml
verifiable_brief:
  question: string
  generated_at: string
  answer: string              # executive summary, one paragraph
  posture: string             # recommended action: act, monitor, defer, investigate
  body: string                # full brief with inline citations [1], [2], etc.
  open_questions: list[string]
  contradictions: list[{claim_a, claim_b, resolution_note}]
  sources_used: number
  sources_unavailable: list[{url, error}]
  evidence_weight_summary:
    strong: number
    moderate: number
    weak: number
  evidence_map:
    - claim_id: number
      claim: string
      evidence_weight: strong | moderate | weak
      sources:
        - url: string
          content_digest: string
          excerpt: string       # verbatim, up to 200 chars
          truncated: boolean
```

## Inputs

- `question` (required): the research question to answer.
- `sources` (required): list of URLs to use as primary evidence. At least one
  must be provided.
- `depth` (optional): `quick` (3-5 sources, lightweight), `standard` (5-10,
  default), or `deep` (10+, heavy).
- `allowlist` (optional): host allowlist forwarded to `web-fetch` for each
  source.
- `max_source_bytes` (optional): per-source byte cap forwarded to `web-fetch`.

## Worked example

Input:
- `question`: "Is runx suitable for production payment processing?"
- `sources`: [runx docs, runx GitHub security policy, 2 third-party reviews]
- `depth`: standard

Output: a brief with 8 claims across 4 sources. 3 claims scored `strong`
(corroborated across docs and reviews), 4 `moderate` (single source), 1 `weak`
(a truncated third-party review). Evidence map links every claim to a specific
paragraph and digest. Posture: `monitor` — runx payment features are maturing
but the docs recommend a human-in-the-loop for production payments.
