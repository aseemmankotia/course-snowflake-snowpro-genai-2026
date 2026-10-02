## Chapter 2: Prompting, Model Selection, and LLM Limitations — Practice Questions

**Q1.** A retail company is building an internal chatbot that answers thousands of simple FAQ questions per day (store hours, return policy, shipping times). Latency and cost per call are the top priority, and the answers only require basic language understanding. Which model choice BEST meets these requirements when calling SNOWFLAKE.CORTEX.COMPLETE?

A) A large, high-parameter model such as llama3.1-405b for maximum accuracy
B) Bypass Cortex entirely and use a fine-tuned custom model hosted outside Snowflake
C) mistral-large2 with max_tokens set to the highest possible value
D) A small, low-parameter model such as llama3.1-8b, tuned with a clear system prompt

<details>
<summary>Answer</summary>

**Correct: D**

Small models have lower latency and lower credit cost per call and are sufficient for simple, well-scoped FAQ tasks when guided by a strong system prompt.

**Why the others are wrong:**
- A 405b-class model adds latency and cost with no accuracy benefit for a simple, bounded task.
- External hosting outside Cortex loses governance, adds operational overhead, and is not what the exam tests as best practice.
- Raising max_tokens does not affect suitability for a simple task and increases cost/latency risk unnecessarily.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A support tool must answer questions using only the company's internal knowledge base, and the answers must be grounded in facts that were created after the LLM's training cutoff. Which approach BEST reduces hallucination and addresses the recency limitation?

A) Use retrieval-augmented generation (e.g., Cortex Search) to inject relevant internal documents into the prompt at query time
B) Increase the temperature parameter so the model generates more varied, creative answers
C) Switch to the largest available Cortex-hosted model, since larger models never hallucinate
D) Ask the model the same question multiple times and average the responses

<details>
<summary>Answer</summary>

**Correct: A**

RAG grounds the model in current, authoritative source data at inference time, directly addressing both the knowledge-cutoff and hallucination problems.

**Why the others are wrong:**
- Higher temperature increases randomness/creativity, which increases hallucination risk rather than reducing it.
- Model size reduces but never eliminates hallucination; it does not solve the recency problem at all.
- Repeated sampling and averaging does not inject new factual knowledge and is not a recognized mitigation pattern.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A data science team needs to perform complex multi-step legal reasoning over long contracts, and cost is a secondary concern compared to answer quality. Which model selection strategy is MOST appropriate?

A) Use the smallest available model with a longer, more detailed prompt to compensate
B) Use the default model with temperature set to 1 for best reasoning
C) Use the largest, most capable Cortex-hosted model suited for complex reasoning (e.g., a 70b+ class model)
D) Use any model, since all Cortex-hosted LLMs produce equivalent reasoning quality

<details>
<summary>Answer</summary>

**Correct: C**

Larger, higher-capacity models handle complex multi-step reasoning more reliably; when cost is secondary, accuracy should drive model choice.

**Why the others are wrong:**
- Prompt length cannot fully compensate for a small model's limited reasoning capacity on complex tasks.
- Temperature controls randomness, not reasoning capability, and high temperature can reduce coherence in complex tasks.
- Model size and architecture materially affect reasoning quality; they are not equivalent.

</details>

---

**Q4.** Which statement correctly distinguishes a system prompt from a user prompt in an LLM API call?

A) The system prompt is optional metadata ignored by the model; only the user prompt affects output
B) The user prompt configures generation parameters like temperature; the system prompt contains the question
C) The system prompt sets persistent behavior, role, and constraints for the model; the user prompt carries the specific request or question
D) System and user prompts are interchangeable and have no defined distinction in Cortex-hosted models

<details>
<summary>Answer</summary>

**Correct: C**

The system prompt establishes persona, tone, and rules the model should follow throughout the interaction, while the user prompt supplies the actual query or task.

**Why the others are wrong:**
- System prompts directly influence model behavior and are not ignored.
- Generation parameters like temperature are set via API options, not embedded in the user prompt text.
- The two roles have distinct, well-defined purposes and are not interchangeable.

</details>

---

**Q5.** A prompt asks the model to classify sentiment without providing any labeled examples in the prompt text. What prompting technique is being used?

A) Few-shot prompting
B) Zero-shot prompting
C) Fine-tuning
D) Chain-of-thought prompting

<details>
<summary>Answer</summary>

**Correct: B**

Zero-shot prompting asks the model to perform a task using only instructions, with no example input/output pairs provided.

**Why the others are wrong:**
- Few-shot prompting requires including example input/output pairs in the prompt, which is absent here.
- Fine-tuning modifies model weights via training data; it is not a prompting technique applied at inference time.
- Chain-of-thought prompting explicitly asks the model to show reasoning steps, which is a separate technique from zero-shot.

</details>

---

**Q6.** An analyst needs a Cortex COMPLETE call to produce highly consistent, repeatable output for a factual data-extraction task where the same input should reliably yield the same output. Which parameter adjustment is MOST appropriate?

A) Set temperature to a low value near 0 to minimize randomness
B) Set temperature to a high value near 1 to maximize output diversity
C) Increase max_tokens to force a longer, more complete answer
D) Remove the system prompt so the model responds more literally

<details>
<summary>Answer</summary>

**Correct: A**

Lowering temperature reduces sampling randomness, producing more deterministic and repeatable output — appropriate for factual extraction tasks.

**Why the others are wrong:**
- High temperature increases variability, which is the opposite of what a consistency requirement needs.
- max_tokens controls output length, not determinism or consistency of content.
- Removing the system prompt does not affect determinism and may reduce output quality/consistency.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A prompt engineer includes two example question/answer pairs before the actual question to show the model the desired answer format. What is this technique called?

A) Few-shot prompting
B) Zero-shot prompting
C) System prompting
D) Temperature scaling

<details>
<summary>Answer</summary>

**Correct: A**

Few-shot prompting provides a small number of labeled examples within the prompt to guide the model's response format and behavior.

**Why the others are wrong:**
- Zero-shot prompting by definition provides no examples, which contradicts the scenario.
- System prompting refers to the separate role used for persistent instructions, not example pairs within the user content.
- Temperature scaling is a generation parameter unrelated to providing examples.

</details>

---

**Q8.** Which of the following is a well-known, inherent limitation of large language models that exam scenarios commonly test?

A) LLMs always retrieve real-time internet data automatically during inference
B) LLMs can generate plausible-sounding but factually incorrect statements, known as hallucination
C) LLMs guarantee identical output every time regardless of temperature setting
D) LLMs have no fixed training data cutoff and are always fully current

<details>
<summary>Answer</summary>

**Correct: B**

Hallucination — generating confident but incorrect or fabricated content — is a well-documented, exam-tested LLM limitation.

**Why the others are wrong:**
- LLMs do not automatically access live internet/external data unless explicitly connected via a retrieval or search integration.
- Output determinism depends on the temperature parameter; it is not guaranteed identical by default.
- LLMs are trained on data up to a fixed knowledge cutoff and are not automatically current.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

