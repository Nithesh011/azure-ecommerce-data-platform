# 🧩 Challenges & Solutions

This document captures the technical issues encountered while building the Azure E-Commerce Data Platform and the steps taken to resolve them.  
The goal is to help anyone implementing a similar architecture avoid these issues.

### 🔹 1. Event Hub Unauthorized (401) Error

**Issue**  
Logic App and Synapse Streaming could not send/read messages from Event Hub.

**Root Cause**  
Access policy was created at the Namespace level instead of the Event Hub entity level.

**Solution**  
Created access policies directly under:

Event Hub → Shared Access Policies


Assigned:

- Logic App → Send
- Synapse → Listen

After updating policies, both sender and consumer were authenticated correctly.

### 🔹 2. Synapse Notebook Not Visible in ADF Linked Service

**Issue**  
ADF could not list the notebooks from Synapse Studio.

**Root Cause**  
ADF’s managed identity did not have required permissions inside Synapse.

**Solution**  
Assigned these roles to ADF’s service principal:

- Synapse Administrator
- Synapse Apache Spark Administrator

Notebooks became visible after role propagation.

### 🔹 3. ADF Copy Failing with ADLS Forbidden Error

**Error**

AuthorizationPermissionMismatch


**Root Cause**  
ADF lacked permissions to write to ADLS.

**Solution**  
Assigned roles at storage account level:

- Storage Blob Data Contributor
- Storage Blob Data Reader

The copy activity succeeded afterward.

### 🔹 4. Logic App Parse JSON Failures

**Issue**  
JSON parsing failed due to nested object arrays in the FakeStore API response.

**Root Cause**  
Incorrect or incomplete schema definition.

**Solution**  
Retrieved actual API response → generated schema → updated Parse JSON step.

### 🔹 5. Streaming Notebook Not Writing to Bronze Layer

**Issue**  
Streaming job started but no files appeared in ADLS.

**Root Cause**  
Checkpoint folder path was incorrect.

**Solution**  
Created folders:

/bronze/fakestore/stream/
/bronze/fakestore/checkpoints/stream/


Job began writing Parquet files properly.

### 🔹 6. Join Failures in Silver Layer Transformations

**Issue**  
Joins between orders, products, and users returned empty results.

**Root Cause**  
Data type mismatches (e.g., productId long vs int).

**Solution**  
Enforced consistent schema for join keys:

cast("product_id", "int")


Joins worked as expected.

### 🔹 7. Notebook Execution Order Issues During Orchestration

**Issue**  
Silver-layer notebooks ran before Bronze-layer processing completed.

**Solution**  
Created an ADF pipeline with sequential activities:
