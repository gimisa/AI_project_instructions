# Gemini Web App: Hidden Architectural Limits & Optimization Guide

This repository contains `Gemini Token Counter - V1.4`, a Tampermonkey UserScript built to expose the invisible engineering boundaries, memory constraints, and behavioral shifts inside the consumer **Gemini Web App** (`gemini.google.com`). 

While Google AI Studio is well-documented for developers, the consumer Web App operates as a "black box." This guide details how the Web App manages its hardware resources behind the scenes and why tracking your tokens is mandatory to maintain accuracy.

---

## The Core Problem: Cost Caps vs. Hidden Performance Reductions

In Google AI Studio, developers pay per request, meaning they have access to full, uncompromised model processing as long as they pay for it. In the **Gemini Web App**, you pay a flat monthly rate ($0.00 for Free, or $19.99/mo for Advanced/Pro).  Google enforces **strict, unannounced computational ceilings** that directly degrade model intelligence over a long session.

| Behavioral Dimension | Gemini Web App (The Black Box) | AI Studio (The Developer Benchmark) | The Reality for Web App Users |
| :--- | :--- | :--- | :--- |
| **32K Token Limit** | ⚠️ **The RAG Downgrade.** On Free tiers, exceeding 32,768 tokens forces the app to abandon full-context reading. It switches to automated fragment retrieval, causing the model to miss layout rules. | **Context Caching.** Keeps the whole 32K block locked in active memory at a 90% discount. | **Why track it:** Your session silently loses linear logic execution the moment you cross the 32K marker. |
| **200K Token Limit** | 🛑 **The Hidden Sliding Window.** In Advanced/Pro tiers, approaching 200,000 tokens triggers an aggressive memory purge. The app silently slices out old chat history to save compute. | **The Pricing Cliff.** The API continues reading everything but instantly doubles the developer's token costs. | **Why track it:** On the web app, you don't pay more money at 200K—instead, **the model actively forgets your instructions** to save Google money. |
| **Thinking Levels** | 🧠 **Binary Option.** Limited to two unmanaged states (Standard Mode vs. Extended Deep Think). | **Granular Control.** Developers set explicit compute budgets (Minimal to High). | **Why track it:** Deep Think tokens generate massive memory strain, accelerating how fast your chat hits the 200K forgetfulness cliff. |

---

## Deep Dive: How Web App Resource Guardrails Sabotage Long Sessions

### 1. The 2-Level "Thinking" Trap & Context Saturation
In the Web App, turning on "Extended / Deep Think" changes the model from a fast text responder to an active reasoning engine. However, this internal reasoning process generates **hundreds of invisible "thinking tokens"** for every single prompt. 
* **The Web App Penalty:** Because you cannot see these tokens natively, a few deep-thinking prompts will rapidly expand your hidden session size. 
* **The Consequence:** You will hit the structural limits (32K or 200K) three to four times faster when using deep reflection mode compared to standard mode.

### 2. The 32K Caching & RAG Asymmetry
When you pass **32,768 tokens** in AI Studio, the developer locks that memory block securely. In the Web App, Google uses caching internally to keep their servers fast, but if your prompt history gets too chaotic, the web app stops tracking the context linearly. It drops into a search-and-retrieval loop (RAG). If your prompt references a technical rule or code syntax block pasted early in the session, the model will fail to retrieve it, resulting in hallucinations or generalized, lazy code outputs.

### 3. The 200K Self-Attention Blindspot
Even on the paid Web App Advanced tier (which advertises a large 1M+ context window), the interface cannot sustain peak reasoning across massive histories. Once the multi-turn session weight climbs near 200,000 tokens, the model's self-attention spreads too thin. It prioritizes the initial system behavior and your immediate last prompt, leaving a massive "blind spot" in the middle of your active session.

---

## 🛠️ How the V1.4 Multimodal Tracker Restores Control

Since the Gemini Web App hides these critical thresholds, `Gemini Token Counter - V1.4` injects an operational dashboard directly into the upper corner of your browser screen. 

```javascript
// Quick Configuration Toggles inside the UserScript:
const CONTEXT_LIMIT = 32000;   // Set to 32000 for Free Tier tracking (RAG boundary)
const CONTEXT_LIMIT = 1000000; // Set to 1000000 for Pro Tier tracking (200K degradation boundary)
