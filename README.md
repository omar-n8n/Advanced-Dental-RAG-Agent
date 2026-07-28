# 🦷 Advanced Dental Clinic RAG Support Agent

An automated, intelligent customer support system built using **n8n**, **OpenAI**, **Pinecone Vector Store**, and **Retrieval-Augmented Generation (RAG)** architecture. 

This workflow seamlessly integrates knowledge base ingestion, real-time messaging, AI-driven decision-making, and intelligent escalation mechanisms for dental practice management.

---

## ✨ Key Features

* **🤖 RAG-Powered Intelligence:** Combines OpenAI LLM with Pinecone Vector Store for precise, context-aware answers based on clinic documentation.
* **🔄 Automated Knowledge Sync:** Dual ingestion pipeline (Automatic via Google Drive triggers + Manual fallback) to keep the clinic's knowledge base always up to date.
* **⚡ Smart Intent Evaluation & Routing:** Dynamically parses AI responses to evaluate confidence, status, and response accuracy before replying.
* **🚨 Seamless Human Escalation:** Automatically routes complex inquiries to human agents by generating unique Ticket IDs, logging them into Google Sheets, and alerting the support team via email.
* **⚡ Real-Time Webhook Processing:** Integrates directly with Meta Messenger Webhooks for instant patient interaction and smooth execution.
* **🛡️ Out-of-Scope Handling:** Gracefully manages off-topic or unsupported user questions with polite, structured fallback responses.

---

## 📐 System Architecture Overview

```text
[ Google Drive / Manual Ingestion ] ➔ [ Vector Database (Pinecone) ]
                                                │
[ Customer (Messenger) ] ➔ [ Customer Request Processing ]
                                                │
                                    [ AI Decision Engine ]
                                   /          |          \
                 [ Answered Response ]  [ Escalation ]  [ Out of Scope ]
                                              │
                                    [ Google Sheets & Email ]
```

---

## 🛠️ Key Modules Breakdown

### 1. 🗄️ Vector Database Ingestion (Knowledge Base)
Automatically updates and synchronizes the clinic’s knowledge base into **Pinecone Vector Store (`Clinic Knowledge Base`)**.
* **Triggers:**
  * **Automatic Trigger:** Detects new document uploads in Google Drive.
  * **Manual Trigger:** Allows on-demand batch ingestion.
* **Text Processing Pipeline:** Uses **Recursive Character Text Splitter** to chunk documents efficiently before generating vector embeddings and storing them in Pinecone.

---

### 2. 📨 Customer Request Processing
Handles real-time incoming messages from patients through social channels.
* **Webhook Ingestion:** Listens to incoming **Messenger Webhooks**.
* **Data Extraction:** Extracts essential parameters such as `Sender ID`, `Customer Name`, and `Message Text`.
* **RAG Retrieval:** Connects to Pinecone via OpenAI Embeddings to retrieve relevant context from the **Clinic Knowledge Base**.

---

### 3. 🧠 AI Decision Engine & Routing
Processes the context and categorizes the response quality.
* **Parse AI Response:** Evaluates response criteria including `Status`, `Confidence Score`, and the candidate `Response`.
* **Route by Status:** Dynamically routes the workflow based on confidence and intent evaluation.

---

### 4. 🔀 Workflow Execution Paths

#### 🟢 Path A: Answered Response
* Triggered when the AI has high confidence and accurate knowledge base information.
* Instantly sends a direct, helpful response back to the customer.

#### 🔵 Path B: Escalation Flow (Human Handoff)
* Triggered when the inquiry requires human support, custom scheduling, or complex clinic inquiries.
* **Ticket Generation:** Generates a unique `Ticket ID`.
* **Logging:** Logs customer details and tickets into **Google Sheets**.
* **Notification:** Automatically sends an alert email to the **Support Team**.
* **Customer Feedback:** Sends a friendly follow-up message to the customer assuring them a support representative will reach out.

#### ⚪ Path C: Out of Scope Response
* Handles questions unrelated to dental services or clinic operations with polite, predefined boundary responses.

---

## ⚡ Tech Stack

* **Workflow Automation:** n8n
* **LLM Engine:** OpenAI (GPT Model)
* **Vector Store:** Pinecone (`Clinic Knowledge Base`)
* **Text Chunking:** Recursive Character Text Splitter
* **Database & Messaging:** Google Drive, Google Sheets, Gmail API, Messenger Webhooks

---

## 🚀 Getting Started

1. Clone this repository:
   ```bash
   git clone [https://github.com/YOUR_USERNAME/Advanced-Dental-RAG-Agent.git](https://github.com/YOUR_USERNAME/Advanced-Dental-RAG-Agent.git)
   ```
2. Import the JSON workflow file into your **n8n** instance.
3. Configure your API credentials for:
   * **OpenAI**
   * **Pinecone**
   * **Google Drive / Sheets / Gmail**
   * **Meta Messenger App**
4. Activate the workflow!