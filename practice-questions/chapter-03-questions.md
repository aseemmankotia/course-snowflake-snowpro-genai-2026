## Chapter 3: Cortex LLM Functions: COMPLETE, SUMMARIZE, TRANSLATE, and More — Practice Questions

**Q1.** A company wants to generate a one-sentence summary of thousands of long customer support transcripts stored in a table column, without specifying a model or writing a custom prompt. Which Cortex function BEST meets this requirement?

A) SNOWFLAKE.CORTEX.COMPLETE with a summarization prompt
B) SNOWFLAKE.CORTEX.SUMMARIZE
C) SNOWFLAKE.CORTEX.EXTRACT_ANSWER
D) SNOWFLAKE.CORTEX.CLASSIFY_TEXT

<details>
<summary>Answer</summary>

**Correct: B**

SUMMARIZE is a purpose-built, single-argument function that takes only the text and returns a condensed version — no model selection or prompt engineering required.

**Why the others are wrong:**
- COMPLETE requires choosing a model and writing/maintaining a prompt — more overhead and inconsistent output for a simple summarization task
- EXTRACT_ANSWER requires a specific question against the text; it does not produce a general summary
- CLASSIFY_TEXT assigns a category label, it does not condense text

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Which SQL statement correctly calls SNOWFLAKE.CORTEX.COMPLETE to generate a response using the 'mistral-large' model?

A) SELECT SNOWFLAKE.CORTEX.COMPLETE('Generate a reply', 'mistral-large')
B) SELECT SNOWFLAKE.CORTEX.COMPLETE('mistral-large', 'Generate a reply')
C) SELECT SNOWFLAKE.CORTEX.COMPLETE(model => 'Generate a reply', prompt => 'mistral-large')
D) SELECT SNOWFLAKE.CORTEX.COMPLETE(prompt = 'mistral-large')

<details>
<summary>Answer</summary>

**Correct: B**

COMPLETE's signature is COMPLETE(<model>, <prompt>) — the model name is always the first argument, the prompt text is the second.

**Why the others are wrong:**
- This reverses the argument order — Snowflake would treat 'Generate a reply' as the model name and fail
- This swaps the named arguments incorrectly — model and prompt values are reversed
- This omits the required model argument and uses invalid single-equals syntax instead of =>

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A support ticket in Portuguese needs to be converted to English before being processed by a downstream English-only classification model. Which function should run FIRST in the pipeline?

A) SNOWFLAKE.CORTEX.SENTIMENT
B) SNOWFLAKE.CORTEX.CLASSIFY_TEXT
C) SNOWFLAKE.CORTEX.TRANSLATE
D) SNOWFLAKE.CORTEX.EXTRACT_ANSWER

<details>
<summary>Answer</summary>

**Correct: C**

TRANSLATE(text, source_lang, target_lang) is the dedicated function for converting text between languages and should run before any English-only downstream step.

**Why the others are wrong:**
- SENTIMENT scores emotional polarity, it does not translate language
- CLASSIFY_TEXT would misclassify Portuguese text against English categories since it doesn't translate first
- EXTRACT_ANSWER pulls an answer to a question from text, it has no translation capability

</details>

---

**Q4.** A data engineer needs to pull the exact shipping date mentioned inside a block of unstructured order notes, in response to the specific question 'When does this order ship?' Which function is the BEST fit?

A) SNOWFLAKE.CORTEX.EXTRACT_ANSWER
B) SNOWFLAKE.CORTEX.SUMMARIZE
C) SNOWFLAKE.CORTEX.COMPLETE with a generic prompt
D) SNOWFLAKE.CORTEX.SENTIMENT

<details>
<summary>Answer</summary>

**Correct: A**

EXTRACT_ANSWER(text, question) is purpose-built for extractive Q&A over a passage and returns the answer plus a confidence score — exactly this use case.

**Why the others are wrong:**
- SUMMARIZE condenses the whole text but does not target a specific question
- COMPLETE could technically do this via a custom prompt, but it's less deterministic and more expensive than the purpose-built function — not the BEST fit when a dedicated function exists
- SENTIMENT only returns an emotional polarity score, unrelated to fact extraction

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** What is the valid output range returned by SNOWFLAKE.CORTEX.SENTIMENT for a given text input?

A) 0 to 100
B) 'positive', 'neutral', 'negative'
C) 0 to 1
D) -1 to 1

<details>
<summary>Answer</summary>

**Correct: D**

SENTIMENT returns a FLOAT score ranging from -1 (most negative) to 1 (most positive), with values near 0 indicating neutral sentiment.

**Why the others are wrong:**
- 0-100 is not the scale used; this resembles a percentage-based scoring system used by other tools, not Cortex SENTIMENT
- SENTIMENT returns a numeric score, not a text label — that behavior would apply to a CLASSIFY_TEXT-based approach instead
- 0 to 1 omits the negative range and does not reflect how polarity is expressed

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** Which CLASSIFY_TEXT call correctly categorizes a product review into one of three custom categories?

A) SNOWFLAKE.CORTEX.CLASSIFY_TEXT(review_text, ['Positive','Negative','Neutral'])
B) SNOWFLAKE.CORTEX.CLASSIFY_TEXT(review_text, 'Positive, Negative, Neutral')
C) SNOWFLAKE.CORTEX.CLASSIFY_TEXT(['Positive','Negative','Neutral'], review_text)
D) SNOWFLAKE.CORTEX.CLASSIFY_TEXT(review_text)

<details>
<summary>Answer</summary>

**Correct: A**

CLASSIFY_TEXT takes the text as the first argument and an array of category labels as the second argument.

**Why the others are wrong:**
- Categories must be passed as an array literal, not a single comma-delimited string
- This reverses the required argument order — text must come first, categories second
- This omits the required categories argument entirely; CLASSIFY_TEXT cannot infer categories without them

</details>

---

**Q7.** A team runs SNOWFLAKE.CORTEX.COMPLETE over 50 million rows nightly using 'llama3-70b' and notices high credit consumption. Which change would MOST reduce cost while keeping the same general task (short text classification tags)?

A) Switch to a smaller/cheaper model or use CLASSIFY_TEXT instead of COMPLETE
B) Increase the max_tokens parameter to speed up processing
C) Run the same query more frequently to spread out load
D) Switch to SUMMARIZE since it processes text faster

<details>
<summary>Answer</summary>

**Correct: A**

Cortex functions bill based on tokens processed and the chosen model's per-token rate; larger models cost more per token. Using a smaller model, or a purpose-built lower-cost function like CLASSIFY_TEXT instead of a large general-purpose LLM via COMPLETE, directly reduces consumption for a simple categorization task.

**Why the others are wrong:**
- Increasing max_tokens increases potential output length and cost, it does not reduce it
- Running the same workload more often increases total token processing and cost, not decreases it
- SUMMARIZE performs a different task (condensation, not classification) and is not a valid substitute here

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A developer wants to generate a creative marketing paragraph from a short list of product features, with full control over tone and style via prompt engineering. Which function should be used?

A) SNOWFLAKE.CORTEX.SUMMARIZE
B) SNOWFLAKE.CORTEX.EXTRACT_ANSWER
C) SNOWFLAKE.CORTEX.COMPLETE
D) SNOWFLAKE.CORTEX.CLASSIFY_TEXT

<details>
<summary>Answer</summary>

**Correct: C**

COMPLETE is the general-purpose LLM function that accepts a free-form prompt and a chosen model, giving full control over tone, style, and generation behavior — required for open-ended creative generation.

**Why the others are wrong:**
- SUMMARIZE only condenses existing text, it cannot generate new creative content from a feature list
- EXTRACT_ANSWER retrieves an answer to a question from existing text, it does not generate new prose
- CLASSIFY_TEXT assigns a category label, it has no generative capability

</details>

---

