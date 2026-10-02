# Answer Key — Practice Test 2

**Q1. Correct: D**

mistral-large2 offers a roughly 128K token context window in Cortex, large enough to hold a ~100 page contract in a single call without chunking, unlike the smaller-context models.

---

**Q2. Correct: C**

EMBED_TEXT_1024 returns a 1024-dimension vector matching the column definition, allowing VECTOR_COSINE_SIMILARITY or VECTOR_L2_DISTANCE to run without a dimension mismatch error.

---

**Q3. Correct: D**

COMPLETE() accepts a free-form prompt where length and style instructions can be embedded directly, giving control over output length that SUMMARIZE() intentionally does not expose.

---

**Q4. Correct: A**

The model's context window is a shared token budget consumed jointly by the input prompt and the generated output; exceeding it causes truncation or an error, regardless of how the tokens are split between input and output.

---

**Q5. Correct: B**

RAG grounds generation in retrieved context, which reduces hallucination risk, but the LLM's generation step remains probabilistic and can still fabricate details not actually present in the retrieved documents — RAG mitigates, it does not eliminate, hallucination.

---

**Q6. Correct: A**

SENTIMENT() is a purpose-built, lightweight Cortex function optimized specifically for sentiment scoring, making it more cost- and latency-efficient at scale than invoking a general-purpose LLM for the same narrow, well-defined task.

---

**Q7. Correct: C**

EMBED_TEXT_768 and EMBED_TEXT_1024 produce vectors of fixed, different dimensionality tied to the specific embedding model. VECTOR_COSINE_SIMILARITY and other vector functions require matching dimensions on both sides; mixing outputs from different embedding models/functions causes a hard error, not a degraded-but-working comparison. Query-time and index-time embeddings must always come from the same embedding model.

---

**Q8. Correct: D**

Cortex LLM functions enforce the selected model's documented context window as a hard input limit. Exceeding it causes an explicit error rather than silent truncation or automatic chunking; the caller is responsible for chunking or summarizing content upstream (e.g., via RAG retrieval or SPLIT_TEXT_RECURSIVE_CHARACTER) to stay within the limit.

---

**Q9. Correct: B**

Cortex AI SQL provides dedicated, task-specific functions (SENTIMENT, TRANSLATE, EXTRACT_ANSWER, CLASSIFY_TEXT, SUMMARIZE) backed by smaller specialized models. These are cheaper and lower-latency than routing the same task through a large general-purpose model via COMPLETE with a custom prompt, making SENTIMENT the correct choice when the task exactly matches an available built-in function.

---

**Q10. Correct: A**

This is a classic unstructured RAG pipeline: parse the PDFs into text, chunk appropriately, embed the chunks, and index them with Cortex Search so users get semantically relevant excerpts with citations back to source documents.

---

**Q11. Correct: B**

SENTIMENT returns a single continuous float score between -1 and 1 representing overall sentiment for the input text, which is exactly the numeric output the dashboard needs.

---

**Q12. Correct: C**

TARGET_LAG behaves like it does for dynamic tables — it defines the maximum acceptable staleness/refresh interval for the underlying index, so a delay of up to the configured lag after a base table load is expected, not an error.

---

**Q13. Correct: C**

Fine-tuning is designed for exactly this scenario: prompt engineering has plateaued, a well-defined output style/format is needed consistently, and sufficient labeled training data exists to teach the model the desired behavior.

---

**Q14. Correct: C**

Cortex Analyst requires a semantic model defined as a YAML file, stored on a Snowflake stage, that describes tables, columns, relationships, synonyms, and verified queries so it can reliably translate natural language into correct SQL.

---

**Q15. Correct: B**

Cortex Fine-Tuning is a serverless, credit-metered operation where cost scales with training data volume and the chosen base model, and only a documented subset of text-generation base models is eligible for fine-tuning.

---

**Q16. Correct: B**

SNOWFLAKE.CORTEX.FINETUNE with the 'CREATE' action is the actual documented function for creating a customized Cortex fine-tuned model, taking the new model name, base model, and SQL queries returning prompt/completion pairs for training and validation — all executed inside Snowflake compute with no data leaving the account.

---

**Q17. Correct: A**

Cortex Analyst is purpose-built for natural-language-to-SQL over structured data using a governed semantic model, producing accurate, explainable, auditable SQL queries and numeric results — exactly the requirement described.

---

**Q18. Correct: C**

TRY_COMPLETE mirrors COMPLETE's full signature, including the response_format parameter for structured JSON output, but returns NULL instead of raising an error when generation fails or output doesn't conform to schema — satisfying both the structured-output and fail-safe requirements in one call.

---

**Q19. Correct: D**

VECTOR_COSINE_SIMILARITY normalizes for vector length by measuring the angle between vectors, so it ranks results based on semantic direction rather than magnitude, which is critical when embeddings come from sources with inconsistent scaling.

---

**Q20. Correct: A**

Cortex Search Service provides managed hybrid search (keyword plus semantic/vector) with automatic embedding generation and incremental refresh based on a configured target lag, eliminating manual index or infrastructure management.

---

**Q21. Correct: A**

SPLIT_TEXT_RECURSIVE_CHARACTER requires overlap to be smaller than chunk_size (overlapping more than the chunk itself is invalid), and the format argument selects whether markdown-aware splitting (headers, code blocks) is applied or plain-text splitting is used.

---

**Q22. Correct: D**

Snowflake's VECTOR data type supports up to 4096 dimensions, which comfortably accommodates common embedding sizes (e.g., 768 or 1024) as well as larger custom embedding models.

---

**Q23. Correct: C**

PARSE_DOCUMENT's LAYOUT mode is designed to preserve document structure — including tables, headers, and reading order — by outputting structured markdown, making it the correct choice when tabular relationships must be retained for RAG.

---

**Q24. Correct: A**

Cortex Search Service is configured with a source text column and an EMBEDDING_MODEL setting; it generates and maintains its own semantic index from raw text, so pre-computed EMBED_TEXT_1024 vectors are not what the service consumes for its internal search index — a common source of confusion.

---

**Q25. Correct: A**

VECTOR(FLOAT, 768) enforces a fixed dimension. EMBED_TEXT_1024 returns a 1024-element vector, which does not match the declared column dimension, so the INSERT raises a dimension mismatch error. To mix embedding models you must use a separate column/table sized for the new dimension or migrate existing data.

---

**Q26. Correct: A**

Cortex Search relies on Snowflake's change tracking metadata (stream-like tracking) on the source table or view to detect inserts, updates, and deletes for incremental refresh. If CHANGE_TRACKING = TRUE is not enabled on the underlying object, the service falls back to requiring full rebuilds instead of automatically syncing new data.

---

**Q27. Correct: C**

Embedding models have a fixed maximum input sequence length; text beyond that limit is truncated before encoding rather than raising an error or being auto-chunked further. When chunk_size is set far larger than the embedding model's token limit, only the beginning of each chunk influences the embedding, so semantically relevant content later in the chunk is effectively invisible to vector search — a common root cause of poor retrieval on long chunks.

---

**Q28. Correct: B**

GPU_NV_M provisions multiple GPUs per node (unlike GPU_NV_S, which has a single A10G with 24 GB), letting an inference framework shard the model's weights across GPUs to exceed the memory ceiling of one GPU without quantization.

---

**Q29. Correct: D**

Outbound network access from an SPCS service to an external endpoint requires a network rule defining the allowed host/port, wrapped in an External Access Integration (EAI), which is then referenced in the service specification so the container is permitted to make the call.

---

**Q30. Correct: B**

readinessProbe defines a health check (e.g., an HTTP GET on a given path/port) that Snowflake uses to determine when a container instance is ready to receive traffic, preventing requests from being routed to a container that is still initializing.

---

**Q31. Correct: A**

EXECUTE JOB SERVICE runs a container as a job-type service designed for run-to-completion batch workloads; once the container process exits, the job service terminates and its compute pool node can be reclaimed automatically, matching the one-time batch requirement.

---

**Q32. Correct: C**

Snowflake supports creating a service function (a UDF defined with SERVICE and ENDPOINT) that routes SQL calls to a specific endpoint of a running SPCS service. A Streamlit in Snowflake app can call this UDF via a SQL query (e.g., through the Snowpark session), which is the officially supported in-account integration pattern.

---

**Q33. Correct: D**

Snowflake Tasks provide native scheduling (CRON or dependency-based) for stored procedures. A stored procedure can call EXECUTE JOB SERVICE to launch the batch container job and, once it completes, run follow-on SQL to update the results table — this is the standard orchestration pattern for scheduled SPCS batch jobs.

---

**Q34. Correct: D**

Custom models requiring GPU inference are deployed via Snowpark Container Services: provision a GPU-enabled compute pool, run the model as a service with an exposed endpoint, and register a service function so SQL/Python callers can invoke it directly inside Snowflake.

---

**Q35. Correct: B**

Outbound network access from Streamlit in Snowflake (or any Snowflake compute) requires a network rule defining the allowed host/port, an external access integration wrapping that rule (plus optional secrets), and then the integration must be attached directly to the Streamlit object via EXTERNAL_ACCESS_INTEGRATIONS so the app's runtime is permitted to call out.

---

**Q36. Correct: C**

Cortex Search Service is a managed offering: once created on a source table with a TARGET_LAG setting, Snowflake automatically tracks changes and incrementally re-embeds and re-indexes new/changed rows without requiring the user to build custom streams, tasks, or scheduled jobs.

---

**Q37. Correct: D**

Cortex LLM functions are serverless and consume credits directly at the account level, independent of any virtual warehouse. Snowflake's Budgets feature is specifically designed to track and alert on this type of serverless/account-level credit consumption, including Cortex, Snowpipe, and other serverless features.

---

**Q38. Correct: B**

Dynamic data masking policies are evaluated at query execution time based on the querying role's masking policy condition, and this evaluation happens before the resulting column value is used as an argument to any downstream function, including Cortex LLM functions. This is the native, low-effort governance control for this exact scenario.

---

**Q39. Correct: C**

Access to Cortex LLM functions is gated specifically by the SNOWFLAKE.CORTEX_USER database role. Once PUBLIC's default grant is revoked, this database role must be explicitly granted to any custom role that needs to invoke Cortex functions.

---

**Q40. Correct: B**

CORTEX_ENABLED_CROSS_REGION is the account parameter that controls whether Cortex function requests may be routed to a different Snowflake region when the requested model isn't hosted locally. Setting it to 'DISABLED' ensures inference never leaves the account's home region, which is the correct control for this data residency requirement.

---

**Q41. Correct: A**

Cortex Guard is enabled by passing 'guardrails': true within the options object of a COMPLETE() call. It uses a safety-classification model to screen generated responses for unsafe content and suppresses or flags output accordingly.

---

**Q42. Correct: A**

Cortex LLM functions run within Snowflake-managed infrastructure, so prompt data, including any sensitive content, is processed inside Snowflake's security and governance perimeter and never transmitted to a third-party endpoint. Calling an external LLM API via an External Access Integration sends request data outside that perimeter to a third-party network location, requiring additional governance controls (e.g., network rules, secrets management, contractual data-handling review) to maintain equivalent protection.

---

**Q43. Correct: B**

CORTEX_MODELS_ALLOWLIST is the account-level parameter specifically designed to restrict which foundation models can be invoked by Cortex LLM functions across the entire account, regardless of caller role.

---

**Q44. Correct: D**

CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY records per-query-id detail of tokens processed and credits consumed for each Cortex LLM function call, enabling precise cost attribution to specific queries, users, or warehouses.

---

**Q45. Correct: D**

Cortex Search Service is designed to honor existing governance controls: row access policies and masking policies defined on the underlying source table are evaluated dynamically based on the privileges of the role executing the search query, so results correctly differ per user.

---

