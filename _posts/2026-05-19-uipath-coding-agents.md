---
layout: post
title: Using UiPath Studio with AI coding agents
hidden: true
date: 2026-05-19 08:00:00 +0200
categories: [automation,uipath,orchestrator,ai,agentic]
tags: [ai,agentic]
mermaid: true
---

# 🌍 Project orchestrator - SaaS

*Documenting steps building my solution server.*

## Architecture Diagram A1 vs A2 (OpenAI vs Gemini)

This diagram explains how multiple agents can share the same runtime while using different model providers.


Agent A1 and Agent A2 


```mermaid
graph TD
    %% Laptop Layer %%
    subgraph Laptop ["💻 Your Windows Laptop"]
        direction TB
        Studio["🛠️ UiPath Studio (Visual Designer)"]
        Terminal["📟 Integrated Terminal Panel"]
        Workspace["📂 Local Workspace <br> (project.json, XAML files)"]
        CLI["⚙️ UiPath CLI (@uipath/cli)<br>+ UiPath Skills"]
        GitLocal["🌿 Git (Local Repo)"]

        Studio --> Terminal
        Terminal -->|Runs Agent| AI["🤖 Coding Agent<br>(Claude Code / Codex)"]
        AI -->|Reads Context| Workspace
        AI -->|Utilizes| CLI
        CLI -->|Safely Modifies Files| Workspace
        Workspace -->|Real-Time Refresh| Studio
        AI -->|Commits Changes| GitLocal
    end

    %% Enterprise Infrastructure %%
    subgraph Backend ["☁️ Cloud / On-Premise Backend"]
        direction LR
        Orchestrator["🏢 UiPath Orchestrator<br>(Tenants, Assets, Queues, Robots)"]
        GitRemote["🌐 Enterprise Git Repo<br>(GitHub / Azure DevOps)"]
    end

    %% Connections %%
    CLI --->|'uip login' Authenticated Session<br>Deploy & Provision Packages| Orchestrator
    GitLocal --->|Git Push / Pull| GitRemote

    %% Styling %%
    style Laptop fill:#f9f9f9,stroke:#333,stroke-width:2px;
    style Backend fill:#f5f7ff,stroke:#4a6fff,stroke-width:2px;
    style Studio fill:#ff6b6b,stroke:#c92a2a,stroke-width:2px,color:#fff;
    style Orchestrator fill:#22b8cf,stroke:#0b7285,stroke-width:2px,color:#fff;
    style AI fill:#fab005,stroke:#e67700,stroke-width:2px,color:#000;
    style Workspace fill:#fff,stroke:#333,stroke-dasharray: 5 5;
````


