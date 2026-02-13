# Vehicle Specification Extraction from Service Manual (LLM + RAG)

## Overview
This project builds a **text-based system** to extract **vehicle specifications**
(such as torque values, capacities, and part-related specifications) from a **large automotive service manual PDF**.

The solution uses:
- PDF text extraction
- Text cleaning and chunking
- Embeddings with a vector database
- Retrieval-Augmented Generation (RAG)
- A **free Hugging Face LLM** (no paid API, no OpenAI)

The focus is **only on text** (images and diagrams are ignored).

---

## Problem Statement
Automotive service manuals are very large (hundreds of pages) and contain
important technical information like:
- Torque specifications
- Installation procedures
- Component descriptions

Manually searching these PDFs is slow and inefficient.

**Goal:**  
Given a natural language query such as  
“Torque for brake caliper bolts”  

the system should:
1. Find the relevant section in the manual
2. Extract the correct specification
3. Return it in **structured JSON format**

---

## System Architecture

PDF Manual  
↓  
Text Extraction  
↓  
Text Cleaning  
↓  
Chunking  
↓  
Embeddings + Vector Store  
↓  
Semantic Retrieval  
↓  
LLM-based Structured Extraction  
↓  
JSON Output  

---

## Tools and Technologies Used

### PDF Processing
- PyMuPDF (fitz)
- Extracts text page by page

### Text Cleaning
- Python `re` (regular expressions)
- Removes extra spaces and formatting noise

### Chunking
- Word-based chunking
- Overlapping chunks to preserve context

### Embeddings
- SentenceTransformers
- Model: `all-MiniLM-L6-v2`

### Vector Store
- FAISS
- Fast semantic similarity search

### Free LLM (No Paid API)
- Hugging Face model
- Model used: `google/flan-t5-base`
- Runs online on Google Colab
- No OpenAI or paid API required

---

## Step-by-Step Pipeline

### Block 1: PDF Text Extraction
- Reads the service manual PDF
- Extracts text page by page
- Stores page number with text

### Block 2: Text Cleaning
- Removes extra spaces and broken lines
- Normalizes text formatting

### Block 3: Chunking
- Splits text into small overlapping chunks
- Keeps page number metadata

### Block 4: Embeddings and Vector Store
- Converts chunks into numeric vectors
- Stores them in FAISS for fast search

### Block 5: Retrieval
- Converts user query into an embedding
- Retrieves the most relevant chunks from FAISS

### Block 6: LLM-based Structured Extraction
- Sends retrieved text to a Hugging Face LLM
- Extracts structured specifications
- Returns output in JSON format

---

## Example Output

```json
[
  {
    "component": "Brake disc shield bolts",
    "spec_type": "Torque",
    "value": "17",
    "unit": "Nm",
    "page": 24
  }
]
