## Chapter 9: Snowpark Container Services for Gen AI Workloads — Practice Questions

**Q1.** A team wants to serve a fine-tuned open-source vision-language model (not available in Cortex) as a low-latency, always-on inference endpoint accessed by an external application. Which approach BEST meets this requirement?

A) Create a long-running SPCS service on a GPU compute pool with a public endpoint defined in the service specification
B) Call the model through the Cortex COMPLETE function using a custom model name parameter
C) Use EXECUTE JOB SERVICE to run the model container once per request
D) Load the model weights into a Snowpark Python UDF running on a virtual warehouse

<details>
<summary>Answer</summary>

**Correct: A**

A persistent, low-latency endpoint requires a long-running SPCS service (CREATE SERVICE) on a GPU-backed compute pool, with the endpoint exposed via the service spec.

**Why the others are wrong:**
- Cortex COMPLETE only works with Snowflake-hosted/supported models, not arbitrary custom or open-source models
- Job services run to completion and stop — they are not designed for standing, low-latency request/response endpoints
- Virtual warehouses and standard UDFs do not provide GPU access or support arbitrary containerized model runtimes

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A company needs to score 10 million rows nightly using a custom containerized model, and wants to minimize compute cost since inference only happens once a day. Which SPCS deployment pattern is MOST cost-efficient?

A) EXECUTE JOB SERVICE on a compute pool that suspends automatically once the batch job completes
B) A long-running CREATE SERVICE on a CPU compute pool left running continuously
C) A Cortex fine-tuning job scheduled nightly
D) A permanent GPU compute pool with AUTO_SUSPEND_SECS disabled

<details>
<summary>Answer</summary>

**Correct: A**

Job services run the container to completion and the pool can suspend afterward, so credits are only consumed during the actual batch run — ideal for a once-nightly workload.

**Why the others are wrong:**
- A continuously running service bills for compute 24/7 even though inference only occurs once daily
- Cortex fine-tuning is unrelated to running a custom container-based batch scoring job
- Disabling auto-suspend guarantees the pool bills continuously, which is the opposite of cost efficiency

</details>

---

**Q3.** An SPCS service container needs to call a third-party REST API outside Snowflake to enrich data before inference. By default, what happens, and what must be configured to allow this?

A) Outbound calls work automatically; no configuration is needed
B) The container must be granted the ACCOUNTADMIN role to make outbound calls
C) Outbound calls are only possible if the compute pool uses a GPU instance family
D) Outbound network access is blocked by default; an External Access Integration with a network rule must be created and attached to the service

<details>
<summary>Answer</summary>

**Correct: D**

SPCS blocks outbound network traffic by default for security; an External Access Integration referencing a network rule for the target host/port must be created and bound to the service to permit it.

**Why the others are wrong:**
- Outbound access is denied by default, not automatic
- Role grants control SQL privileges, not container network egress
- GPU vs CPU instance family has no bearing on network egress permissions

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A team needs to run inference on a model that requires 80GB of GPU memory per device across multiple GPUs. Which compute pool instance family should they select?

A) GPU_NV_S
B) GPU_NV_M
C) GPU_NV_L
D) CPU_X64_L

<details>
<summary>Answer</summary>

**Correct: C**

GPU_NV_L nodes provide multiple NVIDIA A100 GPUs with 80GB memory each, suited for large-model inference requiring high per-GPU memory.

**Why the others are wrong:**
- GPU_NV_S provides only a single smaller-memory GPU, insufficient for this workload
- GPU_NV_M provides multiple GPUs but with less memory per device than the largest family
- CPU_X64_L has no GPU acceleration at all

</details>

---

**Q5.** A developer wants to run text generation using Meta's Llama 3 model, which is already supported by Snowflake Cortex, and does not need custom code or fine-tuning. What is the BEST approach?

A) Deploy Llama 3 as a custom container on an SPCS GPU compute pool
B) Build a job service that loads the model weights from a stage on each invocation
C) Call the Cortex COMPLETE function with the supported model name, avoiding any infrastructure management
D) Register the model in the Model Registry and manually configure a compute pool

<details>
<summary>Answer</summary>

**Correct: C**

When a model is natively supported by Cortex, calling COMPLETE is simplest, serverless, and avoids any compute pool or container management.

**Why the others are wrong:**
- Deploying via SPCS adds unnecessary infrastructure overhead for a model already available natively
- Job services reloading weights per invocation add latency and cost with no benefit over Cortex here
- Model Registry + compute pool setup is unneeded complexity for a fully Cortex-supported model

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** If AUTO_SUSPEND_SECS is left at its default value for a Snowpark Container Services compute pool, what happens after the configured period of inactivity?

A) The pool automatically suspends after 3600 seconds (1 hour) of inactivity by default
B) The pool never suspends unless manually stopped
C) The pool suspends immediately after each service request completes
D) The pool suspends after 600 seconds, matching virtual warehouse defaults

<details>
<summary>Answer</summary>

**Correct: A**

Compute pools default AUTO_SUSPEND_SECS to 3600 seconds; after that period of inactivity the pool suspends automatically to reduce cost.

**Why the others are wrong:**
- AUTO_RESUME/AUTO_SUSPEND behavior is enabled by default, not disabled
- Suspension does not happen immediately after each request — it waits for the configured idle threshold
- 600 seconds is the virtual warehouse default, not the compute pool default

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which set of elements does a Snowpark Container Services service specification YAML define?

A) Warehouse size, query timeout, and result cache settings
B) Snowflake role grants and network policies only
C) Stored procedure definitions and task schedules
D) Containers (image and resources), endpoints (port/protocol/public exposure), and optional volume mounts

<details>
<summary>Answer</summary>

**Correct: D**

The service spec is analogous to a Kubernetes pod spec: it declares container images/resources, exposed endpoints, and any storage volumes.

**Why the others are wrong:**
- Warehouse sizing and query caching are unrelated to container service specs
- Role grants and network policies are managed via separate SQL objects, not the service spec
- Stored procedures and tasks are unrelated Snowflake SQL objects, not part of SPCS specs

</details>

---

**Q8.** What is the fundamental architectural difference between a Snowflake virtual warehouse and a Snowpark Container Services compute pool?

A) Warehouses run arbitrary Docker containers; compute pools only run SQL
B) There is no meaningful difference — they are billed and provisioned identically
C) Compute pools can only be used for storage, not compute
D) Warehouses execute SQL and Snowpark code; compute pools run arbitrary containerized workloads, including custom models and GPUs

<details>
<summary>Answer</summary>

**Correct: D**

Virtual warehouses are Snowflake's SQL/Snowpark execution engine, while compute pools provision CPU/GPU nodes to run arbitrary Docker container workloads such as custom model servers.

**Why the others are wrong:**
- This reverses the actual roles of the two constructs
- They differ in billing granularity, instance families, and use cases
- Compute pools are compute resources for containers, not storage

</details>

---

