# Exercise 4 — End-to-End: Create a Product and Trigger the Campaign

**Feature:** Run end to end scenario

**Duration:** 15 minutes

---

## Business Context

You have built and deployed the integration flows in Exercises 1–3. In this exercise you will experience the full end-to-end business scenario as two roles:

1. **Data Steward** — use an AI agent with your MCP server to create a new product in S/4HANA. The agent handles the API call; you describe the product in plain language.
2. **End User / Customer** — receive the personalised campaign offer triggered by the product creation, approve it, and place the order.

---

## What AI Does Here

| Capability | What happens |
|---|---|
| **MCP Server** | The AI agent discovers your deployed MCP server as a tool and calls the S/4HANA product creation API on your behalf — no API client or manual payload construction required |
| **Natural language used to call APIs** | You describe the product in plain language; the agent maps it to the correct OData fields and handles authentication |

---

## Exercise Structure

| Step | What you do |
|---|---|
| **Step 1 — Get Credentials** | Get your MCP server credentials from Developer Hub — required for all options |
| **Step 2 — Create a Product** | Create a product in S/4HANA — choose one of three options:<br>• **2.1** Joule Work Desktop (JWD)<br>• **2.2** MCP Inspector (browser only)<br>• **2.3** Claude Code CLI |
| **Step 3 — Complete the Process** | Approve the campaign offer in My Inbox and confirm the sales order |

---

## Step 1 — Get Your MCP Server Credentials from Developer Hub

Your MCP server credentials were created in Exercise 3. **All options in Step 2 require these credentials** — complete this step first.

1. Navigate to **Developer Hub → My Workspace → Agents** tab and open your agent subscription **IN262-0XX-Agent-Subscription**

   ![Developer Hub — Agent Subscription page, credentials hidden with eye icon](images/ex04-1-0.png)

2. Click the **eye icon** next to the credentials to reveal the **Token URL**, **Key**, and **Secret**

   ![Developer Hub — Agent Subscription page, credentials hidden with eye icon](images/ex04-1.png)

> [!IMPORTANT]
> **Keep this browser tab open** — you will copy the Token URL, Key, and Secret into whichever tool you use in Step 2.

![Developer Hub — Token URL, Key, and Secret revealed (blurred)](images/ex04-1-1.png)

---

## Step 2 — Create a Product in S/4HANA

Choose one of the three options below depending on what is available on your workstation. All three produce the same result.

| Option | Tool | Best for |
|---|---|---|
| **2.1 — JWD** | Joule Work Desktop | SAP-provided client — recommended if available |
| **2.2 — MCP Inspector** | Browser-based MCP Inspector | No installation required — browser only |
| **2.3 — Claude Code CLI** | Claude Code CLI + Bruno + VS Code | Full CLI experience — pre-installed on TechEd machines |

---

### Option 2.1 — Joule Work Desktop (JWD)

Use this option if Joule Work Desktop is available on your workstation.

> [!NOTE]
> JWD is an SAP-provided AI client. You will connect it to your MCP server as a Connector, then use natural language to create the product — same as Claude Code CLI but within JWD.

---

#### 2.1.1 — Add Your MCP Server as a Connector

1. Open **JWD** → click the **Extensions** icon in the left sidebar

   ![JWD — Extensions panel open, Extensions icon highlighted](images/ex04-jwd-1.png)

2. Click **+ New** → select **Add Connector**

   ![JWD — + New dropdown, Add Connector option highlighted](images/ex04-jwd-2.png)

3. In the **Add Connector** dialog, fill in the following fields and <mark>click Add </mark>:

   | Field | Value |
   |---|---|
   | **Name** | `Product-IN262-0XX` <br> <mark>Replace `0XX` with your participant number </mark>|
   | **URL** | Copy the MCP Server URL from **Step 1** above <br> <mark>Replace `0XX` with your participant number </mark>|

> [!IMPORTANT]
> Replace `0XX` in the **Name** field with your 3-digit participant number (e.g. `049` → `Product-IN262-049`)
> For the **URL** field — copy the MCP Server URL from Step 1 and replace `0XX` with your participant number

   ![JWD — Add Connector dialog, Name and URL filled in, Add button highlighted](images/ex04-jwd-3.png)

4. Enter the **Client ID** and **Client Secret** from **Step 1** above → click **Add**

   ![JWD — Add Connector dialog, Client ID and Client Secret filled in, Add button highlighted](images/ex04-jwd-4.png)

5. The connector **Product-IN262-0XX** appears in the list with status **Connected** and **12 tools** listed

   ![JWD — Connectors tab, Product-IN262-0XX Connected, 12 tools visible](images/ex04-jwd-5.png)

> [!NOTE]
> If the connector shows **Re-Authentication Needed**, click **Re-Authenticate** and repeat the steps above.

---

#### 2.1.2 — Create a Product via JWD

1. Click the **Conversations** icon in the left sidebar → click the **New Conversation** button (pencil icon)

   ![JWD — Conversations screen, New Conversation button highlighted](images/ex04-jwd-6.png)

2. Prepare the prompt — copy and paste it into the message field, then replace the placeholders:
   - Replace `0XX` with your 3-digit participant number
   - Replace `NN` with `10` for your first attempt, `11` for your second, and so on

> [!IMPORTANT]
> `NN` starts from 10 for this workshop

> [!NOTE]
> **Example for participant 049, first attempt:**
> Product ID: `MZ-TG-ZAD01-IN262-04910` — Description: `Office Laptop IN262-04910`

```
Create a new product in S/4HANA with the following details:
- Product ID: MZ-TG-ZAD01-IN262-0XXNN
- Description: Office Laptop IN262-0XXNN
- Product Type: HAWA
- Industry Sector: M
- Base Unit: EA
- Language: EN
- Valuation Area (Plant): 1710
- Moving Average Price: 1000
- Currency: USD
- InventoryValuationProcedure: V
- ValuationClass: 3100
- PriceDeterminationControl: 2
- ProductSalesOrg: 1710
- ProductDistributionChnl: 10
- Item Category Group (MTPOS): NORM
```

   ![JWD — New conversation, prompt pasted with participant number and attempt annotation](images/ex04-jwd-7.png)

3. JWD processes the request and shows an **Activate Connectors** dialog — confirm your connector `Product-IN262-0XX` is listed → click **Allow**

   ![JWD — Activate Connectors dialog, Product-IN262-049 listed with 12 tools, Allow button highlighted](images/ex04-jwd-8.png)

4. JWD shows **Your Approval Is Needed** — review the details → click **Approve** → click **Submit**

   ![JWD — Your Approval Is Needed dialog, Approve selected, Submit button highlighted](images/ex04-jwd-9.png)

5. JWD confirms the product was created — a **Product Master Creation Summary** is shown with all fields

   ![JWD — Product Master Creation Summary showing product MZ-TG-ZAD01-IN262-04915 created successfully](images/ex04-jwd-10.png)

> [!NOTE]
> The product creation triggers an AEM event in the background. The integration flow picks this up, calculates your personalised discount, and creates a campaign task in SAP Build Process Automation. This may take a few seconds.

**Once the product is created, continue with [Step 3 — Complete the Business Process](#step-3--complete-the-business-process).**

---

### Option 2.2 — MCP Inspector (Browser Only)

Use this option if Bruno and Claude Code CLI are not installed on your workstation.

> [!NOTE]
> This option uses the hosted MCP Inspector — a browser-based tool that connects directly to your MCP server and lets you call tools manually. No installation required.

---

#### 2.2.1 — Generate a Bearer Token

You need a short-lived OAuth token to authenticate against the MCP server. Use the hosted **Token Helper app** — no terminal or curl required.

1. Open the Token Helper in your browser:

   | | URL |
   |---|---|
   | Token Helper | [https://url.sap/otqvxd](https://url.sap/otqvxd) |

2. In **Step 1 — Generate OAuth Token**, enter the credentials you retrieved in **Step 1** above (Token URL, Key, Secret)

   ![Token Helper — credentials filled in, Generate Token button highlighted](images/ex04-5-1.png)

3. Click **Generate Token** — the app calls the token endpoint and displays only the `access_token` string

4. Click **Copy Token** — keep this browser tab open, you will need the token in section 2.2.3

   ![Token Helper — token generated, Copy Token button highlighted](images/ex04-5-2.png)

> [!CAUTION]
> The Token Helper extracts only the `access_token` string automatically — the full token is ready to copy with the **Copy Token** button.
> Tokens are valid for ~12 hours. If you get a `401` error later, return here and generate a fresh token.

---

#### 2.2.2 — Open MCP Inspector

1. Open your browser and navigate to the hosted MCP Inspector:

   | | URL |
   |---|---|
   | MCP Inspector | [https://url.sap/22uolm](https://url.sap/22uolm) |

2. Log in with your BTP credentials when prompted (XSUAA login page)

---

#### 2.2.3 — Add Your MCP Server

1. On the **Servers** page, click **Add Servers** → select **+ Add manually**

   ![MCP Inspector — Servers page, Add Servers dropdown, + Add manually highlighted](images/ex04-5-3.png)

2. In the **Add server** dialog, fill in the following fields:

   | Field | Value |
   |---|---|
   | **Server ID** | `Product-IN262-0XX` |
   | **Transport** | `streamable-http` |
   | **URL** | Copy the MCP Server URL from **Step 1** above |

> [!IMPORTANT]
> Replace `0XX` in both the **Server ID** and **URL** fields with your 3-digit participant number (e.g. `007` → `Product-IN262-007`)

   ![MCP Inspector — Add Server dialog with Server ID, Transport and URL filled in](images/ex04-5-4.png)

3. Click **Add** — your server appears in the list. Click **Settings**

   ![MCP Inspector — Product-IN262-0XX server added, Settings button highlighted](images/ex04-5-5.png)

4. In the **Server Settings** dialog, click **Custom Headers** to expand the section

   ![MCP Inspector — Server Settings dialog, Custom Headers section collapsed, arrow pointing to it](images/ex04-5-6.png)

5. The Custom Headers section expands — click **+ Add Header**

   ![MCP Inspector — Custom Headers expanded, + Add Header button highlighted](images/ex04-5-6-1.png)

   ![MCP Inspector — Custom Headers expanded, + Add Header button highlighted](images/ex04-5-6-2.png)

6. A header row appears — fill in the two fields:

   | Field | Value |
   |---|---|
   | **Name** | `Authorization` |
   | **Value** | `Bearer ` + paste your token from section 2.2.1 |

> [!IMPORTANT]
> The value must start with `Bearer` followed by a space, then the full token string — e.g. `Bearer eyJqa3U...`

   ![MCP Inspector — Authorization header filled in with Bearer token, annotation pointing to value field](images/ex04-5-6-3.png)

7. Click **X** (top-right of the dialog) to close Server Settings

   ![MCP Inspector — Server Settings dialog, X close button highlighted](images/ex04-5-6-4.png)

8. Back on the Servers page — find **Product-IN262-0XX** and toggle the switch to **Connect**

   ![MCP Inspector — Servers page, Product-IN262-0XX row, toggle switch highlighted (Disconnected)](images/ex04-5-6-5.png)

9. The server status changes to **Connected** — click the **Tools** tab to confirm 12 tools are listed

   ![MCP Inspector — Product-IN262-0XX Connected, Tools tab highlighted, messages panel showing successful calls](images/ex04-5-6-6.png)

> [!NOTE]
> If you see a `401` error, your token has expired. Return to section 2.2.1 to generate a fresh token.

---

#### 2.2.4 — Create a Product

1. In the **Tools** tab, find and select **Creates product master record** (`post_A_Product`)

   ![MCP Inspector — Tools (12) listed, Creates product master record highlighted](images/ex04-5-7.png)

2. The tool opens in labeled fields mode. Click the **Edit as JSON** toggle to switch to JSON mode

> [!TIP]
> If the **Execute Tool** button is not enabled, enter a space in any field first — then switch to JSON mode.

   ![MCP Inspector — TIP annotation showing space trick to enable Execute Tool button](images/ex04-5-8.png)

3. In JSON mode, **delete the placeholder JSON** that appears and leave the field empty

   ![MCP Inspector — JSON mode with placeholder JSON, annotation: Delete the JSON](images/ex04-5-9.png)

4. Go back to the **Token Helper app** → **Step 2 — Build MCP Inspector Payload**:
   - Enter your 3-digit participant number (e.g. `050`)
   - Enter your attempt number (`01` for first attempt, `02` if you need to retry)
   - Click **Copy Payload**

   ![Token Helper — Step 2, participant number 050 and attempt 01 filled, full payload visible](images/ex04-5-10.png)

   ![Token Helper — Copy Payload button highlighted with annotation](images/ex04-5-10-1.png)

> [!NOTE]
> **Example for participant 050, first attempt:**
> Product: `MZ-TG-ZAD01-IN262-05001` — Description: `Office Laptop IN262-05001`

5. Paste the copied payload into the MCP Inspector JSON input field

> [!IMPORTANT]
> The payload contains your Product ID in **four places** — the Token Helper fills all of them automatically. Do not edit the JSON manually.

   ![MCP Inspector — payload pasted in JSON field, Execute Tool button highlighted](images/ex04-5-10-2.png)

6. Click **Execute Tool** — MCP Inspector sends the request to your MCP server

7. The **Results** panel shows the response — a successful creation shows your new product details

   ![MCP Inspector — Results panel showing successful product creation response](images/ex04-5-11.png)

> [!NOTE]
> The product creation triggers an AEM event in the background. The integration flow picks this up, calculates your personalised discount, and creates a campaign task in SAP Build Process Automation. This may take a few seconds.

**Once the product is created, continue with [Step 3 — Complete the Business Process](#step-3--complete-the-business-process).**

---

### Option 2.3 — Claude Code CLI

Use this option if Bruno, VS Code, and Claude Code CLI are pre-installed on your workstation (standard TechEd setup).

> [!NOTE]
> The following items are **pre-installed on your workstations** by the IT team: Claude Code CLI (`claude`), Bruno, Visual Studio Code. Your participant number is printed on your session card (`001` through `040`).

---

#### 2.3.1 — Set Up Bruno and .mcp.json

1. Right click open the file in new tab or window [Bruno Collection download link](downloads/IN262-Bruno-Collection.zip)
   - click **Download raw file** to save the ZIP

   ![GitHub — IN262-Bruno-Collection.zip file page, Download raw file button highlighted](images/ex04-0-1.png)

2. Open **Bruno** → click the `+` icon next to **Collections** in the left panel → select **Import collection**

   ![Bruno — Collections panel, + icon clicked, Import collection option highlighted](images/ex04-0-2.png)

3. The Import Collection dialog opens — click **choose file(s)**

   ![Bruno — Import Collection dialog, choose file(s) link highlighted](images/ex04-0-3.png)

4. Navigate to your **Downloads** folder → select **IN262-Bruno-Collection.zip** → click **Open**

   ![File picker — Downloads folder, IN262-Bruno-Collection.zip selected, Open button highlighted](images/ex04-0-4.png)

5. Confirm the collection name `IN262-Bruno-Collection` → click **Import**

   ![Bruno — Import dialog, name IN262-Bruno-Collection, Import button highlighted](images/ex04-0-5.png)

6. The collection **IN262-Bruno-Collection** now appears in Bruno — expand and verify the POST request entry is available

   ![Bruno — IN262-Bruno-Collection expanded, Generate Token request highlighted](images/ex04-0-6.png)

7. Right click and open the [.mcp.json download link](downloads/.mcp.json)
   - on the GitHub page click **Download raw file** to save the file to your **Downloads** folder
   - <mark>verify file name is `.mcp.json` **(file name with leading dot)**</mark>
      - if required rename the file to `.mcp.json` with leading dot

> [!CAUTION]
> If the browser saves the file as **`mcp.json`** **(without the leading dot)**, you must rename it to **`.mcp.json`** after downloading:
> - Open your **Downloads** folder → right-click `mcp.json` → **Rename** → change to `.mcp.json` → press **Enter**
> - System may warn that changing the extension could make the file unusable — click **Yes**

   ![GitHub — .mcp.json file page, Download raw file button highlighted](images/ex04-0-8.png)

8. Open **Windows Explorer** — navigate to your Downloads folder and copy the renamed file to `C:\Users\<participant-user>`

> [!TIP]
> If the file already exists you can overwrite it.

   ![Windows Explorer — home directory, .mcp.json file highlighted](images/ex04-0-9.png)

9. Right-click **.mcp.json** → select **Open with → Visual Studio Code**

   ![Windows Explorer — right-click context menu, Open with Visual Studio Code highlighted](images/ex04-0-10.png)

10. Keep VS Code open — you will fill in the values in section 2.3.3

    ![VS Code window with default values](images/ex04-0-11.png)

---

#### 2.3.2 — Get Your MCP Access Token via Bruno

> [!NOTE]
> Your Token URL, Key, and Secret are already open in the **Developer Hub browser tab** from Step 1.

1. Open **Bruno** → expand **IN262-Bruno-Collection** → click **IN262-0XX - Generate Token for Product Creation MCP Server**

   ![Bruno — Generate Token request open](images/ex04-2-0.png)

2. Copy each value from the Developer Hub tab directly into Bruno:
   - **Token URL** → paste into the POST URL field at the top
   - **Key** → paste into the `client_id` value field
   - **Secret** → paste into the `client_secret` value field

   ![Bruno — POST request with Token URL, client_secret and client_id fields annotated](images/ex04-0-7.png)

3. Click **Send** — the response returns a JSON body. Copy the `access_token` value **between the quotes**

   ![Bruno — response body with access_token highlighted, annotation "Copy the Token between the quotes"](images/ex04-0-7-1.png)

---

#### 2.3.3 — Configure .mcp.json in VS Code

1. Switch to **VS Code** — the `.mcp.json` file is already open from section 2.3.1

   Update the following:
   - Replace `[GENERATE NEW TOKEN and PASTE HERE]` with the `access_token` you copied in section 2.3.2
   - Replace `replace this with your MCP URL` with your MCP server URL (from **Step 1** above or from [Exercise 3.2 Step 12](https://github.com/SAP-samples/teched2026-IN262/tree/main/exercises/ex3#exercise-32-create-and-configure-mcp-server))
   - Save the file (`Ctrl+S`)

> [!TIP]
> Do not change the JSON file format.
> There is a space between `Bearer` and the token — e.g. `Bearer eyJqa3U...`

   ![VS Code — .mcp.json open with MCP server URL and Bearer token filled in](images/ex04-2-4.png)

---

#### 2.3.4 — Start Claude Code CLI

1. Press the **Windows key**, type `command`, and click **Command Prompt**

> [!IMPORTANT]
> Open Command Prompt in the **same folder** where you saved the **.mcp.json** file (`C:\Users\<participant-user>`).

   ![Windows Search — "command" typed, Command Prompt result highlighted](images/ex04-2-5.png)

2. Type `claude` and press **Enter**

   ![Command Prompt — "claude" typed at C:\Users\TE-25>](images/ex04-2-6.png)

3. Claude Code prompts: **"Do you trust the files in this folder?"** — click **Yes, I trust this folder**

   ![Claude Code CLI — trust folder prompt, "Yes, I trust this folder" highlighted](images/ex04-2-7.png)

---

#### 2.3.5 — Verify Your MCP Server is Connected

1. Type `/mcp` and press **Enter** — Claude Code lists all connected MCP servers

   ![Claude Code CLI — /mcp command output, IN262-ProductCreation listed as connected with 12 tools](images/ex04-2-8.png)

2. Confirm **IN262-ProductCreation** shows status **connected** and lists **12 tools**

> [!NOTE]
> If the server shows **disconnected**:
> - verify your `.mcp.json` has the correct URL
> - regenerate a valid token, paste it in `.mcp.json` again
> - restart Claude Code: type `exit` → press Enter → type `claude` to start a new session

> [!TIP]
> Press the **Escape** key to go back to the prompt in Claude.

---

#### 2.3.6 — Create a Product via the AI Agent

You will now act as the **Data Steward** — using natural language to instruct the AI agent to create a product in S/4HANA.

1. Prepare the prompt — copy and paste it into a text editor, then replace the placeholders:
   - Replace `0XX` with your 3-digit participant number
   - Replace `NN` with `01` for your first attempt, `02` for your second, and so on

> [!NOTE]
> **Example for participant 007, first attempt:**
> Product ID: `MZ-TG-ZAD01-IN262-00701` — Description: `Office Laptop IN262-00701`

```
Create a new product in S/4HANA with the following details:
- Product ID: MZ-TG-ZAD01-IN262-0XXNN
- Description: Office Laptop IN262-0XXNN
- Product Type: HAWA
- Industry Sector: M
- Base Unit: EA
- Language: EN
- Valuation Area (Plant): 1710
- Moving Average Price: 1000
- Currency: USD
- InventoryValuationProcedure: V
- ValuationClass: 3100
- PriceDeterminationControl: 2
- ProductSalesOrg: 1710
- ProductDistributionChnl: 10
- Item Category Group (MTPOS): NORM
```

   ![Claude Code CLI — product creation prompt entered](images/ex04-3.png)

2. Claude Code processes the request

   ![Claude Code CLI — "Scanning..." processing indicator](images/ex04-3-1.png)

3. Claude Code asks for confirmation to call the MCP tool — click **Yes** (or **Yes, and don't ask again for this server**)

   ![Claude Code CLI — tool use confirmation dialog with Yes / Yes don't ask again / No options](images/ex04-3-2.png)

4. Claude Code calls the **IN262-ProductCreation** tool on your MCP server

   ![Claude Code CLI — calling IN262-ProductCreation tool, product details shown](images/ex04-3-3.png)

5. Claude Code confirms the product was created successfully — a summary table with all product fields is shown

> [!NOTE]
> The product creation triggers an AEM event in the background. The integration flow picks this up, calculates your personalised discount, and creates a campaign task in SAP Build Process Automation. This may take some seconds.

   ![Claude Code CLI — product successfully created, summary table with Product ID, Description, Type, Unit, Group](images/ex04-3-4.png)

6. **[Monitor]** Go back to Integration Suite to monitor integration flows

   ![Message monitor to check product creation](images/ex04-3-5.png)

**Once the product is created, continue with [Step 3 — Complete the Business Process](#step-3--complete-the-business-process).**

---

## Step 3 — Complete the Business Process

> [!NOTE]
> **All options continue here** — whether you used Option 2.1 (JWD), 2.2 (MCP Inspector), or 2.3 (Claude Code CLI), Steps 3.1 and 3.2 are the same.

---

### 3.1 — Verify the MCP Call in Integration Suite Monitor

1. Open **Integration Suite** → **Monitor** → **Integrations and APIs**

2. In the **Monitor Message Processing** screen, set **Time** to `Past Hour` and click **Go**

3. Confirm you see two completed messages for your participant number:

   | Artifact Name | Artifact Type | Status |
   |---|---|---|
   | `Product_API_IN262_0XX_mcpServer` | MCP Server | Completed |
   | `Product_API_IN262_0XX` | API | Completed |

   ![Integration Suite — Monitor Message Processing, two completed messages: MCP Server and API artifact types highlighted](images/ex04-4-0.png)

> [!NOTE]
> If either message shows **Failed**, let the instructor know. The MCP Server artifact processes the tool call; the API artifact handles the OData call to S/4HANA.

---

### 3.2 — Approve the Campaign Offer in My Inbox

You now switch roles — acting as the **End User / Customer** who receives the campaign offer.

1. Open your browser and navigate to SBPA My Inbox: [https://url.sap/lf45f4](https://url.sap/lf45f4)

> [!TIP]
> Sign in with **teched identity provider** or ask the instructor.

2. Log in with your participant credentials (`in262-0XX@education.cloud.sap`)
   - Instructors will provide you the credentials

3. Refresh the task list — locate the task **Place Order - Product Offer** for your product
   - Review the offer details, confirm the quantity, and click **Place Order**

   ![SBPA My Inbox — "Place Order - Product Offer" task open, quantity field and Place Order button visible](images/ex04-4.png)

4. A **"Task processed successfully"** toast confirms the order was submitted

   ![SBPA — "Task processed successfully" confirmation toast](images/ex04-4-1.png)

5. **[Monitor]** Go back to Integration Suite to monitor integration flows

   ![Message monitor to check order creation](images/ex04-4-3.png)

---

### 3.3 — Confirm the Sales Order

1. Click the **inbox icon** again — you should now see a **"A Sales Order Created"** notification

   ![SBPA My Inbox — "A Sales Order Created" notification with Sales Order number and Acknowledge button](images/ex04-4-2.png)

2. Note the **Sales Order number** assigned by S/4HANA — click **Acknowledge** to complete the flow

---

## Exercise 4 Complete

You have completed the full end-to-end journey:

1. Retrieved MCP server credentials from Developer Hub
2. Used an AI agent to create a product in S/4HANA — no API client, no manual payload
3. The integration platform picked up the event, calculated a personalised discount, and raised a campaign task
4. You placed the order and received the Sales Order confirmation

---

## Key Takeaway

This is AI-assisted integration in action: an AI agent bridged natural language to a governed enterprise API via MCP, while the integration platform handled the orchestration, enrichment, and business process automation in the background — without any manual intervention.

---

*[Back to overview](../../README.md)*
