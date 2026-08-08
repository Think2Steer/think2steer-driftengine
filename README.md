# DriftEngine

DriftEngine is a Think2Steer product for evaluating runtime behavior drift in AI systems.

It helps teams compare candidate AI behavior against approved baselines and decide whether a change is safe to release.

## What It Does

- Manages approved AI behavior baselines
- Runs candidate behavior evaluations against test scenarios
- Measures semantic and qualitative drift
- Produces risk classifications and release-safety evidence
- Supports model, prompt, document, and workflow comparison scenarios
- Generates run artifacts that can support review and governance workflows

## Product Problem

AI behavior can change silently when teams update:

- models
- prompts
- retrieval content
- system instructions
- policies
- tools
- workflow logic

Traditional tests often miss semantic regressions. DriftEngine is designed to make behavioral change visible before production release.

## Platform Role

Within AI Impact Platform, DriftEngine is the runtime evaluation layer.

It receives evaluation requests, compares baseline and candidate behavior, and emits structured drift results that can feed release decisions in Delivery Engine and workflow visibility in AI Impact Portal.

## Weighted Scoring Architecture

DriftEngine uses a weighted release-safety model so a decision is explainable instead of being a black-box pass/fail check.

```text
Candidate behavior
       |
       v
Semantic drift score vs baseline
       |
       v
Judge regression signal
       |
       v
Weighted composite score
       |
       v
Risk band -> ImpactGate -> SAFE / BLOCK
```

The scoring flow:

1. Compare candidate behavior against an approved baseline.
2. Calculate a semantic drift score.
3. Normalize that drift score against the workflow's configured threshold.
4. Add qualitative regression evidence from an evaluator/judge signal.
5. Combine the signals with configurable weights.
6. Map the weighted score into risk bands.
7. Return an explicit release gate result: `SAFE` or `BLOCK`.

Default scoring shape:

| Signal | Purpose | Example weight |
| --- | --- | --- |
| Drift score | Measures how far candidate behavior moved from the approved baseline | `0.60` |
| Judge regression flag | Captures qualitative evidence that the behavior regressed | `0.40` |

Risk band mapping:

| Weighted score | Risk level | Release posture |
| --- | --- | --- |
| `0.00 - 0.40` | `LOW` | Usually `SAFE` |
| `0.41 - 0.70` | `MEDIUM` | Usually `BLOCK` or review-required |
| `0.71 - 1.00` | `HIGH` | Usually `BLOCK` |

This lets different workflows tune their own thresholds and weights. A low-risk summarization assistant may tolerate more semantic movement, while a regulated document-reasoning workflow can use stricter bands or hard-block behavior when a judge flags regression.

## Example Use Cases

- Validate model upgrades before production
- Compare prompt versions against approved baselines
- Evaluate document reasoning changes across policy updates
- Detect regression risk in AI-assisted workflows
- Produce review evidence for governance and approval workflows

## My Role

Founder and product engineer. I designed the drift evaluation workflow, baseline/candidate comparison model, release-gate concept, and integration with the broader AI Impact Platform.

## Status

Active private product development.

## Source Code

The source code is private because this is an active commercial product. This public repository is a product overview for recruiters, collaborators, and interviewers.
