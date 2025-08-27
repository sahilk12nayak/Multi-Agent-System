# Multi-Agent-System


---

## **📌 Project Idea: Multi-Agent “Smart Scientific Assistant”**

**Goal:** Automatically research a given scientific topic, gather credible sources, analyze them, and produce a structured report or presentation.

---

### **1. Agent Roles**

#### **1. ResearchAgent**

* **Tasks:**

  * Perform web search on the given topic.
  * Collect scientific papers, reports, and credible blog posts.
* **Tools:**

  1. **Web Search API** (Tavily API or Google Search API)
  2. **ArXiv API / Semantic Scholar API** for academic papers
* **Output:** List of sources with metadata (title, authors, date, link).

---

#### **2. AnalysisAgent**

* **Tasks:**

  * Download PDF/HTML sources from ResearchAgent.
  * Extract key ideas, figures, formulas, citations.
  * Perform numerical/statistical analysis if needed.
* **Tools:**

  1. **LangChain PDFLoader / HTMLLoader** for document ingestion
  2. **Python REPL Tool** for numerical calculations (NumPy, Pandas)
  3. **HuggingFace Summarization Model** for long text summarization
* **Output:** Structured notes and concise summaries.

---

#### **3. WriterAgent**

* **Tasks:**

  * Combine AnalysisAgent’s findings into a logical report.
  * Format it into Markdown, PDF, or presentation slides.
* **Tools:**

  1. **ReportLab** or **Python-pptx** for PDF/slide generation
  2. **Markdown generator** (LangChain built-in)
  3. **Image generator** (DALL·E or Stable Diffusion API) for illustrations
* **Output:** Final scientific report or presentation.

---

### **2. Workflow (LangGraph Flow)**

```text
User → ResearchAgent → AnalysisAgent → WriterAgent → User
```
<img width="722" height="608" alt="image" src="https://github.com/user-attachments/assets/217a8f49-e911-4f00-afdf-8cd0b8a97147" />

**Steps:**

1. **User** enters a topic (e.g., “Impact of Artificial Intelligence on Healthcare”).
2. **ResearchAgent** searches and retrieves relevant sources → passes them to AnalysisAgent.
3. **AnalysisAgent** extracts facts, key data, and insights.
4. **WriterAgent** creates a formatted report or presentation, optionally with illustrations.
5. **User** reviews the final result and requests changes if needed.

---

### **3. Integrated Tools**

At least three distinct tools:

1. **Tavily API / Google Search API** — information retrieval
2. **LangChain PDFLoader + Python REPL Tool** — document parsing & calculations
3. **ReportLab / Python-pptx** — final document generation

(Optional extras: HuggingFace Summarization, DALL·E for images)

---

### **4. Enhancements**

* **Human-in-the-loop:**

  * Approve list of sources before analysis.
  * Choose output format and style before generation.
* **Evaluation metrics:**

  * Compare the multi-agent system’s output vs. a single LLM baseline.
  * Use **ROUGE / BLEU** for summary quality measurement.

---

### **5. Architecture Diagram**

```
[User]
   │
   ▼
[ResearchAgent] --(Tavily API, ArXiv API)--> [Sources]
   │
   ▼
[AnalysisAgent] --(PDFLoader, NumPy/Pandas, HuggingFace Summarizer)--> [Structured Facts]
   │
   ▼
[WriterAgent] --(ReportLab, DALL·E)--> [Final Report/Presentation]
   │
   ▼
[User]
```

 We implemented a simple **MCP (Message-Centric Protocol)** within the LangGraph-style skeleton and added **pytest-compatible automated tests**.


* **MCPMessage & MCPChannel** — lightweight message schema and thread-safe pub/sub channel.
* **Agents** (ResearchAgent, AnalysisAgent, WriterAgent) now **receive/request via MCP** and reply with structured messages.
* **MCPOrchestrator** — an orchestrator that sends requests, waits for replies (with timeout), and runs the full topic flow.
* **Agent lifecycle helpers** (`start_agents`, `stop_agents`) to run agents in threads for integration testing.
* **Automated tests (pytest)**:

  * `test_mcp_message_roundtrip` — verifies ResearchAgent replies to a search request.
  * `test_mcp_full_flow` — end-to-end flow: search → analyze → compose → produce report.
  * `test_agent_unsubscribe_on_stop` — ensures agents unsubscribe properly when stopped.

### How to run the tests

pip install pytest

pytest smart_scientific_assistant_langgraph.py -q







