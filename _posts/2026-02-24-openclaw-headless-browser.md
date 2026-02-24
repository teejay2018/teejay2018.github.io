---
layout: post
title: Project openclaw - setup agent with headless browser
date: 2026-02-24 
categories: [agentic,ai]
tags: [linux,openclaw,clawdbot]
mermaid: true
---

# 🤖 Mastering OpenClaw: Managed Mode & Remote VPS Monitoring

Welcome to this technical deep dive into **OpenClaw**! If you are setting up an autonomous web agent, choosing the right connection method and monitoring it on a headless server is crucial. This guide covers the differences between agent modes and how to "see" what your agent sees on a remote VPS.

---

## 🏎️ 1. Webcast vs. Chromium: Which Mode to Choose?

When configuring OpenClaw, you have two primary ways to let the agent interact with the web:

### **Method A: Chromium/Chrome (Managed Mode)** 🛡️
In this mode, OpenClaw spawns a completely separate browser instance.
*   **Isolation:** It uses its own profile, meaning your personal cookies, history, and passwords stay safe and separate.
*   **Stability:** Controlled via the [Chrome DevTools Protocol (CDP)](https://chromedevtools.github.io), making it faster and more reliable for unattended automation tasks.
*   **Pro Tip:** Using **Google Chrome** instead of the raw **Chromium** often yields better results, as Chrome supports more modern web standards and proprietary codecs, helping the agent avoid bot-detection.

### **Method B: Browser Relay (Extension Mode)** 🔌
This method lets the agent "hijack" your existing browser via a Chrome extension.
*   **Access:** It can use tabs you already have open and act on websites where you are already logged in.

---

## 📸 2. Visual Debugging: Automated Screenshots

When running "headless" on a VPS, screenshots are your best friend. OpenClaw can be configured to capture the browser's view at specific intervals or upon specific actions.

### **How to use it:**
*   **Automatic Captures:** By default, OpenClaw often saves screenshots of the current page state to the `workspace/screenshots/` or `workspace/memory/` folder.
*   **Instruction-based:** You can explicitly tell the agent to "take a screenshot" as part of its task. This is useful for verifying that a complex UI has loaded correctly.
*   **Reviewing:** These files are typically saved as `.png` or `.jpg`. If you are on a VPS, you can download them via SCP or view them directly through the [OpenClaw Dashboard](http://localhost:18789) session history.

This is the perfect middle ground between "blind" automation and a full live stream, as it provides a historical record of exactly what the agent encountered.

---

## 🕵️ 3. How to Monitor Your Agent

Since you’ve opted for **Chrome in Managed Mode**, you get superior tracking and logging:

1.  **The OpenClaw Dashboard:** Accessible via the [OpenClaw Interface](http://127.0.0.1).
2.  **CDP Logs:** Detailed logs of every click and network request can be found in your terminal or under `~/.openclaw/workspace/memory/`.

---

## 🌐 4. Remote Monitoring: Running "Live" on a Headless VPS

Even without a GUI, you can watch the agent work in real-time using an **SSH Tunnel**.

### **Step 1: Create an SSH Tunnel to the Dashboard**
On your local computer, run:
`ssh -L 18789:localhost:18789 user@your-vps-ip`
Then open `http://localhost:18789` locally.

### **Step 2: Advanced Remote Debugging**
1.  **Start Chrome on VPS** with: `--remote-debugging-port=9222`
2.  **Tunnel the port** from your local machine: `ssh -L 9222:localhost:9222 user@your-vps-ip`
3.  **Inspect locally:** Open Chrome and go to `chrome://inspect`. Add `localhost:9222` to "Discover network targets" to see the [live DevTools stream](https://developer.chrome.com). 📺

---

## 💡 Summary
For the best balance of security and performance, use **Chrome in Managed Mode**. Leverage **automatic screenshots** for history and **SSH Port Forwarding** for live troubleshooting.

**Happy Scraping!** 🕸️

---

_Last updated: February 2026_  
_Source: Cantaloop Aps._