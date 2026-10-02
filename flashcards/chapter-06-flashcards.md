## Chapter 6: Cortex Fine-Tuning and Model Evaluation — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | CORTEX.FINETUNE 'CREATE' required arguments (in order)? | operation ('CREATE'), new_model_name, base_model_name, training_data_query [, validation_data_query]. Exam tip: validation query is optional. |
| 2 | Minimum number of training examples for CORTEX.FINETUNE? | 10 examples minimum (documented floor). Exam tip: hundreds+ recommended for real quality gains — don't confuse minimum with recommended. |
| 3 | Fine-tuning vs RAG — when to use RAG? | Use RAG when you need up-to-date or proprietary facts/knowledge retrieved at query time. Exam tip: fast-changing data = RAG, not fine-tuning. |
| 4 | Fine-tuning vs prompt engineering — when to fine-tune? | When consistent output format/style/tone/domain-specific behavior isn't achievable via prompting, or to cut recurring token cost from long few-shot prompts. Exam tip: try prompt engineering first — it's cheapest and fastest to iterate. |
| 5 | Function to call a fine-tuned model for inference? | SNOWFLAKE.CORTEX.COMPLETE('<your_fine_tuned_model_name>', '<prompt>') — the same COMPLETE function used for base models. Exam tip: fine-tuned models are called just like base models, by name. |
| 6 | Best metric for evaluating summarization quality against reference text? | ROUGE (recall-oriented n-gram overlap). Exam tip: BLEU = translation-focused (precision), ROUGE = summarization-focused (recall). |
| 7 | What is Cortex Playground used for? | No-code UI in Snowsight to test/compare prompts across multiple Cortex LLMs interactively. Exam tip: it's for experimentation, not for launching fine-tuning jobs (that's SQL/FINETUNE). |
| 8 | Training data format required by CORTEX.FINETUNE? | A SQL query returning two columns: prompt and completion (matched pairs). Exam tip: data must come from a table/view queryable via SQL, not a raw file upload. |
| 9 | Cost driver of fine-tuning jobs in Snowflake? | Training job compute is billed based on the resources/time consumed during training, separate from ongoing per-token inference cost. Exam tip: fine-tuning has an upfront training cost PLUS ongoing inference cost — factor both into ROI vs prompt engineering. |
| 10 | Example of a base model family supported for CORTEX.FINETUNE? | Llama 3 family (e.g., llama3-8b) — open-weight models Snowflake hosts and can retrain. Exam tip: proprietary hosted models like GPT-4o/Claude/Gemini are NOT fine-tunable via Cortex. |
| 11 | How to check the status of a running fine-tuning job? | SNOWFLAKE.CORTEX.FINETUNE('DESCRIBE', '<job_id>') or ('SHOW') to list jobs. Exam tip: 'CREATE' starts a job asynchronously — you must poll/describe to check progress. |
| 12 | Decision shortcut: prompt engineering vs RAG vs fine-tuning? | Prompt engineering = fastest/cheapest, fixes tone/format quickly. RAG = adds fresh/proprietary knowledge. Fine-tuning = bakes in consistent behavior/style/domain skill, reduces long-prompt cost over time. Exam tip: try in that order — prompt → RAG → fine-tune. |
