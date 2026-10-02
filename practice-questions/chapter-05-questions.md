## Chapter 5: Cortex Analyst: Natural-Language Analytics on Structured Data — Practice Questions

**Q1.** A retail company stores sales transactions in a star-schema of fact and dimension tables in Snowflake. Business users want to ask questions like 'What was total revenue by region last quarter?' in plain English and get an accurate SQL-backed answer. Which Snowflake Cortex feature BEST meets this requirement?

A) Cortex Search, indexed over the sales table columns
B) A fine-tuned LLM using COMPLETE() with the raw table schema in the prompt
C) Cortex Analyst, using a semantic model built on the fact and dimension tables
D) Document AI applied to exported CSV files of the sales data

<details>
<summary>Answer</summary>

**Correct: C**

Cortex Analyst is purpose-built for text-to-SQL over structured, tabular data using a semantic model that defines tables, dimensions, and measures — exactly this scenario.

**Why the others are wrong:**
- Cortex Search is designed for retrieval over unstructured/semi-structured text via semantic/hybrid search, not for generating aggregate SQL over structured tables.
- Manually prompting COMPLETE() with raw schema skips the governed semantic layer, verified queries, and accuracy tuning that Cortex Analyst provides, and is not a supported pattern for reliable analytics.
- Document AI extracts structured data from documents (PDFs, forms) — irrelevant here since the data is already structured in tables.

</details>

---

**Q2.** A support organization has thousands of unstructured PDF knowledge-base articles and wants a chatbot that answers free-text troubleshooting questions by retrieving relevant passages. Which Cortex feature should be used?

A) Cortex Analyst with a semantic model over the article metadata table
B) Cortex Search configured on the extracted article text
C) Cortex Analyst verified queries covering common troubleshooting questions
D) A materialized view joining articles to a metrics table

<details>
<summary>Answer</summary>

**Correct: B**

Cortex Search performs semantic/hybrid search retrieval over unstructured text content, making it the right tool for RAG-style Q&A over documents.

**Why the others are wrong:**
- Cortex Analyst generates SQL against structured tables — it cannot search free text inside PDF content for relevant passages.
- Verified queries only improve SQL generation accuracy for a semantic model; they do not enable retrieval over unstructured document text.
- A materialized view is a structured data construct and does nothing to enable semantic search of unstructured text.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An analyst builds a semantic model joining 9 tables with multiple nested subqueries and window functions to answer a single business question. End users report that Cortex Analyst frequently returns incorrect or incomplete SQL for this question. What is the MOST likely cause and best fix?

A) Cortex Analyst has a hard limit of 3 tables and the model must be redesigned around a single table
B) Cortex Analyst cannot use synonyms, so all column names must exactly match the question wording
C) Switch to Cortex Search since it handles complex joins better than Cortex Analyst
D) The semantic model's complexity exceeds what Cortex Analyst reliably handles; simplify to a star-schema-like model and add a verified query for this exact question

<details>
<summary>Answer</summary>

**Correct: D**

Cortex Analyst accuracy degrades with highly complex multi-table joins, nested subqueries, and window-function logic; the documented mitigation is simplifying the semantic model toward a flatter/star-schema design and adding a verified query so the exact pattern is matched reliably.

**Why the others are wrong:**
- There is no strict 3-table hard limit stated for Cortex Analyst — the issue is complexity of joins/aggregations, not a fixed table count.
- Synonyms are a supported and encouraged feature of the semantic model specifically to map natural-language phrasing to columns; they are not disabled by complexity.
- Cortex Search is for unstructured text retrieval, not for executing complex structured joins/aggregations — it does not solve this problem at all.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A development team is building a chatbot and needs to embed Cortex Analyst so it can be called from a custom web application (not Snowsight). Which integration approach is correct?

A) Export the semantic model as a Streamlit app only — no external API access is possible
B) Call the Cortex Analyst REST API message endpoint, passing the semantic model reference and the user's question
C) Embed the raw table schema as a system prompt to the COMPLETE() function instead of using Cortex Analyst
D) Use Cortex Search's REST API since Cortex Analyst has no API

<details>
<summary>Answer</summary>

**Correct: B**

Cortex Analyst exposes a REST API endpoint that accepts the semantic model and a natural-language message, returning generated SQL and/or results, enabling integration into custom apps and chatbots.

**Why the others are wrong:**
- Streamlit-in-Snowflake is one supported UI pattern but is not the only way to integrate — the REST API is the mechanism for external apps.
- Bypassing Cortex Analyst with raw COMPLETE() prompting loses the semantic model, verified queries, and accuracy tuning that make text-to-SQL reliable.
- Cortex Search and Cortex Analyst are distinct services with separate APIs; Cortex Analyst does have its own REST API.

</details>

---

**Q5.** In a Cortex Analyst semantic model YAML file, which section is used to store curated question-and-SQL pairs that Cortex Analyst matches against before generating new SQL, in order to improve accuracy for common questions?

A) synonyms
B) relationships
C) verified_queries
D) time_dimensions

<details>
<summary>Answer</summary>

**Correct: C**

The verified_queries section stores curated, validated natural-language question and SQL pairs (the Verified Query Repository) that Cortex Analyst prioritizes to improve accuracy on frequently asked or previously problematic questions.

**Why the others are wrong:**
- synonyms maps alternate natural-language terms to specific columns/tables, not full question-SQL pairs.
- relationships defines join paths between logical tables, not curated Q&A examples.
- time_dimensions defines date/time columns used for time-based filtering and grouping, unrelated to storing verified questions.

</details>

---

**Q6.** A company needs an assistant that can answer both 'What was our Q3 revenue by product line?' (from structured tables) and 'What do customers say about product durability?' (from unstructured review text). What is the recommended architecture?

A) Use Cortex Analyst alone with unstructured text loaded into a VARCHAR column
B) Use Cortex Search alone and aggregate results manually into SQL-like summaries
C) Combine Cortex Analyst for the structured query path and Cortex Search for the unstructured query path, routed by an orchestrating agent
D) Use a single semantic model that contains both dimensions/measures and a full-text search index

<details>
<summary>Answer</summary>

**Correct: C**

Cortex Analyst and Cortex Search solve different problems — structured text-to-SQL versus unstructured semantic retrieval — so the correct pattern is combining both, typically orchestrated by an agent that routes each question to the appropriate tool.

**Why the others are wrong:**
- Cortex Analyst generates SQL for structured aggregation/filtering; it is not designed to semantically search free text stored in a column.
- Cortex Search retrieves relevant passages but does not perform reliable numeric aggregation like revenue totals — manual aggregation defeats the purpose of automation.
- A single semantic model YAML is specific to Cortex Analyst's structured schema definition and does not incorporate Cortex Search's indexing configuration.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which of the following is a key architectural difference between Cortex Analyst and Cortex Search regarding data freshness?

A) Cortex Analyst always returns stale, cached results while Cortex Search always queries live data
B) Both services require identical indexing pipelines with the same refresh lag settings
C) Cortex Search executes SQL live and Cortex Analyst uses a pre-built vector index
D) Cortex Analyst generates SQL executed live against the underlying tables, while Cortex Search relies on a search service index that refreshes on a configurable lag

<details>
<summary>Answer</summary>

**Correct: D**

Cortex Analyst produces SQL that is executed directly against the source tables, so results reflect current data, whereas Cortex Search maintains a search service with an index that is refreshed based on a configured lag, introducing potential staleness.

**Why the others are wrong:**
- Cortex Analyst does not use a stale cache — it executes generated SQL live against current table data.
- Cortex Search requires an explicit index/service configuration with a refresh lag; Cortex Analyst has no such indexing pipeline since it queries tables directly.
- This reverses the actual roles: Cortex Search uses the index/vector approach, and Cortex Analyst uses live SQL execution, not the other way around.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** In a Cortex Analyst semantic model, what is the primary distinction between a 'dimension' and a 'measure'?

A) Dimensions are always numeric aggregates, while measures are always text categories
B) Dimensions are used for grouping/filtering (e.g., region, category), while measures are numeric values meant to be aggregated (e.g., revenue, count)
C) There is no functional difference — the terms are interchangeable in the YAML schema
D) Dimensions can only exist in fact tables, and measures can only exist in dimension tables

<details>
<summary>Answer</summary>

**Correct: B**

In a semantic model, dimensions represent categorical/descriptive attributes used for grouping and filtering, while measures represent numeric facts intended for aggregation (sum, avg, count), mirroring standard data-warehouse modeling concepts.

**Why the others are wrong:**
- This reverses the definitions — dimensions are typically categorical, not numeric aggregates; measures are the numeric aggregates.
- The YAML schema explicitly distinguishes dimensions and measures/facts as separate typed elements with different roles in query generation.
- Dimensions and measures can appear in either logical table type in the semantic model; the restriction described does not exist.

</details>

---

