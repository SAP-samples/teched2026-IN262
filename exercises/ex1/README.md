# Exercise 4: Create MCP Server & Expose the API
**AI Feature:** Build MCP for S4/HANA Product Creation API

**Duration:** 20 min

**Business Context:**
Enable AI agents to securely and efficiently create products in SAP S/4HANA by exposing the S/4HANA Product Creation API as an MCP tool. This allows agents to interact with product master data through a governed, standardized interface, reducing manual effort and enabling automated, end-to-end business processes.

**What AI Does Here:**
Create an MCP Server to expose the S/4HANA product creation API as an agent-consumable tool

In this exercise, you will build and configure an MCP Server that exposes the S/4HANA Product API as a tool in SAP Integration Suite, enabling AI agents to automate s.. securely and with proper governance, without compromising business context. 

> [!NOTE]
> For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP paradigm in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP](https://community.sap.com/t5/technology-blog-posts-by-sap/api-centric-integration-in-sap-integration-suite-a-new-paradigm-for-api-amp/ba-p/14438245)
## Exercise 4.1 Create an Integration Package
1. In SAP Integration Suite, go to **Design → Integrations and APIs**.
<br>You will see a list of existing Integration Packages on the tenant.

3. Click **Create**
   
4. Fill in the following integration package details:

| Field | Value |
|---|---|
| Name | `plant-supply-risk-package-XX` (replace XX with your assigned participant number) |
| Technical Name | *(auto-populated)* |
| Short Description | `This package is created to create MCP artifacts using the OpenAPI specification & API composed URL from the plant supply risk graph` |
| Version | `1.0.0` |
| Vendor | `SAP` |

5. Click **Save**
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

