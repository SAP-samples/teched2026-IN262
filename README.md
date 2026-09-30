

# IN262 - Build an integration scenario from scratch with AI assistance at every step

## Description

This repository contains the material for the SAP TechEd 2026 Hands-on workshop session [IN262 - Build an integration scenario from scratch with AI assistance at every step](https://github.com/SAP-samples/teched2026-IN262).
It captures the business scenario, session structure, exercise outlines and content standards. 

## Overview
## 1. Business Scenario - Campaign Management
###  1.1 Overview
**Company:** Best Run Business

**Problem:** Preparing product launch campaigns, festival season offers, and personalised purchase-history-based offers manually is time-consuming and error-prone.

**Goal:** Use AI-assisted integration to automatically curate and deliver campaigns to customers based on their buying history and local festival/product launch schedules.
</br>
### 1.2 Personas
| Persona | Role |
|---|---|
| Requester / Business User | Initiates the request for a new product |
| **Product Master Data Steward** | Creates and enriches product master data in S4/HANA, ensures data quality |
| Data Owner / Product Owner | Validates business attributes and approves the request |
| **End User / Customer** | Receives notification about the new product and associated campaign offers |

> [!NOTE]
Only **Product Master Data Steward** and **End User / Customer** persona will be used in this hands-on session </br>

### 1.3 Technical Overview

his session takes participants through the complete lifecycle of building an AI-assisted integration — from generating the first integration flow to running the scenario end-to-end. At each step, a different AI capability embedded in SAP Integration Suite does the heavy lifting: generating, optimising, profiling, and resolving errors. Participants will also step beyond integration and into the agentic world — creating an MCP Server to expose a backend API, and calling it directly from an AI agent to trigger the full campaign flow.

<img src="/exercises/ex0/images/1_AI_Settings_Page.png" alt="Pic_IN262_1" >

Participants will:

| # | What You Will Do | AI Capability Used |
|---|---|---|
| 1 | Generate an integration flow from a natural language prompt, add scripts and mapping, and configure it | AI-assisted iFlow Generation in Integration Suite |
| 2 | Profile the flow to identify bottlenecks, optimize the script with AI, then deploy | AI Script Optimization + Profiler |
| 3 | Run a test and use AI to diagnose and resolve errors in the flow | AI Error Resolution |
| 4 | Create an MCP Server to expose the S/4HANA product creation API as an agent-callable tool |  MCP Server , API Specification Generation (OPTIONAL) |
| 5 | Trigger the full scenario via an AI agent and complete the campaign end-to-end | Agent Interaction via MCP |


By the end of the session, participants will have hands-on experience with **6 distinct AI capabilities** and a working end-to-end integration connecting S/4HANA, SAP Integration Suite, and Emarsys — all driven by a single natural language prompt.

### 1.4 End-to-End User Journey
1. **[Master Data Steward]** Creates a new product entry in S/4HANA using an AI agent client (MCP Inspector / AI client)
2. **[System]** Event raised automatically on product creation via Integration Suite AEM
3. **[Integration Suite]** Flows orchestrate the campaign — enriching product data, calculating a personalised discount based on customer purchase history, and triggering Emarsys
4. **[End User / Customer]** Receives new product information along with a personalised special discount offer
[TODO revisit based on email and myinbox task] A task will be triggered automatically by the System
5. **[End User / Customer]** Places an order for the product
6. **[System]** Acknowledges the order back to the customer and delivers a cross-selling campaign offer
[TODO revisit based on email and myinbox task] Receives a notifcation task in his / her inbox along with the cross selling campaign details
</br> 

## 2. System Landscape

| System | Purpose |
|---|---|
| SAP Integration Suite | Connects all systems; hosts the integration flows |
| Integration Suite AEM | [TODO] Raises event on product creation in S/4HANA |
| S/4HANA (On-Premise) | [TODO] Source system for product creation; triggers the event |
| Emarsys | [TODO]Campaign management — receives enriched data and executes the campaign |
| Inbox | [TODO]Human approval step in the workflow |
| AI Client | [TODO] MCP Inspector (current); IT-provided AI client (planned) |
> [!Note]
>  All systems will be pre-configured and seeded with data by the session organiser. Participants need not perform any system setup.

</br> </br>

## 3. Exercises

| # | Block | Type | Duration |
|---|-------|------|----------|
| 0 | Welcome, Business Scenario & Technical Overview <br> <sub> Observe the full list of AI features in SAP Integration Suite, not all are included in this hands-on </sub>| Instructor-led | 10 min 
| 1 | **Exercise 1:** [TODO] [Generate and configure an Integration Flow with AI](exercises/ex1#exercise-11-sub-exercise-1-description) <br> <sub> Generate iflow, add script(s), add mapping and configure </sub>| Hands-on | 30 min |
| 2 | **Exercise 2:** [TODO] [[Profile, Optimise the Script with AI and deploy](exercises/ex1#exercise-11-sub-exercise-2-description)  <br> <sub> Memory profile the flow before deployment </sub> | Hands-on | 20 min |
| 3 | **Exercise 3:** [TODO] [Test Run the Scenario to Resolve the Errors if any](exercises/ex1#exercise-11-sub-exercise-3-description) <br> <sub> Run test integration flow and resolve errors</sub> | Hands-on | 10 min |
| 4 | **Exercise 4:** [TODO] [Create MCP Server & Expose the API](exercises/ex1#exercise-11-sub-exercise-4-description) <br> <sub> Generate API specification and build MCP for S4/HANA Product Creation API </sub>| Hands-on | 20 min |
| 5 | **Exercise 5:** [TODO] [Configure MCP server in AI Client](exercises/ex1#exercise-11-sub-exercise-5-description) <br> <sub> Use details of MCP server created before this step</sub> | Hands-on | 20 min |
| 6 | **Exercise 6:** [TODO] [Run the Scenario & Complete the Campaign](exercises/ex1#exercise-11-sub-exercise-5-description) <br> <sub> Create product with given details in natural language in S4/HANA, get offer and create order  </sub> | Hands-on | 20 min |
| 7 | Wrap-up, Q&A & Key Takeaways | Instructor-led | 10 min |

**Total: ~120 min** — with ~5 min buffer across exercises for questions.

  


**IMPORTANT**

Your repo must contain the .reuse and LICENSES folder and the License section below. DO NOT REMOVE the section or folders/files. Also, remove all unused template assets(images, folders, etc) from the exercises folder. 

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (20xx-)20xx SAP SE or an SAP affiliate company and <your-project> contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP/<your-project>).
