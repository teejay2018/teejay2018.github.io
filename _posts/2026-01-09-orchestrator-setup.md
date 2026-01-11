---
layout: post
title: Project Orchestrator - SaaS
hidden: true
date: 2026-01-08 08:00:00 +0200
categories: [linux,orchestrator,saas,ai,agentic]
tags: [linux,ai,agentic]
---

# 🌍 Project orchestrator - SaaS

*Documenting steps building my solution server.*

## Architecture Diagram

A1 vs A2 (OpenAI vs Gemini)

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


