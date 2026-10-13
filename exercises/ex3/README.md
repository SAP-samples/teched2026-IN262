# Exercise 3: Create MCP Server & Expose the API
**AI Feature:** Build MCP for S4/HANA API

**Duration:** 20 min

**Business Context:**
Enable AI agents to securely and efficiently create products in SAP S/4HANA by exposing the S/4HANA Product Creation API as an MCP tool. This allows agents to interact with product master data through a governed, standardized interface, reducing manual effort and enabling automated, end-to-end business processes.

**What AI Does Here:**
Create an MCP Server to expose the S/4HANA product creation API as an agent-consumable tool

In this exercise, you will build and configure an MCP Server that exposes the S/4HANA Product API as a tool in SAP Integration Suite, enabling AI agents to automate s.. securely and with proper governance, without compromising business context. 

> [!NOTE]
> For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP paradigm in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP](https://community.sap.com/t5/technology-blog-posts-by-sap/api-centric-integration-in-sap-integration-suite-a-new-paradigm-for-api-amp/ba-p/14438245)
## Exercise 3.1 Create an API Artifact from pre-configured S/4HANA business systems 
After completing these steps, you will have created an API Artifact by discovering APIs and their metadata from pre-configured business systems (e.g., SAP S/4HANA Cloud and deploying it on Integration Cell Runtime

> **Reference:** [Create an MCP Server from an API Artifact](https://help.sap.com/docs/integration-suite/isuite-integrations-and-apis/create-mcp-server-from-api-artifact)

1. From the left navigation pane, choose **Design → Integrations and APIs**, open the package created in the last exercise IN262-0XX-Campaign-Management (where XX is your user name), Navigate to **Artifacts** tab and click on **Edit**
<br>![](/exercises/ex3/images/ex04-1-1.png)
2. On the package details page, choose **Artifacts**, and under **Add**, select **API**. The **Add API** dialog opens.
<br>![](/exercises/ex3/images/ex04-1-2.png)
3. Select Runtime profile as **Integration Cell** and click **Next**
<br>![](/exercises/ex3/images/ex04-1-3.png)
4. Select a method as **API Provider** and click **Next**
<br>![](/exercises/ex3/images/ex04-1-4.png)
5. Under **Select API Provider** switch to **Destinations** tab, select **APIPORTAL-HE4_Connect**, and click **Next**
<br>![](/exercises/ex3/images/ex04-1-5.png)
6. Fill in the following API details:

| Field | Value |
|---|---|
| Name | `Product_АРІ_IN262-000` (replace XX with your assigned participant number) |
| ID | *(auto-populated)* |
| API Provider | *(auto-populated)* |
| Relative URL | `/sap/opu/odata/sap/API_PRODUCT_SRV` |
| Service Type | *(auto-populated)* |
| API Base Path | `/IN262-000` (replace XX with your assigned participant number) |
| API State | *(auto-populated)* |
| API Version | `1.0.0` |
| Runtime Profile | *(auto-populated)* |
| Virtual Host | *(auto-populated)* |

<br>![](/exercises/ex3/images/ex04-1-6.png)

7. Click **Next**
8. In **Generate Open API Specification** dialog, click **Add and Open in API Designer**
<br>![](/exercises/ex3/images/ex04-1-7.png)
9. API Designer will open, click **Edit**
<br>![](/exercises/ex3/images/ex04-1-8.png)
10. On the next screen, choose **Import**
<br>![](/exercises/ex3/images/ex04-1-9.png)
11. On the Import Dialog, choose the ** Browse ** icon to upload the **API Product.json** file from the local system.
<br>![](/exercises/ex3/images/ex04-1-10.png)
12. Choose **Open**
<br>![](/exercises/ex3/images/ex04-1-11.png)
13. Click on **Import**
<br>![](/exercises/ex3/images/ex04-1-12.png)
14. The API Product specification gets imported. Now choose **Switch to API Details**
<br>![](/exercises/ex3/images/ex04-1-13-1.png)
15. Navigate to **Policies** tab
<br>![](/exercises/ex3/images/ex04-1-13-2.png)   
16. You will see **Authorization** and **Authentication** policies are added by default. Select **Authentication** policy and navigate to the property sheet below.
Make sure the **Type** is **Basic**, **Client Certificate**, and **OAuth**.
br>![](/exercises/ex3/images/ex04-1-13-3.png)
> [!NOTE]
>  The position of the authentication policy on the flow is immutable and it’s the first policy on the flow.
> Authentication policy is a mandatory policy and is added to request flow by default. Neither you can remove this policy, nor you can add another authentication policy to the flow.
> Authentication policy can be added to the request flow only once. All other policies can be added to the flow multiple times.
17. Choose **Authorization** policy and select the **Trust Upstream MCP Authorization** checkbox
br>![](/exercises/ex3/images/ex04-1-13-4.png)
18. Save the API by choosing **Save** in the top right corner.
br>![](/exercises/ex3/images/ex04-1-13.png)
19. Now you will deploy your API to Integration Cell runtime by choosing **Deploy** as shown in the screenshot.
br>![](/exercises/ex3/images/ex04-1-14.png)
20. On the Deploy Wizard, select **Runtime Profile** as ***Integration Cell** and then choose **Deploy**
br>![](/exercises/ex3/images/ex04-1-15.png)
21. You will see a deployment triggered message on the screen **Product API IN262-000 is triggered for deployment** (where XX or the last two zeros are your participant's number)
br>![](/exercises/ex3/images/ex04-1-16.png)
22. Choose **Switch to API Details**
br>![](/exercises/ex3/images/ex04-1-17.png)
23. Navigate to **Policies** tab and check for **Deployment Status**
br>![](/exercises/ex3/images/ex04-1-18.png)
24. The API should be deployed successfully, and **Deployment Status** should be **Deployed**
br>![](/exercises/ex3/images/ex04-1-19.png)
6. After saving, you will land on the package detail page.
7. Click the **Artifacts** tab.
8. In the **Add MCP Server** wizard, select **HTTP Endpoint with OpenAPI Specification**
9. Click **Add → MCP Server**.
10. 
1. In SAP Integration Suite, go to Design → Integrations and APIs.
   You will see a list of existing Integration Packages on the tenant.
2. Click Create
<br>![](/exercises/ex1/images/01_01_0010.png)

3.	Insert this line of code.
```abap
response->set_text( |Hello World! | ). 
```



## Exercise 1.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc <> 0.
    response->set_status( i_code = 400
                     i_reason = 'Bad request').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex1/images/01_02_0010.png)


## Summary

You've now ...

Continue to - [Exercise 2 - Exercise 2 Description](../ex2/README.md)

