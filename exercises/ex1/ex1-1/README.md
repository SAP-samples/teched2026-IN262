# Exercise 1.1 — Generate the iFlow with AI

**AI Feature:** iFlow Generation  
**Duration:** 15 minutes

---

## Business Context

You need to build an integration flow that receives a product launch event from S/4HANA, enriches it with customer and sales order data, calculates a personalised discount, and triggers a campaign workflow in SAP Build Process Automation. Instead of building this from scratch step by step, you describe the scenario in natural language and let AI generate the structure for you — then you wire it to the shared artefacts and deploy it.

---

## Prerequisites

- Access to the Integration Suite tenant:  
  `https://url.sap/78ju6v`
- The `IN262-Common-Artifacts` package is deployed in the tenant (contains the Script Collection and Message Mapping used in this exercise)
- Your participant number `[NNN]` — printed on your session card (001–040)

---

## Naming Convention

Replace `0XX` with your 3-digit participant number throughout this exercise.

| What | Format | Example for participant 050 |
|---|---|---|
| Package name | `IN262-0XX-Campaign-Management` | `IN262-050-Campaign-Management` |
| iFlow name | `IN262-0XX-Calculate-Discount` | `IN262-050-Calculate-Discount` |
| ProcessDirect address | `/IN262-0XX` | `/IN262-050` |

---

## Step 1 — Create Your Integration Package

1. In the Integration Suite home, navigate to **Design → Integrations and APIs**

   ![Design — Integrations and APIs showing existing packages](images/ex01-1.png)

2. Click **Create** (top right)

3. Fill in the package header:

   | Field | Value |
   |---|---|
   | **Name** | `IN262-0XX-Campaign-Management` |
   | **Technical Name** | `IN2620XXCampaignManagement` *(auto-filled)* |
   | **Short Description** | `The package to trigger a campaign for a newly launched product automatically using AI features like generation of iFlows, profiling, script optimisation, error resolution, MCP servers etc.` |
   | **Version** | `1.0.0` |
   | **Vendor** | `SAP` |

   ![Package header form filled in](images/ex01-2.png)

4. Click **Save** — you will see the confirmation toast *"The package has been successfully created"*

   ![Package created confirmation](images/ex01-3.png)

---

## Step 2 — Add an Integration Flow Using AI Generation

1. Click the **Artifacts** tab
2. Click **Add → Integration Flow**

   ![Artifacts tab — Add dropdown with Integration Flow highlighted](images/ex01-4.png)

3. In the dialog, select **"Generate Integrations with assistance from AI"**

   ![Add Integration Flow dialog — AI generation option selected](images/ex01-5.png)

4. The **Generate Integration Flow** dialog opens with a prompt text area

   ![Generate Integration Flow dialog — empty prompt area](images/ex01-6.png)

---

## Step 3 — Paste the AI Prompt

Copy the full prompt below and paste it into the **"Describe your integration scenario"** field:

```
I need to build an integration flow called IN262-0XX-Calculate-Discount where 0XX is my participant ID (for example IN262-050-Calculate-Discount for participant 050). It should use a ProcessDirect sender adapter with address /IN262-0XX where 0XX is my participant ID.

The flow works like this: it receives a JSON payload, converts it to XML using a JSON to XML Converter with root element name "root" and no namespaces are required. Before that conversion, add a Groovy Script step called "Log Payload" that references "Log Payload.groovy" from the script collection "IN262-Script-Collection" in package "IN262-Common-Artifacts" which contains all commonly use artifacts across this exercise.

After the converter, add a Content Modifier called "Store data" that sets these exchange properties: defaultDiscount as constant value 5, ThresholdeValue as constant value 5000, discount as an empty constant, email as XPath //participantEmail with datatype java.lang.String, productDescription as XPath //productDescription with datatype java.lang.String, ProductID as XPath //productId with datatype java.lang.String, price as XPath //price with datatype java.lang.Integer, currency as XPath //currency with datatype java.lang.String, businessPartner as XPath //businessPartner with datatype java.lang.String, participantId as XPath //participantId with datatype java.lang.String, and testMode as XPath //testMode with datatype java.lang.String.

Next, add a Message Mapping step called "Mapping for RFC Call" that uses the mapping "IN262-BP-to-BUPA-Mapping" from the same Common Artifacts package.

Then add a Request Reply step called "RFC call" with an RFC receiver adapter using destination S4HANA_RFC, BAPI BAPI_BUPA_ADDRESS_GETDETAIL, and Synchronous RFC protocol.

After the RFC call, add a Content Modifier called "Store BP ID" with one exchange property: BP using XPath //*[local-name()='PARTNER'][starts-with(text(), 'USCU-PT')] as java.lang.String.

Then add a Request Reply called "S4HANA Call" using an OData V2 receiver adapter with address http://s4hana-he4-teched:50080/sap/opu/odata/sap/API_SALES_ORDER_SRV, SAP Cloud Connector proxy with location ID IN262, Basic authentication with credential alias HE4, Query GET operation on resource A_SalesOrder, CSRF disabled, and query options: $select=PurchaseOrderByCustomer,to_Item/SalesOrder,to_Item/SalesOrderItem,to_Item/Material,to_Item/RequestedQuantity,to_Item/RequestedQuantityUnit,to_Item/TransactionCurrency,TransactionCurrency,TotalNetAmount,to_Item/SalesOrderItemText,to_Item/MaterialByCustomer,SoldToParty&$expand=to_Item&$filter=SoldToParty eq '${property.BP}'

After the OData call, invoke a script called "CalculatedDiscount" from the common artifacts package (mentioned above) and script collection.

Then add a Groovy Script step called "Log Summary" referencing "Log Summary" script, followed by another Groovy Script called "Prepare SBPA Payload" referencing "SBPA_Payload" — both scripts are from the common artifacts package (mentioned above) and script collection.

Finally, add a Request Reply step called "Send Offer" using an HTTP receiver adapter with POST method, URL https://spa-api-gateway-bpi-eu-prod.cfapps.eu10.hana.ondemand.com/workflow/rest/v1/workflow-instances, and OAuth2 Client Credentials authentication with credential name BPA-OAuth-Credential. End the flow with a Message End Event.
```

> **Remember to replace `0XX` with your participant number before clicking Generate.**

![Prompt pasted in — AI response area shows analysis of systems and actions identified](images/ex01-7.png)

---

## Step 4 — Review AI Analysis and Generate

1. The AI analyses your prompt and shows the identified systems and actions — review it

   ![AI response showing identified systems and proposed iFlow structure](images/ex01-8.png)

2. Verify the **Name** field at the bottom shows `IN262-0XX-Calculate-Discount` with your participant number

3. Click **Generate**

   ![Generate button highlighted — name field shows IN262-0XX-Calculate-Discount](images/ex01-9.png)

4. AI generates the iFlow — the dialog closes and the new artefact appears in the package

   ![Generation in progress — spinner visible in AI response area](images/ex01-9-1.png)

---

## Step 5 — Open the Generated iFlow

1. Click the iFlow name `IN262-0XX-Calculate-Discount` to open it
2. Click **Edit** (top right) to enter edit mode

   ![Generated iFlow in view mode — Edit button highlighted, initial flow structure visible](images/ex01-10.png)

> The AI has generated the flow structure. You will now see some steps with error indicators — these are local placeholder artefacts (scripts and a mapping) that were created inside this iFlow. In the next steps you will replace them with the shared artefacts from the Common Artifacts package.

---

## Step 6 — Remove Local Artefacts (Fix 1)

The AI generated local copies of the scripts and mapping. These must be deleted and replaced with the global shared versions.

1. In the bottom panel, click the **References** tab, then **Local (5)**

   ![References tab — Local tab showing 1 mapping and 4 scripts](images/ex01-11.png)

2. Delete **`IN262-BP-to-BUPA-Mapping`** (Message Mapping) — click the delete icon → confirm **Delete**

   ![Delete Resource dialog for IN262-BP-to-BUPA-Mapping](images/ex01-11-1.png)

3. Delete **`CalculatedDiscount`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for CalculatedDiscount.groovy](images/ex01-11-2.png)

4. Delete **`Log Payload`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for Log Payload.groovy](images/ex01-11-3.png)

5. Delete **`Log Summary`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for Log Summary.groovy](images/ex01-11-4.png)

6. Delete **`SBPA_Payload`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for SBPA_Payload.groovy — Local(1) remaining](images/ex01-11-5.png)

> Local tab now shows **Local (0)**.

---

## Step 7 — Add Global Message Mapping Reference (Fix 2)

1. Click the **Global (0)** tab
2. Click **Add References → Message Mapping**

   ![Global tab empty — Add References dropdown showing Message Mapping option](images/ex01-12.png)

3. In the References dialog, click the Package dropdown — it defaults to your own package

   ![References dialog — package dropdown showing own package selected](images/ex01-12-1.png)

4. Clear the current package and select **`IN262-Common-Artifacts`**

   ![Package dropdown open — IN262-Common-Artifacts highlighted](images/ex01-12-3.png)

   ![IN262-Common-Artifacts selected in dropdown](images/ex01-12-4.png)

5. The artifact list shows **`IN262-BP-to-BUPA-Mapping`** — tick the checkbox

   ![References dialog — IN262-BP-to-BUPA-Mapping listed, checkbox ticked, OK highlighted](images/ex01-12-5.png)

6. Click **OK** — the mapping appears under Global (1)

   ![Global (1) tab — IN262-BP-to-BUPA-Mapping from IN262-Common-Artifacts confirmed](images/ex01-12-6.png)

---

## Step 8 — Add Global Script Collection Reference (Fix 3)

1. Click **Add References → Script Collection**

   ![Add References dropdown — Script Collection highlighted](images/ex01-13.png)

2. In the References dialog, clear the package and select **`IN262-Common-Artifacts`**

   ![References dialog for Script Collection — own package shown initially](images/ex01-13-1.png)

   ![Package dropdown — IN262-Common-Artifacts selected](images/ex01-13-2.png)

3. The artifact list shows **`IN262-Script-Collection`** — tick the checkbox

   ![IN262-Script-Collection listed — checkbox ticked, OK highlighted](images/ex01-13-3.png)

4. Click **OK** — Global tab now shows **Global (2)**

   ![Global (2) — both IN262-BP-to-BUPA-Mapping and IN262-Script-Collection listed](images/ex01-13-4.png)

5. Click **Save** — confirm the *"saved"* toast

   ![Save button highlighted — Global (2) confirmed, saved toast visible](images/ex01-13-5.png)

   ![Saved confirmation toast](images/ex01-13-6.png)

---

## Step 9 — Wire Scripts to the Script Collection (Fix 4)

The script steps still point to the deleted local files. Wire each one to the correct script in the Script Collection.

> For each step below: click the step on the canvas → **Processing** tab → click **Select** → switch to **Global Resources** tab → select the script → click **OK** → **Save**.

### 9a — Log Payload

1. Click the **Log Payload** step on the canvas
2. Processing tab → click **Select**

   ![Log Payload step selected — Processing tab, Select button highlighted](images/ex01-14.png)

3. Switch to **Global Resources** tab → select **`Log Payload`** from the list → click **OK**

   ![Select Resource dialog — Global Resources tab, Log Payload highlighted](images/ex01-14-1.png)

4. Click **Save**

   ![Script File shows /script/Log Payload.groovy — saved toast](images/ex01-14-2.png)

### 9b — Message Mapping step

1. Click the **IN262-BP-to-BUPA-Mapping** step on the canvas
2. Processing tab → click **Select**

   ![Message Mapping step selected — Select button highlighted](images/ex01-15.png)

3. Switch to **Global Resources** tab → select **`IN262262BPtoBUPAMapping`** → click **OK**

   ![Select Mapping Resource — Global Resources tab, IN262262BPtoBUPAMapping selected](images/ex01-15-1.png)

4. Click **Save**

   ![Resource shows IN262BPtoBUPAMapping — saved toast](images/ex01-15-2.png)

### 9c — CalculatedDiscount

1. Click the **CalculatedDiscount: Compute…** step on the canvas
2. Processing tab → click **Select**

   ![CalculatedDiscount step selected — Select button highlighted](images/ex01-16.png)

3. Global Resources → select **`CalculatedDiscount`** → click **OK**

   ![Select Resource — CalculatedDiscount highlighted in script list](images/ex01-16-1.png)

4. Click **Save**

   ![Script File shows /script/CalculatedDiscount — saved](images/ex01-16-2.png)

### 9d — Log Summary

1. Click the **Log Summary** step → Processing tab → **Select**

   ![Log Summary step selected](images/ex01-17.png)

2. Global Resources → select **`Log Summary`** → click **OK**

   ![Select Resource — Log Summary highlighted](images/ex01-17-1.png)

3. Click **Save**

   ![Saved toast confirmed](images/ex01-17-2.png)

### 9e — Prepare SBPA Payload

1. Click the **Prepare SBPA Payload** step → Processing tab → **Select**

   ![Prepare SBPA Payload step selected](images/ex01-18.png)

2. Global Resources → select **`SBPA_Payload`** → click **OK**

   ![Select Resource — SBPA_Payload highlighted](images/ex01-18-1.png)

3. Click **Save**

   ![Saved toast confirmed](images/ex01-18-2.png)

---

## Step 10 — Verify the Full iFlow and Adapter Settings

1. Click the **Arrange Horizontally** button (bottom of canvas) to see the complete flow

   ![Full iFlow overview at 47% zoom — all steps visible including local integration process](images/ex01-19.png)

   ![Full iFlow — canvas showing complete flow](images/ex01-19-1.png)

2. Click the **ProcessDirect** sender step → **Connection** tab — confirm the address shows `/IN262-0XX` with your participant number

3. Click the **RFC call** step → **Connection** tab — confirm Destination = `S4HANA_RFC`

4. Click the **S4HANA Call** OData step → **Processing** tab — confirm Resource Path = `A_SalesOrder` and Query Options are populated

5. Click the **Send Offer** HTTP step → **Connection** tab — confirm:
   - Address: `https://spa-api-gateway-bpi-eu-prod.cfapps.eu10.hana.ondemand.com/workflow/rest/v1/workflow-instances`
   - Method: `POST`
   - Authentication: `OAuth2 Client Credentials`
   - Credential Name: `BPA-OAuth-Credential`

---

## Step 11 — Save as Version

1. Click **Save as Version**

2. In the Version Information dialog:
   - **Version:** `1.0.1`
   - **Comment:** `Adjusted the common artifacts`
   - Click **Save**

3. Toast confirms *"saved as version"*

---

## Exercise 1.1 Complete

Your iFlow is saved and ready for Memory Profiling. It was generated from a plain-language description and wired to shared artefacts — without writing a single line of code or configuring adapters manually.

---

*Next: [Exercise 1.2 — Memory Profiling and Script Optimisation](../ex1-2/README.md)*
