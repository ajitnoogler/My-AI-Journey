# Enterprise Production RAG Architecture — Forward Deployed Engineer (FDE) Study Notes

> **Session Summary:** End-to-end production Retrieval-Augmented Generation (RAG) architecture covering enterprise data ingestion, parsing, cleaning, chunking, metadata governance, 1M+ vector scaling, hybrid retrieval, and citation-backed generation—demonstrated through a **Banking Loan Policy (`LN-2026-107`)** eligibility use case where the minimum salary requirement is **₹25,000**.

---

## 1. End-to-End Architecture Diagram

### A. Visual Blueprint (Mermaid Flowchart)

```mermaid
flowchart TB
    subgraph OFFLINE["PART 1: OFFLINE INGESTION & INDEXING PIPELINE"]
        direction TB
        S1["<b>1. Enterprise Data Sources</b><br/>• Google Drive<br/>• AWS S3<br/>• Azure Blob Storage<br/>• SharePoint (~2,000+ docs)"]
        S2["<b>2. Security & Gatekeeping</b><br/>• Format & Size Validation<br/>• Malware / Virus Scanning<br/>• Document Classification<br/>• Lineage Tracking (Doc ID, Version, Owner)"]
        S3["<b>3. Parsing & Data Cleaning</b><br/>• PDF Native Text Extraction<br/>• OCR for Scanned PDFs / Images<br/>• Table Extraction (Preserve Rows/Cols)<br/>• Header & Footer Removal<br/>• Page Number Identification<br/>• Duplicate Content Removal"]
        S4["<b>4. Chunking (100,000+ Words Scale)</b><br/>• Recursive Character Chunking (Overlap)<br/>• Semantic Chunking (Topic Shifts)"]
        S5["<b>5. Metadata Schema Tagging</b><br/>• documents_id | document_name<br/>• department | category | classification<br/>• version | pagenumber | tenant_id"]
        S6["<b>6. Embeddings & Vector Database (1M+ Chunks)</b><br/>• Dense Numerical Vector Conversion<br/>• Stores: FAISS | ChromaDB | Pinecone<br/>• Enterprise: Amazon OpenSearch | PostgreSQL + pgvector"]

        S1 --> S2 --> S3 --> S4 --> S5 --> S6
    end

    subgraph ONLINE["PART 2: ONLINE RETRIEVAL & GROUNDED GENERATION"]
        direction TB
        Q1["<b>User Query</b><br/>'What is the eligibility / minimum salary rule for Loan Policy LN-2026-107?'"]
        Q2A["<b>Dense Query Embedding</b><br/>Numerical Vector Representation"]
        Q2B["<b>Sparse Keyword Tokenizer</b><br/>Exact Token: 'LN-2026-107'"]
        Q3["<b>Governance Pre-Filtering (RBAC & Multi-Tenancy)</b><br/>WHERE tenant_id = 'bank_org' AND department = 'Retail_Loans' AND version = 'latest'"]
        Q4["<b>Hybrid Search & Top-K Retrieval</b><br/>• Cosine Similarity + BM25 Exact Match (RRF)<br/>• Retrieve top_k (k = 5, 10, or 20)<br/>• Optional Cross-Encoder Reranker (top_n = 5)"]
        Q5["<b>Augmented Prompt Assembly</b><br/>• System Rule: Answer strictly from context & cite source + page<br/>• Context: 'Loan Policy LN-2026-107 requires minimum monthly salary of ₹25,000'<br/>• Metadata: {document_name: 'Banking Loan policy', pagenumber: 14}"]
        Q6["<b>Grounded LLM Output (Zero Hallucination)</b><br/>• Answer: Minimum salary required for LN-2026-107 is ₹25,000<br/>• Source: Banking Loan policy<br/>• Page: 14"]

        Q1 --> Q2A & Q2B
        Q2A & Q2B --> Q3 --> Q4 --> Q5 --> Q6
    end

    S6 -.->|"Indexed Knowledge Base (1M Chunks)"| Q4
```

### B. ASCII System Architecture View

```text
====================================================================================================
                             PART 1: OFFLINE INGESTION & INDEXING PIPELINE
====================================================================================================

[1. DATA SOURCES]         [2. SECURITY & GATEKEEPING]      [3. PARSING & CLEANING]
  • Google Drive            • Format & Size Validation       • PDF Text Extraction (Digital)
  • AWS S3             ───► • Malware / Virus Scan      ───► • OCR (Scanned Docs / Images)
  • Azure Blob              • Document Classification        • Table Extraction (Structure)
  • SharePoint (~2000)      • Governance & Lineage           • Header / Footer Removal
                              (Doc ID, Version, Owner)       • Page Number Identification
                                                             • Duplicate Content Removal
                                                                          │
                                                                          ▼
[6. VECTOR DATABASE]      [5. EMBEDDINGS & METADATA]       [4. CHUNKING (100,000 Words)]
  • FAISS (Local/PoC)       • Dense Vector Conversion        • Recursive Character Chunking
  • ChromaDB (Dev/Med) ◄─── • Attach Metadata JSON:     ◄───   (Hierarchical + Overlap)
  • Pinecone (Managed)        - documents_id, version        • Semantic Chunking
  • Amazon OpenSearch         - document_name, pagenumber      (Topic/Similarity Shifts)
  • PostgreSQL+pgvector       - tenant_id, department
  (Scale: 1M+ Chunks)         - category, classification

====================================================================================================
                           PART 2: ONLINE RETRIEVAL & GROUNDED GENERATION
====================================================================================================

[USER QUERY]
"What is the eligibility criteria / minimum salary requirement for loan policy LN-2026-107?"
      │
      ├──────────────────────────────────────────┐
      ▼                                          ▼
[DENSE QUERY EMBEDDING]                    [SPARSE KEYWORD TOKENIZER]
  • Convert query to numerical vector        • Extract exact alphanumeric ID: "LN-2026-107"
      │                                          │
      └────────────────────┬─────────────────────┘
                           ▼
[GOVERNANCE PRE-FILTERING (RBAC & MULTI-TENANCY)]
  • Enforce WHERE tenant_id = 'bank_org' AND department = 'Retail_Loans' AND version = 'latest'
                           │
                           ▼
[HYBRID SEARCH (SEMANTIC + BM25) & TOP-K RETRIEVAL]
  • Combine Cosine Similarity + BM25 Exact Match via Reciprocal Rank Fusion (RRF)
  • Retrieve top_k candidates (k = 5, 10, or 20) ──► Optional Cross-Encoder Reranker (top_n = 5)
                           │
                           ▼
[AUGMENTED PROMPT ASSEMBLY]
  • System Rule: "Answer strictly from context. Cite document_name and pagenumber."
  • Context Chunk: "...Loan Policy LN-2026-107 requires a minimum monthly salary of ₹25,000..."
  • Metadata Attached: { document_name: "Banking Loan policy", pagenumber: 14 }
                           │
                           ▼
[GROUNDED LLM GENERATION (ZERO HALLUCINATION)]
  ┌────────────────────────────────────────────────────────────────────────────────────────────┐
  │ Answer: Minimum monthly salary required for Loan Policy LN-2026-107 is ₹25,000.            │
  │ Source: Banking Loan policy                                                                │
  │ Page: 14                                                                                   │
  └────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Stage-by-Stage Technical Breakdown

### Stage 1: Enterprise Ingestion & Security Gatekeeping
Before raw files from cloud repositories (**Google Drive, AWS S3, Azure Blob Storage, SharePoint**) reach an AI pipeline, a Forward Deployed Engineer enforces strict ingestion controls:
* **Format & Size Validation:** Reject unsupported file extensions, corrupted binaries, or oversized files before spending compute.
* **Malware Scanning:** Scan every incoming file to protect internal enterprise storage and workers.
* **Document Classification:** Route files by type (e.g., *Banking Policies, Contracts, SOPs, Invoices*) to specialized parsing pipelines.
* **Governance & Lineage:** Record **Document IDs, versions, and ownership** so updated policies overwrite stale versions and compliance audit trails remain intact.

---

### Stage 2: Parsing & Data Cleaning
Raw enterprise PDFs contain structural noise that corrupts embeddings and causes LLM hallucinations:
* **PDF Text Extraction:** Extract native digital text from standard PDFs (e.g., via PyMuPDF, `pdfplumber`).
* **OCR (Optical Character Recognition):** Convert scanned documents, faxes, and embedded images into machine-readable text (e.g., via AWS Textract, Azure Document Intelligence, Tesseract).
* **Table Extraction:** Preserve row-to-column relationships (converting tables into Markdown or JSON) so eligibility matrices and salary slabs stay intact.
* **Header, Footer & Duplicate Removal:** Strip repetitive page headers/footers and deduplicate boilerplate clauses to avoid polluting search results.
* **Page Number Tracking:** Retain exact page numbers per text block for verifiable citations.

---

### Stage 3: Chunking Strategy (100,000+ Words Scale)
Large enterprise policies cannot fit into a single embedding call or LLM prompt without losing precision:
* **Recursive Character Chunking:** Splits text hierarchically (`\n\n` → `\n` → `.` → ` `) with a fixed token/character overlap (e.g., 500–800 tokens with 10–15% overlap) so rules split across boundaries are not lost.
* **Semantic Chunking:** Uses embedding similarity between adjacent sentences to split chunks only when the topic shifts, keeping complete policy clauses intact.

---

### Stage 4: Metadata Schema Tagging & Governance
Every chunk is stored alongside structured metadata JSON to enforce security and auditability:

```json
{
  "documents_id": "DOC-9941",
  "document_name": "Banking Loan policy",
  "department": "Retail_Loans",
  "category": "Credit_Eligibility",
  "classification": "Internal_Confidential",
  "version": "2026.1",
  "pagenumber": 14,
  "tenant_id": "bank_org"
}
```

---

### Stage 5: Embeddings & Vector Database Scaling (1M+ Chunks)
* **Dense Embeddings:** Pass each clean chunk through an embedding model (e.g., `text-embedding-3-large`, `BGE-M3`, `Cohere Embed`) to generate dense numerical vectors.
* **Vector Store Selection by Scale:**
  * **Local / PoC:** FAISS, ChromaDB
  * **Managed Cloud:** Pinecone, Qdrant, Weaviate
  * **Enterprise Integrated:** Amazon OpenSearch, PostgreSQL + `pgvector`

---

### Stage 6: Hybrid Retrieval, Reranking & Citation-Backed Generation
1. **Metadata Pre-Filtering (RBAC & Multi-Tenancy):** Filter vectors *before* similarity search (`tenant_id = 'bank_org'`, `department = 'Retail_Loans'`, `version = 'latest'`) to prevent cross-tenant data leaks and outdated policy answers.
2. **Hybrid Search (Dense + Sparse):**
   * **Dense Vector Search (Cosine Similarity):** Captures semantic meaning (*"eligibility criteria"* ≈ *"minimum salary requirement"*).
   * **Sparse Keyword Search (BM25):** Captures exact alphanumeric identifiers (`LN-2026-107`) that dense embeddings often blur.
   * **Reciprocal Rank Fusion (RRF):** Merges dense and sparse rankings into a single candidate list (`top_k = 10` or `20`).
3. **Cross-Encoder Reranking (Optional):** Scores query-chunk pairs jointly to select the `top_n = 5` most relevant chunks.
4. **Grounded Prompt & Citations:** Instruct the LLM to answer *only* from the retrieved context and attach `document_name` and `pagenumber` (`Banking Loan policy`, Page `14` → **₹25,000** minimum salary).
