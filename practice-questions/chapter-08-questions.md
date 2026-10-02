## Chapter 8: Chunking, RAG Patterns, and Unstructured Data Prep — Practice Questions

**Q1.** A team is building a RAG system over a 300-page technical manual where procedures often reference steps defined several paragraphs earlier. Retrieval quality is poor because chunks cut procedures in half. Which chunking adjustment BEST addresses this?

A) Increase chunk overlap and use structure-aware (recursive/semantic) splitting on section boundaries instead of fixed character counts
B) Reduce chunk size to 100 tokens to increase retrieval precision
C) Remove overlap entirely and rely on a larger embedding model
D) Chunk the document once per page regardless of content boundaries

<details>
<summary>Answer</summary>

**Correct: A**

Structure-aware splitting keeps semantically related content (a full procedure) together, and overlap preserves context across boundary cuts — both directly fix broken references.

**Why the others are wrong:**
- Shrinking chunk size makes context loss worse, not better, for multi-step procedures
- Removing overlap increases the chance of losing referenced context; embedding model size doesn't fix chunk boundary problems
- Fixed page-based chunking ignores logical structure and will still split procedures arbitrarily

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A retail company wants users to filter RAG answers to only their region's product catalog before generation happens. Which design choice enables this?

A) Increase the number of retrieved chunks (top-k) so all regions are represented
B) Fine-tune the LLM separately for each region
C) Tag each chunk with region metadata at ingestion time and apply a metadata filter during retrieval
D) Store all documents in one large chunk to simplify retrieval

<details>
<summary>Answer</summary>

**Correct: C**

Metadata tagging at ingestion (region, date, department, etc.) allows the retriever to apply filters and return only relevant chunks — the standard filtered-retrieval pattern in RAG.

**Why the others are wrong:**
- Increasing top-k retrieves more irrelevant results, it doesn't isolate a region
- Fine-tuning per region is costly and unrelated to retrieval filtering — RAG's advantage is avoiding this
- One large chunk destroys retrieval granularity entirely

</details>

---

**Q3.** A company has two data sources for a Gen AI application: a SQL table of customer orders and a folder of unstructured support-ticket PDFs. Which data prep approach is correct?

A) Convert both sources into embeddings directly without preprocessing
B) Apply feature framing/cleaning to the structured order data for direct querying or tool use, and extract/chunk/embed the unstructured PDFs for RAG retrieval
C) Chunk the SQL table the same way as the PDFs since both feed the same LLM
D) Only prepare the PDFs since structured data does not need any preparation

<details>
<summary>Answer</summary>

**Correct: B**

Structured data is typically cleaned and reshaped for direct querying, aggregation, or as tool/function-call input, while unstructured text requires extraction, chunking, and embedding for RAG — the two paths are handled differently by design.

**Why the others are wrong:**
- Structured tabular rows are not naturally chunk-and-embed candidates; skipping prep produces poor retrieval and poor query accuracy
- SQL rows lack the semantic continuity that chunking is designed to preserve — chunking a table the same way is inappropriate
- Structured data still requires cleaning (nulls, duplicates, schema normalization) before use

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which step, when performed BEFORE adding a generation model to a new RAG pipeline, is the correct way to validate the pipeline is ready for production?

A) Deploy the full pipeline to production and monitor user complaints
B) Skip evaluation since the LLM will correct any retrieval errors
C) Manually inspect retrieved chunks for a labeled set of test queries and measure retrieval relevance (e.g., precision/recall or hit rate) before wiring in generation
D) Only evaluate the embedding model's benchmark score in isolation

<details>
<summary>Answer</summary>

**Correct: C**

Evaluating retrieval quality independently (relevance/hit-rate on known queries) isolates retrieval problems before generation masks or compounds them — a core RAG evaluation best practice.

**Why the others are wrong:**
- Waiting for user complaints in production is reactive, not a validation step
- LLMs generate plausible-sounding answers even from irrelevant context, hiding retrieval failures rather than correcting them
- A generic embedding benchmark doesn't reflect performance on the specific corpus and query set in use

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Which technique is most appropriate for extracting usable text from a mixed-layout PDF containing tables, multi-column text, and embedded images before chunking?

A) Direct byte-level reading of the PDF file with no parsing
B) Feeding the raw PDF binary directly into the embedding model
C) Converting the PDF to a single unformatted string using basic text extraction with no structure awareness
D) A layout-aware document/PDF parser that separates text blocks, tables, and reading order prior to chunking

<details>
<summary>Answer</summary>

**Correct: D**

Layout-aware parsing correctly separates tables, columns, and reading order, which is necessary before meaningful chunking — this is the recommended light-overview approach for semi-structured/PDF prep.

**Why the others are wrong:**
- Raw byte reading produces unusable, non-textual data
- Embedding models expect text input, not raw PDF binaries
- Basic unstructured extraction often garbles multi-column reading order and merges table cells incorrectly, degrading chunk quality

</details>

---

**Q6.** A team sets chunk overlap to 0% across all documents to save on embedding costs. What is the most likely downstream problem in the RAG pipeline?

A) Embedding costs will increase instead of decrease
B) Context that spans a chunk boundary may be lost, causing the retriever to miss relevant information split across adjacent chunks
C) The retriever will return duplicate chunks for every query
D) The generation model will refuse to answer without overlap

<details>
<summary>Answer</summary>

**Correct: B**

Overlap exists specifically to preserve continuity of meaning across chunk boundaries; zero overlap risks splitting a relevant sentence or fact between two chunks so neither is fully retrievable.

**Why the others are wrong:**
- Zero overlap reduces the total number of chunks/tokens embedded, lowering cost, not raising it
- Overlap amount doesn't cause duplicate retrieval — that's a separate deduplication issue
- Generation models don't inspect overlap settings; they only see whatever chunks are retrieved

</details>

---

**Q7.** An engineering team is chunking documentation for embedding, where the embedding model has a hard input token limit. Which chunk size configuration is most appropriate?

A) Set chunk size well above the model's token limit to reduce chunk count
B) Set chunk size to exactly the model's max token limit with no margin
C) Set chunk size safely below the model's max token limit, accounting for tokenizer overhead, so no chunk is silently truncated
D) Chunk size is irrelevant as long as overlap is configured

<details>
<summary>Answer</summary>

**Correct: C**

Leaving margin below the hard limit prevents silent truncation caused by tokenizer variance (e.g., special tokens, metadata prepended to the chunk), which is a common real-world gotcha.

**Why the others are wrong:**
- Exceeding the limit causes truncation or outright request failure
- Exact-limit sizing leaves no margin for tokenizer overhead or added metadata, risking truncation
- Overlap does not compensate for chunks that exceed the model's token limit

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Why must raw documents be chunked before embedding in a RAG pipeline?

A) Embedding models have limited input context length, and smaller, semantically coherent chunks improve retrieval precision by matching queries to focused content rather than an entire document
B) Embedding models can only process a single word at a time
C) Chunking is only needed to reduce storage costs, not for retrieval quality
D) Chunking is required by law for compliance with data protection regulations

<details>
<summary>Answer</summary>

**Correct: A**

Chunking addresses both the practical constraint of embedding input limits and the retrieval-quality goal of returning focused, relevant passages instead of whole documents.

**Why the others are wrong:**
- Embedding models process token sequences, not single words
- Storage savings are a minor side effect, not the primary reason for chunking
- Chunking is a technical/architectural practice, not a legal requirement

</details>

---

