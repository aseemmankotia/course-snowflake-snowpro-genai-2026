## Streamlit in Snowflake (SiS) & Orchestration — Cheat Sheet

### Creating a Streamlit App
```sql
CREATE STREAMLIT my_app
  ROOT_LOCATION = '@my_stage'
  MAIN_FILE = 'streamlit_app.py'
  QUERY_WAREHOUSE = 'MY_WH';

GRANT USAGE ON STREAMLIT my_app TO ROLE analyst_role;
```
- **ROOT_LOCATION**: stage holding app files
- **MAIN_FILE**: entry Python script
- **QUERY_WAREHOUSE**: warehouse used for all queries the app issues
- Sharing is **role-based, in-account only** (GRANT USAGE). Cross-account distribution requires the Native App Framework.

### Calling Cortex from Streamlit
| Function | Purpose |
|---|---|
| COMPLETE | Chat/text generation |
| EMBED_TEXT_768 / EMBED_TEXT_1024 | Generate embeddings for retrieval |
| SUMMARIZE | Condense long text |
| SENTIMENT | Sentiment score |
| EXTRACT_ANSWER | Q&A over a passage |
| TRANSLATE | Language translation |

Call via `session.sql("SELECT SNOWFLAKE.CORTEX.COMPLETE(...)").collect()` inside the app.

### Caching in Streamlit
| Decorator | Use case | Returns |
|---|---|---|
| `st.cache_data` | Serializable data (query results, DataFrames) | Copy each call |
| `st.cache_resource` | Non-serializable objects (models, connections) | Same singleton |
| `st.session_state` | Per-session UI state (chat history) | Not a cache — resets per session |

### Orchestration: Streams + Tasks
```sql
CREATE STREAM my_stream ON TABLE staging_tbl;

CREATE TASK summarize_task
  WAREHOUSE = MY_WH
  SCHEDULE = '5 MINUTE'
WHEN SYSTEM$STREAM_HAS_DATA('my_stream')
AS
  INSERT INTO summaries
  SELECT SNOWFLAKE.CORTEX.SUMMARIZE(text) FROM my_stream;

ALTER TASK summarize_task RESUME;
```
- Tasks are created **SUSPENDED** by default — must `RESUME` explicitly.
- Chain steps: `CREATE TASK step2 ... AFTER step1 ...`
- **Serverless tasks**: no warehouse; Snowflake auto-sizes compute (`USER_TASK_MANAGED_INITIAL_SIZE`).
- **User-managed tasks**: bound to a specified warehouse you size/scale yourself.
- Minimum schedule interval: **1 minute**.
- Stream validity tied to table's `DATA_RETENTION_TIME_IN_DAYS` (default 1 day, up to 90 on Enterprise+); consuming the stream in a DML transaction advances its offset.

### External / Open-Source Components
- SiS package picker installs only from the **Snowflake Anaconda channel** — arbitrary PyPI/pip packages are not directly installable.
- For full custom environments/dependencies, consider Snowpark Container Services instead of SiS.

### RAG Chatbot Packaging Pattern
1. Ingest & parse documents → stage
2. `EMBED_TEXT_*` → populate vector column
3. Similarity search (vector distance function) → retrieve top-k chunks
4. `COMPLETE` with retrieved context → generate answer
5. Streamlit front end: `st.chat_input` / `st.chat_message`, cache the embedding/model handle with `cache_resource`, store conversation in `session_state`
6. Bind app to right-sized warehouse; set aggressive auto-suspend to control cost between sessions.
