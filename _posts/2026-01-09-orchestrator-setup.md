---
layout: post
title: Project Orchestrator - SaaS
hidden: true
date: 2026-01-08 08:00:00 +0200
categories: [linux,orchestrator,saas,ai,agentic]
tags: [linux,ai,agentic]
mermaid: true
---

# 🌍 Project orchestrator - SaaS

*Documenting steps building my solution server.*

## Architecture Diagram A1 vs A2 (OpenAI vs Gemini)

This diagram explains how multiple agents can share the same runtime while using different model providers.


Agent A1 and Agent A2 share the exact same runtime pipeline: observe, decide, execute, and log. The only difference between them is the decision provider used in the “Decide” phase.

Agent A1 uses OpenAI as its language model provider, while Agent A2 is intended to use Gemini. Everything else — filesystem access, execution logic, validation, and logging — remains identical.

This design makes agent behavior comparable across providers. Differences in output quality, latency, or cost can be attributed to the model itself rather than differences in infrastructure or execution logic.

By isolating the LLM behind a stable decision interface, the system avoids vendor lock-in and enables controlled experimentation.

```mermaid
flowchart LR
    subgraph Runtime["Agent Runtime"]
        OBS[Observe]
        DEC[Decide]
        EXEC[Execute]
        LOG[Log]
        OBS --> DEC --> EXEC --> LOG
    end

    subgraph A1["Agent A1"]
        OA[OpenAI Model]
    end

    subgraph A2["Agent A2"]
        GM[Gemini Model]
    end

    DEC --> OA
    DEC --> GM
````


