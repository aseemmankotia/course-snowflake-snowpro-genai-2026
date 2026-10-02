## Chapter 10: Streamlit in Snowflake and Application Orchestration — Practice Questions

**Q1.** A team builds a RAG chatbot using Streamlit in Snowflake. The app calls SNOWFLAKE.CORTEX.COMPLETE() on every user message and queries a vector-enabled table for context. Users report the app feels slow to first load but fast on subsequent Cortex calls in the same session. What is the MOST likely cause?

A) Cortex functions always take longer on the first call per account
B) Streamlit in Snowflake apps do not support warehouse auto-resume
C) The bound warehouse was auto-suspended and had to resume before the first query executed
D) The vector table needs to be re-clustered before every session

<details>
<summary>Answer</summary>

**Correct: C**

Streamlit in Snowflake apps are bound to a query warehouse via QUERY_WAREHOUSE; if that warehouse auto-suspended due to inactivity, the first query pays the resume latency, while subsequent queries in the session hit a running warehouse.

**Why the others are wrong:**
- Cortex functions do not have a mandatory first-call account-wide penalty
- Auto-resume is standard warehouse behavior and is fully supported for SiS-bound warehouses
- Re-clustering is unrelated to session-to-session latency patterns described here

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Which Cortex function should be used to generate the vector embeddings needed to populate a retrieval table for a RAG pipeline in Snowflake?

A) SNOWFLAKE.CORTEX.COMPLETE
B) SNOWFLAKE.CORTEX.EMBED_TEXT_768
C) SNOWFLAKE.CORTEX.SUMMARIZE
D) SNOWFLAKE.CORTEX.EXTRACT_ANSWER

<details>
<summary>Answer</summary>

**Correct: B**

EMBED_TEXT_768 (and EMBED_TEXT_1024) produce vector embeddings from text, which is the required input for populating a vector column used in similarity search for retrieval.

**Why the others are wrong:**
- COMPLETE generates chat/text completions, not embeddings
- SUMMARIZE condenses text but does not produce a vector representation
- EXTRACT_ANSWER performs Q&A extraction over a given passage, not embedding generation

</details>

---

**Q3.** Which SQL statement correctly creates a Streamlit in Snowflake object pointing to an entry file staged in a Snowflake stage and bound to a specific warehouse for compute?

A) CREATE STREAMLIT my_app FROM '@my_stage/app.py' WAREHOUSE = 'MY_WH';
B) CREATE APPLICATION my_app STAGE = '@my_stage' ENTRYPOINT = 'app.py';
C) CREATE STREAMLIT my_app USING TEMPLATE '@my_stage/app.py' WITH WAREHOUSE 'MY_WH';
D) CREATE STREAMLIT my_app ROOT_LOCATION = '@my_stage' MAIN_FILE = 'app.py' QUERY_WAREHOUSE = 'MY_WH';

<details>
<summary>Answer</summary>

**Correct: D**

The correct DDL uses CREATE STREAMLIT with ROOT_LOCATION pointing to the stage, MAIN_FILE naming the entry Python file, and QUERY_WAREHOUSE binding the compute warehouse.

**Why the others are wrong:**
- This syntax invents a nonexistent WAREHOUSE clause and FROM path format not used by CREATE STREAMLIT
- CREATE APPLICATION with STAGE/ENTRYPOINT is not the syntax for Streamlit in Snowflake objects
- USING TEMPLATE and WITH WAREHOUSE are not valid clauses for CREATE STREAMLIT

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A pipeline must run a Cortex SUMMARIZE step automatically only when new rows are inserted into a staging table, and should avoid running (and incurring compute cost) when no new data exists. Which combination of objects achieves this MOST efficiently?

A) A Stream on the staging table combined with a Task whose WHEN clause checks SYSTEM$STREAM_HAS_DATA()
B) A single Task scheduled every minute that always runs SUMMARIZE over the full staging table
C) A Snowpipe that triggers Cortex COMPLETE directly on file load
D) A materialized view refreshed manually before each SUMMARIZE call

<details>
<summary>Answer</summary>

**Correct: A**

A Stream tracks change data on the table, and a Task's WHEN clause using SYSTEM$STREAM_HAS_DATA() lets the task skip execution (avoiding compute cost) when the stream has no unconsumed changes.

**Why the others are wrong:**
- This runs unconditionally on schedule regardless of whether new data exists, wasting compute
- Snowpipe loads files into tables but does not natively trigger Cortex functions on load
- Materialized views are for query acceleration, not conditional pipeline triggering, and don't gate task execution

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** In a Streamlit in Snowflake RAG chatbot, which caching decorator should be used to store a loaded embedding model object across reruns without Streamlit attempting to serialize it?

A) st.cache_data
B) st.cache_resource
C) st.session_state
D) st.experimental_memo

<details>
<summary>Answer</summary>

**Correct: B**

st.cache_resource is designed for non-serializable, shared resources like models or connections, returning the same singleton reference across reruns and sessions.

**Why the others are wrong:**
- st.cache_data serializes and copies return values, which is unsuitable and often fails for model objects
- session_state stores per-session values but does not provide caching/reuse across sessions or handle expensive re-instantiation
- st.experimental_memo is a deprecated alias, not the current recommended API

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A developer wants to add a third-party open-source Python package via pip inside a Streamlit in Snowflake app to handle custom text chunking. What is the key constraint they must account for?

A) Streamlit in Snowflake package installation is limited to packages available in the Snowflake Anaconda channel
B) Streamlit in Snowflake apps can install any PyPI package with no restriction
C) External packages can only be used if the app is converted into a Snowpark Container Service
D) Package installation requires a separate compute pool outside the warehouse

<details>
<summary>Answer</summary>

**Correct: A**

Streamlit in Snowflake apps run in a Snowflake-managed environment and can only add packages from the curated Snowflake Anaconda channel through the package picker; arbitrary pip installs of non-Anaconda packages are not supported.

**Why the others are wrong:**
- Unrestricted pip installs are not supported in the managed SiS runtime
- Converting to Snowpark Container Services is a workaround for full custom environments, but is not required simply to add most open-source packages available via Anaconda
- Package installation is tied to the Anaconda channel restriction, not a separate compute pool

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A company wants other roles within the SAME Snowflake account to access a Streamlit in Snowflake app, without rebuilding it as a Native App. Which action grants this access?

A) SHARE STREAMLIT my_app TO ACCOUNT other_account;
B) ALTER STREAMLIT my_app SET PUBLIC = TRUE;
C) CREATE SHARE my_app_share AS STREAMLIT my_app;
D) GRANT USAGE ON STREAMLIT my_app TO ROLE analyst_role;

<details>
<summary>Answer</summary>

**Correct: D**

Access to a Streamlit in Snowflake object is controlled through standard role-based access control; granting USAGE privilege on the STREAMLIT object to a role allows users with that role to open the app within the same account.

**Why the others are wrong:**
- SHARE is used for cross-account data sharing via Secure Data Sharing, not for granting in-account Streamlit access, and this syntax is not valid
- There is no PUBLIC=TRUE setting for Streamlit objects; access is controlled purely via grants
- CREATE SHARE constructs a data share object for databases/tables, not a mechanism for distributing a Streamlit app

</details>

---

**Q8.** Which statement correctly distinguishes a serverless Task from a user-managed (warehouse-based) Task in Snowflake?

A) Serverless tasks require an explicitly assigned virtual warehouse just like user-managed tasks
B) User-managed tasks cannot be chained into a DAG using AFTER, while serverless tasks can
C) Serverless tasks can only be triggered by Streams, never on a schedule
D) Serverless tasks let Snowflake automatically manage and size compute resources, while user-managed tasks run on a warehouse the user specifies and sizes

<details>
<summary>Answer</summary>

**Correct: D**

Serverless tasks offload compute sizing and scaling to Snowflake-managed resources (configured via parameters like USER_TASK_MANAGED_INITIAL_SIZE), whereas user-managed tasks explicitly bind to a customer-specified virtual warehouse.

**Why the others are wrong:**
- This describes user-managed tasks, not serverless tasks, which explicitly avoid requiring a customer-assigned warehouse
- Both serverless and user-managed tasks support AFTER-based DAG chaining
- Both task types can be scheduled via CRON/interval and both can be gated by stream conditions

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

