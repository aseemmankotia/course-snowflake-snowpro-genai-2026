# Practice Test 1: SnowPro Specialty: Gen AI (GES-C01) Exam Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A support operations team wants to route thousands of incoming tickets per day into a fixed set of categories (Billing, Technical, Shipping, Other) without training a custom model or writing category-specific prompts for each ticket. Which Snowflake Cortex approach BEST meets this requirement?

A) Call SNOWFLAKE.CORTEX.SUMMARIZE on each ticket and manually inspect the summary for keywords
B) Call SNOWFLAKE.CORTEX.COMPLETE with a large general-purpose model and a detailed few-shot prompt for every ticket
C) Generate embeddings with EMBED_TEXT_1024 for each ticket and manually cluster the vectors
D) Call SNOWFLAKE.CORTEX.CLASSIFY_TEXT, passing the ticket text and the list of category labels

---

**Q2.** A developer calls SNOWFLAKE.CORTEX.COMPLETE with a smaller-context-window model and passes in a very long legal document plus a question about it. The call fails because the combined prompt and expected response exceed the model's context window. What is the BEST fix?

A) Increase the temperature parameter to allow the model to compress the input automatically
B) Retry the exact same call; Cortex will automatically queue and process oversized prompts in batches
C) Switch to a model with a larger context window, or chunk the document and summarize/retrieve only relevant sections before calling COMPLETE
D) Convert the document to embeddings using EMBED_TEXT_768 and pass the embeddings directly into COMPLETE instead of the raw text

---

**Q3.** You need to generate 1024-dimensional vector embeddings for English product descriptions so you can build a custom similarity-search table using VECTOR data types and cosine distance. Which Cortex function should you call?

A) SNOWFLAKE.CORTEX.EMBED_TEXT_768
B) SNOWFLAKE.CORTEX.COMPLETE
C) SNOWFLAKE.CORTEX.EMBED_TEXT_1024
D) SNOWFLAKE.CORTEX.EXTRACT_ANSWER

---

**Q4.** A global support team receives chat messages in Spanish, French, and German that must be converted to English before an English-only downstream analytics pipeline processes them. Which Cortex LLM function is purpose-built for this task?

A) SNOWFLAKE.CORTEX.SENTIMENT
B) SNOWFLAKE.CORTEX.EMBED_TEXT_768
C) SNOWFLAKE.CORTEX.COMPLETE with a zero-shot classification prompt
D) SNOWFLAKE.CORTEX.TRANSLATE

---

**Q5.** A finance team is generating compliance summaries with SNOWFLAKE.CORTEX.COMPLETE and needs the wording to be as consistent and deterministic as possible across repeated runs on the same input. Which options object parameter should be set to a low value (near 0) to achieve this?

A) max_tokens
B) temperature
C) guardrails
D) top_k

---

**Q6.** In the context of large language models and Snowflake Cortex, what does a text embedding represent?

A) A losslessly compressed copy of the original text used for cheaper storage
B) A count of the number of tokens contained in the input text
C) A cryptographic hash used to detect duplicate documents
D) A numeric vector that captures the semantic meaning of the text so similar meanings map to nearby vectors

---

**Q7.** A data engineering team needs to build a semantic search feature over a table of product descriptions. They plan to generate vector embeddings using a Cortex function and store the results in a column so they can later run VECTOR_COSINE_SIMILARITY queries against user search text. Which approach correctly matches the embedding output to a valid column definition?

A) Use SNOWFLAKE.CORTEX.COMPLETE and store the result in a column defined as VARIANT
B) Use SNOWFLAKE.CORTEX.EMBED_TEXT_768 and store the result in a column defined as VECTOR(FLOAT, 1024)
C) Use SNOWFLAKE.CORTEX.EMBED_TEXT_1024 and store the result in a column defined as VECTOR(FLOAT, 1024)
D) Use SNOWFLAKE.CORTEX.SUMMARIZE and store the result in a column defined as VECTOR(FLOAT, 768)

---

**Q8.** A developer is testing SNOWFLAKE.CORTEX.COMPLETE and wants the model's responses to be more deterministic and consistent across repeated calls with the same prompt, rather than varied and creative. Which adjustment BEST achieves this?

A) Lower the temperature parameter toward 0
B) Increase the max_tokens parameter to allow longer responses
C) Switch to a larger parameter-count model for more consistency
D) Increase the temperature parameter toward 1

---

**Q9.** A team is estimating the cost of running SNOWFLAKE.CORTEX.COMPLETE across a large batch of customer support tickets before committing to a production workload. Which statement correctly describes how Cortex LLM function consumption is measured for billing purposes?

A) Consumption is measured based on the number of tokens processed, combining both input prompt and generated output, at rates that vary by model
B) Consumption is measured by the number of API calls made, regardless of prompt or response length
C) Consumption is measured by the number of distinct users invoking the function within a warehouse
D) Consumption is measured by the size of the warehouse used to execute the SQL statement calling the function

---

**Q10.** A retail company wants business analysts who don't know SQL to ask questions like 'What were total sales by region last quarter?' directly against a Snowflake table containing verified sales data, and get accurate, governed SQL-backed answers. Which Cortex AI capability BEST meets this requirement?

A) Cortex Search configured over the sales table
B) Cortex Analyst with a semantic model built on the sales table
C) The COMPLETE function with the sales schema embedded in the prompt
D) Cortex Fine-Tuning a custom model on historical Q&A pairs

---

**Q11.** An organization has 50,000 unstructured PDF legal contracts stored in Snowflake stages. They want to build a chatbot that retrieves relevant contract passages and lets an LLM answer employee questions using retrieval-augmented generation (RAG). Which Cortex AI service should be used to build the retrieval layer?

A) Cortex Analyst
B) The TRANSLATE function
C) Cortex Search
D) The SENTIMENT function

---

**Q12.** A support team wants to automatically flag customer support tickets with a numeric score indicating how positive or negative the customer's tone is, without building a custom model. Which Cortex LLM function should they call?

A) SNOWFLAKE.CORTEX.SUMMARIZE
B) SNOWFLAKE.CORTEX.SENTIMENT
C) SNOWFLAKE.CORTEX.CLASSIFY_TEXT
D) SNOWFLAKE.CORTEX.EXTRACT_ANSWER

---

**Q13.** When creating a Cortex Search service with CREATE CORTEX SEARCH SERVICE, which parameter controls how frequently the underlying search index is refreshed to reflect changes in the source table?

A) WAREHOUSE
B) TARGET_LAG
C) EMBEDDING_MODEL
D) ATTRIBUTES

---

**Q14.** What artifact must be provided to Cortex Analyst so it can correctly translate natural-language business questions into SQL against your specific schema?

A) A fine-tuned LLM checkpoint
B) A Cortex Search service definition
C) A semantic model YAML file describing tables, dimensions, and measures
D) A vector embedding index over the table

---

**Q15.** A company runs millions of daily classification calls using COMPLETE with a long few-shot prompt to categorize support tickets into 12 fixed internal categories. Costs and latency are becoming a problem, and the categories rarely change. What is the MOST cost-effective long-term solution using Cortex AI?

A) Fine-tune a smaller base model on labeled examples using Cortex Fine-Tuning
B) Increase the context window by adding more examples to the prompt
C) Switch to Cortex Search to retrieve category definitions at runtime
D) Call SUMMARIZE first, then COMPLETE for classification

---

**Q16.** A retail company stores sales transactions in structured Snowflake tables and wants business analysts to ask questions like 'What were total sales in the Northeast region last quarter?' in plain English and receive accurate SQL-based answers without writing SQL themselves. Which Cortex AI feature BEST meets this requirement?

A) Cortex Search configured with a semantic model over the sales tables
B) Cortex LLM COMPLETE function called with a prompt containing raw table data
C) Cortex Search Service using hybrid search over exported CSV files
D) Cortex Analyst configured with a semantic model over the sales tables

---

**Q17.** Which SQL statement correctly initiates a Cortex Fine-Tuning job to create a custom model named my_custom_model based on the mistral-7b base model, using a training dataset produced by a SELECT statement against table TRAIN_DATA?

A) SELECT SNOWFLAKE.CORTEX.COMPLETE('mistral-7b', 'FINETUNE', 'my_custom_model', 'TRAIN_DATA');
B) SELECT SNOWFLAKE.CORTEX.FINETUNE('CREATE', 'my_custom_model', 'mistral-7b', 'SELECT prompt, completion FROM TRAIN_DATA');
C) CREATE CORTEX MODEL my_custom_model USING BASE_MODEL='mistral-7b' TRAINING_TABLE='TRAIN_DATA';
D) SELECT SNOWFLAKE.CORTEX.FINETUNE('my_custom_model', 'mistral-7b', 'TRAIN_DATA');

---

**Q18.** A support-ticket search tool needs to pull the precise excerpt of text that answers a customer's specific question (e.g., 'What is the refund window?') directly from a long support document, without generating a general summary of the whole document. Which Cortex LLM function is PURPOSE-BUILT for this task?

A) SNOWFLAKE.CORTEX.COMPLETE
B) SNOWFLAKE.CORTEX.SUMMARIZE
C) SNOWFLAKE.CORTEX.TRANSLATE
D) SNOWFLAKE.CORTEX.EXTRACT_ANSWER

---

**Q19.** You are ranking retrieved passages for a RAG pipeline using two VECTOR(FLOAT, 768) columns. Which Snowflake function should you use so that a HIGHER returned value always means the two vectors are MORE similar?

A) VECTOR_COSINE_SIMILARITY
B) VECTOR_L2_DISTANCE
C) APPROXIMATE_SIMILARITY
D) COSINE_DISTANCE

---

**Q20.** Before generating embeddings for a knowledge base of long support articles, a developer needs to split each article into smaller overlapping passages of a configurable size. Which Cortex function should be used?

A) SNOWFLAKE.CORTEX.SUMMARIZE
B) SNOWFLAKE.CORTEX.SPLIT_TEXT_RECURSIVE_CHARACTER
C) SNOWFLAKE.CORTEX.PARSE_DOCUMENT
D) SNOWFLAKE.CORTEX.EXTRACT_ANSWER

---

**Q21.** You create a Cortex Search Service with TARGET_LAG = '1 hour' and WAREHOUSE = 'SEARCH_WH' over a base table that is updated hourly by an ETL job. What does the TARGET_LAG parameter control?

A) The maximum time allowed between updates to the base table and the search index reflecting those changes
B) The maximum end-to-end latency allowed for a single search query response
C) The auto-suspend timeout of the warehouse used to serve search queries
D) How often the embedding model itself is retrained on new data

---

**Q22.** Your team wants embeddings compatible with the multilingual-e5-large model, which produces 1024-dimensional vectors, to populate a VECTOR(FLOAT, 1024) column outside of the Cortex Search Service's automatic embedding pipeline. Which function should be called directly?

A) SNOWFLAKE.CORTEX.TRANSLATE
B) SNOWFLAKE.CORTEX.COMPLETE
C) SNOWFLAKE.CORTEX.EMBED_TEXT_1024
D) SNOWFLAKE.CORTEX.EMBED_TEXT_768

---

**Q23.** A company built a customer-support chatbot that uses a Cortex Search Service to retrieve relevant knowledge-base chunks for each user question. They now want the chatbot's final answer to be grounded strictly in the retrieved chunks rather than the model's general knowledge. Which approach BEST implements this RAG pattern in Snowflake?

A) Fine-tune a brand-new LLM on the entire knowledge base and retire the Cortex Search Service
B) Return the single closest-matching chunk directly to the user as the final answer, with no LLM call
C) Call SNOWFLAKE.CORTEX.COMPLETE with a prompt that includes the user's question plus the retrieved chunks as grounding context
D) Run SNOWFLAKE.CORTEX.SUMMARIZE once over the whole knowledge base and reuse that single cached summary for every question

---

**Q24.** A company has thousands of PDF contracts sitting in an internal Snowflake stage and needs to extract their text content to build embeddings for a RAG pipeline, all without leaving Snowflake. Which combination of features BEST accomplishes this?

A) Create a directory table on the stage to track file metadata, then use SNOWFLAKE.CORTEX.PARSE_DOCUMENT to extract text from each PDF before chunking
B) Export the PDFs to an external Python service for OCR, then import only the resulting file names back into Snowflake
C) Load the PDF binaries directly into a VARIANT column and call VECTOR_COSINE_SIMILARITY on the raw bytes
D) Use Snowpipe to continuously load the PDFs, then call SNOWFLAKE.CORTEX.TRANSLATE to convert them to plain text

---

**Q25.** A company is building a RAG pipeline over long technical manuals (50-100 pages each) stored as PDFs in an internal stage. Text has already been extracted, and the team now needs to split it into chunks for embedding while preserving context across chunk boundaries. Which function should they use, and which parameter controls the amount of shared text between adjacent chunks?

A) SNOWFLAKE.CORTEX.EMBED_TEXT_768; the dimension parameter
B) SNOWFLAKE.CORTEX.PARSE_DOCUMENT; the mode parameter
C) SNOWFLAKE.CORTEX.SPLIT_TEXT_RECURSIVE_CHARACTER; the chunk_overlap parameter
D) SNOWFLAKE.CORTEX.SUMMARIZE; the max_tokens parameter

---

**Q26.** A team needs to extract text from scanned PDF invoices (image-based, with no embedded text layer) stored in an internal stage, as a prerequisite step before chunking and embedding for a RAG application. Which function and mode should they use?

A) SNOWFLAKE.CORTEX.TRANSLATE
B) SNOWFLAKE.CORTEX.PARSE_DOCUMENT with mode 'LAYOUT'
C) SNOWFLAKE.CORTEX.EXTRACT_ANSWER
D) SNOWFLAKE.CORTEX.PARSE_DOCUMENT with mode 'OCR'

---

**Q27.** A developer defines a table column to store embeddings generated by SNOWFLAKE.CORTEX.EMBED_TEXT_768, to be used for nearest-neighbor retrieval in a RAG application. Which combination of VECTOR data type declaration and similarity function is correct for this use case?

A) VARIANT with VECTOR_INNER_PRODUCT
B) VECTOR(FLOAT, 1024) with VECTOR_L2_DISTANCE
C) ARRAY(FLOAT) with APPROX_TOP_K
D) VECTOR(FLOAT, 768) with VECTOR_COSINE_SIMILARITY

---

**Q28.** An organization must fine-tune an open-source 7B-parameter LLM on proprietary support-ticket text while keeping all data and compute inside Snowflake's governance boundary, and the workload requires dedicated GPU acceleration for training. Which approach BEST meets these requirements?

A) Call SNOWFLAKE.CORTEX.FINETUNE from a stored procedure running on a standard virtual warehouse
B) Provision a GPU instance family compute pool in Snowpark Container Services and run a custom training container
C) Resize a Snowflake virtual warehouse to XXLARGE and run the training job there
D) Deploy a Streamlit in Snowflake app with a background thread that performs the training

---

**Q29.** A team wants to deploy an interactive chatbot UI backed by Cortex Search and Cortex LLM functions. The UI must run entirely inside Snowflake's security perimeter, be shareable through Snowsight via role-based access, and require no separate web server, hosting account, or ingress configuration to manage. Which deployment option BEST fits?

A) Host the app on Streamlit Community Cloud and connect to Snowflake through a connector
B) Build a custom Flask application deployed as a Snowpark Container Services service with its own ingress endpoint
C) Package the app as a Native App Framework consumer application hosted on a partner cloud marketplace
D) Create the app with CREATE STREAMLIT to deploy it as a Streamlit in Snowflake (SiS) app

---

**Q30.** You need to create a Snowpark Container Services compute pool with a single GPU node that automatically suspends after 5 minutes of inactivity. Which statement correctly creates this compute pool?

A) ALTER COMPUTE POOL my_pool SET INSTANCE_FAMILY = GPU_NV_S;
B) CREATE WAREHOUSE my_pool WAREHOUSE_SIZE = 'GPU_NV_S' AUTO_SUSPEND = 300;
C) CREATE COMPUTE POOL my_pool WAREHOUSE_SIZE = 'MEDIUM' AUTO_RESUME = TRUE;
D) CREATE COMPUTE POOL my_pool INSTANCE_FAMILY = GPU_NV_S MIN_NODES = 1 MAX_NODES = 1 AUTO_SUSPEND_SECS = 300;

---

**Q31.** A pipeline must automatically generate embeddings with SNOWFLAKE.CORTEX.EMBED_TEXT_768 whenever new rows are inserted into a RAW_DOCS table, then load the results into a vector table used by Cortex Search. Which native Snowflake approach requires the LEAST external tooling?

A) Use an external Apache Airflow DAG that polls RAW_DOCS every minute for new rows
B) Create a Stream on RAW_DOCS and a Task that runs WHEN SYSTEM$STREAM_HAS_DATA(), invoking the Cortex embedding function and inserting results into the vector table
C) Use Snowpipe alone, configuring it to call the Cortex embedding function directly during file ingestion
D) Use a Dynamic Table that is refreshed manually only when new data is expected

---

**Q32.** You need to run a one-time batch inference job in Snowpark Container Services that processes one million records, then automatically releases its compute resources once finished — it should not remain running like a persistent endpoint. Which command should you use?

A) EXECUTE JOB SERVICE ... to run the container as a run-to-completion job
B) CREATE SERVICE ... to run the container as a long-running service
C) CREATE COMPUTE POOL ... to provision the compute resources directly
D) ALTER SERVICE ... RESUME to restart a suspended long-running service

---

**Q33.** A container running in Snowpark Container Services needs to download open-source model weights from an external Hugging Face Hub URL at startup. The account's network policies block all outbound traffic by default. Which object must be created and attached to the service to allow this specific outbound call?

A) An External Access Integration referencing a Network Rule that allows the Hugging Face host
B) A stage with directory table enabled, pointing to the external model source
C) A Cortex Search service configured with a web crawler source
D) A Streamlit app with an EXTERNAL_ACCESS = TRUE parameter

---

**Q34.** A data science team needs to run nightly batch embedding jobs against 2 million documents using an open-source embedding model packaged in a custom Docker container. The workload requires GPU acceleration to finish within the nightly window, but the team wants to avoid paying for idle GPU compute the rest of the day. Which Snowpark Container Services deployment approach BEST meets these requirements?

A) Deploy the container as an SPCS job on a GPU compute pool with AUTO_SUSPEND_SECS configured, so the pool suspends and stops billing once the batch job completes
B) Deploy the container as a long-running SPCS service on a GPU compute pool with MIN_NODES and MAX_NODES both set to 1 so it is always available
C) Deploy the container as an SPCS service on a CPU compute pool and increase the number of nodes to compensate for the lack of GPU acceleration
D) Register the open-source model in the Cortex Search service so it runs automatically whenever a SELECT query references the embedding column

---

**Q35.** A Streamlit in Snowflake app needs to call a third-party REST API hosted outside Snowflake to retrieve a specialized embedding not available in Cortex. What must be configured before the Streamlit app's Python code can make this outbound call?

A) Grant the Streamlit app owner the ACCOUNTADMIN role so outbound HTTPS calls are permitted
B) Create an external stage pointing to the API endpoint and use COPY INTO to pull the response into a table
C) Enable the CROSS_CLOUD_AUTO_SCALING account parameter to allow outbound network traffic from Streamlit apps
D) Create a network rule and secret, then create an External Access Integration and set it on the Streamlit object so egress is allowed

---

**Q36.** A pipeline needs to: (1) extract new document chunks using Cortex Search, (2) call an LLM via Cortex COMPLETE to summarize each chunk, and (3) load the summaries into a reporting table — running automatically every night with step (2) only starting after step (1) succeeds. Which native Snowflake orchestration approach should be used?

A) A single stored procedure that loops through all steps synchronously, scheduled to run manually by an operator each night
B) A Stream on the source table with no scheduling object, relying on the stream to trigger execution automatically at midnight
C) A DAG of Snowflake Tasks using AFTER dependencies, where each task executes the relevant SQL or Python step and the root task runs on a CRON schedule
D) A single Streamlit app with a background thread that polls the source table every few seconds and executes each step in order

---

**Q37.** A Snowflake account administrator wants to grant a subset of roles the ability to call SNOWFLAKE.CORTEX.COMPLETE and other Cortex LLM functions, while preventing all other roles from using them. Which approach correctly implements this access control?

A) Grant the SNOWFLAKE.CORTEX_USER database role to the target roles
B) Grant USAGE on the SNOWFLAKE.CORTEX schema to the target roles
C) Grant the EXECUTE TASK privilege to the target roles
D) Grant the ACCOUNTADMIN role to any user who needs Cortex access

---

**Q38.** A Snowflake admin needs to identify which roles and warehouses are consuming the most credits from Cortex LLM function calls (such as COMPLETE and SUMMARIZE) over the past 30 days. Which SNOWFLAKE.ACCOUNT_USAGE view should they query?

A) QUERY_HISTORY
B) WAREHOUSE_METERING_HISTORY
C) CORTEX_FUNCTIONS_USAGE_HISTORY
D) CORTEX_SEARCH_SERVING_USAGE_HISTORY

---

**Q39.** A column named CUSTOMER_NOTES has a masking policy that redacts its contents unless the querying role holds the UNMASK privilege. An analyst without UNMASK runs: SELECT SNOWFLAKE.CORTEX.SUMMARIZE(customer_notes) FROM support_tickets. What is the result?

A) The function bypasses the masking policy and summarizes the true underlying data
B) The function summarizes the masked/redacted value, since masking policies are enforced based on the querying role before the function executes
C) The query fails because Cortex functions cannot accept masked columns as input
D) The function automatically requests temporary elevated privileges to unmask the data for processing

---

**Q40.** What is the primary purpose of Cortex Guard when used alongside SNOWFLAKE.CORTEX.COMPLETE?

A) It caches previous LLM responses to reduce credit consumption on repeated prompts
B) It automatically fine-tunes the selected model using the account's historical query data
C) It filters or blocks potentially unsafe, harmful, or inappropriate content in LLM-generated responses
D) It encrypts prompts and responses in transit between the client and Snowflake

---

**Q41.** A prospective customer asks whether prompts and data sent to Snowflake Cortex LLM functions are used by Snowflake or the underlying model provider to train or improve general-purpose models. What is the accurate response?

A) Only prompts flagged as low-confidence responses are used for retraining
B) No, customer data submitted to Cortex functions is not used to train the underlying LLMs, and it remains within Snowflake's governance boundary
C) Yes, all inputs are continuously used to fine-tune the hosted models for the benefit of all customers
D) Only Cortex Search inputs are excluded from training; Cortex LLM function inputs are used for training

---

**Q42.** A team runs a high-volume pipeline that classifies millions of short support tickets into one of five categories using SNOWFLAKE.CORTEX.COMPLETE with the largest available model. Costs are far higher than expected. What is the MOST cost-effective change to reduce spend while maintaining acceptable accuracy for this simple classification task?

A) Switch to a smaller, less expensive Cortex LLM model suited to simpler tasks, such as an 8B-class model, instead of the largest available model
B) Continue using the large model but reduce the number of tickets processed per day
C) Increase the warehouse size so tickets are processed faster
D) Move the workload off Cortex functions and onto a custom GPU instance in Snowpark Container Services

---

**Q43.** A finance team needs to track and attribute LLM inference costs generated by calls to Snowflake Cortex functions (e.g., COMPLETE, SUMMARIZE) across different departments, since these functions run on Snowflake-managed serverless compute rather than a customer-managed virtual warehouse. Which approach BEST meets this requirement?

A) Create a resource monitor on the virtual warehouse executing the queries and set a credit quota
B) Enable the query acceleration service on the warehouse to reduce serverless compute costs
C) Query SNOWFLAKE.ACCOUNT_USAGE.ACCESS_HISTORY to determine which roles accessed the Cortex functions
D) Query SNOWFLAKE.ACCOUNT_USAGE.CORTEX_FUNCTIONS_USAGE_HISTORY, filtering and grouping by query tag or user to attribute token consumption

---

**Q44.** A healthcare application calls SNOWFLAKE.CORTEX.SUMMARIZE on a table column containing clinical notes that include patient names and identifiers. The generated summaries are stored in a downstream table accessible to analysts who should never see raw patient identifiers, even indirectly through the LLM output. Which approach BEST enforces this requirement while still allowing the Cortex function to operate on the data?

A) Apply a dynamic data masking policy on the identifier columns so masked values are what get passed into the Cortex function pipeline for non-privileged roles
B) Restrict SELECT on the source table to the ACCOUNTADMIN role only
C) Apply a network policy that limits which IP ranges can query the source table
D) Encrypt the clinical notes column using client-side encryption before loading it into Snowflake

---

**Q45.** A developer wants to enable Snowflake Cortex's built-in content-safety filtering (Cortex Guard) when calling SNOWFLAKE.CORTEX.COMPLETE to reduce the risk of harmful or unsafe model outputs in a customer-facing chatbot. Which option, passed in the function's options object, enables this behavior?

A) filter_content => 'enabled'
B) guardrails => true
C) moderation_level => 'high'
D) safe_mode => true

---

