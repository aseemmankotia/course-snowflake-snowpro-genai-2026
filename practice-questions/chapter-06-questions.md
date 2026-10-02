## Chapter 6: Cortex Fine-Tuning and Model Evaluation — Practice Questions

**Q1.** A support team's chatbot must always output responses in a strict JSON schema matching their ticketing system's categories, but prompt engineering with few-shot examples produces inconsistent formatting across thousands of daily calls. Which approach BEST solves this at scale?

A) Increase the few-shot examples in the prompt to 20 examples
B) Use Cortex Search to retrieve schema examples at query time
C) Fine-tune a base model on prompt-completion pairs demonstrating the exact JSON schema
D) Switch to a larger base model with a higher context window

<details>
<summary>Answer</summary>

**Correct: C**

Fine-tuning bakes the desired output format into model weights, producing consistent behavior at scale without per-call prompt overhead.

**Why the others are wrong:**
- More few-shot examples increase token cost and latency but don't guarantee consistency at scale; format drift still occurs.
- RAG retrieves knowledge/context, not formatting/style consistency; it doesn't constrain output structure.
- A bigger model doesn't guarantee schema adherence; this is a formatting/behavior problem, not a knowledge/size problem.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An e-commerce company wants its Cortex-powered assistant to answer questions using their product catalog, which changes hourly with new prices and inventory. Which approach is the BEST fit?

A) Fine-tune a base model nightly on the updated catalog
B) Add the entire catalog to the system prompt on every call
C) Fine-tune once and rely on the model's internal knowledge
D) Use Retrieval Augmented Generation (RAG) with Cortex Search over the live catalog table

<details>
<summary>Answer</summary>

**Correct: D**

RAG retrieves current data at query time, so answers always reflect live inventory/pricing without retraining.

**Why the others are wrong:**
- Nightly fine-tuning is expensive and slow, and still stale for hourly changes; fine-tuning is for behavior/style, not fast-changing facts.
- Embedding the entire catalog in every prompt is impractical and hits context/token limits, plus is costly per call.
- Fine-tuning bakes in knowledge at training time; the model would only know the catalog snapshot from when it was tuned.

</details>

---

**Q3.** An enterprise sends a 2,000-token few-shot prompt with every Cortex COMPLETE call to enforce a specific tone and structure, driving up per-call cost. Which change reduces per-call cost while keeping the same output quality?

A) Fine-tune a model on examples of the desired tone/structure, then call it with a short prompt
B) Switch to RAG to retrieve tone examples
C) Cache the prompt to avoid re-sending it
D) Truncate the few-shot examples to reduce tokens

<details>
<summary>Answer</summary>

**Correct: A**

Fine-tuning internalizes tone/structure into the model, letting you drop most of the few-shot prompt and reduce cost per call.

**Why the others are wrong:**
- RAG is for injecting knowledge/context, not for teaching consistent tone/structure; it wouldn't meaningfully reduce prompt size for this purpose.
- Independent inference calls each incur token cost; there is no mechanism to 'cache' a prompt to avoid resending it and still guarantee behavior.
- Truncating few-shot examples would likely degrade output quality/consistency, defeating the goal.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which of the following is a base model officially supported for fine-tuning via SNOWFLAKE.CORTEX.FINETUNE?

A) gpt-4o
B) claude-3-5-sonnet
C) gemini-1.5-pro
D) llama3-8b

<details>
<summary>Answer</summary>

**Correct: D**

Snowflake Cortex fine-tuning supports select open-weight models hosted in Cortex, including the Llama 3 family (e.g., llama3-8b), which Snowflake can retrain and host.

**Why the others are wrong:**
- GPT-4o is an OpenAI-hosted proprietary model not available for fine-tuning inside Cortex.
- Claude models are Anthropic-hosted and not exposed for Cortex fine-tuning.
- Gemini is a Google-hosted proprietary model, not part of Cortex's fine-tunable model set.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team is evaluating a fine-tuned model's ability to generate accurate document summaries compared to human-written reference summaries. Which metric is BEST suited to measure this?

A) BLEU
B) Perplexity
C) ROUGE
D) Token throughput

<details>
<summary>Answer</summary>

**Correct: C**

ROUGE measures recall-oriented overlap between generated summaries and reference summaries, making it the standard metric for summarization quality.

**Why the others are wrong:**
- BLEU is precision-oriented and designed primarily for machine translation, not summarization.
- Perplexity measures how well a model predicts text statistically, not how well output matches a human reference for a specific task.
- Token throughput measures inference speed/cost, not text quality.

</details>

---

**Q6.** Which SQL statement correctly starts a fine-tuning job using SNOWFLAKE.CORTEX.FINETUNE?

A) SELECT SNOWFLAKE.CORTEX.FINETUNE('TRAIN', 'mistral-7b', 'support_bot_v1', training_data)
B) CALL SNOWFLAKE.CORTEX.FINETUNE(model => 'mistral-7b', name => 'support_bot_v1')
C) SELECT SNOWFLAKE.CORTEX.FINETUNE('CREATE', 'support_bot_v1', 'mistral-7b', 'SELECT prompt, completion FROM train_data')
D) SELECT SNOWFLAKE.CORTEX.COMPLETE('FINETUNE', 'mistral-7b', 'support_bot_v1')

<details>
<summary>Answer</summary>

**Correct: C**

The CREATE operation takes the new model name, base model name, and a SQL query supplying prompt/completion training data (validation data query optional).

**Why the others are wrong:**
- 'TRAIN' is not a valid operation keyword for FINETUNE; argument order and syntax are wrong.
- FINETUNE is a SQL function called with SELECT and positional string args, not a CALL procedure with named parameters in this form.
- COMPLETE is used for inference, not for initiating a fine-tuning job.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What is the minimum number of prompt-completion examples required in the training dataset when using SNOWFLAKE.CORTEX.FINETUNE?

A) 10
B) 1
C) 100
D) 1000

<details>
<summary>Answer</summary>

**Correct: A**

Snowflake's documented minimum for a fine-tuning training set is 10 examples, though far more (hundreds+) are recommended for meaningful quality gains.

**Why the others are wrong:**
- A single example is not accepted; the platform enforces a minimum threshold above 1.
- 100 is a reasonable practical recommendation but not the documented hard minimum.
- 1000 is a good target for quality but not the required minimum to start a job.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary purpose of Cortex Playground in Snowsight?

A) To interactively test and compare prompts across different Cortex LLMs without writing code
B) To write and schedule SQL fine-tuning jobs
C) To monitor compute credit consumption for Cortex functions
D) To manage role-based access control for Cortex functions

<details>
<summary>Answer</summary>

**Correct: A**

Cortex Playground provides a no-code UI to enter prompts and compare responses/behavior across available Cortex models side by side.

**Why the others are wrong:**
- Fine-tuning jobs are launched via SQL (the FINETUNE function), not through Playground.
- Credit/cost monitoring is done through Snowsight's usage/account views, not Playground.
- RBAC is managed through standard Snowflake grants/roles, unrelated to Playground.

</details>

---

