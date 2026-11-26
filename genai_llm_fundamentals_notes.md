# Generative AI & LLM Fundamentals: Complete Study Notes

## Table of Contents
1. [LLM Inference Pipeline (End-to-End Flow)](#1-llm-inference-pipeline-end-to-end-flow)
2. [Context Window](#2-context-window)
3. [Parameters vs. Context Window (GenAI Perspective)](#3-parameters-vs-context-window-genai-perspective)
4. [Temperature & Decoding Controls](#4-temperature--decoding-controls)
5. [Prompting & Prompt Engineering](#5-prompting--prompt-engineering)

---

## 1. LLM Inference Pipeline (End-to-End Flow)

The inference pipeline describes how a Large Language Model (LLM) converts raw human text into numbers, processes those numbers through a neural network, and decodes the output back into readable text.

```
[1. User Prompt] 
       │
       ▼
[2. Tokenization] ──► Splits text into subword chunks
       │
       ▼
[3. Token IDs] ────► Maps chunks to vocabulary integers
       │
       ▼
[4. Model Layers] ◄───┐  (Embeddings + Self-Attention + Feed-Forward)
       │              │
       ▼              │  Autoregressive Loop:
[5. Next-Token Scores]│  Newly generated token is appended
       │              │  and fed back in until <EOS>
       ▼              │
[6. Decoding Strategy]│  (Greedy, Temperature, Top-k, Top-p)
       │              │
       ▼              │
[7. Generated Tokens] ┘
       │
       ▼
[8. Readable Text] ──► Detokenizes IDs back into human language
```

### Step-by-Step Breakdown
1. **User Prompt:** The raw string typed by the user, often wrapped behind the scenes with hidden system instructions and formatting tags.
2. **Tokenization:** A tokenizer algorithm (e.g., Byte-Pair Encoding / BPE) breaks raw text into smaller chunks called **tokens** (words, subwords, punctuation, or leading spaces). On average, 1 token $\approx \frac{3}{4}$ of an English word.
3. **Token IDs:** Each token is mapped to a unique integer index from the model's fixed vocabulary table (typically 32,000 to 100,000+ entries).
4. **Model Layers (Transformer Forward Pass):**
   * **Embedding Layer:** Converts integer Token IDs into dense high-dimensional vectors representing semantic meaning, combined with positional encodings to track word order.
   * **Self-Attention & Feed-Forward Blocks:** Stacked Transformer layers allow each token to attend to previous tokens to gather context and refine internal representations.
5. **Next-Token Scores (Logits & Probabilities):**
   * The final layer outputs raw numerical scores (**logits**) for every token in the vocabulary.
   * A **Softmax** function converts those logits into a probability distribution summing to $1.0$ ($100\%$).
6. **Decoding Strategy:** Rules (such as Greedy Decoding, Temperature, Top-$k$, and Top-$p$) used to pick a single token from the probability distribution.
7. **Generated Tokens (Autoregressive Loop):** LLMs generate text **one token at a time**. Each newly chosen token ID is appended to the sequence, and steps 4–6 repeat until an End-of-Sequence (`<EOS>`) token or maximum length limit is reached.
8. **Readable Text (Detokenization):** Once generation finishes, the sequence of generated token IDs is mapped back into text strings and stitched together for display.

---

## 2. Context Window

A **context window** is the maximum number of tokens an LLM can hold in its active working memory at one time during a single pass through the pipeline.

```
+-----------------------------------------------------------------------------------+
|                   TOTAL CONTEXT WINDOW CAPACITY (e.g., 128K Tokens)               |
|                                                                                   |
|  [ System Prompt ] + [ Chat History & Docs ] + [ Current Prompt ] + [ Generated ] |
|  |<------------------- Input Tokens ------------------->|   |<- Output Tokens ->| |
+-----------------------------------------------------------------------------------+
```

### Key Mechanics & Implications
* **Shared Capacity:** The limit applies to the **combined total** of input tokens (system instructions, chat history, uploaded files, user prompt) and generated output tokens.
* **Autoregressive Consumption:** Every new token generated in Step 7 consumes 1 slot of remaining space in the window.
* **Truncation / Forgetting:** When the total token count exceeds the context window limit, older tokens at the beginning of the conversation are dropped or summarized, causing the model to "forget" earlier details.
* **Compute & Memory Cost:** Larger context windows (128K to 1M+ tokens) allow entire books or codebases to fit in one prompt, but require significantly more GPU memory (VRAM) for the **KV (Key-Value) Cache** and increase latency.
* **Lost in the Middle:** Models recall information placed at the **very beginning** or **very end** of the context window much more reliably than information buried in the middle.

---

## 3. Parameters vs. Context Window (GenAI Perspective)

In Generative AI, **parameters** and the **context window** represent two distinct forms of memory: **long-term learned knowledge** versus **short-term working memory**.

| Dimension | Parameters (Model Weights) | Context Window (Token Capacity) |
| :--- | :--- | :--- |
| **Analogy** | The model's **brainpower & education** | The model's **desk space & scratchpad** |
| **Definition** | Internal numerical weights learned during training | Maximum input + output tokens held simultaneously |
| **Typical Scale** | Billions (`7B`, `70B`, `405B`) | Thousands to Millions (`8K`, `128K`, `1M+` tokens) |
| **Mutability** | Static/Frozen at inference (updated only via training/fine-tuning) | Dynamic; changes with every prompt and resets per session |
| **Primary Impact** | **Intelligence:** Reasoning depth, coding ability, world knowledge | **Capacity:** Amount of reference data and chat history visible at once |

### Architectural Independence
* **Small Parameters + Large Context (e.g., 8B model, 128K context):** Fast and low-cost; ideal for summarizing or extracting facts from large documents, though weaker at deep multi-step reasoning.
* **Large Parameters + Small Context (e.g., 70B model, 8K context):** Strong at complex logic and nuanced coding, but quickly loses track of long conversations or large files.

---

## 4. Temperature & Decoding Controls

**Temperature ($T$)** is a decoding parameter (applied at Step 6 of the pipeline) that controls the randomness and predictability of generated text. Before Softmax converts raw logits ($z_i$) into probabilities, logits are scaled by $T$:

$$P(w_i) = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$$

### Temperature Settings & Behavior

| Temperature | Effect on Probability Distribution | Output Characteristics | Recommended Use Cases |
| :--- | :--- | :--- | :--- |
| **$T = 0$** | **Greedy Decoding:** Highest-scoring token receives $100\%$ probability. | Completely deterministic and repeatable. | Data extraction, classification, math, SQL/code |
| **$T = 0.1 - 0.4$** | **Sharpens distribution:** Top tokens dominate; low-probability tokens become negligible. | Focused, factual, consistent, minor wording variation. | Summarization, technical Q&A, customer support |
| **$T = 0.5 - 0.8$** | **Preserves natural distribution:** Keeps learned token probabilities balanced. | Natural flow, coherent, moderate lexical variety. | General chat, emails, standard content drafting |
| **$T = 0.9 - 1.5+$** | **Flattens distribution:** Low-probability tokens have a much higher chance of selection. | Creative, surprising, diverse; higher risk of hallucinations or incoherence. | Brainstorming, fiction, poetry, ad copy |

> **Interaction Note:** Temperature works alongside **Top-$p$ (Nucleus Sampling)** and **Top-$k$ sampling**. If `top_p` is set very low (e.g., `0.1`), raising temperature will have minimal effect because low-probability tokens are already filtered out before sampling.

---

## 5. Prompting & Prompt Engineering

**Prompting** is the practice of structuring input text inside the context window to steer the model's next-token probability distribution toward accurate, reliable, and properly formatted outputs—without modifying model parameters.

### 5.1 Anatomy of a Structured Prompt
1. **Role / Persona:** Defines expertise, tone, and perspective (*"Act as a senior security engineer..."*).
2. **Task / Instruction:** Clear action verb specifying the goal (*"Audit," "Summarize," "Extract," "Convert"*).
3. **Context & Grounding Data:** Reference facts, documents, or rules to anchor answers and reduce hallucinations.
4. **Constraints & Guardrails:** Boundaries on length, scope, and fallback behavior (*"Respond in under 100 words. If unsure, state 'Insufficient data.'"*).
5. **Output Format:** Explicit structural template (JSON schema, Markdown table, numbered list).

### 5.2 Core Prompting Techniques

| Technique | Mechanism | Best Use Cases |
| :--- | :--- | :--- |
| **Zero-Shot** | Direct instruction with no examples provided. | Simple Q&A, translation, straightforward drafting. |
| **Few-Shot** | Providing 2–5 input/output examples inside the prompt to establish a pattern. | Strict formatting, edge-case classification, custom tone. |
| **Chain-of-Thought (CoT)** | Instructing the model to reason step-by-step before giving the final answer. | Math, logic puzzles, multi-step debugging, complex analysis. |
| **System / Role Prompting** | Setting persistent global rules at the start of the context window. | Chatbots, specialized tutors, consistent brand voice. |
| **Delimiter Framing** | Wrapping external data in tags (`<doc>...</doc>`) or triple quotes (`"""`). | Document summarization, mitigating **prompt injection**. |

### 5.3 Why Chain-of-Thought Works Mechanically
* Because LLMs predict **one token at a time**, forcing an immediate final answer gives the model only a single forward pass to compute the result.
* Asking the model to *"think step-by-step"* causes it to generate intermediate reasoning tokens that are appended back into the **context window**. These tokens act as external working memory ("scratchpad") for subsequent forward passes, significantly improving accuracy on multi-step logic.

### 5.4 Best Practices
* **Use Positive Instructions:** State what *to do* rather than only what *not to do* (negative phrasing still activates the unwanted concept in self-attention).
* **Anchor Rules at the Edges:** Place core behavioral rules at the **start** (System Prompt) and repeat critical formatting constraints at the **end** of long prompts to counter the *Lost in the Middle* effect.
* **Align Prompting with Decoding Settings:** Pair strict, factual, or structured prompts with **low temperature (`0.0–0.2`)** and open-ended creative prompts with **higher temperature (`0.7–0.9`)**.
