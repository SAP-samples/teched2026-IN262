# Exercise 2: Resolve an Integration Error with AI

**AI Feature:** AI Error Analysis  
**Duration:** 15 minutes

---

## Business Context

Your integration flow is deployed and running. The next run fails silently — the S/4HANA steps all succeed, but the campaign workflow is never created in SAP Build Process Automation. The error is buried in an HTTP response body from an external system. Instead of manually tracing through logs and configuration, you will use AI-assisted error analysis in the Message Monitor to identify the root cause and apply the fix.

---

## What AI Does Here

| Capability | When it fires | What you see |
|---|---|---|
| **AI Error Analysis** | You click "Analyse" on a failed message | AI identifies the root cause, names the first failed step, and proposes a targeted fix |

---

## Prerequisites

- Exercise 1 complete — your `IN262-0XX-Calculate-Discount` iFlow is deployed and running <mark>**(where 0XX is your participant number)**</mark>
- Your participant number `[NNN]` — printed on your session card (001–040)

---

## Step 1 — Copy the Test Trigger iFlow into Your Package

The test trigger iFlow fires your campaign flow on demand without waiting for a real product creation event from S/4HANA.

1. Navigate to **Design → Integrations and APIs**

   ![Design — Integrations and APIs showing existing packages](images/ex2-1.png)

2. Click **IN262-Common-Artifacts** to open it

   ![IN262-Common-Artifacts highlighted in package list](images/ex2-1-1.png)


3. Click the **Artifacts** tab — locate **`IN262-Trigger-Campaign-Flow-Test`** in the list and Click `...` → **Copy**

   ![Artifacts tab — IN262-Trigger-Campaign-Flow-Test listed, three-dot menu open, Copy highlighted](images/ex2-1-3.png)

4. In the Copy dialog:
   - Enter **Name:** `IN262-Trigger-Campaign-Flow-Test_IN262-0XX` 

> [!NOTE]
> Replace XX with your parcipant number

   - Click **Select** next to Package

   ![Copy dialog — name filled in, Select button highlighted](images/ex2-1-4.png)

5. In the Package Selection dialog, choose **`IN262-0XX-Campaign-Management`** <mark> (select the package that you created with your participant number)</mark>
> [!NOTE]
> Select the package which you created with your participant number XX 

   ![Package Selection dialog — IN262-0XX-Campaign-Management highlighted](images/ex2-1-5.png)

6. Click **Copy**

   ![Copy dialog — IN262-0XX-Campaign-Management selected, Copy button highlighted](images/ex2-1-6.png)

7. Success toast confirms the copy — click **Navigate**

   ![Success toast — Navigate button highlighted](images/ex2-1-7.png)


---

## Step 2 — Configure Your Product ID and Deploy the Trigger iFlow

1. You are now in your `IN262-0XX-Campaign-Management` package — locate **`IN262-Trigger-Campaign-Flow-Test_IN262-0XX`** and click **Navigate** (→)
> [!NOTE]
> Select the test iFlow which you created with your participant number XX 

   ![Package artifacts list — IN262-Trigger-Campaign-Flow-Test_IN262-0XX highlighted](images/ex2-2.png)

2. Click **Edit**

   ![Trigger iFlow in view mode — Edit button highlighted](images/ex2-2-1.png)

3. Click the **`Get Product ID from Event`** Content Modifier step on the canvas → **Exchange Property** tab

   ![Trigger iFlow in edit mode — Prepare Process Direct Data step selected, Exchange Property tab visible](images/ex2-2-2.png)

4. Verify the constant value `MZ-TG-ZAD01-IN262-0503` <mark> **(do not change it)** </mark>

> [!NOTE]
> This is a shared test product pre-loaded in S/4HANA for this exercise. All participants use the same product ID — do not change it.

   ![Content Modifier — productId property with value MZ-TG-ZAD01-IN262-0503](images/ex2-2-3.png)

5. Click receiver **Process Direct** adapter, click **Connection** Tab
   - Enter `/IN262-0XX` <mark>(replace `0XX` with your participant number)</mark>
   ![Call particpant process for testing](images/ex2-2-3-0-1.png)

7. Click **Save as Version**
   ![Save as Version](images/ex2-2-3-1.png)

8. In Version Information dialog **Comment**, copy and paste
   - `Changed the process direct address to /IN262-0XX`
   - Click **Save**
   ![Save as Version button highlighted](images/ex2-2-4.png)

9. Toast confirms saved as version

   ![Version Information dialog — version and comment filled, Save highlighted](images/ex2-2-5.png)

10. Click **Deploy**

   ![Saved as version toast](images/ex2-2-6.png)

11. Confirm Runtime Profile as **Cloud Integration** → click **Deploy**

   ![Deploy button highlighted](images/ex2-2-7.png)

11. Click **OK** on the deployment confirmation

   ![Deploy dialog — Cloud Integration runtime profile, Deploy highlighted](images/ex2-2-8.png)

11. Toast confirms successfully deployed

    ![Deployment triggered dialog — OK highlighted](images/ex2-2-9.png)

12. Confirm **Deployment Status as Deployed** and **Runtime Status as Started**.
>[!TIP]   
> Click on the canvas to see Deployment Status tab 

   ![Deployment Status tab — Deployed and Started confirmed](images/ex2-2-13.png)

13. The iFlow triggers immediately after deployment

---

## Step 3 — Observe the Failure in the Message Monitor

1. Navigate to **Monitor → Integrations and APIs**

   ![Monitor — Integrations and APIs selected in left navigation](images/ex2-3.png)

2. Click the **All Artifacts — Past Hour** tile

   ![Monitor overview — All Artifacts Past Hour tile highlighted](images/ex2-3-1.png)

3. The message list shows `IN262-Trigger-Campaign-Flow-Test_IN262-0XX` and `IN262-0XX-Calculate-Discount` both with status **Failed**
> [!NOTE]
> Notice the iFlows which you created with your participant number XX 

   ![Monitor Message Processing — two Failed messages highlighted](images/ex2-3-2.png)

4. Click the failed `IN262-Trigger-Campaign-Flow-Test_IN262-0XX` message to open its details — click the **Analyse** button

   ![Message detail — Error Details tab, HTTP 404 error visible, Analyse button highlighted](images/ex2-3-3.png)

5. AI analysis will take a little time

   ![Message detail — Analyse button loading](images/ex2-3-4.png)

6. AI analysis completes — review the result:

   ![Message detail — AI-Generated Analysis showing Root Cause, First Failed Step, and Suggested Solution](images/ex2-3-5.png)


> [!NOTE]
> The AI identifies that an HTTP POST to the SAP Build Process Automation workflow endpoint returned a **404 Not Found**. The root cause is an incorrect workflow definition ID in the HTTP adapter configuration — the URL path `createCampaignForNewProduc` does not exist. The suggested fix is to verify the workflow definition ID.

---

## Step 4 — Fix the Script Reference in Your iFlow and Redeploy

The `SBPA_Payload` script in your iFlow contains a typo in the BPA workflow `definitionId` that makes the definition name wrong. A corrected script `SBPA_Payload-Fix` is already available in the Script Collection. You will update your iFlow to reference the correct script.

> [!NOTE]
> The typing error led to 404 issue could also be corrected by updating the script manually but for the hands-on exercise we just replace the script


1. Navigate to **Design → Integrations and APIs** → open **`IN262-0XX-Campaign Management`** package
> [!NOTE]
> Choose the package which you created with your participant number

   ![Design — Integrations and APIs, IN262-0XX-Campaign-Management highlighted](images/ex2-4-1.png)

2. Click **Artifacts** tab → open **`IN262-0XX-Calculate-Discount`**
> [!NOTE]
> Choose the iFlows which you created with your participant number

   ![Artifacts tab — IN262-0XX-Calculate-Discount highlighted](images/ex2-4-2.png)

3. Click **Edit**

   ![IN262-0XX-Calculate-Discount — Edit button highlighted, iFlow canvas visible](images/ex2-4-3.png)

4. Select the step `Prepare SBPA Payload`, in property sheet select `Processing` tab and click on `Select` button

   ![Script Collection in edit mode — SBPA_Payload selected, script content visible](images/ex2-4-4.png)

5. On `Select Resource` dialog, select `Global Resource`

   ![SBPA_Payload script — full content visible showing definitionId with trailing s](images/ex2-4-5.png)

6. Scroll down to open `SBPA_Payload-Fix` script and click **OK**

   ![Save as Version button highlighted](images/ex2-4-6.png)

7. Check the script is updated and click on `Save as Version`

   ![Version Information dialog — version 1.0.5, comment filled, Save highlighted](images/ex2-4-7.png)

8. Enter comment in the dialog:

   - **Comment:** `Fixed 404 error after analysing the error`
   - Click **Save**

   ![Saved as version toast — IN262-Script-Collection saved as version](images/ex2-4-8.png)

9. Toast confirms saved as version 

   ![Deploy button highlighted](images/ex2-4-9.png)

10. Click **Deploy** 

   ![Deploy dialog — Cloud Integration, Deploy highlighted](images/ex2-4-10.png)


11. Confirm Runtime Profile = **Cloud Integration** → **Deploy** 
    ![Deployment triggered toast for IN262-Script-Collection](images/ex2-4-11.png)


12. Toast confirms `IN262-0XX-Calculate-Discount` successfully deployed
    ![Successfully deployed toast for IN262-0XX-Calculate-Discount](images/ex2-4-13.png)


13. Confirm **Deployment Status = Deployed**, **Runtime Status = Started**

    ![Deployment Status tab — Deployed and Started confirmed](images/ex2-4-14.png)

<!-- To check if this is required 14. Toast confirms `IN262-Script-Collection` triggered for deployment 
    ![IN262-0XX-Calculate-Discount — Prepare SBPA Payload step, Processing tab, SBPA_Payload-Fix selected](images/ex2-4-12.png)
Navigate back to **`IN262-0XX-Campaign-Management`** package → open **`IN262-0XX-Calculate-Discount`** → **Edit** → click the **Prepare SBPA Payload** step → **Processing** tab → click **Select** → switch to **Global Resources** → select **`SBPA_Payload-Fix`** → **OK** → **Deploy**-->

---

## Step 5 — Re-trigger and Confirm Success

1. Navigate back to list of packages → click **Integrations and APIs** breadcrumb

   ![IN262-Script-Collection — Integrations and APIs breadcrumb highlighted](images/ex2-5.png)

2. Open **`IN262-0XX-Campaign-Management`** package

> [!NOTE]
> Choose the package which you created with your participant number  

   ![Design — IN262-0XX-Campaign-Management highlighted](images/ex2-5-1.png)

3. Click **Artifacts** tab


   ![IN262-0XX-Campaign-Management — Artifacts tab highlighted](images/ex2-5-2.png)

4. On **`IN262-Trigger-Campaign-Flow-Test_IN262-0XX`** → click `...` → **Deploy**

> [!NOTE]
> Choose the test iFlow which you copied from IN262-Script-Collection and renamed it your participant number  

   ![Artifacts list — three-dot menu open, Deploy highlighted](images/ex2-5-3.png)

5. Confirm Runtime Profile = **Cloud Integration** → click **Deploy**

   ![Deploy dialog — Cloud Integration, Deploy highlighted](images/ex2-5-4.png)

6. Toast confirms deployment triggered

   ![Deployment triggered toast](images/ex2-5-5.png)

7. Navigate to **Monitor → Integrations and APIs**

   ![Monitor — Integrations and APIs highlighted in left navigation](images/ex2-5-6.png)

8. Click **All Artifacts — Past Hour** tile

   ![Monitor overview — All Artifacts Past Hour tile highlighted, showing 9 messages](images/ex2-5-7.png)

9. Both `IN262-Trigger-Campaign-Flow-Test_IN262-0XX` and `IN262-0XX-Calculate-Discount` now show **Completed**

> [!NOTE]
> Observe Test and Calculate Discount iFlow with your participant number 

   ![Monitor Message Processing — both iFlows show Completed status](images/ex2-5-8.png)

---

## Exercise 2 Complete

The integration flow is running end-to-end. AI identified the root cause of a 404 error in under 30 seconds — a wrong workflow definition ID that would have required manual inspection of the HTTP adapter, the SBPA API documentation, and the Groovy script to find manually.

---

## Key Takeaway

AI error analysis works best when the error is in an external system's response — the kind of failure that doesn't produce a clear Java exception but returns an opaque HTTP status code. AI reads the full error context, traces it to the exact failing step, and proposes a fix.

---

*Next: [Exercise 3 — Create an MCP Server](../ex3/README.md)*
