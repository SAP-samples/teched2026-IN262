# Exercise 3: Create MCP Server & Expose the API
**AI Feature:** Build MCP for S4/HANA API

**Duration:** 20 min

**Business Context:**
 MCP Gateway in SAP Integration Suite enables enterprises to securely connect AI agents to existing APIs, business processes, and backend services using the Model Context Protocol (MCP), e.g., in this exercise, to an SAP S/4HANA on-premises system, by exposing the S/4HANA Product Creation API as an MCP tool. This allows agents to interact with product master data through a governed, standardized interface, reducing manual effort and enabling automated, end-to-end business processes.

## What the MCP Server Enables

| Capability | What happens |
|---|---|
| **API Discovery** | Connects to the pre-configured S/4HANA business system  |
| **MCP Server** | Exposes API as a governed, agent-consumable tool via the Model Context Protocol |
| **Agent Access** | Any MCP-compatible AI agent (e.g. Claude Code CLI) can discover and call the S/4HANA product creation API as a tool — no manual API client or payload construction needed |

> [!NOTE]
> For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP paradigm in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP](https://community.sap.com/t5/technology-blog-posts-by-sap/api-centric-integration-in-sap-integration-suite-a-new-paradigm-for-api-amp/ba-p/14438245)

## Exercise 3.1 Create an API Artifact from pre-configured S/4HANA business systems 
After completing these steps, you will have created an API Artifact by discovering APIs and their metadata from pre-configured business systems (e.g., SAP S/4HANA) and deploying it on Integration Cell Runtime.

> **Reference:** [Create an MCP Server from an API Artifact](https://help.sap.com/docs/integration-suite/isuite-integrations-and-apis/create-mcp-server-from-api-artifact)

1. Navigate to **Design → Integrations and APIs**, open the already-created package **IN262-0XX-Campaign-Management** (where XX is your participant number). Click the **Artifacts** tab and click **Edit**.

   ![](images/ex03-1.png)

2. Click **Add → API**. The **Add API** dialog opens.

   ![](images/ex03-1-1.png)

3. Select Runtime profile as **Integration Cell** and click **Next**.

   ![](images/ex03-1-2.png)

4. Select **API Provider** as the method and click **Next**.

   ![](images/ex03-1-3.png)

5. Switch to the **Destinations** tab, select **APIPORTAL-HE4_Connect**, and click **Next**.

   ![](images/ex03-1-4.png)

6. Fill in the following API details and click **Next**:

   | Field | Value |
   |---|---|
   | Name | `Product_API_IN262-0XX` — replace `XX` with your two-digit participant number (e.g. `Product_API_IN262-007` for participant 007) |
   | ID | *(auto-populated)* |
   | API Provider | *(auto-populated)* |
   | Relative URL | `/sap/opu/odata/sap/API_PRODUCT_SRV` |
   | Service Type | **OData** |
   | API Base Path | `/IN262-0XX` — replace `0XX` with your 3 digit participant number (e.g. `/IN262-007` for participant 007) |
   | API State | *(auto-populated)* |
   | API Version | `1.0.0` |
   | Runtime Profile | *(auto-populated)* |
   | Virtual Host | *(auto-populated)* |

   ![](images/ex03-1-5.png)

7. In the **Generate Open API Specification** dialog, click **Add and Open in API Designer**.

   ![](images/ex03-1-6.png)

8. API Designer opens showing the generated API specification. Click **Edit**.

   ![](images/ex03-1-7.png)

9. We need to import specification file, for that we will download the file as given below
  - Open the specification file in <mark> NEW tab (right click, open link in new tab) </mark>  [API_PRODUCT.json](downloads/API_PRODUCT.json)
  - Click on download button to download
   
   ![](images/ex03-1-8-1.png)
   
10. Click **Import**.

   ![](images/ex03-1-8.png)

11. The Import dialog opens — click the **Browse** icon to select the specification file.

   ![](images/ex03-1-10-1.png)
    
12. On the file picker, select the downloaded **API_PRODUCT.json** file, and click **Open**.

    ![](images/ex03-1-9.png)

13. The file name appears in the Import dialog.
>[!CAUTION]
> <mark> **An error on import is expected**, see step 15 to fix it.</mark>
> This happens as import causes overwriting of some API Details like **Name** and **API Base Path**.

  
  - Click **Import** to confirm
  
   ![](images/ex03-1-10.png)

14. The API Product specification gets imported with all operations. 
>[!TIP]
> <mark> **An error on import is expected**, see step 15 to fix it.</mark>
> This happens as import causes overwriting of some API Details like **Name** and **API Base Path**.

 - Click **Switch to API Details**.

![](images/ex03-1-11.png)

15. In the API Details view, <mark>**update the following fields AGAIN** </mark> — replace `0XX` with your 3 digit participant number:
>[!IMPORTANT]
>After import Name and API Base Path are overwritten, update these values again.

   | Field | Value |
   |---|---|
   | **Name** | `Product_API_IN262-0XX` <br> Replace `0XX` with your 3 digit participant number (e.g. `Product_API_IN262-007` for participant 007) |
   | **API Base Path** | `/IN262-0XX` <br> Replace `0XX` with your 3 digit participant number (e.g. `/IN262-007` for participant 007)|

   ![](images/ex03-1-11-1.png)

16. Click the **Policies** tab.

    ![](images/ex03-1-12.png)

17. You will see the **Authentication** policy is added by default. Click **Authentication**.
  - add **Basic** if not available
  - check the property sheet below — make sure **Type** includes **Basic**, **Client Certificate**, and **OAuth**
     
> [!NOTE]
> The position of the authentication policy on the flow is immutable and it's the first policy on the flow.
> Authentication policy is a mandatory policy and is added to request flow by default. Neither you can remove this policy, nor you can add another authentication policy to the flow.
> Authentication policy can be added to the request flow only once. All other policies can be added to the flow multiple times.

   ![](images/ex03-1-13.png)

18. Click the **Authorization** policy and select the **Trust Upstream MCP Authorization** checkbox.

   ![](images/ex03-1-14.png)

19. Click **Save** in the top right corner.

    ![](images/ex03-1-15.png)

20. You will see a **saved** confirmation toast. Click the **...** menu in the top right corner and click **Deploy**.

    ![](images/ex03-1-16.png)

21. Click **Deploy** from the dropdown.

    ![](images/ex03-1-17.png)

22. On the Deploy dialog, confirm **Runtime Profile** is **Integration Cell** and click **Deploy**.

    ![](images/ex03-1-18.png)

23. You will see a deployment triggered toast: **'Product_API_IN262-0XX' is triggered for deployment**.

    ![](images/ex03-1-19.png)

24. Click the **Deployment Status** tab. The status will initially show **Deploying / Starting**.

    ![](images/ex03-1-20.png)

25. Refresh until Deployment Status shows **Deployed** and Runtime Status shows **Started**.

    ![](images/ex03-1-21.png)

## Exercise 3.2 Create and configure MCP Server
After completing these steps, you will have exposed a deployed API artifact (for example, the S/4HANA on-premise **API Product SRV** API) as an MCP server, so that AI agents can discover and invoke its operations as tools.

1. Navigate to **Design → Integrations and APIs**, open the package **IN262-0XX-Campaign-Management** (where XX is your participant number).

   ![](images/ex03-2-1.png)

2. Click the **Artifacts** tab and click **Edit**.

   ![](images/ex03-2-2.png)

3. Click **Add → MCP Server**. The **Add MCP Server** dialog opens.

   ![](images/ex03-2-3.png)

4. Select **API** as the source type and click **Next**.

   ![](images/ex03-2-4.png)

5. In the **Provide MCP Details** step, click the icon next to the **API** field.

   ![](images/ex03-2-5.png)

6. In the **Select an API** dialog, select your deployed API **Product_API_IN262-0XX** and click **Add**.
> [!NOTE]
> Select the <mark>API that you created in previous step with **your participant number**</mark>

   ![](images/ex03-2-6.png)

7. Fill in the MCP server details:

   | Field | Value |
   | --- | --- |
   | **Name** | `Product_API_IN262_0XX_mcpServer` (where XX is your participant number) |
   | **ID** | `Product_API_IN262-0XX_mcpServer` (where XX is your participant number) |
   | **MCP Path** | `/mcp_in262-0XX` (where XX is your participant number) |
   | **Version** | Optional. Add a version to manage multiple concurrent versions of the server. |
   | **Runtime Profile** | Select the **Integration Cell** runtime node. |
   | **Virtual Host** | Review the virtual host URL; it is combined with the base path to form the public endpoint. |

   ![](images/ex03-2-7.png)

8. Click **Next** to proceed to **Create Tools**.

9. In **Create Tools**, select the API resources (operations) you want to expose as MCP tools by checking the checkbox next to each one. Remember the limit of **30 tools** per MCP server.

   ![](images/ex03-2-8.png)

10. Scroll down to confirm your tool selection and click **Add** to finalize and create the MCP server.

    ![](images/ex03-2-9.png)

11. The MCP Server details page opens. Note the **MCP URL** shown at the top — this is the endpoint your AI agent will use. Click **Deploy**.

    ![](images/ex03-2-10.png)

12. On the Deploy dialog, confirm **Runtime Profile** is **Integration Cell** and click **Deploy**.

    ![](images/ex03-2-11.png)

13. You will see a deployment triggered toast: **'Product_API_IN262_0XX_mcpServer' is triggered for deployment**.

    ![](images/ex03-2-12.png)

14. The MCP Server is successfully deployed. The toast confirms: **'Product_API_IN262_0XX_mcpServer' successfully deployed**.

    ![](images/ex03-2-13.png)

15. Click the **Policies** tab and then the **Deployment Status** tab. Deployment Status should be **Deployed** and Runtime Status **Started**.

    ![](images/ex03-2-14.png)



## Exercise 3.3 Create agent subscription in Developer Hub
After completing this exercise, you will have an MCP Server that can be invoked using credentials obtained through a Developer Hub subscription.

1. From the deployed MCP Server page, click the nine dots icon in the top right corner.

   ![](images/ex03-3.png)

2. Click **Developer Hub**.

   ![](images/ex03-3-1.png)

3. In Developer Hub, navigate to **Admin Center → Manage Content**.

   ![](images/ex03-3-2.png)

4. A list of all business systems will be displayed. Select the Integration Suite business system **workshop02b** to discover the MCP servers within that business system.

   ![](images/ex03-3-3.png)

5. Click the **AI Artifacts** tab. A list of all MCP servers within the business system is displayed.

   ![](images/ex03-3-4.png)

6. Select the checkbox next to your MCP server **Product_API_IN262_0XX_mcpServer** and click **Create Product**.

   ![](images/ex03-3-5.png)

7. The Create Product dialog opens. Fill in the following product details:

   | Field | Value |
   | --- | --- |
   | **Name** | `IN262-0XX-Product` (where XX is your participant number) |
   | **ID** | `IN262-0XX-Product` (where XX is your participant number) |
   | **Short Text** | `This is DH product for IN262-0XX` (where XX is your participant number) |
   | **Description** | `This is Developer Hub product for IN262-0XX` (where XX is your participant number) |

   Click **Publish**.

   ![](images/ex03-3-6.png)

8. An information dialog confirms: **Your request to publish this product has been created.** Click the **Scheduled Requests** link to monitor progress.

   ![](images/ex03-3-7.png)

9. The Status of the request will be **In Progress**.

   ![](images/ex03-3-8.png)

10. Click **Refresh** until the status turns to **Success**.

    ![](images/ex03-3-9.png)

11. Navigate to **My Workspace**.

    ![](images/ex03-3-10.png)

12. Click the **Agents** tab and then click **Subscription for Agent** from the **Create** dropdown.

    ![](images/ex03-3-11.png)

13. On the **Create New Subscription for Agent** dialog, select your product **IN262-0XX-Product** and click **Next**.

    ![](images/ex03-3-12.png)

14. Fill in the subscription details and click **Next**:

    | Field | Value |
    | --- | --- |
    | **Name** | `IN262-0XX-Agent-Subscription` (where XX is your participant number) |
    | **Short Text** | `Agent Subscription for IN262-0XX-Product` (where XX is your participant number) |
    | **Description** | `Agent Subscription for IN262-0XX-Product, will be used to access through AI client or agent.` |

    ![](images/ex03-3-13.png)

15. Review the subscription summary and click **Save**.

    ![](images/ex03-3-14.png)

16. Your subscription is being finalized. Click **Refresh** after a moment.

    ![](images/ex03-3-15.png)

17. Once finalized, your **Credentials** (Token URL, Key, Secret) are shown. Copy these credentials and save them — you will need them in Exercise 4.

    ![](images/ex03-3-16.png)

## Summary

You have created an API artifact from the S/4HANA business system, deployed it on Integration Cell runtime, wrapped it as an MCP server, and obtained an agent subscription with credentials from Developer Hub. Your MCP server is ready to be used by an AI agent in Exercise 4.

Continue to - [Exercise 4 — End-to-End: Create a Product and Trigger the Campaign](../ex4/README.md)
