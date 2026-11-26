# AI Embeddings, Vectors & Retrieval Notes

## 1. What Are Embeddings in AI?

An **embedding** in AI is a numerical vector (a list of decimal numbers) that represents complex data—such as text, images, audio, or graph nodes—so machines can process meaning and relationships mathematically. Items with similar meanings or traits sit closer together in this vector space, while unrelated items sit farther apart.

### Where Embeddings Sit in the AI Landscape
Embeddings fall primarily under **Representation Learning**, a core subfield of **Machine Learning (ML)** and **Deep Learning (DL)**:
* **Representation Learning:** Automatically discovers useful numerical features from raw data without manual feature engineering.
* **Natural Language Processing (NLP):** Word, sentence, and document embeddings (*Word2Vec*, *BERT*) capture language meaning and context.
* **Generative AI & LLMs:** Every modern LLM (*GPT*, *Claude*, *Gemini*) begins with an embedding layer to translate tokens into vectors and relies on embeddings for **Retrieval-Augmented Generation (RAG)**.
* **Computer Vision & Multimodal AI:** Vision models embed images for search and recognition, while multimodal models (*CLIP*) align text and images in one shared vector space.
* **Self-Supervised Learning:** Models learn patterns directly from unlabeled data (such as predicting missing words in context).

### Common Types of Embeddings
* **Static Word Embeddings:** One fixed vector per word regardless of context (e.g., *Word2Vec*, *GloVe*).
* **Contextual Embeddings:** Adjusts a token's vector based on surrounding words (e.g., *BERT*, *GPT*), distinguishing *"river bank"* from *"investment bank"*.
* **Sentence, Image, Audio & Multimodal Embeddings:** Encodes full passages, visuals, audio waves, or aligned text-image pairs (e.g., *Sentence-BERT*, *CLIP*, *Wav2Vec 2.0*).
* **Graph Embeddings:** Encodes nodes and edges into vectors for network tasks (e.g., *Node2Vec*, *GraphSAGE*).

---

## 2. The 3-Step Embedding Pipeline & Neural Network Math

1. **Raw Data (Input):** Unstructured data (text like *"Cat"*, *"Dog"*, *"Car"*; image pixels; or audio frequencies) is converted into initial numbers.
2. **Embedding Model (Neural Network):**
   * **Weighted Sums (Matrix Multiplication):** Each neuron multiplies incoming numbers by learned weights ($w$) and adds a bias ($b$):
     $$Y = W \cdot X + B$$
   * **Activation Functions:** Non-linear filters (such as **ReLU** or **GELU**) pass meaningful patterns forward and suppress noise.
   * **Output Projection:** Compresses signals into a fixed-length **dense vector** (e.g., $[0.82, -0.14, 0.91, \dots]$).
3. **Vector Space (Semantic Map):** Plots vectors as coordinates in high-dimensional space. Similar concepts (*Cat* and *Dog*) cluster together, while unrelated concepts (*Car*) sit far away.

---

## 3. Vector Math & Similarity Metrics

### Basic Vector Operations
Given two vectors $A = [a_1, a_2, \dots, a_n]$ and $B = [b_1, b_2, \dots, b_n]$:
* **Vector Addition:** Combines components position by position:
  $$A + B = [a_1 + b_1, \, a_2 + b_2, \, \dots, \, a_n + b_n]$$
* **Vector Subtraction:** Measures the difference or direction between two vectors:
  $$A - B = [a_1 - b_1, \, a_2 - b_2, \, \dots, \, a_n - b_n]$$
* **Dot Product ($A \cdot B$):** Multiplies matching positions and sums the products:
  $$A \cdot B = \sum_{i=1}^{n} a_i b_i = a_1 b_1 + a_2 b_2 + \dots + a_n b_n$$
* **Vector Magnitude / Length ($\lVert A \rVert$):** Measures the length of the vector from the origin:
  $$\lVert A \rVert = \sqrt{a_1^2 + a_2^2 + \dots + a_n^2}$$

### Euclidean Distance vs. Cosine Similarity

| Feature | Euclidean Distance | Cosine Similarity |
| :--- | :--- | :--- |
| **What It Measures** | Straight-line gap between vector endpoints | Angle ($\theta$) between vector directions |
| **Formula** | $d(A, B) = \sqrt{\sum_{i=1}^{n} (b_i - a_i)^2}$ | $\cos(\theta) = \frac{A \cdot B}{\lVert A \rVert \cdot \lVert B \rVert}$ |
| **Length Sensitivity** | Sensitive to vector magnitude/length | Ignores vector length/size |
| **Exact Match Score** | $0$ | $1$ |
| **Best Use Cases** | Spatial data, coordinates, physical clustering | Text embeddings, RAG, semantic search |

> **Key Difference:** A short tweet and a long article on the exact same topic have a **large Euclidean distance** (due to length difference) but a **high Cosine Similarity** (because both point in the same semantic direction).

---

## 4. Worked Numerical Examples

### Example 1: 2D Vectors — $A = [3, 4]$ and $B = [4, 3]$
* **Addition:** $[3 + 4, \, 4 + 3] = [7, 7]$
* **Subtraction:** $[3 - 4, \, 4 - 3] = [-1, 1]$
* **Dot Product ($A \cdot B$):** $(3 \times 4) + (4 \times 3) = 12 + 12 = 24$
* **Magnitudes:** $\lVert A \rVert = \sqrt{3^2 + 4^2} = \sqrt{25} = 5$, $\lVert B \rVert = \sqrt{4^2 + 3^2} = \sqrt{25} = 5$
* **Cosine Similarity:**

$$\frac{24}{5 \times 5} = \frac{24}{25} = \mathbf{0.96} \text{ (96\% similar)}$$

---

### Example 2: 2D Vectors — $A = [3, 5]$ and $B = [4, 6]$
* **Addition:** $[3 + 4, \, 5 + 6] = [7, 11]$
* **Subtraction:** $[3 - 4, \, 5 - 6] = [-1, -1]$
* **Dot Product ($A \cdot B$):** $(3 \times 4) + (5 \times 6) = 12 + 30 = 42$
* **Magnitudes:** $\lVert A \rVert = \sqrt{3^2 + 5^2} = \sqrt{34} \approx 5.83$, $\lVert B \rVert = \sqrt{4^2 + 6^2} = \sqrt{52} \approx 7.21$
* **Cosine Similarity:**

$$\frac{42}{\sqrt{34} \times \sqrt{52}} = \frac{42}{42.05} \approx \mathbf{0.999} \text{ (99.9\% similar)}$$

---

### Example 3: 3D Vectors — $A = [2, 3, 4]$ and $B = [3, 4, 5]$
* **Addition:** $[2 + 3, \, 3 + 4, \, 4 + 5] = [5, 7, 9]$
* **Subtraction:** $[2 - 3, \, 3 - 4, \, 4 - 5] = [-1, -1, -1]$
* **Dot Product ($A \cdot B$):** $(2 \times 3) + (3 \times 4) + (4 \times 5) = 6 + 12 + 20 = 38$
* **Magnitudes:** $\lVert A \rVert = \sqrt{2^2 + 3^2 + 4^2} = \sqrt{29} \approx 5.39$, $\lVert B \rVert = \sqrt{3^2 + 4^2 + 5^2} = \sqrt{50} \approx 7.07$
* **Cosine Similarity:**

$$\frac{38}{\sqrt{29} \times \sqrt{50}} = \frac{38}{38.08} \approx \mathbf{0.9979} \text{ (99.79\% similar)}$$

---

### Example 4: Scaled 3D Vectors — $A = [2, 3, 4]$ and $B = [5, 6, 7]$
* **Addition:** $[2 + 5, \, 3 + 6, \, 4 + 7] = [7, 9, 11]$
* **Subtraction:** $[2 - 5, \, 3 - 6, \, 4 - 7] = [-3, -3, -3]$
* **Dot Product ($A \cdot B$):** $(2 \times 5) + (3 \times 6) + (4 \times 7) = 10 + 18 + 28 = 56$
* **Magnitudes:** $\lVert A \rVert = \sqrt{2^2 + 3^2 + 4^2} = \sqrt{29} \approx 5.39$, $\lVert B \rVert = \sqrt{5^2 + 6^2 + 7^2} = \sqrt{110} \approx 10.49$
* **Cosine Similarity:**

$$\frac{56}{\sqrt{29} \times \sqrt{110}} = \frac{56}{56.48} \approx \mathbf{0.9926} \text{ (99.26\% similar)}$$

> **Takeaway:** Even though $B$ has much larger numbers than $A$, both vectors point in nearly the same direction, yielding a cosine similarity of **99.26%**.

---

## 5. Chunking & Fast Retrieval (ANN)

### What Is Chunking?
**Chunking** breaks large documents (PDFs, manuals, documentation) into smaller pieces before generating embeddings so each vector represents a focused idea and fits within model token limits.

* **Fixed-Size Chunking:** Splits every $N$ characters or tokens.
* **Recursive / Paragraph Chunking:** Splits along natural boundaries (headings, paragraphs, sentences).
* **Semantic Chunking:** Splits when the topic shifts.
* **Chunk Overlap (10%–20%):** Repeats a small slice of text between adjacent chunks so context is not lost at boundaries.

### Approximate Nearest Neighbor (ANN) Search
Checking a query against **1,00,000+ chunks** one by one (brute-force k-NN) is too slow. **ANN** organizes vectors into topic neighborhoods ahead of time so search skips unrelated sections and retrieves top matches in milliseconds with 98%–99% accuracy.

* **Example (1,00,000 AWS Docs):**
  1. Documentation for EC2, S3, Lambda, and RDS is chunked, embedded, and indexed in a vector database.
  2. When you query *"How does AWS Lambda work?"*, the query becomes a vector.
  3. **ANN** jumps straight to the **Serverless / Lambda** neighborhood (skipping EC2 and S3) and computes cosine similarity only on nearby vectors to return the top Lambda chunk.
* **Popular ANN Indexes:**
  * **HNSW (Hierarchical Navigable Small World):** Multi-layer graph routing like express highways to local streets.
  * **IVF (Inverted File Index):** Divides vectors into labeled topic buckets.
