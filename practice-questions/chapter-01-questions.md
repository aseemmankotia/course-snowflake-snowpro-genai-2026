## Chapter 1: Gen AI Foundations in the Snowflake Data Cloud — Practice Questions

**Q1.** A retail company wants to let employees search product manuals using natural language queries and retrieve the most semantically relevant passages, without generating new text. Which Snowflake Cortex capability BEST fits this requirement?

A) SNOWFLAKE.CORTEX.COMPLETE with a large context window
B) SNOWFLAKE.CORTEX.SUMMARIZE run on every manual
C) SNOWFLAKE.CORTEX.TRANSLATE to normalize all manuals to English
D) EMBED_TEXT_768 or EMBED_TEXT_1024 combined with vector similarity search

<details>
<summary>Answer</summary>

**Correct: D**

Semantic retrieval requires converting text to vector embeddings and comparing similarity (e.g., VECTOR_COSINE_SIMILARITY); embedding functions are purpose-built for this.

**Why the others are wrong:**
- COMPLETE generates new text output rather than ranking existing passages by similarity — wrong function class for retrieval.
- SUMMARIZE condenses text but does not enable semantic search or ranking by relevance.
- TRANSLATE changes language but has nothing to do with semantic retrieval.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A user sends a very long prompt plus document to SNOWFLAKE.CORTEX.COMPLETE that, combined with the expected response, exceeds the target model's context window. What is the MOST likely outcome?

A) Snowflake automatically upgrades to a larger model at no extra cost
B) The request is rejected or truncated because input+output tokens must fit within the model's context window
C) The function silently ignores the token limit and returns a complete answer regardless of length
D) Cortex converts the prompt to an embedding to bypass the limit

<details>
<summary>Answer</summary>

**Correct: B**

Context window is a hard architectural limit on total tokens (input + output) a model can process in one call; exceeding it causes truncation or an error.

**Why the others are wrong:**
- Snowflake does not silently auto-upgrade models to solve token overflows.
- Token limits are enforced, not silently ignored — this is a core exam distinction.
- Embeddings and completions are separate function types; one cannot substitute for the other to avoid limits.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A financial services company must use generative AI on sensitive customer data but is prohibited from letting that data leave their governed environment or be sent to third-party AI vendors. Which architectural reason makes Snowflake Cortex AI the appropriate choice?

A) Cortex functions always run for free regardless of compute usage
B) Cortex requires exporting data to a third-party LLM provider for processing, which is compliant by default
C) Cortex only supports embeddings, so completions never touch sensitive data
D) Cortex AI functions execute within the Snowflake account boundary, keeping data governed instead of exporting it to external AI services (data gravity)

<details>
<summary>Answer</summary>

**Correct: D**

Data gravity means bringing AI/compute to where data already lives; Cortex functions run inside Snowflake's governed environment, avoiding data movement to external services.

**Why the others are wrong:**
- Cortex functions consume compute credits like any warehouse operation — not free.
- This is the opposite of the actual architecture; exporting data to third parties is what data gravity avoids.
- Cortex supports both embedding and completion functions; both can process sensitive data within the governed boundary.

</details>

---

**Q4.** Which pair of Snowflake Cortex functions correctly matches embedding output dimensionality to function name?

A) EMBED_TEXT_768 → 1024 dimensions, EMBED_TEXT_1024 → 768 dimensions
B) Both functions always return 512 dimensions regardless of name
C) The numeric suffix refers to token limit, not vector dimensionality
D) EMBED_TEXT_768 → 768 dimensions, EMBED_TEXT_1024 → 1024 dimensions

<details>
<summary>Answer</summary>

**Correct: D**

The numeric suffix in the function name directly indicates the length (dimensionality) of the returned embedding vector.

**Why the others are wrong:**
- This reverses the correct mapping.
- Dimensionality is not fixed at 512; it matches the function's numeric suffix.
- The suffix refers to vector dimensionality, not a token limit — a common point of confusion.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** You must store output from EMBED_TEXT_1024 in a Snowflake table column for later similarity search. What data type and dimension must the column be defined with?

A) VARCHAR with no length restriction
B) VECTOR(FLOAT, 768) since Snowflake normalizes all embeddings
C) ARRAY with dynamic length
D) VECTOR(FLOAT, 1024)

<details>
<summary>Answer</summary>

**Correct: D**

Embeddings must be stored in a VECTOR column whose declared dimension matches the embedding function's output size — here, 1024.

**Why the others are wrong:**
- VARCHAR cannot support native vector similarity operations like VECTOR_COSINE_SIMILARITY.
- Snowflake does not normalize embeddings to a different dimension; the column must match the function output exactly.
- ARRAY does not support Snowflake's optimized vector similarity functions.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** For rough token estimation when planning prompt size for a Cortex COMPLETE call, which approximation is the standard industry rule of thumb the exam expects you to apply?

A) 1 token ≈ 1 character
B) 1 token ≈ 4 characters of English text
C) 1 token ≈ 1 full sentence
D) 1 token ≈ 100 words

<details>
<summary>Answer</summary>

**Correct: B**

The widely used heuristic across LLM platforms, including for planning Cortex prompt sizing, is roughly 4 characters of English text per token (or about 0.75 words per token).

**Why the others are wrong:**
- 1 token per character drastically overestimates tokenization, since common words compress into single tokens.
- Sentences vary widely in length and cannot be used as a token unit.
- 100 words per token wildly underestimates tokenization — this ratio would fit millions of words in a small context window.

</details>

---

**Q7.** A company currently uses traditional ML models that require retraining on labeled data for every new task (e.g., separate models for sentiment, translation, and summarization). They want to switch to a single foundation model approach available natively in Snowflake. Which capability reflects the KEY architectural shift they are making?

A) Moving from supervised task-specific models to a single pre-trained foundation model accessed via Cortex functions like COMPLETE, usable across many tasks without retraining
B) Replacing SQL with Python UDFs for all data processing
C) Migrating from Snowflake warehouses to a third-party GPU cluster
D) Switching from batch processing to streaming ingestion

<details>
<summary>Answer</summary>

**Correct: A**

The defining shift from traditional ML to generative AI is using a single large pre-trained foundation model for many tasks (zero/few-shot) instead of training separate task-specific models — exactly what Cortex COMPLETE enables.

**Why the others are wrong:**
- Language choice (SQL vs Python) is unrelated to the ML-to-GenAI architectural shift.
- Cortex runs within Snowflake-managed compute; no external GPU cluster migration is implied.
- Streaming vs batch ingestion is a data pipeline concern, not the ML paradigm shift being tested.

</details>

---

**Q8.** Which statement correctly differentiates an embedding model from a completion (generative) model in the context of Snowflake Cortex?

A) An embedding model converts text into a fixed-length numeric vector representing meaning; a completion model generates new natural language text as output
B) An embedding model generates new natural language text; a completion model returns a fixed-length numeric vector
C) Both model types always return identical output formats in Cortex
D) Embedding models require more tokens of output than completion models

<details>
<summary>Answer</summary>

**Correct: A**

Embedding models (EMBED_TEXT_768/1024) output fixed-length numeric vectors for similarity comparison; completion models (COMPLETE) generate variable-length natural language responses.

**Why the others are wrong:**
- This reverses the correct definitions of embedding vs completion models.
- Output formats differ fundamentally — vector vs text — so they are never identical.
- Embedding output length is fixed by the model (e.g., 768/1024 floats), unrelated to completion output token counts.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

