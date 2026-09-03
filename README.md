# 📄 RAG Document Chatbot

A **Retrieval-Augmented Generation (RAG) based document question-answering chatbot** built with Python and Streamlit.

The application allows users to upload a PDF document and ask questions about its content. The system extracts text from the PDF, divides it into smaller chunks, converts the chunks into vector embeddings, stores them in a FAISS index, retrieves the most relevant chunks for a user's question, and uses a Groq-hosted LLM to generate an answer based on the retrieved document context.


## 🌐 Live Demo

🔗 **[Try the RAG Document Chatbot](https://ragasandhiya05-rag-document-chatbot-app-oammgt.streamlit.app/)**


## 🚀 Features

* 📄 Upload PDF documents
* 🔍 Extract text from PDF files
* ✂️ Split documents into overlapping text chunks
* 🧠 Generate semantic embeddings using Sentence Transformers
* ⚡ Perform fast similarity search using FAISS
* 🔎 Retrieve the top relevant document chunks
* 🤖 Generate answers using a Groq-hosted LLM
* 💬 Interactive Streamlit chat interface
* 📊 Display document statistics:

  * Number of pages
  * Number of words
  * Number of chunks
* 🧹 Automatically reset chat when a new PDF is uploaded
* 🔐 Keep the Groq API key in a `.env` file


## 🧠 How RAG Works

This project uses a basic **Retrieval-Augmented Generation (RAG)** pipeline.

Instead of sending the entire PDF directly to the language model, the document is processed and only the most relevant sections are provided as context.

```text
                ┌──────────────────┐
                │    Upload PDF    │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │  Extract Text    │
                │     PyPDF        │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │  Split into      │
                │  Text Chunks     │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │   Generate       │
                │   Embeddings     │
                │  MiniLM Model    │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │   FAISS Vector   │
                │      Index       │
                └────────┬─────────┘
                         │
                         │
                User Question
                         │
                         ▼
                ┌──────────────────┐
                │ Question         │
                │ Embedding        │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Similarity Search│
                │     FAISS        │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Top 3 Relevant   │
                │     Chunks       │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │   Groq LLM       │
                │ Answer Generation│
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │  Final Answer    │
                └──────────────────┘
```


## 🔄 Application Workflow

### 1. PDF Upload

The user uploads a PDF through the Streamlit interface.

### 2. Text Extraction

The application uses **PyPDF** to extract readable text from each page.

### 3. Text Chunking

The extracted text is divided into smaller chunks.

Current configuration:

```text
Chunk Size: 180 words
Chunk Overlap: 40 words
```

The overlap helps preserve contextual information between neighboring chunks.

### 4. Embedding Generation

Each chunk is converted into a numerical vector using:

```text
sentence-transformers/all-MiniLM-L6-v2
```

### 5. FAISS Indexing

The generated embeddings are stored in a **FAISS Inner Product index**.

Since the embeddings are normalized, the similarity search effectively measures semantic similarity between the question and document chunks.

### 6. Question Processing

When the user asks a question, the question is converted into an embedding using the same embedding model.

### 7. Relevant Chunk Retrieval

FAISS searches the document index and retrieves the **top 3 most relevant chunks**.

### 8. Context Construction

The retrieved chunks are combined into a context that is sent to the language model along with the user's question.

### 9. Answer Generation

The Groq API sends the retrieved context and question to the configured language model.

The system prompt instructs the model to answer using only the provided document context.

### 10. Response Display

The generated answer is displayed in the Streamlit chat interface.


## 🛠️ Tech Stack

| Technology                | Purpose                         |
| ------------------------- | ------------------------------- |
| **Python**                | Core programming language       |
| **Streamlit**             | Web application interface       |
| **PyPDF**                 | PDF text extraction             |
| **Sentence Transformers** | Text embeddings                 |
| **all-MiniLM-L6-v2**      | Embedding model                 |
| **FAISS**                 | Vector similarity search        |
| **Groq**                  | LLM API                         |
| **python-dotenv**         | Environment variable management |


## 📁 Project Structure

```text
RAG-Document-Chatbot/
│
├── app.py
├── requirements.txt
├── .gitignore
└── README.md
```

### `app.py`

Contains the complete Streamlit application, including:

* PDF processing
* Text chunking
* Embedding generation
* FAISS indexing
* Similarity retrieval
* Groq API integration
* Chat interface

### `requirements.txt`

Contains the Python dependencies required to run the project.

### `.gitignore`

Prevents sensitive and unnecessary files such as `.env` and `venv/` from being committed.


## ⚙️ Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/RAG-Document-Chatbot.git
```

Move into the project directory:

```bash
cd RAG-Document-Chatbot
```


### 2. Create a Virtual Environment

```bash
python -m venv venv
```


### 3. Activate the Virtual Environment

#### Windows

```bash
venv\Scripts\activate
```

#### macOS / Linux

```bash
source venv/bin/activate
```


### 4. Install Dependencies

```bash
pip install -r requirements.txt
```


## 🔑 Groq API Configuration

Create a Groq API key through the Groq Console.

Then create a file named:

```text
.env
```

in the root directory of the project.

Add:

```env
GROQ_API_KEY=your_groq_api_key_here
GROQ_MODEL=openai/gpt-oss-120b
```

Replace:

```text
your_groq_api_key_here
```

with your actual Groq API key.

### 🔐 Security

Never hardcode your API key directly inside `app.py`.

Do not commit:

```text
.env
```

to GitHub.

Your `.gitignore` should contain:

```text
.env
venv/
__pycache__/
*.pyc
.streamlit/secrets.toml
.vscode/
```


## ▶️ Running the Application

After activating the virtual environment, run:

```bash
streamlit run app.py
```

Streamlit will provide a local URL, usually:

```text
http://localhost:8501
```

Open the URL in your browser.


## 💬 How to Use

### Step 1

Open the application.

### Step 2

Click:

```text
Upload a PDF document
```

### Step 3

Select a PDF file.

The application will display document statistics:

```text
Pages     Words     Chunks
  10       2500       18
```

### Step 4

Ask a question about the uploaded document.

For example:

```text
What is the main objective of this paper?
```

### Step 5

The application retrieves relevant document chunks and generates an answer.

You can continue asking questions about the same document.


## 💡 Example Usage

The repository includes **`Sample_ai_ml_terms_glossary.pdf`** as a sample document for testing the chatbot.

Upload the PDF and try questions such as:

- What is Machine Learning?
- Explain overfitting.
- What is the Transformer architecture?
- What is Retrieval-Augmented Generation?
- What is semantic search?
- What is model drift?
- What is federated learning?

### ❌ Out-of-Document Question

**Question:** What is Reinforcement Learning from Human Feedback (RLHF)?

**Expected Response:** `I could not find that information in the document.`


<h2>📸 Screenshots</h2>

<h3>🏠 RAG Document Chatbot — Initial Interface</h3>
<img width="1783" height="365" alt="UI_Interface" src="https://github.com/user-attachments/assets/9679c6d8-0527-4bc3-aa58-b4cd02868e46" />

<h3>📄 Document Upload & Processing</h3>
<img width="1778" height="712" alt="After_uploading_pdf" src="https://github.com/user-attachments/assets/9054635d-5fd3-44c4-9154-b52548c89cc1" />

<h3>💬 Context-Aware Question Answering</h3>
<img width="1747" height="787" alt="Screenshot 2026-09-03 145428" src="https://github.com/user-attachments/assets/efd5a79e-6467-4cd9-89b7-378853b26f11" />
<img width="1757" height="773" alt="Screenshot 2026-09-03 145602" src="https://github.com/user-attachments/assets/1eff7a48-2015-4cb8-8384-78862a382406" />
<img width="1781" height="343" alt="Screenshot 2026-09-03 145637" src="https://github.com/user-attachments/assets/84556fee-0b6c-4fae-a989-552ea2a16d32" />

<h3>🚫 Out-of-Document Query Handling</h3>
<img width="1746" height="347" alt="Screenshot 2026-09-03 145744" src="https://github.com/user-attachments/assets/1e962651-32e9-4916-b39c-e0db92cfc20e" />


## 📊 Document Statistics

After uploading a PDF, the application displays:

### 📄 Pages

The total number of pages detected in the PDF.

### 📝 Words

The approximate number of extracted words.

### 🧩 Chunks

The number of text chunks created for retrieval.

These statistics provide a quick overview of how the uploaded document was processed.


## 🧩 Chunking Strategy

The application currently uses a simple word-based chunking strategy.

```text
Chunk Size   = 180 words
Overlap      = 40 words
Step Size    = 140 words
```

Example:

```text
Chunk 1 → Words 1–180
Chunk 2 → Words 141–320
Chunk 3 → Words 281–460
```

The overlapping region helps retain context between neighboring chunks.


## 🔍 Retrieval Strategy

The system uses:

```text
FAISS IndexFlatIP
```

for similarity search.

The application retrieves:

```text
Top K = 3
```

relevant chunks for each question.

The retrieved chunks are then passed to the language model as document context.


## 🤖 Answer Generation

The language model receives:

```text
Document Context
       +
User Question
       ↓
    LLM
       ↓
Generated Answer
```

The system prompt instructs the model to:

* Use only the retrieved document context
* Avoid outside knowledge
* Clearly indicate when information cannot be found
* Keep responses concise

If the required information is not present in the retrieved context, the application instructs the model to respond:

```text
I could not find that information in the document.
```


## ⚠️ Limitations

This is a basic RAG implementation and has some limitations.

### 1. Text-Based PDFs

The application works best with PDFs containing selectable text.

Scanned/image-based PDFs may not produce readable text because OCR is not currently implemented.

### 2. Basic Chunking

The current implementation uses word-based chunking rather than semantic or section-aware chunking.

### 3. Retrieval Accuracy

The system retrieves the top 3 semantically similar chunks, but the retrieved chunks are not guaranteed to contain the exact answer.

### 4. Tables and Complex Formatting

PDF extraction can sometimes alter the structure of:

* Tables
* Columns
* Headers
* Footers
* Special formatting

### 5. No Reranking

The current system does not use a separate reranking model to refine the retrieved chunks.

### 6. Single PDF Workflow

The current interface is designed around one uploaded document at a time.

Uploading a new PDF resets the conversation.


## 🔮 Future Improvements

The project can be extended with:

* 📑 Page-level source citations
* 🔍 Display retrieved chunks
* 🎯 Similarity-score thresholding
* 🧠 Reranking models
* 🔀 Hybrid keyword + vector retrieval
* 📚 Multiple PDF support
* 💾 Persistent vector databases
* 🖼️ OCR for scanned PDFs
* 📊 Retrieval evaluation metrics
* 🧩 Semantic/section-based chunking
* 💬 Improved conversational memory
* 🔐 Production-grade secret management
* ⚡ Caching document embeddings and FAISS indexes


## 🧠 RAG Concepts Demonstrated

This project demonstrates several important Generative AI concepts:

```text
Document Processing
        ↓
Text Chunking
        ↓
Embeddings
        ↓
Vector Indexing
        ↓
Semantic Retrieval
        ↓
Context Augmentation
        ↓
LLM Generation
```

It provides a practical implementation of the core **Retrieval-Augmented Generation** architecture.


## 🎯 Use Cases

This type of application can be used for:

* 📚 Research paper analysis
* 📖 Study material question answering
* 📄 Document analysis
* 🧑‍💼 Business document assistance
* 📋 Report analysis
* 📑 Technical documentation search
* 🎓 Academic knowledge assistants


## 🔒 Security Notes

* Keep your Groq API key private.
* Never commit `.env` to GitHub.
* Use environment variables for local development.
* For deployment, use the platform's secure secrets management system.
* Do not expose API keys in frontend code.


## 🚀 Deployment

The application can be deployed using a Streamlit-compatible hosting platform.

Before deployment, make sure the repository contains:

```text
app.py
requirements.txt
.gitignore
README.md
```

Do **not** upload:

```text
.env
venv/
```

Configure your Groq API key through the deployment platform's secret/environment-variable settings.


## 📌 Project Highlights

* Built a complete end-to-end RAG pipeline.
* Implemented PDF document processing using PyPDF.
* Generated semantic embeddings using Sentence Transformers.
* Implemented vector similarity search using FAISS.
* Integrated an LLM through the Groq API.
* Developed an interactive document chatbot using Streamlit.
* Added document statistics for pages, words, and chunks.
* Implemented chat history using Streamlit session state.


## 👩‍💻 Author

**Raga Sandhiya R**
