# Answer Key — Practice Test 1

**Q1. Correct: D**

CLASSIFY_TEXT is the purpose-built Cortex function for zero-shot text classification: you supply the text and a list of candidate labels and it returns the best-matching category, with no prompt engineering or training required.

---

**Q2. Correct: C**

Context window limits are a hard token-count constraint that vary by model (e.g., some Cortex-hosted models support far larger windows than others). The correct fix is either choosing a model with a larger window or reducing the effective input size via chunking, summarization, or retrieval (e.g., Cortex Search) before generation.

---

**Q3. Correct: C**

EMBED_TEXT_1024 generates 1024-dimensional embedding vectors using a supported embedding model, matching the dimensionality needed for the VECTOR column and distance comparison described.

---

**Q4. Correct: D**

TRANSLATE is the dedicated Cortex function for machine translation, accepting source text along with source and target language codes and returning translated text with a simple, low-overhead call.

---

**Q5. Correct: B**

Temperature controls the randomness of token selection during generation. Setting it near 0 makes the model favor the highest-probability tokens consistently, producing more deterministic, repeatable output — important for compliance-style use cases tested on the exam.

---

**Q6. Correct: D**

An embedding is a dense numeric vector produced by an embedding model such that texts with similar meaning are positioned close together in vector space, enabling semantic search and similarity comparisons using distance metrics like cosine similarity.

---

**Q7. Correct: C**

EMBED_TEXT_1024 returns a 1024-dimension embedding vector, which must be stored in a VECTOR(FLOAT, 1024) column so the dimensionality matches for downstream similarity functions like VECTOR_COSINE_SIMILARITY.

---

**Q8. Correct: A**

Temperature controls the randomness of token selection during generation. Lowering temperature toward 0 makes the model favor the highest-probability tokens, producing more deterministic and repeatable output for the same prompt.

---

**Q9. Correct: A**

Cortex LLM functions such as COMPLETE are billed based on tokens processed — the combined input prompt and output response — with per-token (per-million-token) credit rates that differ depending on which model is selected.

---

**Q10. Correct: B**

Cortex Analyst is purpose-built for natural-language-to-SQL over structured data. It uses a semantic model (mapping business terms, synonyms, dimensions, and measures to real tables/columns) to reliably generate governed, accurate SQL from plain-English questions.

---

**Q11. Correct: C**

Cortex Search is Snowflake's managed hybrid search service purpose-built for RAG — it indexes unstructured text/document content and returns the most relevant passages to feed into an LLM prompt for grounded answers.

---

**Q12. Correct: B**

SENTIMENT is a built-in Cortex LLM function that returns a numeric polarity score (negative to positive) for a piece of text, exactly matching the requirement with no custom model needed.

---

**Q13. Correct: B**

TARGET_LAG, analogous to its use in dynamic tables, defines the maximum acceptable staleness before Cortex Search re-indexes the base table's changes into the search service.

---

**Q14. Correct: C**

Cortex Analyst requires a semantic model (a YAML specification) that maps business-friendly names, synonyms, dimensions, and measures to the underlying tables and columns, which is what enables it to generate accurate, grounded SQL.

---

**Q15. Correct: A**

Cortex Fine-Tuning lets you train a smaller/cheaper base model on labeled examples for a fixed, stable label set, eliminating the need for a long few-shot prompt on every call and significantly cutting token cost and latency at high volume.

---

**Q16. Correct: D**

Cortex Analyst is purpose-built to translate natural-language business questions into governed SQL against structured tables, using a semantic model (YAML) that defines tables, columns, relationships, and business terminology.

---

**Q17. Correct: B**

The correct FINETUNE signature requires the operation keyword 'CREATE', the new model name, the base model name, and a SELECT statement supplying the training data (prompt/completion columns).

---

**Q18. Correct: D**

EXTRACT_ANSWER is designed to take a question and a text passage and return the specific answer span found in that text, rather than producing a general summary or open-ended generation.

---

**Q19. Correct: A**

VECTOR_COSINE_SIMILARITY returns a value between -1 and 1, where a higher value indicates greater similarity, making it directly usable for ORDER BY ... DESC ranking in retrieval.

---

**Q20. Correct: B**

SPLIT_TEXT_RECURSIVE_CHARACTER is the Cortex function designed to chunk text into smaller passages with configurable chunk size and overlap, which is exactly what's needed before embedding for retrieval.

---

**Q21. Correct: A**

TARGET_LAG defines the freshness SLA for the Cortex Search Service, controlling how much time can pass before changes in the source table are reflected in the searchable index, similar to dynamic table lag semantics.

---

**Q22. Correct: C**

EMBED_TEXT_1024 generates 1024-dimensional embeddings and supports models like multilingual-e5-large, matching the target VECTOR column dimensionality.

---

**Q23. Correct: C**

The core RAG pattern is: retrieve relevant chunks per query, then pass them as context in the prompt to an LLM (COMPLETE) so the generated answer is grounded in that retrieved content while still producing a natural-language response.

---

**Q24. Correct: A**

A directory table exposes file-level metadata (including URLs) for files in a stage, and PARSE_DOCUMENT is the Cortex function designed to extract text (and optionally layout) from documents like PDFs, giving usable text for downstream chunking and embedding entirely within Snowflake.

---

**Q25. Correct: C**

SPLIT_TEXT_RECURSIVE_CHARACTER is Snowflake Cortex's built-in chunking function. It takes chunk_size and chunk_overlap parameters, where chunk_overlap defines how many characters/tokens are repeated between consecutive chunks so semantic context isn't lost at boundaries — a core RAG data-prep pattern.

---

**Q26. Correct: D**

PARSE_DOCUMENT's 'OCR' mode uses optical character recognition to extract plain text from image-based/scanned documents that have no embedded text layer, making it the correct choice for scanned invoices.

---

**Q27. Correct: D**

EMBED_TEXT_768 produces 768-dimensional embeddings, so the column must be declared as VECTOR(FLOAT, 768) to match. VECTOR_COSINE_SIMILARITY is the standard function for ranking semantic similarity between embeddings in RAG retrieval since it measures directional similarity independent of vector magnitude.

---

**Q28. Correct: B**

Snowpark Container Services supports GPU-enabled compute pools (e.g. GPU_NV_S/M/L instance families) that let you run arbitrary custom containers, including training/fine-tuning jobs on open-source checkpoints, entirely within Snowflake's account boundary so data never leaves governance controls.

---

**Q29. Correct: D**

Streamlit in Snowflake apps run natively on a Snowflake warehouse, require no external hosting or ingress setup, and are shared to other users purely through Snowflake role grants and Snowsight — exactly matching the stated requirements.

---

**Q30. Correct: D**

CREATE COMPUTE POOL uses INSTANCE_FAMILY to select the GPU node type, MIN_NODES/MAX_NODES to size the pool, and AUTO_SUSPEND_SECS to control idle suspension — this is the correct object type and parameter set for SPCS.

---

**Q31. Correct: B**

Streams capture row-level changes on RAW_DOCS, and a conditional Task fires only when new data exists, calling the Cortex function and writing to the vector table — this is a fully native, event-driven orchestration pattern requiring no external scheduler.

---

**Q32. Correct: A**

EXECUTE JOB SERVICE runs a container as a job — it executes to completion for a bounded task like batch inference and then automatically releases resources, unlike a persistent service.

---

**Q33. Correct: A**

Outbound network access from SPCS containers is controlled by External Access Integrations, which reference Network Rules whitelisting specific hosts/ports; the integration is then attached to the service so its container can reach the external endpoint.

---

**Q34. Correct: A**

SPCS JOBS are designed for finite, run-to-completion workloads. Running the batch embedding job as a job (not a service) on a GPU compute pool with auto-suspend ensures the GPU nodes spin down after the job finishes, minimizing cost while still meeting the latency/throughput needs of a GPU workload.

---

**Q35. Correct: D**

Outbound network access from Streamlit in Snowflake (and from SPCS) is governed by External Access Integrations, which reference a network rule (allowed hosts/ports) and optionally a secret for credentials. The integration must be granted to the app/service before egress calls succeed.

---

**Q36. Correct: C**

Snowflake Tasks support building a DAG via AFTER dependencies, letting each downstream task (e.g., the summarization step) execute only after its predecessor succeeds. The root task's CRON schedule provides the automated nightly trigger, making this the native orchestration mechanism for multi-step, dependency-aware Gen AI pipelines.

---

**Q37. Correct: A**

Access to Cortex LLM functions is controlled by granting the built-in SNOWFLAKE.CORTEX_USER database role to the roles that should be permitted to call functions like COMPLETE, SUMMARIZE, and TRANSLATE.

---

**Q38. Correct: C**

CORTEX_FUNCTIONS_USAGE_HISTORY records credit and token consumption for Cortex LLM function calls, broken down by function name, model, and consumer, making it the correct view for this monitoring task.

---

**Q39. Correct: B**

Masking policies are evaluated at query time based on the invoking role's privileges. Cortex functions operate on whatever value that role is authorized to see, so an analyst without UNMASK receives a summary of the masked value.

---

**Q40. Correct: C**

Cortex Guard is a safety guardrail feature that screens LLM-generated output for unsafe or inappropriate content before it is returned to the caller, supporting responsible-AI use of COMPLETE.

---

**Q41. Correct: B**

Snowflake's data privacy commitments for Cortex AI state that customer prompts and data are not used to train the underlying third-party or Snowflake-hosted models, and processing stays within Snowflake's security perimeter.

---

**Q42. Correct: A**

Cortex LLM function billing scales with model size and token volume. Simple classification tasks generally perform well on smaller, cheaper models, making a model downgrade the most direct and cost-effective optimization.

---

**Q43. Correct: D**

Cortex LLM functions consume Snowflake-managed serverless compute billed in credits based on tokens processed, independent of the calling warehouse. ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY records token counts and credits per function call, and can be grouped by query tag or user to attribute cost by team/department.

---

**Q44. Correct: A**

A dynamic data masking policy evaluated at query time replaces sensitive values (based on the querying role) before the data is returned or processed, so a pipeline invoking Cortex functions under a non-privileged role context only ever sees masked identifiers, while authorized roles can still view unmasked data when needed.

---

**Q45. Correct: B**

Cortex Guard is enabled by passing guardrails => true in the options parameter of SNOWFLAKE.CORTEX.COMPLETE, which applies a safety layer that filters unsafe or harmful content from model responses.

---

