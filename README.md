[Config](#core-profile)
# SQL Based Mock Data Generator

This is a workflow designed to generate mock customer data. It has been designed to generate the source tables that customers would typically provide when setting up a CDP for Implementation/POC. It designed to ensure records will logically link across tables and ultimately tie back to a unified profile. 

It's helpful to **NOT** think about the values being generated as attributes and behaviors rather as as source tables like an email activity table or CRM table. Each column in each table can be configured to generate a variety of different data types (see documentation below for more detail)

## Outputs

**gen_${vertical}** - this the primary output table where an ID Graph is generated as well as mock tables that are configured to be generated. This can be considered an equivalent to the golden `gld_` layer in the CDP data flow. 

**src_${vetical}** - this takes the tables that have been configured to be generated and removes `td_id` - creating the equivalent of a production `src_` layer in the CDP data flow. This can be used to generate subsequent `stg_` and `cdp_unification_` layers if needed. 

## Technical Components 

### Configuration 
The configuration for what data needs to be generated should be passed from another workflow. That workflow will contain the configuration yaml as well as any specific post processing/custom logic is needed for the Demo or Vertical that is being built. 

### Orchestrator 
This is the central workflow that accepts a config from another workflow (`mock_${vertical}`) and orchestrates the necessary tasks below. 

### Core Profile 

This workflow generates a set of new unique TD IDs and core attributes like first name, last name, dob, gender, country. 

All subsequent data will be joined to these TD IDs and the core attributes are single source of truth attributes that will never change or have multiple values. These attributes will be used to derive things like address or email. 

#### Config 
```
vertical: retail
max_profiles_to_create: 5000
```
### ID Graph 

This workflow orchestrates the creation and management of an ID graph. This workflow includes steps to create the ID graph table, select profiles to generate IDs for, and generate single or multiple IDs of various types.

ID Types Supported - Email, Phone, Address & UUID 
 
#### Config 
``` 
ids: 
  - name: Loyalty ID
    type: uuid
    percent_with_id: 20
    percent_multiple: 10
    max_multiple_ids: 3
    index: 4
```



### Table

