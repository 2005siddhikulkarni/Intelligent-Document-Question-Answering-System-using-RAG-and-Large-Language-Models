# 📘 Intelligent Document Question Answering System using RAG and Large Language Models

An **AI-powered document question answering system** that allows users to upload a PDF and ask questions about its content.

The system uses **Retrieval-Augmented Generation (RAG)** to extract information from the uploaded document, convert text into embeddings, retrieve the most relevant content using **FAISS**, and generate context-aware answers using a **local LLM through Ollama and Llama 3**.

---

## 🚀 Project Overview

The **Intelligent Document Question Answering System** is a Python-based Streamlit application designed to provide intelligent answers from user-uploaded PDF documents.

Instead of sending the complete document directly to an LLM, the system follows a retrieval-based approach:

**PDF → Text Extraction → Text Chunking → Embeddings → FAISS Vector Search → Relevant Context → LLM → Final Answer**

The application is designed to answer questions **only from the retrieved content of the uploaded PDF**.

---

## 🎯 Objectives

* Upload and process PDF documents.
* Extract readable text from PDF pages.
* Divide large documents into smaller overlapping chunks.
* Convert text chunks into numerical embeddings.
* Store embeddings using the FAISS vector database.
* Perform semantic similarity search.
* Retrieve the most relevant document content.
* Generate answers using a local Llama 3 model.
* Provide an interactive web interface using Streamlit.
* Reduce dependency on external LLM APIs by using Ollama locally.

---

## 🧠 RAG Architecture

The project implements a **Retrieval-Augmented Generation (RAG)** pipeline.

```text
                 ┌──────────────────┐
                 │    Upload PDF    │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │  Extract Text    │
                 │    using         │
                 │     PyPDF2       │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │  Split Text into │
                 │     Chunks       │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │ Sentence         │
                 │ Transformer      │
                 │   Embeddings     │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │ FAISS Vector     │
                 │    Database      │
                 └────────┬─────────┘
                          │
                    User Question
                          ↓
                 ┌──────────────────┐
                 │ Question         │
                 │   Embedding      │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │ Similarity Search│
                 │   Top 3 Chunks   │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │ Retrieved        │
                 │    Context       │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │ Llama 3 via      │
                 │     Ollama       │
                 └────────┬─────────┘
                          ↓
                 ┌──────────────────┐
                 │  Final Answer    │
                 └──────────────────┘
```

---

## 🛠️ Technologies Used

### Programming Language

* Python

### Frontend / Web Application

* Streamlit

### PDF Processing

* PyPDF2

### Embeddings

* Sentence Transformers
* `all-MiniLM-L6-v2`

### Vector Database

* FAISS

### Numerical Processing

* NumPy

### LLM

* Llama 3

### Local LLM Runtime

* Ollama

### API Communication

* Python Requests

---

## 📦 Python Libraries

The project uses the following major libraries:

```text
streamlit
PyPDF2
faiss
numpy
requests
sentence-transformers
```

---

## 🔄 Project Workflow

### 1. PDF Upload

The user uploads a PDF document through the Streamlit interface.

```python
uploaded_file = st.file_uploader(
    "Upload your PDF file here",
    type=["pdf"]
)
```

---

### 2. PDF Text Extraction

The system uses **PyPDF2** to extract readable text from every page of the uploaded PDF.

---

### 3. Text Chunking

Large extracted text is divided into smaller overlapping chunks.

The current implementation uses:

* **Chunk size:** 500 characters
* **Overlap:** 100 characters

This helps preserve contextual information between neighboring chunks.

---

### 4. Text Embeddings

The project uses the Sentence Transformers model:

```text
all-MiniLM-L6-v2
```

The model converts each text chunk into a numerical vector representation.

---

### 5. FAISS Vector Database

The generated embeddings are converted into `float32` arrays and stored in a FAISS `IndexFlatL2` index.

```python
index = faiss.IndexFlatL2(dimension)
index.add(embeddings)
```

This allows the system to perform similarity-based retrieval of relevant document chunks.

---

### 6. Question Processing

When the user enters a question, the question is converted into an embedding using the same Sentence Transformer model.

The FAISS index then searches for the **top 3 most relevant chunks**.

---

### 7. Context Retrieval

The retrieved chunks are combined to create the context provided to the LLM.

```text
Retrieved Chunk 1
        +
Retrieved Chunk 2
        +
Retrieved Chunk 3
        ↓
     Context
```

The application also displays the retrieved context through an expandable Streamlit section.

---

### 8. Prompt Engineering

The system creates a structured prompt containing:

* Instructions
* Retrieved context
* User question

The LLM is instructed to answer only from the provided context and respond that the answer could not be found if the information is absent.

---

### 9. Llama 3 + Ollama

The project communicates with the local Ollama API:

```text
http://localhost:11434/api/generate
```

and uses:

```text
llama3
```

as the selected model.

---

### 10. Final Answer

The generated response is displayed in the Streamlit application as the final answer to the user's question.

---

## 🖥️ Streamlit Application

The application contains three main sections:

### 📄 Upload PDF

Allows users to:

* Upload a PDF.
* Preview extracted text.
* View total characters.
* View total chunks.
* View chunk size.
* Create the FAISS vector database.
* Ask questions about the uploaded document.
* View retrieved context.
* View the generated answer.

### 🔄 Project Flow

Displays the complete RAG workflow inside the application.

### 🧠 Technical Concepts

Explains the major technologies and concepts used:

* PDF Processing
* Text Chunking
* Embeddings
* Vector Database
* Semantic Search
* RAG
* LLM
* Prompt Engineering

These sections are implemented directly in the Streamlit application.

---

## 📊 Application Features

* 📄 PDF document upload
* 🔍 Semantic document search
* 🧠 Retrieval-Augmented Generation
* 📚 Automatic text chunking
* 🔢 Sentence Transformer embeddings
* ⚡ FAISS similarity search
* 🤖 Local Llama 3 LLM
* 🖥️ Streamlit interactive interface
* 👀 Retrieved context preview
* 💬 Natural-language question answering
* 🔐 Local LLM inference through Ollama

## 📋 requirements

streamlit
PyPDF2
faiss-cpu
numpy
requests
sentence-transformers

## 🤖 Install and Run Ollama

Install Ollama on your system and download the Llama 3 model.

Run:

ollama run llama3

The Python application communicates with Ollama through its local API.

Make sure Ollama is running before asking questions in the application.

## ▶️ Run the Application

Start the Streamlit application using:

streamlit run Intelligent_Document_Question_Answering_System.py

The application will open in your browser.

Then:

1. Upload a PDF
        ↓
2. Wait for text extraction
        ↓
3. Wait for FAISS database creation
        ↓
4. Enter your question
        ↓
5. Relevant chunks are retrieved
        ↓
6. Llama 3 generates the answer

## 💡 Example Questions

After uploading a suitable PDF, users can ask questions such as:

What is the main objective of this document?

What are the important findings?

Explain the methodology used.

What are the advantages mentioned in the document?

What are the future improvements?

The answer is generated from the retrieved PDF context rather than directly passing the complete document to the LLM.

## 🧩 Key Concepts Demonstrated

### Retrieval-Augmented Generation (RAG)

RAG combines:

```text
Information Retrieval
        +
Large Language Model
        =
Context-Aware Answer Generation
```

The system first retrieves relevant information from the uploaded document and then provides that information to the LLM.

### Semantic Search

Instead of relying only on exact keyword matching, the system represents text and questions as embeddings and searches for semantically similar content.

### Vector Database

FAISS is used to store and search the generated embedding vectors.

### Prompt Engineering

The application creates a structured prompt containing the retrieved context and the user's question.

### Local LLM

Llama 3 is executed locally through Ollama, allowing the application to communicate with the model through a local API.

---

## 🎓 Learning Outcomes

Through this project, the following concepts are demonstrated:

* Python application development
* Streamlit application development
* PDF processing
* Text preprocessing
* Text chunking
* Sentence embeddings
* Vector databases
* FAISS similarity search
* Semantic search
* Retrieval-Augmented Generation
* Large Language Models
* Prompt engineering
* REST API communication
* Local LLM deployment
* AI-powered document processing

---

## 🔮 Future Enhancements

Possible improvements include:

* Support for multiple PDF documents.
* Persistent FAISS vector databases.
* Chat history and conversational memory.
* Improved chunking strategies.
* Metadata-based retrieval.
* Hybrid keyword + semantic search.
* Support for additional document formats.
* Reranking retrieved chunks.
* Streaming LLM responses.
* Source citations for retrieved passages.
* Authentication and user management.
* Deployment with a cloud-compatible LLM backend.

---

## ⚠️ Limitations

* The current application focuses on PDF documents.
* PDF text extraction depends on the PDF containing readable/extractable text.
* Ollama must be installed and running locally.
* The current implementation retrieves the top 3 matching chunks.
* Answers depend on the quality of the extracted text, embeddings, retrieval, and LLM response.

---

## 👩‍💻 Author

**Siddhi Milind Kulkarni**

**Project:** Intelligent Document Question Answering System using RAG and Large Language Models

**Date:** 09/05/2026

---

## ⭐ Project Highlights

```text
Python
    ↓
PDF Processing
    ↓
Sentence Transformers
    ↓
FAISS
    ↓
Semantic Search
    ↓
RAG
    ↓
Ollama
    ↓
Llama 3
    ↓
Intelligent Document Answers
```

> **Ask Questions. Get Intelligent Answers from Your Documents.**

---

## 📌 Disclaimer

This project is developed for **educational and practical AI/ML learning purposes** and demonstrates the implementation of a RAG-based document question answering system.
