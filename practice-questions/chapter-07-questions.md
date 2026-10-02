## Chapter 7: Embeddings and Vector Search on Snowflake — Practice Questions

**Q1.** A team is building a RAG pipeline in Snowflake. They need to embed 5 million support tickets and later run semantic search filtered by ticket status. Which approach BEST meets this requirement?

A) Store embeddings in a VECTOR column alongside the STATUS column in the same table, then filter with WHERE status = ... combined with a distance function in ORDER BY
B) Store embeddings as VARCHAR JSON arrays and parse them at query time with a UDF
C) Export embeddings to an external vector database and join back to Snowflake for filtering
D) Compute embeddings on the fly for every query instead of storing them

<details>
<summary>Answer</summary>

**Correct: A**

Storing the VECTOR column in the same table as filterable metadata lets Snowflake prune rows with the WHERE clause before computing distance, which is the recommended pattern for combining SQL filters with vector search.

**Why the others are wrong:**
- VARCHAR JSON storage loses native VECTOR type optimizations and requires manual parsing, adding cost and complexity
- Round-tripping to an external vector DB adds latency and network cost with no benefit since Snowflake supports VECTOR natively
- Recomputing embeddings per query wastes Cortex credits and adds unnecessary latency

</details>

---

**Q2.** A developer generates document embeddings with SNOWFLAKE.CORTEX.EMBED_TEXT_768 and query embeddings with the same function. They then rank results using VECTOR_L2_DISTANCE. Results seem inconsistent with expected relevance ranking. What is the MOST likely cause?

A) L2 distance cannot be used with 768-dimensional vectors
B) EMBED_TEXT_768 does not support query text, only documents
C) VECTOR_L2_DISTANCE only works on INT vector types
D) The embeddings were not normalized, so cosine similarity (VECTOR_COSINE_SIMILARITY) would give more consistent relevance ranking than raw L2 distance

<details>
<summary>Answer</summary>

**Correct: D**

For text embedding models, cosine similarity is typically preferred because it measures direction (semantic similarity) rather than magnitude, which is more robust to variations in vector length; L2 distance can be skewed by magnitude differences.

**Why the others are wrong:**
- L2 distance works fine with any dimensionality including 768
- EMBED_TEXT_768 embeds any input text, queries and documents alike
- VECTOR_L2_DISTANCE works on both FLOAT and INT vector element types

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A table has 200 million rows with a VECTOR(FLOAT, 1024) column. A query needs the top 10 nearest neighbors to a query vector, filtered to only rows where region = 'US'. Which query structure is MOST efficient?

A) Apply the vector distance function to all 200 million rows first, then filter by region in an outer query
B) Run two separate queries and manually intersect the results in the application layer
C) Filter by region = 'US' in the WHERE clause first, then order by the distance function and LIMIT 10
D) Materialize all distances into a temporary table before filtering

<details>
<summary>Answer</summary>

**Correct: C**

Filtering on a selective SQL predicate before computing distance reduces the number of rows the distance function must scan, since Snowflake vector search is not index-accelerated ANN by default and benefits heavily from predicate pushdown/pruning.

**Why the others are wrong:**
- Computing distance over all rows first wastes compute on rows that will be discarded anyway
- Manual intersection in the application layer adds latency and loses SQL optimizer benefits
- Materializing all distances into a temp table computes unnecessary work before filtering

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A company wants to reduce storage costs and query latency for a vector search feature while keeping acceptable semantic quality. Which choice BEST achieves this?

A) Always use the highest-dimensional embedding model available regardless of the use case
B) Store embeddings as FLOAT64 arrays instead of the VECTOR type
C) Avoid using SNOWFLAKE.CORTEX.EMBED_TEXT and compute embeddings outside Snowflake for every table
D) Choose a lower-dimensional embedding model appropriate for the task (e.g., 768-dim instead of 1024-dim) when the accuracy tradeoff is acceptable

<details>
<summary>Answer</summary>

**Correct: D**

Lower embedding dimensionality reduces storage size and distance-computation cost; choosing dimensionality based on the task's accuracy requirements is the standard tradeoff the exam expects.

**Why the others are wrong:**
- Higher dimensionality always increases storage and compute cost, not appropriate if not needed
- The VECTOR type is specifically optimized for storage/compute of embeddings versus generic array types
- Moving computation outside Snowflake adds data movement cost and loses native integration benefits

</details>

---

**Q5.** Which SQL function call correctly generates a 1024-dimension embedding for the string stored in column DOC_TEXT using Cortex?

A) SNOWFLAKE.CORTEX.EMBED_TEXT_768(DOC_TEXT, 'e5-base-v2')
B) SNOWFLAKE.CORTEX.EMBED_TEXT_1024('snowflake-arctic-embed-l-v2.0', DOC_TEXT)
C) SNOWFLAKE.CORTEX.VECTOR_EMBED(DOC_TEXT, 1024)
D) SNOWFLAKE.CORTEX.EMBED('snowflake-arctic-embed-l-v2.0', DOC_TEXT, 1024)

<details>
<summary>Answer</summary>

**Correct: B**

EMBED_TEXT_1024 takes the model name as the first argument and the text as the second, and returns a VECTOR(FLOAT, 1024); this matches a valid 1024-dim embedding model like snowflake-arctic-embed-l-v2.0.

**Why the others are wrong:**
- EMBED_TEXT_768 with e5-base-v2 produces a 768-dim vector, not 1024, and the argument order is reversed (model first, text second)
- VECTOR_EMBED is not a real Cortex function name
- EMBED is not the correct function name; the dimension is fixed by function suffix (768 or 1024), not passed as a parameter

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** An architect needs to select the correct function to rank stored document vectors by similarity to a query vector, where higher output values should mean more similar. Which function should be used?

A) VECTOR_L2_DISTANCE
B) VECTOR_COSINE_SIMILARITY
C) VECTOR_INNER_PRODUCT with descending L2 sort
D) APPROX_TOP_K

<details>
<summary>Answer</summary>

**Correct: B**

VECTOR_COSINE_SIMILARITY returns a value between -1 and 1 where higher values indicate greater similarity, directly matching the 'higher = more similar' requirement.

**Why the others are wrong:**
- VECTOR_L2_DISTANCE returns a distance where LOWER values mean more similar, the opposite semantic
- Mixing inner product with an L2-based sort direction is inconsistent and not a standard pattern
- APPROX_TOP_K is not a Snowflake vector similarity function

</details>

---

**Q7.** Which column definition correctly creates a table column to store 768-dimension floating point embeddings in Snowflake?

A) EMBEDDING ARRAY(FLOAT, 768)
B) EMBEDDING VECTOR(FLOAT, 768)
C) EMBEDDING FLOAT[768]
D) EMBEDDING VARIANT(768)

<details>
<summary>Answer</summary>

**Correct: B**

Snowflake's VECTOR data type syntax is VECTOR(<element_type>, <dimension>), e.g., VECTOR(FLOAT, 768), which is the required syntax for storing and querying embeddings with distance functions.

**Why the others are wrong:**
- ARRAY(FLOAT, 768) is not valid Snowflake ARRAY syntax and ARRAY types don't support native distance functions
- FLOAT[768] is not valid Snowflake column type syntax
- VARIANT does not accept a dimension parameter and lacks native vector distance function support

</details>

---

**Q8.** What does a text embedding numerically represent?

A) A compressed version of the original text that can be losslessly decoded back to the source string
B) A hash of the text used only for deduplication
C) A probability distribution over the next token in a sequence
D) A fixed-length vector of floating point numbers representing the semantic meaning of the text in a high-dimensional space

<details>
<summary>Answer</summary>

**Correct: D**

Embeddings map text into a fixed-length dense vector where geometric proximity (via distance/similarity functions) approximates semantic similarity between texts.

**Why the others are wrong:**
- Embeddings are not designed for lossless decoding back to original text; that's not their purpose
- A hash is designed for exact-match deduplication, not semantic similarity comparisons
- A next-token probability distribution describes generative model output, not an embedding vector

</details>

---

