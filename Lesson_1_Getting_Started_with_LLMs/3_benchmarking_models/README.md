# Benchmarking

Working with Large Language Models might seem like easy work, but make no mistake, every seemingly instant reply comes at a very real computational cost. Before jumping into training or deploying a model, it's crucial to understand the potential costs of running it in a real world environment. The aim here isn't just to work with the latest models, it's to do so efficiently and create real, sustainable value.

So how do we make sure we're choosing the right infrastructure? This is where performance benchmarking becomes essential.

## What is Benchmarking

Benchmarking is the practice of running targeted performance tests on a model to evaluate how it behaves under different conditions.

It's not about just knowing that a model "works." It's about knowing:

- How fast it generates responses
- How much memory it uses
- How much latency you can expect across varying inputs
- How performance scales with user load or longer prompts

Let's say you've picked out a promising model: it's accurate, flexible, maybe even fine-tuned for your domain. But before you push it into production, ask yourself:

- Can I afford to run this at scale — really?
- What happens when I have 10x or 100x more users?
- Will my model crash or stall on complex prompts?
- How much latency is acceptable for my use case?

These aren't philosophical questions. They're performance levers that can make or break your system.

Benchmarking gives you data — not guesses — to answer these questions.

## The Metrics That Matter

When you're performance testing LLMs, the numbers you track are everything. But not all metrics are created equal, some tell you how your model feels to end-users, while others reveal the hidden costs chewing through your compute budget.

Let's break down the three most essential metrics every serious benchmark should include.

### Time To First Token (TTFT)

**What is it?**  
The delay between sending your prompt and receiving the first generated token. This includes input tokenization, prompt encoding, queueing (if applicable), and initial computation.

**Why does it matter?**  
In user-facing applications like chat assistants, AI pair programmers, or voice agents, TTFT defines responsiveness. A model that feels sluggish here ruins user experience — even if it eventually spits out great answers.

**When it spikes:**
- Long or complex inputs (e.g., deeply nested code or docs)
- Improper prompt formatting
- Inefficient backend infrastructure or cold starts

**How to improve it:**
- Cache common prefixes (e.g., instructions or system prompts)
- Optimize the model server setup (disable quantization at startup, warm up the model)
- Use streaming for better perceived latency

### Tokens Per Second (TPS)

**What is it?**  
TPS is the throughput metric — how many tokens (input and output combined) your model handles per second.

There are two key flavors:
1. Average TPS (across full request)
2. Output TPS (for pure generation speed)

**Why does it matter?**  
This metric reflects how fast the model can process or generate text — critical for apps with long outputs (e.g., summarization, content generation, etc.).

**What affects TPS:**
- Batch size and concurrency
- Hardware type (GPU, vRAM bandwidth, etc.)
- Model size and architecture (e.g., Mistral vs. Falcon)
- Presence of adapters or quantization

**What's good?**
- Small models (3B–7B): 50–150 TPS
- Mid-size (13B): 25–70 TPS
- Large models (30B+): 10–30 TPS (depending on GPU)

**Pro tip:**  
Look at Output TPS when comparing generation speed across models, it isolates post-processing and focuses on decoder performance.

### GPU Utilization & Memory Footprint

Your LLM's performance is also bottlenecked by hardware, and two GPU metrics tell the full story:

#### Volatile GPU Utilization (%)

**What does it tell you?**  
The real-time percentage of your GPU's compute power being used. If it's stuck at 30–40% during inference, you're underutilizing that expensive A100 or 4090.

**Why does it matter?**  
High utilization means you're getting value for your money. Low utilization means there's likely a bottleneck — maybe CPU-bound input preprocessing, I/O waits, or suboptimal batching.

#### Memory Consumption (VRAM)

**What you should monitor?**
- Idle VRAM: How much is needed just to load the model
- Peak inference VRAM: Your max usage when handling long inputs or complex outputs
- VRAM at scale: What happens when you serve multiple concurrent users

**Why does it matter?**  
Knowing your memory profile is critical to avoid:
- OOM (Out-of-Memory) crashes
- Overpaying for overkill hardware
- Unexpected bottlenecks in production

# Datasets in Benchmarking

## The Importance of Data in Benchmarking

All the fancy metrics and infrastructure tuning in the world won't mean much without one foundational element: data. Data isn't just fuel for model training, it's the lens through which you measure performance, relevance, and reliability. In benchmarking, the quality and structure of your data define the quality of your insights.

Your benchmark dataset should be:

- **Representative:** It needs to mirror the kind of inputs your system will actually face in the wild.
- **Diverse:** Covering edge cases, complex queries, short prompts, long documents — the full spectrum.
- **Ground-truthed:** You need known, trusted outputs to compare against. Otherwise, you're just eyeballing performance.
- **Stable:** For consistent testing over time, your data must remain unchanged unless you're intentionally updating the benchmark.

Well-chosen data can expose weaknesses like hallucinations, inconsistency, or poor context integration. It also ensures that any performance gain is meaningful, not just a quirk of a cherry picked test.

## Existing Benchmarks: Standing on the Shoulders of Giants

Fortunately, you don't have to start from scratch. There are several battle tested benchmarks built specifically to evaluate LLM performance in context heavy tasks, including those highly relevant to Retrieval-Augmented Generation (RAG).

These focus on open-book question answering, where a model is asked to answer a question using a provided document or passage, exactly like how a RAG pipeline works.

Some standout examples:

### NarrativeQA

**What it tests:** Long-document comprehension, narrative reasoning.

**Structure:** Questions based on entire stories or summaries, requiring multi-paragraph reasoning.

**Why it's valuable:** Challenges models to stay coherent across long spans of text, great for real-world documents.

### HotpotQA

**What it tests:** Multi-hop reasoning and retrieval.

**Structure:** Questions that require reading multiple Wikipedia passages to find an answer.

**Why it's valuable:** Simulates tasks where no single paragraph has the full answer.

### DROP

**What it tests:** Discrete reasoning over paragraphs (dates, numbers, comparisons).

**Structure:** Contexts followed by questions with precise, often numerical answers.

**Why it's valuable:** Measures both reading comprehension and logical reasoning.

These benchmarks provide solid foundations when testing model capabilities out of the box. But sometimes, off-the-shelf just won't cut it.

## Why You Might Need to Create Your Own Benchmarks

While standardized benchmarks are helpful, real world applications often demand more: more specificity, more domain relevance, and more control.

Here's when rolling your own benchmark is not just useful, but necessary:

- You're working with domain-specific data (e.g., legal contracts, healthcare notes, customer support transcripts).
- Your inputs are non-standard (tables, charts, multimodal data, messy internal docs).
- You have unique output expectations, like style, tone, or structure that public benchmarks don't measure.
- You need to simulate production, including latency, concurrency, and failure recovery.

## How to Create a Custom Benchmark

1. **Define your task clearly**  
   What is the model actually supposed to do? Summarize? Extract? Explain? Answer questions?

2. **Gather examples**  
   Collect inputs and expected outputs from actual users, past logs, or expert designed cases.

3. **Establish your ground truth**  
   Use subject matter experts or annotation tools to create reliable reference answers.

4. **Choose your metrics**  
   Combine performance (TTFT, TPS, GPU use) with task accuracy (F1, EM, BLEU) and user-perceived quality.

5. **Automate your testing loop**  
   Set up scripts or dashboards that run benchmarks consistently, tracking drift, regressions, and improvements over time.

Creating your own benchmark transforms your model testing from a science fair into an engineering discipline. You're not just asking, does this work? You're answering, does this work at scale, in my use case, for my users, on my budget?