# Exercise 1.1 — Generate the iFlow with AI

**AI Feature:** iFlow Generation  
**Duration:** 20 minutes

---

## Business Context

You need to build an integration flow that receives a product launch event from S/4HANA, enriches it with customer and sales order data, calculates a personalised discount, and triggers a campaign workflow in SAP Build Process Automation. Instead of building this from scratch step by step, you describe the scenario in natural language and let AI generate the structure for you — then you wire it to the shared artefacts and deploy it.

---

## Prerequisites

- Access to the Integration Suite tenant: [Integration Suite Tenant (Primary)](https://url.sap/ei6p8k)
- The `IN262-Common-Artifacts` package is deployed in the tenant (contains the Script Collection and Message Mapping used in this exercise)
- Your participant number `[NNN]` — printed on your session card (001–040)

---

## Naming Convention

Replace `0XX` with your 3-digit participant number throughout this exercise. <mark> **(be careful when to use hyphen '-' and underscore '_')** </mark>

| What | Format | Example for participant 050 |
|---|---|---|
| Package name | `IN262-0XX-Campaign-Management` | `IN262-050-Campaign-Management` |
| iFlow name | `IN262-0XX-Calculate-Discount` | `IN262-050-Calculate-Discount` |
| ProcessDirect address | `/IN262-0XX` | `/IN262-050` |

---

## Step 1 — Create Your Integration Package

1. In the Integration Suite home, navigate to **Design → Integrations and APIs**
2. Click **Create** (top right)

   ![Design — Integrations and APIs showing existing packages with Create button highlighted](images/ex01-1.png)



3. Fill in the package header:

   | Field | Value |
   |---|---|
   | **Name** | `IN262-0XX-Campaign-Management` |
   | **Technical Name** | `IN2620XXCampaignManagement` *(auto-filled)* |
   | **Short Description** | `The package to trigger a campaign for a newly launched product automatically using AI features like generation of iFlows, profiling, script optimisation, error resolution, MCP servers etc.` |
   | **Version** | `1.0.0` |
   | **Vendor** | `SAP` |

   ![Package header form filled in — Save button highlighted](images/ex01-1-1.png)

4. Click **Save** — you will see the confirmation toast *"The package has been successfully created"*

   ![Package created confirmation toast](images/ex01-1-2.png)

---

## Step 2 — Add an Integration Flow Using AI Generation

1. Click the **Artifacts** tab
2. Click **Add → Integration Flow**

   ![Artifacts tab — Add dropdown with Integration Flow highlighted](images/ex01-1-3.png)

3. In the dialog, select **"Generate Integrations with assistance from AI"**

   ![Add Integration Flow dialog — AI generation option selected](images/ex01-1-4.png)

4. The **Generate Integration Flow** dialog opens with a prompt text area
> [!TIP]
> Adjust the dialog size to get more space to work with — drag the resize handle in the bottom-right corner

   ![Generate Integration Flow dialog — empty prompt area with resize handle highlighted](images/ex01-1-5.png)

---

## Step 3 — Paste the AI Prompt <mark> (changes required at 2 places) </mark>

Copy the full prompt below, paste it into the **"Describe your integration scenario"** field and click on send (arrow):
> [!IMPORTANT]
> Remember to <mark>replace `0XX` at 2 places</mark> with your participant number before clicking Generate — the iFlow name and the ProcessDirect address.
> 
> **Example:** for participant 050:
> 
> iFlow name = <mark> `IN262-050-Calculate-Discount`</mark>
>
> ProcessDirect address =  <mark> `/IN262-050` </mark>

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

![Prompt pasted in — AI response area shows analysis of systems and actions identified](images/ex01-1-6.png)

---

## Step 4 — Review AI Analysis and Generate

1. The AI analyses your prompt and shows the identified systems, actions and the proposed iFlow structure — review it, then verify the **Name** field at the bottom shows `IN262-0XX-Calculate-Discount` with your participant number

2. Click **Generate**

> [!IMPORTANT]
> The name of the iFlow should have your participant number instead of `0XX`

   ![AI response showing identified systems and proposed iFlow structure — Generate button highlighted](images/ex01-1-7.png)



3. The iFlow is created — a confirmation toast appears

   ![iFlow created toast — IN262-0XX-Calculate-Discount created](images/ex01-1-8.png)

---

## Step 5 — Open the Generated iFlow

1. iFlow  `IN262-0XX-Calculate-Discount` automatically opens <mark> (where `0XX` is your 3-digit participant number) </mark>
> [!NOTE]
> The AI has generated the flow structure. You will now see some steps with error indicators — these are local placeholder artefacts (scripts and a mapping) that were created inside this iFlow. In the next steps you will replace some of them with the shared artefacts from the Common Artifacts package.

   ![Generated iFlow in view mode — Edit button highlighted](images/ex01-1-9.png)

2. Click **Edit** (top right) to enter edit mode, click on the editor canvas and pull up the property sheet
   ![iFlow in view mode — Edit button and Expand icon highlighted](images/ex01-2.png)

---

## Step 6 — Remove Local Artefacts <mark> EXCEPT CalculatedDiscount SCRIPT </mark> (Fix 1) 

The AI generated local copies of the scripts and mapping. Delete all of them <u> **except `CalculatedDiscount`**</u> — that script stays local to your iFlow and will be used in Exercise 1.2.

> [!IMPORTANT]
> Do NOT delete the `CalculatedDiscount` script

1. In the bottom panel, click the **References** tab, then **Local (5)** (visible when you click on canvas)

   ![References tab — Local(5) showing 1 mapping and 4 scripts, delete icons highlighted](images/ex01-2-1.png)

2. Delete **`IN262-BP-to-BUPA-Mapping`** (Message Mapping) — click the delete icon → confirm **Delete**

   ![Delete Resource dialog for IN262-BP-to-BUPA-Mapping](images/ex01-2-2.png)

3. Delete **`Log Payload`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for Log Payload.groovy — Local(4) showing](images/ex01-2-3.png)

4. Delete **`Log Summary`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for Log Summary.groovy — Local(3) showing](images/ex01-2-4.png)

5. Delete **`SBPA_Payload`** Groovy Script → confirm **Delete**

   ![Delete Resource dialog for SBPA_Payload.groovy — Local(2) showing](images/ex01-2-5.png)

6. Re-confirm that `CalculatedDiscount` is not deleted
> [!NOTE]
> Local tab now shows **Local (1)** — only `CalculatedDiscount` remains. Click **Global (0)** to proceed.

   ![Local(1) with only CalculatedDiscount remaining — Global(0) tab highlighted](images/ex01-3.png)

---

## Step 7 — Add Global Message Mapping Reference (Fix 2)

1. Click the **Global (0)** tab
2. Click **Add References → Message Mapping**

   ![Global(0) tab — Add References dropdown showing Message Mapping highlighted](images/ex01-3-1.png)

3. In the References dialog, click the **×** next to the current package to clear it

   ![References dialog — own package shown, × to clear highlighted](images/ex01-3-2.png)

4. Click the dropdown and select **`IN262-Common-Artifacts`**

   ![Package dropdown open — IN262-Common-Artifacts highlighted](images/ex01-3-3.png)

5. The artifact list shows **`IN262-BP-to-BUPA-Mapping`** — tick the checkbox, then click **OK**

   ![References dialog — IN262-BP-to-BUPA-Mapping listed, checkbox ticked, OK highlighted](images/ex01-3-4.png)

6. The mapping appears under Global (1) — click **Add References → Script Collection**

   ![Global(1) tab — IN262-BP-to-BUPA-Mapping from IN262-Common-Artifacts confirmed, Add References → Script Collection highlighted](images/ex01-3-5.png)

---

## Step 8 — Add Global Script Collection Reference (Fix 3)

1. In the References dialog, click the **×** to clear the current package

   ![References dialog for Script Collection — own package shown, × to clear highlighted](images/ex01-3-6.png)

2. Select **`IN262-Common-Artifacts`** from the dropdown

   ![Package dropdown — IN262-Common-Artifacts highlighted](images/ex01-3-7.png)

3. The artifact list shows **`IN262-Script-Collection`** — tick the checkbox, then click **OK**

   ![IN262-Script-Collection listed — checkbox ticked, OK highlighted](images/ex01-3-8.png)

4. Global tab now shows **Global (2)**

   ![Global(2) — both IN262-BP-to-BUPA-Mapping and IN262-Script-Collection listed](images/ex01-3-9.png)

5. Click **Save** — confirm the saved toast

   ![Save button highlighted — Global(2) confirmed, saved toast visible](images/ex01-3-10.png)

---

## Step 9 — Wire Scripts to the Script Collection (Fix 4)

The script steps for Log Payload, Log Summary, and SBPA Payload still point to the deleted local files. Wire each one to the correct script in the Script Collection. `CalculatedDiscount` stays wired to its local file — do not change it.



> [!TIP]
> For each step below: click the step on the canvas → **Processing** tab → click **Select** → switch to **Global Resources** tab → select the script → click **OK** → **Save**.

### 9a — Log Payload

1. Click the **Log Payload** step on the canvas
2. Processing tab → click **Select**

   ![Log Payload step selected — Processing tab, Select button highlighted](images/ex01-4.png)

3. Switch to **Global Resources** tab → select **`Log Payload`** from the list → click **OK**

   ![Select Resource dialog — Global Resources tab, Log Payload highlighted, OK highlighted](images/ex01-4-1.png)

4. Click **Save**

   ![Script File shows /script/Log Payload.groovy — saved toast](images/ex01-4-1-1.png)

### 9b — Message Mapping step

1. Click the **IN262-BP-to-BUPA-Mapping** step on the canvas
2. Processing tab → click **Select**

   ![Message Mapping step selected — Processing tab, Select button highlighted](images/ex01-4-2.png)

3. Switch to **Global Resources** tab → select **`IN262BPtoBUPAMapping`** → click **OK**

   ![Select Mapping Resource — Global Resources tab, IN262BPtoBUPAMapping selected, OK highlighted](images/ex01-4-3.png)

4. Click **Save**

   ![Message Mapping saved — saved toast visible](images/ex01-4-4.png)

### 9c — Log Summary

1. Click the **Log Summary** step → Processing tab → **Select**

   ![Log Summary step selected — Processing tab, Select button highlighted](images/ex01-4-5.png)

2. Global Resources → select **`Log Summary`** → click **OK**

   ![Select Resource — Global Resources tab, Log Summary highlighted, OK highlighted](images/ex01-4-6.png)

3. Click **Save**

   ![Script File shows /script/Log Summary.groovy — Save button highlighted](images/ex01-4-7.png)

   ![Saved toast confirmed](images/ex01-4-8.png)

### 9d — Prepare SBPA Payload

1. Click the **Prepare SBPA Payload** step → Processing tab → **Select**

   ![Prepare SBPA Payload step selected — Processing tab, Select button highlighted](images/ex01-4-9.png)

2. Global Resources → select **`SBPA_Payload`** → click **OK**

   ![Select Resource — Global Resources tab, SBPA_Payload highlighted, OK highlighted](images/ex01-4-10.png)

3. Click **Save**

   ![Script File shows /script/SBPA_Payload.groovy — Save button highlighted](images/ex01-4-11.png)

   ![Saved toast confirmed](images/ex01-4-12.png)

---

## Step 10 — Verify the Full iFlow and Adapter Settings

1. Click on integration process and the click on  **Arrange Horizontally** button (bottom-right of canvas) to see the complete flow

   ![Arrange Horizontally button highlighted](images/ex01-5.png)

   ![Full iFlow overview at 48% zoom — all steps visible](images/ex01-5-1.png)

2. Click the **ProcessDirect** sender step → **Connection** tab — confirm the address shows <mark> **`/IN262-0XX` with your participant number** </mark>

   ![ProcessDirect Connection tab — Address field shows /IN262-0XX](images/ex01-5-2.png)

3. Click the **RFC call** step → **Connection** tab — confirm Destination = `S4HANA_RFC`

   ![RFC Connection tab — Destination S4HANA_RFC confirmed](images/ex01-5-3.png)

4. Click the **S4HANA Call** OData step → **Processing** tab — confirm Resource Path = `A_SalesOrder` and Query Options are populated

   ![OData Processing tab — Resource A_SalesOrder and query options visible](images/ex01-5-4.png)

5. Scroll down in Processing — confirm Request/Response Headers = `traceparent`

   ![OData Processing tab scrolled — traceparent headers visible](images/ex01-5-5.png)

6. Click on canvas (see arrow in screenshot), select **Externalized Parameters** tab — confirm the query options and OData body parameters are listed

   ![Externalized Parameters tab — query options and body params visible](images/ex01-5-6.png)

7. Click the **Send Offer** HTTP step → **Connection** tab — confirm:
   - Address: `https://spa-api-gateway-bpi-eu-prod.cfapps.eu10.hana.ondemand.com/workflow/rest/v1/workflow-instances`
   - Method: `POST`
   - Authentication: `OAuth2 Client Credentials`
   - Credential Name: `BPA-OAuth-Credential`

   ![HTTP Connection tab — BPA URL, POST method, OAuth2, BPA-OAuth-Credential confirmed](images/ex01-5-7.png)

8. Click **Save** — confirm the saved toast

   ![Save button highlighted — saved toast visible](images/ex01-5-8.png)

---

## Step 11 — Update the CalculatedDiscount Script

The AI generated a placeholder script in the `CalculatedDiscount` step. Replace it with the exercise script below.

1. Click the **CalculatedDiscount** step on the canvas → **Processing** tab
   - Click the script filename to open the editor
     
   ![CalculatedDiscount step selected — Processing tab, Script File highlighted](images/ex01-6.png)

2. In the editor, default template is shown

   ![Script editor open — default template content visible](images/ex01-6-1.png)

3. Select all the existing content and replace it by pasting the script below:

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.util.XmlSlurper

def Message processData(Message message) {

    double totalPrice = 0.0
    double highValueTotal = 0.0
    double standardTotal = 0.0
    int orderCount = 0
    int highValueCount = 0
    String orderSummary = ""
    String itemSummary = ""

    try {
        def body = message.getBody(java.io.Reader)
        def root = new XmlSlurper().parse(body)

        root.A_SalesOrderType.each { order ->
            def salesOrderId = order.SalesOrder.text().trim()
            def currency = order.TransactionCurrency.text().trim()
            def amount = order.TotalNetAmount.text().trim()
            double orderAmount = 0.0

            if (amount) {
                try {
                    orderAmount = Double.parseDouble(amount)
                    totalPrice += orderAmount
                    orderCount++
                } catch (NumberFormatException e) {
                }
            }

            if (orderAmount >= 5000) {
                highValueTotal += orderAmount
                highValueCount++
                orderSummary = orderSummary + "HIGH|" + salesOrderId + "|" + currency + "|" + orderAmount + "\n"
            } else {
                standardTotal += orderAmount
                orderSummary = orderSummary + "STD|" + salesOrderId + "|" + currency + "|" + orderAmount + "\n"
            }

            order.to_Item.A_SalesOrderItemType.each { item ->
                def itemId = item.SalesOrderItem.text().trim()
                def material = item.Material.text().trim()
                def itemText = item.SalesOrderItemText.text().trim()
                def qty = item.RequestedQuantity.text().trim()
                def unit = item.RequestedQuantityUnit.text().trim()
                itemSummary = itemSummary + salesOrderId + "|" + itemId + "|" + material + "|" + itemText + "|" + qty + " " + unit + "\n"
            }
        }
    } catch (Exception e) {
    }

    message.setProperty("TotalPrice", totalPrice.toString())
    message.setProperty("OrderCount", orderCount.toString())
    message.setProperty("HighValueOrderCount", highValueCount.toString())
    message.setProperty("HighValueTotal", highValueTotal.toString())
    message.setProperty("StandardTotal", standardTotal.toString())

    double productPrice = 1000.0
    double discountRate = totalPrice > 5000 ? 15 : 5
    double discountPercentage = discountRate / 100.0
    double finalPrice = productPrice - (productPrice * discountPercentage)

    message.setProperty("ProductPrice", productPrice.toString())
    message.setProperty("DiscountDecimal", discountPercentage.toString())
    message.setProperty("FinalPrice", finalPrice.toString())
    message.setProperty("Discount", discountRate.toString() + "%")

    return message
}
```

   ![Script editor — exercise script pasted in, scrolled to show bottom](images/ex01-6-2.png)

4. Click **Apply**

   ![Apply button highlighted](images/ex01-6-3.png)

5. An **Incompatibilities** warning appears — this is expected (Groovy 1.x vs 2.x version notice). Click **OK**

   ![Incompatibilities dialog — OK button highlighted, Script changed successfully toast visible](images/ex01-6-4.png)

6. Click **Close** to return to the canvas

   ![Close button highlighted](images/ex01-6-5.png)

7. Click **Save** — confirm the saved toast

   ![CalculatedDiscount step on canvas — Save button highlighted](images/ex01-6-6.png)

   ![Saved toast confirmed](images/ex01-6-7.png)

---

## Exercise 1.1 Complete

Your iFlow is wired and ready for Exercise 1.2. It was generated from a plain-language description and wired to shared artefacts — without writing a single line of code or configuring adapters manually.

---

*Next: [Exercise 1.2 — Script Optimisation and Memory Profiling ](../ex1-2/README.md)*
