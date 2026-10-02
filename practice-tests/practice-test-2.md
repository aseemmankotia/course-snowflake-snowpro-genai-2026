# Practice Test 2: SnowPro Specialty: Gen AI (GES-C01) Exam Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** A legal team is building a Streamlit app that uses SNOWFLAKE.CORTEX.COMPLETE to answer questions about 100-page contracts, and they want to pass the entire contract as context in a single call without chunking or RAG. Given typical context window sizes for Cortex LLM functions models, which model choice BEST supports this requirement?

A) llama3-8b (8K token context window)
B) snowflake-arctic (4K token context window)
C) mistral-7b (32K token context window)
D) mistral-large2 (128K token context window)

---

**Q2.** A table has a column defined as EMBEDDING_VECTOR VECTOR(FLOAT, 1024) storing document embeddings used for similarity search. Which Cortex function must be used to generate new embeddings that are dimensionally compatible with this column?

A) SNOWFLAKE.CORTEX.EMBED_TEXT_768
B) SNOWFLAKE.CORTEX.EXTRACT_ANSWER
C) SNOWFLAKE.CORTEX.EMBED_TEXT_1024
D) SNOWFLAKE.CORTEX.COMPLETE

---

**Q3.** A developer calls SNOWFLAKE.CORTEX.SUMMARIZE() on a long support ticket and needs the output capped at approximately 50 words for a dashboard tile. SUMMARIZE() does not accept a length or max_tokens parameter. What is the BEST approach to enforce this length constraint?

A) Increase the max_tokens parameter on SUMMARIZE() to 50
B) Call SUMMARIZE() twice and concatenate the results, then truncate to 50 words
C) Use SNOWFLAKE.CORTEX.EXTRACT_ANSWER() instead, since it supports a length parameter
D) Use SNOWFLAKE.CORTEX.COMPLETE() with a custom prompt that explicitly instructs the model to produce a summary of about 50 words

---

**Q4.** For SNOWFLAKE.CORTEX.COMPLETE(), which statement about tokens and context windows is CORRECT?

A) Both the input prompt tokens and the generated output tokens count together against the model's total context window
B) Context window size is determined by the Snowflake warehouse size, not the model
C) Tokens correspond exactly to words, so a 1,000-word prompt always uses exactly 1,000 tokens
D) Only the input prompt tokens count against a model's context window; output tokens are unlimited

---

**Q5.** A team combines Cortex Search with COMPLETE() to build a RAG chatbot over internal documentation, believing this will eliminate hallucinations entirely. During UAT, the model still occasionally invents details not present in the retrieved documents. What is the MOST accurate explanation of this behavior for the exam?

A) This indicates a bug in Cortex Search that requires a Snowflake support ticket
B) RAG reduces but does not fully guarantee against hallucination, since the LLM can still generate plausible-sounding but ungrounded text even when given relevant retrieved context
C) Cortex Search returned no results, which is the only reason hallucination can occur in a RAG pipeline
D) Hallucination only occurs with COMPLETE and never with EXTRACT_ANSWER, so EXTRACT_ANSWER should always replace COMPLETE in RAG pipelines

---

**Q6.** A company needs to score the sentiment of 20 million customer reviews per day for a cost-sensitive batch analytics job, with no need for custom categories or explanations. Which Cortex function is the MOST cost- and performance-efficient choice for this specific task?

A) SNOWFLAKE.CORTEX.SENTIMENT(), a purpose-built function optimized for this exact task
B) SNOWFLAKE.CORTEX.CLASSIFY_TEXT() with a large custom category list
C) SNOWFLAKE.CORTEX.EXTRACT_ANSWER() with a sentiment-related question
D) SNOWFLAKE.CORTEX.COMPLETE() with a general-purpose large model and a custom sentiment prompt

---

**Q7.** A team built a custom RAG pipeline over 500K historical documents. Document embeddings were generated and stored using SNOWFLAKE.CORTEX.EMBED_TEXT_768('snowflake-arctic-embed-m', doc_text) in a column of type VECTOR(FLOAT, 768). To improve multilingual retrieval, an engineer changes ONLY the query-time embedding call to SNOWFLAKE.CORTEX.EMBED_TEXT_1024('nv-embed-qa-4', query_text) and then calls VECTOR_COSINE_SIMILARITY between the query vector and the stored document vectors. What happens?

A) The query executes successfully, but similarity scores are systematically lower for multilingual queries since the two models were trained differently
B) Snowflake automatically pads the 1024-dimension query vector down to 768 dimensions so the comparison can proceed
C) The query fails with a dimension mismatch error because VECTOR_COSINE_SIMILARITY requires both input vectors to share the same dimensionality, and embeddings from different embedding functions/models are not interchangeable
D) The query succeeds because Cortex re-embeds the stored documents on the fly using nv-embed-qa-4 whenever a dimension mismatch is detected

---

**Q8.** An engineer calls SNOWFLAKE.CORTEX.COMPLETE() with a single prompt built by concatenating a 15,000-token knowledge-base article with a 40-token question, targeting a model whose documented maximum context window is 8,192 tokens. What is the expected behavior?

A) COMPLETE ignores context-window limits for input prompts and only enforces limits on the generated output length
B) COMPLETE automatically splits the input into multiple sub-calls and returns one concatenated answer summarizing all chunks
C) COMPLETE silently truncates the prompt to the last 8,192 tokens and generates an answer based only on that truncated portion
D) COMPLETE returns an error indicating the input exceeds the model's maximum context length, and no completion is generated

---

**Q9.** A customer support team must classify the sentiment of 2 million short (under 50-word) support tickets per day into Positive, Negative, or Neutral. Cost per call and per-ticket latency are the primary constraints. Which approach BEST meets these requirements?

A) Call SNOWFLAKE.CORTEX.COMPLETE() using a large general-purpose model such as llama3.1-405b with a few-shot prompt instructing it to output one of the three labels
B) Call SNOWFLAKE.CORTEX.SENTIMENT() directly on each ticket, since it uses a smaller task-specific model purpose-built for sentiment scoring
C) Fine-tune a custom classification model inside Snowpark Container Services and serve it behind a Streamlit-in-Snowflake front end
D) Call SNOWFLAKE.CORTEX.SUMMARIZE() on each ticket first, then have a human agent read the summary to manually determine sentiment

---

**Q10.** A financial services firm stores 2 million scanned PDF contracts in an internal stage. They want employees to ask natural-language questions like 'What is the termination clause in the Acme Corp contract?' and receive exact excerpted text with source citations. Which Cortex-based architecture BEST meets this requirement?

A) Use CORTEX.PARSE_DOCUMENT to extract text, chunk it, embed the chunks, and create a Cortex Search service over the chunked table for RAG-based retrieval
B) Build a Cortex Analyst semantic model over a table of contract metadata and let users query it via natural-language-to-SQL translation
C) Use CORTEX.SUMMARIZE on each PDF nightly and store the summaries in a table queried with CORTEX.COMPLETE
D) Use CORTEX.CLASSIFY_TEXT to categorize each contract type, then rely on CORTEX.EXTRACT_ANSWER alone without any vector index

---

**Q11.** A data team needs to add a numeric column to a Snowflake table that scores each customer support ticket's overall sentiment from -1 to 1 for a downstream analytics dashboard. Which single Cortex LLM function should they call directly in a SQL SELECT statement to produce this score?

A) SNOWFLAKE.CORTEX.ENTITY_SENTIMENT
B) SNOWFLAKE.CORTEX.SENTIMENT
C) SNOWFLAKE.CORTEX.CLASSIFY_TEXT
D) SNOWFLAKE.CORTEX.SUMMARIZE

---

**Q12.** A developer creates a Cortex Search service with TARGET_LAG = '1 hour' over a base view built from a change-tracking-enabled table. After loading 50,000 new rows into the base table, users report the search results are missing the new documents even 30 minutes after the load. What is the MOST likely explanation?

A) Cortex Search services do not support incremental updates and must be manually recreated after every data load
B) The base table must have CHANGE_TRACKING explicitly disabled for new rows to propagate into the search index
C) The TARGET_LAG of 1 hour means the search index can take up to an hour to refresh after base table changes, which is expected behavior
D) ATTRIBUTES columns specified at creation time cannot be updated afterward, requiring a full DROP and CREATE of the service

---

**Q13.** A team wants a Cortex LLM to consistently produce legal documents in a highly specific in-house format and tone that has proven difficult to achieve through prompt engineering alone, even with detailed instructions and few-shot examples. They have 5,000 labeled example documents available. Which Cortex capability is the MOST appropriate next step?

A) Increase the MAX_TOKENS parameter passed to CORTEX.COMPLETE to allow longer generated outputs
B) Switch from CORTEX.COMPLETE to CORTEX.SUMMARIZE with a custom system prompt
C) Use SNOWFLAKE.CORTEX.FINETUNE to create a customized model using their labeled example documents
D) Use CORTEX.CLASSIFY_TEXT to pre-classify documents before applying formatting rules

---

**Q14.** A retail analytics team wants business users to ask questions like 'What were total sales by region last quarter?' directly against Snowflake tables using natural language, with Cortex Analyst generating and executing the correct SQL. During setup, the team defines table relationships, column descriptions, synonyms, and sample verified queries. Where must this configuration be stored for Cortex Analyst to use it?

A) A JSON configuration embedded directly inside the CORTEX.COMPLETE prompt text
B) A Snowflake table using a dedicated SEMANTIC_MODEL column data type
C) A YAML semantic model file uploaded to a Snowflake stage
D) An external S3 bucket referenced by a Cortex Search service definition

---

**Q15.** A team wants to fine-tune a Cortex LLM for a specialized medical coding task using SNOWFLAKE.CORTEX.FINETUNE with a chosen base model parameter. Which of the following is a valid consideration they must account for BEFORE starting the job?

A) Fine-tuning jobs run on user-managed Snowpark Container Services GPU compute pools that must be manually provisioned in advance
B) Fine-tuning consumes credits based on training data size and the selected base model, and only a specific set of supported base models can be fine-tuned
C) Only embedding models such as snowflake-arctic-embed can be specified as the base model for a fine-tuning job
D) Once training completes, the fine-tuned model automatically replaces the base model for all COMPLETE function calls account-wide

---

**Q16.** A financial services team wants to fine-tune the llama3.1-8b base model in Snowflake Cortex on their internal support-ticket data, entirely inside Snowflake's security boundary, without exporting rows to an external training pipeline. Which statement correctly creates this fine-tuned model?

A) SELECT SNOWFLAKE.CORTEX.COMPLETE('llama3.1-8b', prompt, {'temperature': 0}) run repeatedly over the training set to adapt the weights
B) SELECT SNOWFLAKE.CORTEX.FINETUNE('CREATE', 'support_model_v1', 'llama3.1-8b', 'SELECT prompt, completion FROM training_data', 'SELECT prompt, completion FROM validation_data')
C) CREATE MODEL support_model_v1 USING (SELECT * FROM training_data) OPTIONS (base_model='llama3.1-8b')
D) SELECT SNOWFLAKE.ML.FORECAST('CREATE', 'support_model_v1', 'llama3.1-8b', training_data)

---

**Q17.** A retail company's business analysts want to ask natural-language questions such as 'What were total sales by region last quarter?' against a governed semantic model built on structured sales and dimension tables, and receive accurate, auditable SQL-generated numeric answers. Which Cortex capability BEST meets this requirement?

A) Cortex Analyst using a semantic model that maps business terms to the underlying tables
B) Cortex Search configured over the sales tables
C) Cortex Fine-Tuning on a model trained with historical sales Q&A pairs
D) SNOWFLAKE.CORTEX.COMPLETE with a retrieval-augmented prompt containing sample rows from the sales tables

---

**Q18.** A developer needs a Cortex LLM function call that requests strictly-typed JSON output matching a given schema, but the pipeline must never fail the batch job if the model occasionally cannot produce valid structured output — instead it should return NULL for that row so downstream filtering can handle it. Which call correctly satisfies both requirements?

A) SELECT SNOWFLAKE.CORTEX.COMPLETE('llama3.1-70b', prompt, {'response_format': schema_object}) with ON_ERROR => 'CONTINUE' as a session parameter
B) SELECT SNOWFLAKE.CORTEX.EXTRACT_ANSWER('llama3.1-70b', prompt, schema_object)
C) SELECT SNOWFLAKE.CORTEX.TRY_COMPLETE('llama3.1-70b', prompt, {'response_format': schema_object})
D) SELECT SNOWFLAKE.CORTEX.COMPLETE('llama3.1-70b', prompt, {'response_format': schema_object})

---

**Q19.** A team stores document embeddings generated by different embedding models, resulting in vectors with varying magnitudes but consistent semantic direction. Which Snowflake vector function should they use in a similarity search query to rank results primarily by semantic direction rather than raw vector magnitude?

A) VECTOR_L2_DISTANCE
B) EDIT_DISTANCE
C) VECTOR_INNER_PRODUCT
D) VECTOR_COSINE_SIMILARITY

---

**Q20.** A support team needs a search experience over a growing knowledge base that combines exact keyword matches (e.g., product SKU numbers) with semantic similarity, refreshes automatically as new articles are added, and requires no manual index management or infrastructure provisioning. Which Snowflake capability should they use?

A) Cortex Search Service
B) Manually compute embeddings with EMBED_TEXT_768 and query them on a schedule using VECTOR_COSINE_SIMILARITY
C) Cortex Analyst
D) A custom vector database deployed via Snowpark Container Services

---

**Q21.** Which configuration constraint applies when calling SNOWFLAKE.CORTEX.SPLIT_TEXT_RECURSIVE_CHARACTER to chunk documents for a RAG pipeline?

A) The overlap value must be smaller than the chunk_size value, and the format argument must be set to either 'none' or 'markdown'
B) The function automatically determines the model-specific token limit and ignores any explicit chunk_size input
C) The chunk_size and overlap parameters are always specified in tokens, regardless of the format argument
D) The overlap value must be greater than the chunk_size value to guarantee no information loss between chunks

---

**Q22.** What is the maximum number of dimensions supported by Snowflake's VECTOR data type?

A) 1536
B) 2048
C) 768
D) 4096

---

**Q23.** A pipeline extracts text from scanned PDF invoices containing multi-column tables of line items and totals. Preserving row and column relationships in the extracted text is critical for downstream RAG accuracy. Which configuration of SNOWFLAKE.CORTEX.PARSE_DOCUMENT should be used?

A) mode => 'OCR', since OCR mode produces richer structural markdown specifically for tabular content
B) mode => 'OCR', since it is the only mode that supports scanned, image-based PDFs
C) mode => 'LAYOUT', since LAYOUT mode preserves structural elements such as tables in the extracted output
D) Call PARSE_DOCUMENT twice, once for text and once for tables, and merge results manually, since neither mode preserves tables

---

**Q24.** A team is building a multilingual RAG pipeline using SNOWFLAKE.CORTEX.EMBED_TEXT_1024 with the multilingual-e5-large model, and plans to feed those embeddings into a Cortex Search Service for retrieval. Which statement about aligning the two is correct?

A) Cortex Search Service always generates its own embeddings internally using its configured EMBEDDING_MODEL from the source text column, so manually pre-computed EMBED_TEXT_1024 vectors are not what the service uses for its semantic index
B) The VECTOR column feeding the Cortex Search Service must be manually declared as VECTOR(FLOAT, 1024) to match multilingual-e5-large's output, or the service will silently truncate vectors
C) EMBED_TEXT_1024 and EMBED_TEXT_768 outputs are interchangeable for the same Cortex Search Service as long as VECTOR_COSINE_SIMILARITY is used
D) Cortex Search Service does not support multilingual embedding models; only English-only models such as e5-base-v2 are supported

---

**Q25.** A data engineering team stores document embeddings in a table with column EMBEDDINGS VECTOR(FLOAT, 768), populated using SNOWFLAKE.CORTEX.EMBED_TEXT_768 with the snowflake-arctic-embed-m model. To improve retrieval quality for multilingual content, they switch new document ingestion to SNOWFLAKE.CORTEX.EMBED_TEXT_1024 (multilingual-e5-large) but continue inserting into the same EMBEDDINGS column without altering the schema. What happens when the new INSERT statements run?

A) The INSERT fails with a dimension mismatch error because VECTOR(FLOAT, 768) requires exactly 768 elements per value
B) The 1024-dimension vectors are automatically truncated to 768 dimensions to fit the existing column
C) Snowflake automatically widens the column to VECTOR(FLOAT, 1024) the first time a larger vector is inserted
D) The vectors are stored successfully as VARIANT internally since VECTOR columns support variable-length arrays

---

**Q26.** A team runs CREATE CORTEX SEARCH SERVICE against a base table containing product documentation chunks, intending to enable incremental refresh as new rows are added. The command succeeds, but the search service never picks up newly inserted rows and requires manual full rebuilds. What is the most likely root cause?

A) The base table does not have CHANGE_TRACKING enabled, so Cortex Search cannot detect incremental changes
B) Cortex Search Services only support views, not base tables, for automatic refresh
C) The warehouse used to create the service was suspended after creation
D) The ON column specified in the service definition is not of type VARCHAR

---

**Q27.** A developer chunks long PDF manuals using SNOWFLAKE.CORTEX.SPLIT_TEXT_RECURSIVE_CHARACTER with a chunk_size of 8000 characters (roughly 2000 tokens), then passes each chunk directly into SNOWFLAKE.CORTEX.EMBED_TEXT_768 using the snowflake-arctic-embed-m model, which has a maximum input sequence length of 512 tokens. Retrieval quality on later sections of long chunks is poor. What is the most likely cause?

A) EMBED_TEXT_768 automatically splits each chunk into 512-token sub-segments and returns an averaged embedding vector
B) EMBED_TEXT_768 throws a runtime error whenever input text exceeds the model's max sequence length, blocking the pipeline entirely
C) The embedding model truncates input beyond its max token length, so only the first ~512 tokens of each 2000-token chunk are actually represented in the vector
D) SPLIT_TEXT_RECURSIVE_CHARACTER silently drops any chunk longer than the embedding model's token limit before returning results

---

**Q28.** A team must deploy a custom fine-tuned LLM for real-time inference using Snowpark Container Services. Profiling shows the model requires more GPU memory than a single NVIDIA A10G GPU (24 GB) can provide, and the team wants to avoid quantizing the model. Which compute pool instance family should they select to run the model across multiple GPUs on one node?

A) GPU_NV_S, since it is the default GPU family for SPCS workloads
B) GPU_NV_M, since it provisions multiple GPUs on a single node, allowing the model to be split across GPUs via tensor/model parallelism
C) CPU_X64_L, since CPU nodes have larger addressable memory than GPU nodes
D) HIGHMEM_X64_L, since it maximizes system RAM available to the container

---

**Q29.** A container running in Snowpark Container Services must call an external third-party embedding API over HTTPS from inside the running service. Which combination of steps correctly enables this outbound call?

A) Enable the public endpoint flag on the service specification so the container can reach external hosts
B) Grant the service's owner role the EXECUTE JOB SERVICE privilege on the compute pool
C) Create a storage integration pointing to the external API's domain and mount it as an external stage inside the container
D) Create a network rule specifying the external host/port, then create an External Access Integration (EAI) referencing that rule and attach the EAI to the service

---

**Q30.** In a Snowpark Container Services service specification YAML, which field tells Snowflake to check that a container has finished starting up and is able to accept traffic before routing requests to it?

A) endpoints
B) readinessProbe
C) volumeMounts
D) resources

---

**Q31.** A team needs to generate embeddings for 5 million documents once, using a GPU-accelerated container image, and wants the compute pool to release the node automatically as soon as processing completes rather than remaining active for future requests. Which SPCS execution approach fits this requirement?

A) Use EXECUTE JOB SERVICE to run the container as a run-to-completion job, which automatically releases its compute pool resources once finished
B) Create a long-running SERVICE with MIN_INSTANCES=1 and manually suspend the compute pool afterward
C) Schedule a Python UDF through a Task that loops through documents synchronously on a virtual warehouse
D) Create an External Function that invokes the container's public endpoint once per document from a SQL script

---

**Q32.** A Streamlit in Snowflake app needs to display real-time predictions produced by a custom PyTorch model deployed as a long-running SPCS service. What is the correct, supported way for the Streamlit app's Python code to obtain predictions from that service?

A) The Streamlit app pulls and runs the container's Docker image locally inside the Streamlit in Snowflake sandbox to perform inference in-process
B) An External Function is created pointing to an external API Gateway that proxies HTTP calls to the SPCS service's public endpoint
C) A service function (UDF) is created with SERVICE and ENDPOINT clauses referencing the SPCS service; the Streamlit app calls this UDF via SQL to retrieve predictions
D) The Streamlit app's Python code uses the `requests` library to call the container's public endpoint URL directly over the internet

---

**Q33.** A pipeline must run a nightly GPU batch-inference job in SPCS after new data lands in a table, and then update a results table once the job finishes. Which orchestration design correctly implements this within Snowflake?

A) A Stream is created on the compute pool object to detect when new GPU capacity becomes available, which triggers the job
B) An Alert monitors compute pool GPU utilization and automatically executes the job service whenever utilization drops below a threshold
C) The service's AUTO_RESUME setting is relied upon exclusively, since SPCS job services run automatically every night without any external scheduler
D) A Snowflake Task on a CRON schedule calls a stored procedure that executes EXECUTE JOB SERVICE for the batch container, then updates the results table on completion

---

**Q34.** A team fine-tunes an open-source embedding model (not available as a Cortex function) and must expose it for inference so analysts can generate embeddings from SQL. The model requires GPU acceleration and a custom Python inference server. What is the correct sequence to deploy and expose this on Snowflake?

A) Register the model weights as a Python package in a Snowpark UDF and run it on a standard virtual warehouse, since UDFs can use any installed package regardless of hardware needs
B) Wrap the model behind an External Function using an API Integration to a third-party GPU host, since Snowflake compute pools cannot run custom containers
C) Call CORTEX.EMBED_TEXT_768 and pass the custom model name as a parameter, since Cortex embedding functions support arbitrary user-supplied model weights
D) Create a GPU compute pool (e.g. INSTANCE_FAMILY = GPU_NV_S), deploy the model as a Snowpark Container Services service with an inference endpoint, then create a service function pointing to that endpoint so it can be called from SQL

---

**Q35.** A Streamlit in Snowflake application needs to make outbound HTTPS calls to a third-party SaaS API for a Gen AI comparison feature. What must be configured before the app's Python code can successfully reach the external endpoint?

A) Grant the app owner role the IMPORTED PRIVILEGES on SNOWFLAKE database so outbound HTTP is enabled globally
B) Create a network rule and external access integration for the target host, then set EXTERNAL_ACCESS_INTEGRATIONS on the Streamlit object to reference that integration
C) Create an API Integration object and use CREATE EXTERNAL FUNCTION so the Streamlit app can proxy calls through the external function
D) Enable the 'internet_access' parameter at the account level, which automatically whitelists all outbound Streamlit traffic

---

**Q36.** A company stores support documents in a table and wants semantic search over them that stays current as new documents are inserted, with minimal operational overhead. Which approach correctly minimizes unnecessary orchestration?

A) Build an Airflow DAG that calls the Cortex Analyst API on a schedule to regenerate embeddings and rebuild the search index
B) Create a Stream on the documents table and a Task that runs every few minutes to manually regenerate embeddings and rebuild the search index from scratch
C) Create a CORTEX SEARCH SERVICE directly on the source table specifying a TARGET_LAG value; Snowflake automatically maintains and incrementally refreshes the index as underlying data changes
D) Deploy a Snowpark Container Services job scheduled hourly to call an embedding function and repopulate a vector column, then query it with vector similarity functions

---

**Q37.** An account team uses SNOWFLAKE.CORTEX.COMPLETE() extensively and wants an automatic email notification when serverless Cortex LLM function credit consumption exceeds 500 credits in a single day. Which approach correctly implements this?

A) Create a resource monitor on the virtual warehouse used to issue Cortex function calls, set a 500-credit quota, and configure a notify action
B) Enable query tag-based alerting in the Snowsight cost dashboard with a 500-credit threshold trigger
C) Apply a masking policy to the SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY view to restrict visibility once 500 credits are consumed
D) Create a Snowflake Budget object scoped to monitor account-level serverless compute spend, set a 500-credit threshold, and configure email notification

---

**Q38.** A table contains a customer NOTES column with embedded PII. Analysts without the PII_VIEWER role should be able to run SNOWFLAKE.CORTEX.COMPLETE() to summarize NOTES, but the LLM must never receive unmasked PII when called by unauthorized roles. Which built-in mechanism achieves this with the least custom engineering?

A) Apply a row access policy on the table filtering out rows containing PII keywords
B) Apply a dynamic data masking policy to the NOTES column; the policy evaluates before the value is passed into COMPLETE(), so unauthorized roles pass masked text into the function
C) Route all COMPLETE() calls through an External Access Integration to an AWS Lambda function that redacts PII before returning results
D) Grant SELECT only on a view excluding the NOTES column, then have analysts manually retype summaries requested from a privileged role

---

**Q39.** By default, PUBLIC is granted the SNOWFLAKE.CORTEX_USER database role, allowing any role to call Cortex LLM functions. A security team wants to revoke this default and instead explicitly control which custom roles can invoke functions like SNOWFLAKE.CORTEX.COMPLETE(). Which grant statement is required to authorize a specific custom role after revoking PUBLIC access?

A) GRANT USAGE ON DATABASE SNOWFLAKE TO ROLE analyst_role;
B) GRANT EXECUTE TASK ON ACCOUNT TO ROLE analyst_role;
C) GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER TO ROLE analyst_role;
D) GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE_SAMPLE_DATA TO ROLE analyst_role;

---

**Q40.** A healthcare company operating entirely within Snowflake's Switzerland region must guarantee that prompts sent to Cortex LLM functions are never processed by infrastructure located in another geographic region, even if the requested model is temporarily unavailable locally. Which configuration enforces this?

A) Revoke SNOWFLAKE.CORTEX_USER from PUBLIC and grant it only to a compliance-approved role
B) Set the account parameter CORTEX_ENABLED_CROSS_REGION to 'DISABLED'
C) Enable Tri-Secret Secure encryption for the account
D) Configure a network policy restricting inbound connections to Swiss IP address ranges

---

**Q41.** A team wants SNOWFLAKE.CORTEX.COMPLETE() to automatically filter and suppress potentially unsafe or harmful generated content before returning results, using Snowflake's built-in content-safety capability. Which configuration accomplishes this?

A) Set 'guardrails': true within the options object parameter passed to COMPLETE()
B) Set the session parameter TRUST_LEVEL = 'HIGH' before calling COMPLETE()
C) Attach a CONTENT_FILTER_POLICY object to the model using CREATE CONTENT FILTER POLICY
D) Set safe_mode = 'STRICT' as a top-level argument to COMPLETE()

---

**Q42.** From a data governance standpoint, what is the fundamental difference between invoking SNOWFLAKE.CORTEX.COMPLETE() versus calling a third-party LLM API through an External Network Access Integration?

A) Cortex functions process data entirely within Snowflake's governance boundary without data leaving the platform, while calling an external API sends data outside that boundary unless additional controls are configured
B) Cortex functions require configuring an External Access Integration, whereas third-party API calls do not require any Snowflake object configuration
C) Calling external APIs is inherently cheaper because it bypasses Snowflake credit consumption entirely, with no cost tradeoffs
D) Cortex functions cannot be restricted using Snowflake RBAC, while external function calls can be fully governed by role-based grants

---

**Q43.** A financial services company uses AI_COMPLETE and other Cortex LLM functions extensively. The security team wants to ensure that regardless of which role calls these functions, only a specific, approved subset of foundation models (e.g., 'llama3.1-8b' and 'mistral-large2') can ever be invoked account-wide, to control both cost and data-residency risk. Which approach BEST meets this requirement?

A) Create a resource monitor on the warehouse used for AI_COMPLETE calls
B) Set the CORTEX_MODELS_ALLOWLIST account parameter to include only the approved models
C) Grant USAGE on the SNOWFLAKE.CORTEX schema only to specific roles
D) Create a row access policy on the table used as input for the Cortex functions

---

**Q44.** A FinOps team needs to attribute Cortex LLM function costs to specific queries so they can chargeback individual teams based on tokens processed per query. Which ACCOUNT_USAGE object provides per-query-id granularity of tokens and credits consumed by Cortex LLM function calls?

A) SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY
B) SNOWFLAKE.ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY
C) SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY
D) SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_QUERY_USAGE_HISTORY

---

**Q45.** A healthcare company built a Cortex Search Service over a PATIENT_RECORDS table that has a row access policy restricting visibility by department, and a masking policy on the SSN column. When two users in different departments run the same search query against this Cortex Search Service, what should happen to the results?

A) Cortex Search bypasses all table-level security policies because it queries a separate vector index built ahead of time
B) The row access policy must be manually re-created directly on the Cortex Search Service object for it to take effect
C) The masking policy applies to search results, but the row access policy does not
D) Row access and masking policies on the base table are enforced dynamically — each user only sees search results for rows and column values they are authorized to access

---

