# Exercise 3: Create MCP Server & Expose the API
**AI Feature:** Build MCP for S4/HANA API

**Duration:** 20 min

**Business Context:**
 MCP Gateway in SAP Integration Suite enables enterprises to securely connect AI agents to existing APIs, business processes, and backend services using the Model Context Protocol (MCP), e.g., in this exercise, to an SAP S/4HANA on-premises system, by exposing the S/4HANA Product Creation API as an MCP tool. This allows agents to interact with product master data through a governed, standardized interface, reducing manual effort and enabling automated, end-to-end business processes.

**What AI Does Here:**
Create an MCP Server to expose the S/4HANA product creation API as an agent-consumable tool.

> [!NOTE]
> For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP paradigm in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP](https://community.sap.com/t5/technology-blog-posts-by-sap/api-centric-integration-in-sap-integration-suite-a-new-paradigm-for-api-amp/ba-p/14438245)

## Exercise 3.1 Create an API Artifact from pre-configured S/4HANA business systems 
After completing these steps, you will have created an API Artifact by discovering APIs and their metadata from pre-configured business systems (e.g., SAP S/4HANA and deploying it on Integration Cell Runtime)
> **Reference:** [Create an MCP Server from an API Artifact](https://help.sap.com/docs/integration-suite/isuite-integrations-and-apis/create-mcp-server-from-api-artifact)
1. From the left navigation pane, choose **Design → Integrations and APIs**, open the already-created package in the last exercise IN262-0XX-Campaign-Management (where XX is your participant number). Navigate to **Artifacts** tab and click on **Edit**
<br>![](/exercises/ex3/images/ex03-1.png)
2. On the package details page, choose **Artifacts**, and under **Add**, select **API**. The **Add API** dialog opens.
<br>![](/exercises/ex3/images/ex03-1-1.png)
3. Select Runtime profile as **Integration Cell** and click **Next**
<br>![](/exercises/ex3/images/ex03-1-2.png)
4. Select a method as **API Provider** and click **Next**
<br>![](/exercises/ex3/images/ex03-1-3.png)
5. Under **Select API Provider** switch to **Destinations** tab, select **APIPORTAL-HE4_Connect**, and click **Next**
<br>![](/exercises/ex3/images/ex03-1-4.png)
6. Fill in the following API details:

   | Field | Value |
   |---|---|
   | Name | `Product_API_IN262-0XX` — replace `XX` with your two-digit participant number (e.g. `Product_API_IN262-007` for participant 007) |
   | ID | *(auto-populated)* |
   | API Provider | *(auto-populated)* |
   | Relative URL | `/sap/opu/odata/sap/API_PRODUCT_SRV` |
   | Service Type | **OData** |
   | API Base Path | `/IN262-0XX` — replace `XX` with your two-digit participant number (e.g. `/IN262-007` for participant 007) |
   | API State | *(auto-populated)* |
   | API Version | `1.0.0` |
   | Runtime Profile | *(auto-populated)* |
   | Virtual Host | *(auto-populated)* |
<br>![](/exercises/ex3/images/ex03-1-5.png)
7. Click **Next**
   
8. In **Generate Open API Specification** dialog, click **Add and Open in API Designer**
<br>![](/exercises/ex3/images/ex03-1-6.png)
9. API Designer will open showing the generated API specification. Click **Edit**
<br>![](/exercises/ex3/images/ex03-1-7.png)
10. On the next screen, choose **Import**
<br>![](/exercises/ex3/images/ex03-1-8.png)
11. On the Import Dialog, choose the **Browse** icon to upload the **API_PRODUCT.json** file from the local system and click **Open**.
<br>![](/exercises/ex3/images/ex03-1-9.png)
12. Click on **Import**
<br>![](/exercises/ex3/images/ex03-1-11.png)
13. The API Product specification gets imported with all operations. Now choose **Switch to API Details**
<br>![](/exercises/ex3/images/ex03-1-12.png)
14. Navigate to **Policies** tab
<br>![](/exercises/ex3/images/ex03-1-13.png)
15. You will see **Authentication** policy is added by default. Select **Authentication** policy and navigate to the property sheet below.
Make sure the **Type** includes **Basic**, **Client Certificate**, and **OAuth**.
<br>![](/exercises/ex3/images/ex03-1-14.png)
> [!NOTE]
>  The position of the authentication policy on the flow is immutable and it's the first policy on the flow.
> Authentication policy is a mandatory policy and is added to request flow by default. Neither you can remove this policy, nor you can add another authentication policy to the flow.
> Authentication policy can be added to the request flow only once. All other policies can be added to the flow multiple times.
16. Choose **Authorization** policy and select the **Trust Upstream MCP Authorization** checkbox
<br>![](/exercises/ex3/images/ex03-1-15.png)
17. Save the API by choosing **Save** in the top right corner.
<br>![](/exercises/ex3/images/ex03-1-16.png)
18. You will see a **saved** confirmation toast. Now deploy your API to Integration Cell runtime by clicking the **...** menu in the top right corner and choosing **Deploy**.
<br>![](/exercises/ex3/images/ex03-1-17.png)
19. On the Deploy dialog, select **Runtime Profile** as **Integration Cell** and then choose **Deploy**
<br>![](/exercises/ex3/images/ex03-1-18.png)
20. You will see a deployment triggered toast: **'Product_API_IN262-0XX' is triggered for deployment**
<br>![](/exercises/ex3/images/ex03-1-19.png)
21. Navigate to the **Deployment Status** tab. The status will initially show **Deploying / Starting**
<br>![](/exercises/ex3/images/ex03-1-20.png)
22. Refresh until the Deployment Status shows **Deployed** and Runtime Status shows **Started**
<br>![](/exercises/ex3/images/ex03-1-21.png)

## Exercise 3.2 Create and configure MCP Server
After completing these steps, you will have exposed a deployed API artifact (for example, the S/4HANA Onpremise **API Product SRV** API) as an MCP server, so that AI agents can discover and invoke its operations as tools.
1. From the left navigation pane, choose **Design → Integrations and APIs**, open the already-created package in the last exercise IN262-0XX-Campaign-Management (where XX is your participant number).
<br>![](/exercises/ex3/images/ex03-2-1.png)
2. Navigate to **Artifacts** tab and click on **Edit**
<br>![](/exercises/ex3/images/ex03-2-2.png)
3. Under **Add**, select **MCP Server**. The **Add MCP Server** dialog opens.
<br>![](/exercises/ex3/images/ex03-2-3.png)
4. Select **API** as the source type and choose **Next**.
<br>![](/exercises/ex3/images/ex03-2-4.png)
5. In the **Provide MCP Details** step, choose the icon next to the **API** field.
<br>![](/exercises/ex3/images/ex03-2-5.png)  
6. In the **Select an API** dialog, select your deployed API **API_PRODUCT (Product_API_IN262-0XX)** and choose **Add**.
<br>![](/exercises/ex3/images/ex03-2-6.png)    
7. Fill in the MCP server details:
   | Field | Value |
   | --- | --- |
   | **Name** | `API_PRODUCT_mcpServer_in262_0XX` (where XX is your participant number)| 
   | **ID** | `API_PRODUCT_mcpServer_in262_0XX` (where XX is your participant number) |
   | **MCP Path** | `/mcp_in262-0XX` (where XX is your participant number) |
   | **Version** | Optional. Add a version to manage multiple concurrent versions of the server. |
   | **Runtime Profile** | Select the **Integration Cell** runtime node. |
   | **Virtual Host** | Review the virtual host URL; it is combined with the base path to form the public endpoint. |
<br>![](/exercises/ex3/images/ex03-2-7.png)
8. Choose **Next** to proceed to **Create Tools**.

9. In **Create Tools**, select the API resources (operations) you want to expose as MCP tools by checking the checkbox next to each one. Remember the limit of **30 tools** per MCP server.
<br>![](/exercises/ex3/images/ex03-2-8.png)
10. Scroll down to confirm your tool selection and choose **Add** to finalize and create the MCP server.
<br>![](/exercises/ex3/images/ex03-2-9.png)
11. The MCP Server details page opens. Note the **MCP URL** shown at the top — this is the endpoint your AI agent will use. Choose **Deploy**.
<br>![](/exercises/ex3/images/ex03-2-10.png)
12. On the Deploy dialog, confirm **Runtime Profile** is **Integration Cell** and choose **Deploy**.
<br>![](/exercises/ex3/images/ex03-2-11.png)
13. You will see a deployment triggered toast: **'API_PRODUCT_mcpServer_in262_0XX' is triggered for deployment.**
<br>![](/exercises/ex3/images/ex03-2-12.png)
14. The MCP Server is successfully deployed. The toast confirms: **'API_PRODUCT_mcpServer_in262_0XX' successfully deployed.**
<br>![](/exercises/ex3/images/ex03-2-13.png)
15. Navigate to the **Policies** tab and check the **Deployment Status** tab. Deployment Status should be **Deployed** and Runtime Status **Started**.
<br>![](/exercises/ex3/images/ex03-2-14.png)
16. Navigate back to **Design → Integrations and APIs** to confirm the API artifact also shows **Deployed** status.
<br>![](/exercises/ex3/images/ex03-2.png)

## Exercise 3.3 Create agent subscription in Developer Hub
After completing this exercise, you will have an MCP Server that can be invoked using credentials obtained through a Developer Hub subscription.
 
1. On SAP Integration Suite tenant, from the deployed MCP Server page, choose the nine dots icon in the top right corner.
<br>![](/exercises/ex3/images/ex03-3.png)
2. Choose **Developer Hub**
<br>![](/exercises/ex3/images/ex03-3-1.png)
3. In Developer Hub, navigate to **Admin Center → Manage Content**.
<br>![](/exercises/ex3/images/ex03-3-2.png)
4. A list of all business systems will be displayed. Select the Integration Suite business system **workshop02b** to discover the MCP servers within that business system.
<br>![](/exercises/ex3/images/ex03-3-3.png)
5. Navigate to the **AI Artifacts** tab. A list of all MCP servers within the business system is displayed.
<br>![](/exercises/ex3/images/ex03-3-4.png)
6. Select the checkbox next to your MCP server **API_PRODUCT_mcpServer_in262_0XX** and then choose **Create Product**.
<br>![](/exercises/ex3/images/ex03-3-5.png)
7. The Create Product dialog opens. Provide all required product information:
    
| Field | Value |
| --- | --- |
| **Name** | `IN262-0XX-Product` (where XX is your participant number)| 
| **ID** | `IN262-0XX-Product` (where XX is your participant number) |
| **Short Text** | `This is DH product for IN262-0XX` (where XX is your participant number) |
| **Description** | `This is Developer Hub product for IN262-0XX` (where XX is your participant number) | 

Then choose **Publish**.
<br>![](/exercises/ex3/images/ex03-3-6.png)
8. An information dialog confirms: **Your request to publish this product has been created.** Choose the **Scheduled Requests** link to monitor progress.
<br>![](/exercises/ex3/images/ex03-3-7.png)
9. The Status of the request will be **In Progress**.
<br>![](/exercises/ex3/images/ex03-3-8.png)
10. Refresh the page by choosing **Refresh** and the status should soon turn into **Success**.
<br>![](/exercises/ex3/images/ex03-3-9.png)
11. Navigate to **My Workspace**.
<br>![](/exercises/ex3/images/ex03-3-10.png)
12. Choose **Agents** tab and then choose **Subscription for Agent** from the **Create** dropdown.
<br>![](/exercises/ex3/images/ex03-3-11.png)
13. On **Create New Subscription for Agent** dialog, step 1: select your product **IN262-0XX-Product** and choose **Next**.
<br>![](/exercises/ex3/images/ex03-3-12.png)
14. Enter the subscription details and click **Next**:

   | Field | Value |
   | --- | --- |
   | **Name** | `IN262-0XX-Agent-Subscription` (where XX is your participant number)| 
   | **Short Text** | `Agent Subscription for IN262-0XX-Product` (where XX is your participant number) |
   | **Description** | `Agent Subscription for IN262-0XX-Product, will be used to access through AI client or agent.` | 
   
<br>![](/exercises/ex3/images/ex03-3-13.png)

15. Review the subscription summary and click **Save**.
<br>![](/exercises/ex3/images/ex03-3-14.png)
16. Your subscription is being finalized. Refresh the page after a moment.
<br>![](/exercises/ex3/images/ex03-3-15.png)
17. Once finalized, your **Credentials** (Token URL, Key, Secret) are shown. Copy these credentials and save them — you will need them in Exercise 4.
<br>![](/exercises/ex3/images/ex03-3-16.png)

## Summary

You have created an API artifact from the S/4HANA business system, deployed it on Integration Cell runtime, wrapped it as an MCP server, and obtained an agent subscription with credentials from Developer Hub. Your MCP server is ready to be used by an AI agent in Exercise 4.

Continue to - [Exercise 4 — End-to-End: Create a Product and Trigger the Campaign](../ex4/README.md)
