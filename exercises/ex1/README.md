# Exercise 4: Create MCP Server & Expose the API
**AI Feature:** Build MCP for S4/HANA Product Creation API

**Duration:** 20 min

**Business Context:**
Enable AI agents to securely and efficiently create products in SAP S/4HANA by exposing the S/4HANA Product Creation API as an MCP tool. This allows agents to interact with product master data through a governed, standardized interface, reducing manual effort and enabling automated, end-to-end business processes.

**What AI Does Here:**
Create an MCP Server to expose the S/4HANA product creation API as an agent-consumable tool

In this exercise, you will build and configure an MCP Server that exposes the S/4HANA Product API as a tool in SAP Integration Suite, enabling AI agents to automate s.. securely and with proper governance, without compromising business context. 

Note-For a deeper understanding of the Model Context Protocol (MCP) and MCP paradigm in SAP Integration Suite, refer to What is the Model Context Protocol (MCP)? and API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP

## Exercise 4.1 Create an Integration Package

After completing these steps you will have created...

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

