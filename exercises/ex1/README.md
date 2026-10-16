# Exercise 1: Generate an Integration Flow with AI and Optimise It

**AI Features:** iFlow Generation · Memory Profiling · Script Optimisation  
**Duration:** 30 minutes

---

## Overview

This exercise covers three AI capabilities across two sub-exercises:

| Sub-exercise | What you do | AI Capability |
|---|---|---|
| [Exercise 1.1 — Generate the iFlow with AI](ex1-1/README.md) | Generate the flow from a natural language prompt, wire shared artefacts, save as version | iFlow Generation |
| [Exercise 1.2 — Memory Profiling and Script Optimisation](ex1-2/README.md) | Profile memory usage in Simulator, optimise the flagged script, deploy | Memory Profiling · Script Optimisation |

---

## Business Context

You need to build an integration flow that receives a product launch event from S/4HANA, enriches it with customer and sales order data, calculates a personalised discount, and triggers a campaign workflow in SAP Build Process Automation. Instead of building this from scratch step by step, you describe the scenario in natural language and let AI generate the structure for you — then you wire it to the shared artefacts, profile its memory usage, and optimise it before deploying.

---

## What AI Does Here

| Capability | When it fires | What you see |
|---|---|---|
| **iFlow Generation** | You paste a prompt describing the scenario | AI generates the complete flow structure: steps, adapters, local processes, and a pre-filled name |
| **Memory Profiling** | You run the Simulator with Memory Profiling enabled | AI identifies which script step consumes the most memory and explains why |
| **Script Optimisation** | You click "Optimise with AI" on the flagged script | AI proposes targeted code fixes across memory, precision, and error handling |

---

*Start with: [Exercise 1.1 — Generate the iFlow with AI](ex1-1/README.md)*
