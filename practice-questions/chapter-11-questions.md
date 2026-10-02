## Chapter 11: Governing, Securing, and Costing Gen AI on Snowflake — Practice Questions

**Q1.** A retail company uses Cortex Analyst on top of a SALES table containing data for all regions. Managers should only see rows for their own region when querying through Cortex Analyst. Which control achieves this with the LEAST additional objects?

A) Create a separate semantic model per region and grant access to the matching one
B) Apply a masking policy to the REGION column
C) Apply a row access policy to the underlying SALES table
D) Create a Cortex Search service scoped per region

<details>
<summary>Answer</summary>

**Correct: C**

Row access policies are enforced at the base table regardless of which layer (SQL, Cortex Analyst, Cortex Search) issues the query, so one policy governs all access paths.

**Why the others are wrong:**
- Duplicating semantic models per region multiplies maintenance and doesn't stop a user with the wrong model assigned from seeing all data if misconfigured
- A masking policy hides/obscures column values, it does not filter which rows are returned
- Cortex Search services are for unstructured/document retrieval, not row-level filtering of structured sales data

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Which grant is required before a role can execute SNOWFLAKE.CORTEX.COMPLETE, TRANSLATE, or SUMMARIZE functions?

A) GRANT ROLE ACCOUNTADMIN TO ROLE <role>
B) GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE <role>
C) GRANT USAGE ON WAREHOUSE <wh> TO ROLE <role>
D) GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE <role>

<details>
<summary>Answer</summary>

**Correct: D**

Cortex LLM functions require the built-in SNOWFLAKE.CORTEX_USER database role to be explicitly granted to a role before that role can call the functions.

**Why the others are wrong:**
- ACCOUNTADMIN is overly broad and not the specific mechanism the exam tests for Cortex access
- IMPORTED PRIVILEGES applies to shared databases, not Cortex function access
- Warehouse USAGE lets the role run queries generally but does not authorize Cortex LLM function calls

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** What is the default value of the CORTEX_ENABLED_CROSS_REGION account parameter?

A) ANY_REGION
B) DISABLED
C) AWS_US_ONLY
D) ENABLED

<details>
<summary>Answer</summary>

**Correct: B**

By default CORTEX_ENABLED_CROSS_REGION is DISABLED, meaning Cortex LLM inference only runs in-region unless an admin explicitly enables cross-region processing.

**Why the others are wrong:**
- ANY_REGION is a value an admin can set, but it is not the out-of-the-box default
- There is no region-specific default value like this
- 'ENABLED' is not a valid parameter value — valid values are DISABLED, ANY_REGION, or a specific region list

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A healthcare company must ensure patient prompt data sent to Cortex LLM functions never leaves their compliance region. What must an admin verify?

A) CORTEX_ENABLED_CROSS_REGION is set to DISABLED or restricted to approved regions only
B) A masking policy is applied to the patient ID column
C) A network policy blocks all outbound internet traffic
D) The virtual warehouse is set to the compliance region

<details>
<summary>Answer</summary>

**Correct: A**

CORTEX_ENABLED_CROSS_REGION directly controls whether the model inference call can be routed to a different geographic region, which is the residency risk for LLM prompt data.

**Why the others are wrong:**
- Masking hides sensitive column values but doesn't control where the LLM inference runs
- Network policies restrict client IP access to Snowflake, not Cortex's internal inference routing
- Warehouse region does not control which region hosts the underlying LLM model used for inference

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team's chatbot uses Cortex COMPLETE with a large flagship model for simple intent classification and is generating unexpectedly high credit costs. What is the BEST cost optimization?

A) Increase the virtual warehouse size to process requests faster
B) Switch to a smaller, lower-cost model that is sufficient for the simple classification task
C) Disable the Cortex Search service
D) Move the workload to a larger warehouse and enable auto-suspend

<details>
<summary>Answer</summary>

**Correct: B**

Cortex LLM function costs are billed per token based on the model used; simple tasks rarely need the largest/most expensive model, so downsizing the model directly reduces token cost.

**Why the others are wrong:**
- Warehouse size has no effect on Cortex LLM function token billing, which is separate from warehouse compute credits
- Cortex Search is unrelated to COMPLETE-based classification cost
- Warehouse sizing/auto-suspend affects warehouse credits, not the token-based LLM billing that is causing the cost

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which Snowflake Cortex feature is specifically designed to filter or flag unsafe/harmful content generated by an LLM function's output?

A) Dynamic Data Masking
B) Row Access Policy
C) Cortex Guard
D) Cortex Search

<details>
<summary>Answer</summary>

**Correct: C**

Cortex Guard applies a safety/content-moderation classification to model outputs, supporting responsible AI requirements around harmful content.

**Why the others are wrong:**
- Dynamic Data Masking obscures stored column values, not generated LLM text
- Row access policies filter table rows, unrelated to output content moderation
- Cortex Search is a retrieval service for search over documents, not an output safety filter

</details>

---

**Q7.** Where should a governance team look to monitor credit and token consumption specifically from Cortex LLM function calls?

A) SNOWFLAKE.ACCOUNT_USAGE.LOGIN_HISTORY
B) SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY
C) SNOWFLAKE.ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY only
D) SNOWFLAKE.ACCOUNT_USAGE.GRANTS_TO_ROLES

<details>
<summary>Answer</summary>

**Correct: B**

CORTEX_FUNCTIONS_USAGE_HISTORY tracks function-level usage and tokens for Cortex LLM calls, which is the correct view for this specific cost monitoring need.

**Why the others are wrong:**
- LOGIN_HISTORY tracks authentication events, not Cortex usage
- WAREHOUSE_METERING_HISTORY only shows warehouse credit usage, and Cortex LLM token billing is separate from warehouse credits
- GRANTS_TO_ROLES shows RBAC assignments, not consumption metrics

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A company must ensure that SSNs are never sent unmasked into a prompt built from a CUSTOMER table for Cortex COMPLETE summarization, regardless of which role runs the query. What is the correct approach?

A) Filter and mask the SSN value in the application after the LLM returns a response
B) Apply a masking policy on the SSN column so it is masked at query time before being assembled into the prompt
C) Rely on a network policy restricting which IPs can call Cortex functions
D) Apply a row access policy on the CUSTOMER table to hide rows containing SSNs

<details>
<summary>Answer</summary>

**Correct: B**

A masking policy on the column enforces protection at query execution time for any caller without an unmasking exemption, ensuring the masked value — not the raw SSN — is what gets concatenated into the LLM prompt.

**Why the others are wrong:**
- Masking after the LLM call is too late — the unmasked SSN has already been sent to the model
- Network policies control client connectivity, not column-level data exposure
- Row access policies hide entire rows, not specific sensitive column values, so non-SSN data in the same row would be lost unnecessarily

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

