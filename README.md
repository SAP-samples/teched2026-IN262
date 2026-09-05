

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
</br> </br> 

### 1.2 Personas
| Persona | Role |
|---|---|
| Requester / Business User | Initiates the request for a new product |
| **Product Master Data Steward** | Creates and enriches product master data in S4/HANA, ensures data quality |
| Data Owner / Product Owner | Validates business attributes and approves the request |
| **End User / Customer** | Receives notification about the new product and associated campaign offers |

> [!NOTE]
Only Product Master Data Steward and End User / Customer persona will be used in this hands-on session
</br>

### 1.3 End-to-End User Journey

1. **[Master Data Steward]** Creates a new product entry in S/4HANA using an AI agent client (MCP Inspector / AI client)
2. **[System]** Event raised automatically on product creation via Integration Suite AEM
3. **[Integration Suite]** Flows orchestrate the campaign — enriching product data, calculating a personalised discount based on customer purchase history, and triggering Emarsys
4. **[End User / Customer]** Receives new product information along with a personalised special discount offer
[TODO revisit based on email and myinbox task] A task will be triggered automatically by the System
5. **[End User / Customer]** Places an order for the product
6. **[System]** Acknowledges the order back to the customer and delivers a cross-selling campaign offer
[TODO revisit based on email and myinbox task] Receives a notifcation task in his / her inbox along with the cross selling campaign details

</br> </br>

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

## Exercises

Provide the exercise content here directly in README.md using [markdown](https://guides.github.com/features/mastering-markdown/) and linking to the specific exercise pages, below is an example.

- [Getting Started](exercises/ex0/)
- [Exercise 1 - First Exercise Description](exercises/ex1/)
    - [Exercise 1.1 - Exercise 1 Sub Exercise 1 Description](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - Exercise 1 Sub Exercise 2 Description](exercises/ex1#exercise-12-sub-exercise-2-description)
- [Exercise 2 - Second Exercise Description](exercises/ex2/)
    - [Exercise 2.1 - Exercise 2 Sub Exercise 1 Description](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Exercise 2 Sub Exercise 2 Description](exercises/ex2#exercise-22-sub-exercise-2-description)

  
**OR** Link to the Tutorial Navigator for example...

Start the exercises [here](https://developers.sap.com/tutorials/abap-environment-trial-onboarding.html).

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
