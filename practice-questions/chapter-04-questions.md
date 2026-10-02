## Chapter 4: Cortex Search: Building Retrieval and RAG Pipelines — Practice Questions

**Q1.** A support team stores 500,000 troubleshooting articles in a Snowflake table. They need a chatbot that retrieves the most relevant articles using both semantic meaning and exact keyword matches (e.g., error codes like 'ERR-4021'), then feeds the results into an LLM prompt. Which approach BEST meets this requirement?

A) Create a Cortex Search service on the article text column and query it with SEARCH_PREVIEW before calling COMPLETE
B) Create a VECTOR column, generate embeddings manually, and query with VECTOR_COSINE_SIMILARITY only
C) Use SNOWFLAKE.CORTEX.COMPLETE directly on the full table with the user question appended
D) Export the table to an external vector database and query it from the LLM application

<details>
<summary>Answer</summary>

**Correct: A**

Cortex Search natively performs hybrid retrieval (vector + keyword/BM25) in one managed service, which correctly surfaces exact tokens like error codes as well as semantically similar text, and its output is designed to be passed as context into a COMPLETE prompt (RAG pattern).

**Why the others are wrong:**
- Pure vector similarity misses exact keyword matches like specific error codes, which lexical/BM25 scoring handles better
- COMPLETE has no retrieval mechanism over a large table and cannot ground answers on 500,000 rows directly
- Exporting to an external vector database adds unnecessary operational overhead when Cortex Search already provides managed hybrid retrieval in-platform

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** You run CREATE CORTEX SEARCH SERVICE support_svc ON description ATTRIBUTES product, severity WAREHOUSE = WH_XS TARGET_LAG = '1 hour' AS SELECT description, product, severity FROM tickets. What does the ATTRIBUTES clause enable?

A) It specifies columns available for filtering search results, not for embedding
B) It defines which columns are embedded into the vector index
C) It sets the refresh cadence for those specific columns only
D) It designates the columns returned in the JSON response, replacing the need for a SELECT list

<details>
<summary>Answer</summary>

**Correct: A**

ATTRIBUTES designates metadata columns that can be used as filters in search queries; only the ON column is embedded/searched semantically and lexically.

**Why the others are wrong:**
- Only the column named in ON is embedded/indexed for search, not the ATTRIBUTES columns
- TARGET_LAG controls overall service refresh freshness, not per-column, and ATTRIBUTES is unrelated to refresh scheduling
- The returned columns are controlled by the columns parameter in the search request/query, not solely by ATTRIBUTES

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A company's Cortex Search service is built on a base table that receives new rows every few seconds via a streaming pipeline. Search results must reflect new tickets within 5 minutes for an SLA. Which configuration decision is most important?

A) Increase the warehouse size to XXLARGE to speed up query execution
B) Rebuild the entire service manually every 5 minutes using DROP and CREATE
C) Set TARGET_LAG to a value at or below 5 minutes so the service refreshes frequently enough
D) Add more ATTRIBUTES columns so filtering compensates for stale data

<details>
<summary>Answer</summary>

**Correct: C**

TARGET_LAG directly controls how fresh (up to date) the search service's index is relative to the base table, similar to dynamic tables; setting it at or below the SLA threshold ensures data freshness requirements are met.

**Why the others are wrong:**
- Warehouse size affects query/refresh compute performance, not the freshness target itself
- Manually dropping/recreating the service is unnecessary, expensive, and unsupported as a freshness mechanism — TARGET_LAG automates this
- Adding attribute columns affects filtering capability, not data freshness

</details>

---

**Q4.** Which statement correctly differentiates Cortex Search from a manually built vector similarity search using a VECTOR column and VECTOR_COSINE_SIMILARITY?

A) Cortex Search only supports keyword/lexical matching, while manual VECTOR columns support semantic matching
B) Cortex Search automatically manages embedding generation, indexing, and combines semantic with lexical (hybrid) relevance out of the box; manual VECTOR columns require you to generate embeddings and implement retrieval logic yourself
C) Manual VECTOR columns automatically refresh on a TARGET_LAG schedule, whereas Cortex Search requires manual re-embedding
D) Cortex Search and manual VECTOR_COSINE_SIMILARITY queries are functionally identical with no operational difference

<details>
<summary>Answer</summary>

**Correct: B**

Cortex Search is a managed retrieval service that handles embedding generation, index maintenance, and hybrid (vector + keyword) scoring automatically, reducing the operational work needed compared to manually building and maintaining a vector similarity pipeline.

**Why the others are wrong:**
- Cortex Search supports both semantic and lexical matching (hybrid), not just keyword matching
- TARGET_LAG-based automatic refresh is a Cortex Search feature; manual VECTOR columns have no built-in refresh scheduling
- There are meaningful operational differences — Cortex Search removes the need to manage embeddings and index updates manually

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A developer wants to retrieve the top 5 most relevant document chunks for a user query and restrict results to only documents where department = 'Finance'. Which is the correct way to achieve this with Cortex Search?

A) Add a WHERE clause after calling SEARCH_PREVIEW since it behaves like a standard SQL table function
B) Create a separate Cortex Search service for each department value
C) Filter the results client-side in the application after retrieving all matches
D) Pass a filter object (e.g., specifying department = 'Finance') and a limit of 5 in the JSON request to SEARCH_PREVIEW / the search API

<details>
<summary>Answer</summary>

**Correct: D**

Cortex Search queries accept a structured request (JSON) that includes a filter on ATTRIBUTES columns and a limit parameter, allowing attribute-based filtering combined with relevance ranking in a single call.

**Why the others are wrong:**
- SEARCH_PREVIEW does not expose a queryable result set for a trailing WHERE clause the way a normal table does; filtering must be passed in the request payload
- Creating a separate service per attribute value is unnecessary and does not scale as attribute values grow
- Client-side filtering after retrieval wastes retrieval budget and may exclude relevant top-k results before filtering, unlike native filter push-down

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** In a RAG pipeline using Cortex Search, what is the correct high-level flow to answer a user's natural language question?

A) Call COMPLETE with only the user's question, then use Cortex Search to fact-check the answer afterward
B) Call Cortex Search and COMPLETE independently and simply concatenate their outputs without passing context between them
C) Query Cortex Search with the user's question to retrieve relevant context, then include that context plus the question in a prompt passed to COMPLETE
D) Use Cortex Search to generate the final natural language answer directly, bypassing COMPLETE

<details>
<summary>Answer</summary>

**Correct: C**

The standard RAG pattern retrieves relevant context first via Cortex Search, then constructs a prompt that includes both the retrieved context and the original question, which is passed to COMPLETE to generate a grounded answer.

**Why the others are wrong:**
- Calling COMPLETE first without retrieved context defeats the purpose of grounding the answer in retrieved documents
- Running them independently without wiring the retrieved context into the COMPLETE prompt fails to ground the LLM response
- Cortex Search returns ranked search results/passages, not a generated natural language answer — that generation step is COMPLETE's role

</details>

---

**Q7.** A team notices that Cortex Search queries are returning results with high query latency during peak hours. Which action is most appropriate to improve query performance?

A) Reduce the TARGET_LAG value to refresh more frequently
B) Remove all ATTRIBUTES columns to reduce filter overhead
C) Increase the size of, or scale out, the warehouse associated with the search service to handle higher query concurrency/load
D) Switch the base table to a view instead of a table

<details>
<summary>Answer</summary>

**Correct: C**

Query and refresh performance for Cortex Search is driven by the compute (warehouse) backing the service; scaling the warehouse addresses throughput/latency issues during high concurrency.

**Why the others are wrong:**
- Lowering TARGET_LAG increases refresh frequency (freshness), which does not address query-time latency and can even increase compute contention
- Removing ATTRIBUTES columns limits filtering capability but is not the primary lever for query latency at scale
- Basing the service on a view vs. a table does not directly resolve compute-driven latency issues

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which of the following BEST describes how Cortex Search combines semantic and lexical signals when ranking results, as tested on the exam?

A) It runs two completely separate searches and lets the application developer manually merge and re-rank the two result sets
B) It performs hybrid search internally, blending vector similarity (semantic) and keyword-based (lexical) relevance into a single ranked result set automatically
C) It only uses lexical/keyword matching and does not perform any embedding-based similarity
D) It requires the developer to explicitly choose either 'vector mode' or 'keyword mode' per query, never both simultaneously

<details>
<summary>Answer</summary>

**Correct: B**

Cortex Search's hybrid retrieval combines semantic (vector) and lexical (keyword) relevance signals automatically within the service, returning a single blended ranking without requiring manual merging.

**Why the others are wrong:**
- There is no need for the developer to manually merge separate result sets — hybrid scoring happens within the service
- Cortex Search does perform embedding-based semantic search in addition to lexical matching
- Developers do not need to choose exclusively between modes; hybrid scoring is applied by default

</details>

---

