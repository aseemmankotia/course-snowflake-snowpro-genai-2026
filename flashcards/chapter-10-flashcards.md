## Chapter 10: Streamlit in Snowflake and Application Orchestration — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Which SQL clauses are required to bind a Streamlit in Snowflake app's entry file and compute warehouse? | ROOT_LOCATION (stage path), MAIN_FILE (entry .py file), QUERY_WAREHOUSE (bound warehouse). Exam tip: forgetting QUERY_WAREHOUSE is a common syntax trap. |
| 2 | SQL function used inside a Task's WHEN clause to check if a Stream has unconsumed changes? | SYSTEM$STREAM_HAS_DATA('<stream_name>') — returns BOOLEAN. Lets a Task skip execution (and cost) when there's no new data. |
| 3 | st.cache_data vs st.cache_resource — when to use each? | cache_data: serializable data (DataFrames, query results), returns a copy each call. cache_resource: non-serializable shared objects (ML models, DB connections), returns the same singleton instance. |
| 4 | Serverless Task vs user-managed Task? | Serverless: Snowflake auto-manages/sizes compute, no warehouse needed. User-managed: task runs on a customer-specified virtual warehouse. Exam tip: serverless simplifies ops but user-managed gives explicit sizing control. |
| 5 | Can Streamlit in Snowflake install arbitrary pip packages from PyPI? | No — package selection is limited to the curated Snowflake Anaconda channel via the package picker. Exam gotcha for 'integrating open-source components' questions. |
| 6 | How do you grant in-account access to a Streamlit in Snowflake app? | GRANT USAGE ON STREAMLIT <app_name> TO ROLE <role>; Access is role-based, same-account only — not cross-account sharing. |
| 7 | Typical RAG pipeline pattern built with native Snowflake objects? | Stage/parse docs → EMBED_TEXT_768/1024 to generate vectors → store in table with vector column → similarity search retrieves context → COMPLETE generates final answer in Streamlit UI. |
| 8 | How is a multi-step Task DAG chained together? | Use CREATE TASK ... AFTER <predecessor_task> to define dependency order; root task carries the SCHEDULE (CRON or interval). |
| 9 | Does st.session_state persist across different user sessions or browser reloads? | No — session_state is scoped to a single active session; it resets on a new session/reload. Not a substitute for cache_resource/cache_data. |
| 10 | Default Stream/table data retention period (Standard Edition) affecting how long a Stream stays valid? | 1 day by default (DATA_RETENTION_TIME_IN_DAYS), extendable up to 90 days on Enterprise Edition+. Exam tip: stream can go stale if retention period is exceeded without consuming offset. |
| 11 | Minimum scheduling interval allowed for a Snowflake Task? | 1 minute. Exam tip: tasks cannot be scheduled more frequently than this. |
| 12 | Cortex function to use for direct chat/completion generation vs summarization vs sentiment? | COMPLETE = free-form generation/chat. SUMMARIZE = condense text. SENTIMENT = polarity score. Exam tests picking the right single-purpose function per scenario. |
