# 🏥 Maya: Clinical AI Agent

<div align="center">
  <p><strong>An advanced, Agentic Retrieval-Augmented Generation (RAG) medical assistant powered by Llama-3 and LangGraph.
   Code doesnt include the latest version of the system</strong></p>
</div>

---

**Maya** is an evidence-based clinical AI assistant designed to answer medical questions, explain health concepts, and provide information from reliable medical literature. It utilizes a sophisticated multi-node decision graph to intelligently triage queries, translate languages, retrieve academic context, and cross-verify its own answers to prevent hallucinations.

> ⚠️ **Medical Disclaimer:** *Maya is an AI, not a licensed healthcare professional. This project is for educational and research purposes only. It cannot officially diagnose conditions, prescribe medications, or replace professional medical advice. In case of an emergency, call your local emergency services (e.g., 112 / 911).*

---

## ✨ Key Features

* 🧠 **Agentic LangGraph Workflow:** Employs a complex state graph for routing, including dedicated nodes for Triage, Hypothetical Document Embeddings (HyDE), Retrieval, and Emergency protocol generation.
* 🛡️ **Zero-Hallucination Guardrails:** Uses a DeBERTa-based Natural Language Inference (NLI) Cross-Encoder to self-correct and verify that the generated response is strictly entailed by the retrieved medical context.
* 🌍 **Bilingual Support (EN/TR):** Integrated with Meta's NLLB-200, allowing users to ask questions in Turkish while the AI searches English medical databases (e.g., PubMed) and translates the evidence-based answer back to Turkish.
* 🎯 **High-Precision RAG:** Combines `S-PubMedBert-MS-MARCO` embeddings with a FAISS vector database and a `MiniLM-L-6-v2` Cross-Encoder for highly accurate document retrieval and reranking.
* 🚨 **Smart Triage:** Automatically detects life-threatening emergency queries and bypasses the standard RAG pipeline to immediately output strict, step-by-step first-aid protocols.
* 💻 **Dual UI Options:** Includes both a dual-pane Gradio interface (with real-time LangGraph background debugging) and a decoupled Streamlit + FastAPI frontend/backend architecture.

---

## ⚙️ System Architecture

Maya's cognitive engine is built on **LangGraph** and follows a strict execution path:

1. **Input Translation:** Detects user language and translates to English if necessary.
2. **Query Correction:** Optimizes clinical terminology and fixes typos to improve database search accuracy.
3. **Smart Triage:** An LLM routes the query to exactly one category: `EMERGENCY`, `GENERAL`, or `RAG`.
4. **HyDE (Hypothetical Document Embeddings):** Generates a hypothetical medical textbook excerpt based on the query to improve vector similarity matching.
5. **Retrieve & Rerank:** Searches the FAISS database (1.2M+ medical documents) and reranks results using a Cross-Encoder.
6. **Generate:** Llama-3-8B synthesizes an empathetic, patient-friendly response strictly using the retrieved context.
7. **Self-Correction (NLI):** Verifies that the response doesn't contradict the context. If it fails, it falls back to a safe "insufficient data" response.
8. **Output Translation:** Translates the final clinical response back to the user's native language.

---

## 🛠️ Technology Stack

| Category | Technology |
| :--- | :--- |
| **Core LLM** | Unsloth / `Llama-3-8B-Instruct` (4-bit quantized) |
| **Orchestration** | LangGraph & LangChain |
| **Vector Database** | FAISS (CPU) |
| **Embeddings** | `S-PubMedBert-MS-MARCO` |
| **Reranker** | `ms-marco-MiniLM-L-6-v2` (Cross-Encoder) |
| **NLI / Verification** | `nli-deberta-v3-small` (Cross-Encoder) |
| **Translation** | `nllb-200-distilled-600M` |
| **Frontend Options** | Gradio / Streamlit |

---

## 🚀 Quick Start (Google Colab / Local)

### Option 1: Gradio (Single Node Deployment)
The easiest way to run Maya is using the integrated Gradio app, which handles the backend models and frontend UI in a single script.

1. Clone the repository.
2. Upload the notebook to Google Colab (recommended for free GPU access).
3. Ensure you have your FAISS vector database in your Google Drive (update the `DB_PATH` in the code).
4. Run the Gradio cell. A `gradio.live` link will be generated automatically for immediate access.

### Option 2: Decoupled Architecture (FastAPI Backend + Streamlit Frontend)
For a production-like setup, run the heavy models on a GPU backend and the UI locally.

1. **Backend (Colab GPU):**
   * Run `maya_backend.py` in Colab.
   * Use Ngrok to expose the FastAPI server (requires a free Ngrok Authtoken).
   * Copy the generated `https://...ngrok-free.app` URL.
2. **Frontend (Local Machine):**
   * Install requirements: `pip install streamlit requests`
   * Open `maya_app.py` and paste the Ngrok URL into the `MAYA_API_URL` variable.
   * Run the app: `streamlit run maya_app.py`

---

## 🤝 Contributing
Contributions, issues, and feature requests are welcome! Feel free to check the issues page if you want to contribute to the project.

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
