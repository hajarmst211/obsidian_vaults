### Phase 1: The Foundation (Data & Tools)

*   **Step 1: Set up the Folder Structure.** 
    *   Create the folders I mentioned earlier (`src`, `app`, `data`, etc.). Initialize a Git repository.
*   **Step 2: Create the Mock Database.** 
    *   Write a script (`setup_db.py`) to create an SQLite database with 3-4 tables (Customers, Orders, Products). Populate them with 10 rows of fake data.
*   **Step 3: Establish the Connection.**
    *   Write a file `src/database/connection.py` that simply connects to your DB and returns a "List of Table Names." If you can see the table names in Python, you’ve won this step.

---

### Phase 2: The Core Engine (The SQL Agent)

*   **Step 4: The "Schema Explainer."**
    *   Write a function that reads your DB schema and turns it into a text string (e.g., "Table 'Orders' has columns id, date, and total"). This string is what you will send to the LLM.
*   **Step 5: The Simple Text-to-SQL.**
    *   Create a script that takes a user question + the schema string, sends it to GPT-4, and gets a SQL query back. Print the SQL query to your terminal.
*   **Step 6: The "Try-Except" Loop (Self-Correction).**
    *   Write a function that tries to execute that SQL. If it hits an error (like a wrong column name), send the error back to the LLM and ask it to provide a fixed query. **This is your first "Agentic" behavior.**

---

### Phase 3: Brains & Logic (The Multi-Agent Flow)

*   **Step 7: Define the LangGraph State.**
    *   Create `src/graph/state.py`. Define your "Shared Clipboard" (the `TypedDict`). It should hold the user query, the generated SQL, and the data results.
*   **Step 8: Connect the Nodes.**
    *   In `src/graph/workflow.py`, create two functions: `sql_agent` and `analyst_agent`. Use LangGraph to connect them so the output of the SQL agent automatically flows into the Analyst.

---

### Phase 4: Interpretation & RAG

*   **Step 9: The "So What?" Agent (The Analyst).**
    *   Create a prompt for the Analyst Agent. Its job is to take the raw numbers (e.g., `[100, 200, 150]`) and write a sentence: "Sales peaked on Tuesday but dropped by 20% on Wednesday."
*   **Step 10: The Strategist (RAG).**
    *   Create a simple text file called `business_rules.txt` (e.g., "If sales are down, suggest checking marketing spend"). Use a RAG tool to let the Strategist Agent read this file before giving advice.

---

### Phase 5: The Interface (Streamlit)

*   **Step 11: The Chat Interface.**
    *   Build a simple Streamlit app with a `st.chat_input`. When the user types a question, it triggers your LangGraph workflow.
*   **Step 12: Visualization.**
    *   Ask your Analyst agent to return a JSON format for a chart. Use `st.plotly_chart` in Streamlit to render that JSON as a beautiful graph.


---
```
ai_bi_system/
├── app/                        # Streamlit UI Layer
│   ├── main.py                 # Entry point for the UI
│   ├── components/             # Reusable UI elements (charts, chat bubbles)
│   │   ├── charts.py           # Plotly rendering logic
│   │   └── sidebar.py          # Config/Settings UI
│   └── session_state.py        # Managing conversation history
│
├── src/                        # Core Logic (The "Brain")
│   ├── agents/                 # Individual Agent Definitions
│   │   ├── planner.py          # Agent 1: Orchestrator logic
│   │   ├── sql_engineer.py     # Agent 2: Text-to-SQL + Self-Correction
│   │   ├── analyst.py          # Agent 3: Python/Pandas logic
│   │   └── strategist.py       # Agent 4: RAG-based recommendations
│   │
│   ├── graph/                  # LangGraph Orchestration
│   │   ├── state.py            # Definition of the TypedDict State
│   │   └── workflow.py         # Connecting nodes and edges
│   │
│   ├── tools/                  # Tools available to agents
│   │   ├── db_tools.py         # SQL execution and schema inspection
│   │   ├── python_tool.py      # Python REPL for data analysis
│   │   └── rag_tool.py         # Vector DB search logic
│   │
│   ├── database/               # Database Connection & Setup
│   │   ├── connection.py       # SQLAlchemy engine setup
│   │   └── schema_info.py      # Metadata/Comments about your tables
│   │
│   └── config.py               # Environment variables & LLM settings
│
├── data/                       # Local Data Storage
│   ├── raw/                    # Initial CSVs for loading
│   └── vector_store/           # ChromaDB or FAISS local files
│
├── prompts/                    # System Prompts (stored as Markdown/Text)
│   ├── sql_prompt.md
│   ├── analyst_prompt.md
│   └── strategist_prompt.md
│
├── tests/                      # Testing the agents
│   └── test_sql_gen.py
│
├── .env                        # API Keys (OpenAI, Anthropic, DB_URL)
├── requirements.txt            # Python dependencies
├── README.md                   # Project documentation
└── setup_db.py                 # Script to initialize Postgres and load mock data
```