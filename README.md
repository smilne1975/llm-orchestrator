# LLM Orchestrator

A standalone, app-neutral system for recommending a model and reasoning level for each task in a workflow. **Status: design only.** No collector, published catalog, or app integration is available yet.

## User controls

1. **Auto** proposes the best-fit model and supported reasoning level for each task. It ranks task-relevant quality first, uses cost and latency to distinguish similarly qualified options, and prefers measured local results when available. Budget and permissions are constraints, not evidence of quality.
2. **Global slider** rebalances the whole draft from economy to quality. It can change the model or reasoning level on unpinned tasks, but never relaxes required verification, permissions, or a separately enforceable spending limit.
3. **Per-task overrides** let the user change the model and reasoning level and pin that choice. Pins survive Auto, slider moves, and catalog refreshes. If a pinned choice becomes unavailable, flag it and require a new choice rather than substituting silently.
4. **Review and confirm** shows what changed, why each route was picked, alternatives, evidence and its date, estimated cost and time, and any uncertainty. Nothing is applied or run merely by drafting or refreshing.

## Evidence pipeline

Check all nine sources daily. Give each source a recorded status (updated, unchanged, archival, or unavailable); do not force every source into the score.

| Source | Intended use |
|---|---|
| [Artificial Analysis](https://artificialanalysis.ai/models) | Capability, price, latency and speed by measured configuration |
| [Rank / LLM Leaderboard](https://llmleaderboard.com/sources) | Discover and cross-check dated results against their original publisher |
| [BenchLM](https://benchlm.ai/data) | Discover candidates and assess coverage; licensed data only for production use |
| [Scale Labs](https://labs.scale.com/leaderboard) | Independent checks for specific tasks where comparable |
| [Hugging Face](https://huggingface.co/docs/leaderboards) | Open-weight model cards, licenses and evaluation discovery; flag archives |
| [Stanford HELM](https://crfm.stanford.edu/helm) | Reproducible, task-specific evidence such as long-context retrieval |
| [WhatLLM](https://whatllm.org/) | Cross-check configurations and pricing; do not recount upstream results |
| [Evidently](https://www.evidentlyai.com/llm-evaluation-benchmarks-datasets) | Discover evaluation datasets, not current model rankings |
| [LiveBench](https://livebench.ai/#/?sort=Coding&dir=desc) | Dated task suites including coding, research, reasoning and tool use |

Compare exact model, reasoning level, task suite version and test setup. Deduplicate results that share the same underlying run. Retain dates, citations, price basis and confidence; mark missing evidence as unknown. Use official feeds where permitted and verify price with the endpoint actually in use. Estimate total workflow cost, including verification and retries, not just output-token price. Keep subscription quota or request-credit estimates separate from API dollar estimates. A public benchmark score is not a user's task-success probability. Recommend meaningful changes for review; do not publish raw datasets without permission.

## App integration

The planned public release publishes a **versioned data-only catalog** of recommendations, capabilities, model identities, supported effort evidence, prices and source links. Apps fetch and validate it, retain the last known good release, and never execute instructions or code from the feed. Catalog updates do not alter an active workflow; approved plans retain their catalog version.

Each app maps its own steps to generic capabilities and intersects the catalog with models and reasoning levels actually usable through the user's connected endpoints, account permissions, and organization policy. A Claude-only connection shows only the Claude choices that connection exposes; a chat subscription does not imply API or tool access. If the endpoint cannot verify a choice, flag it rather than silently switching providers. Keep account entitlements, workflow data, and app-specific evaluation results local to the app. Each app owns its checks, approvals and execution.

## First milestone

Build the catalog format and read-only recommendation preview first. Test that Auto picks the best eligible configuration, slider changes affect only unpinned steps, model and effort overrides persist, unavailable models are excluded, and refreshing the catalog cannot change an approved workflow. Add one app integration only after those checks pass.
