## Chapter 7: Embeddings and Vector Search on Snowflake — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Snowflake Cortex function to embed text into a 768-dim vector? | SNOWFLAKE.CORTEX.EMBED_TEXT_768(model, text). Exam tip: model name comes FIRST, text SECOND — reversed order is a common wrong answer. |
| 2 | Snowflake Cortex function to embed text into a 1024-dim vector? | SNOWFLAKE.CORTEX.EMBED_TEXT_1024(model, text). Dimension is fixed by the function name suffix (768 or 1024), not a passed parameter. |
| 3 | How do you declare a column to store 768-dim float embeddings in Snowflake? | col_name VECTOR(FLOAT, 768). Syntax is VECTOR(<element_type>, <dimension>) — element type is FLOAT or INT. |
| 4 | Function to compute cosine similarity between two VECTOR columns? | VECTOR_COSINE_SIMILARITY(v1, v2). Returns value in range -1 to 1; HIGHER = more similar. |
| 5 | Function to compute Euclidean (L2) distance between two VECTOR values? | VECTOR_L2_DISTANCE(v1, v2). Returns a distance; LOWER = more similar (opposite semantics from cosine similarity). |
| 6 | Cosine similarity vs L2 distance — when does the exam expect cosine? | Cosine similarity is preferred when only the direction (semantic meaning) matters and vector magnitude may vary — the standard choice for most text embedding retrieval tasks. |
| 7 | Best practice: combining vector search with a SQL WHERE filter on metadata (e.g., region, status)? | Apply the WHERE filter BEFORE/together with the ORDER BY distance function so the engine prunes rows first, reducing the number of distance computations. Exam trap: computing distance over the full table before filtering. |
| 8 | Does Snowflake VECTOR search use an ANN index like HNSW by default? | No — vector similarity queries are computed as a scan using the distance function per row (no dedicated ANN index by default). This is why predicate pushdown/filtering matters for performance at scale. |
| 9 | VECTOR_INNER_PRODUCT — what does it compute and when is it used? | Computes the dot product of two vectors. Common with embeddings that are not normalized or where raw magnitude carries meaning; higher value generally = more similar for normalized vectors. |
| 10 | Choosing embedding dimensionality — what's the core exam tradeoff? | Higher dimensions (e.g., 1024) can capture more semantic nuance but increase storage size and distance-computation cost; lower dimensions (e.g., 768) are cheaper/faster but may lose some accuracy. Pick based on task requirements. |
| 11 | What does an embedding vector numerically represent? | A fixed-length array of floats representing semantic meaning in a high-dimensional space — similar texts map to vectors close together by distance/similarity functions. |
| 12 | Two example Cortex embedding models and their dimensions? | e5-base-v2 → 768 dims (used with EMBED_TEXT_768); snowflake-arctic-embed-l-v2.0 → 1024 dims (used with EMBED_TEXT_1024). Model choice must match the correct EMBED_TEXT function. |
