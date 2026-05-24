## Stategraph example
``` python
from typing import TypedDict, Optional
from langgraph.graph import StateGraph, END

# --- STEP 1: Define the State ---
# This is the "Shared Clipboard" that all agents see.
class AgentState(TypedDict):
    question: str
    sql_query: Optional[str]
    db_results: Optional[str]
    error: Optional[str]
    iterations: int

# --- STEP 2: Define the Nodes (The Workers) ---

def sql_generator_node(state: AgentState):
    """Simulates an LLM generating SQL."""
    print(f"--- NODE: SQL GENERATOR (Attempt {state['iterations'] + 1}) ---")
    
    # In a real app, this is where you'd call GPT-4.
    # We will simulate a mistake on the first try.
    if state['iterations'] == 0:
        query = "SELECT total_sales FROM non_existent_table" 
    else:
        query = "SELECT total_sales FROM sales_data"
        
    return {"sql_query": query, "iterations": state['iterations'] + 1}

def db_executor_node(state: AgentState):
    """Simulates executing the SQL against a database."""
    print("--- NODE: DB EXECUTOR ---")
    query = state['sql_query']
    
    # Simulating a database error
    if "non_existent_table" in query:
        return {"error": "Error: Table 'non_existent_table' does not exist.", "db_results": None}
    else:
        return {"db_results": "$150,000 USD", "error": None}

# --- STEP 3: Define the Logic (The Router) ---

def should_we_stop(state: AgentState):
    """Decides whether to go back to the generator or finish."""
    if state['error'] and state['iterations'] < 3:
        print("Result: Error found. Routing back to Generator...")
        return "retry"
    else:
        print("Result: Success or Max Retries reached. Routing to End...")
        return "end"

# --- STEP 4: Build the Graph ---

workflow = StateGraph(AgentState)

# Add our nodes
workflow.add_node("sql_writer", sql_generator_node)
workflow.add_node("db_executor", db_executor_node)

# Set the Entry Point
workflow.set_entry_point("sql_writer")

# Connect nodes with Edges
workflow.add_edge("sql_writer", "db_executor")

# Add Conditional Logic (The Loop)
workflow.add_conditional_edges(
    "db_executor",
    should_we_stop,
    {
        "retry": "sql_writer",  # If should_we_stop returns "retry", go to sql_writer
        "end": END              # If it returns "end", stop the program
    }
)

# Compile the Graph
app = workflow.compile()

# --- STEP 5: Run it! ---
initial_state = {
    "question": "What were the total sales?",
    "iterations": 0,
    "error": None
}

final_output = app.invoke(initial_state)

print("\n--- FINAL RESULT ---")
print(f"SQL Used: {final_output['sql_query']}")
print(f"Data Found: {final_output['db_results']}")
```
### 3. How does this actually "build" the graph?

Think of workflow = StateGraph(AgentState) as starting a **construction project**.

1. **workflow = StateGraph(AgentState)**: You lay down the foundation. You decide what kind of "materials" (the State) the building will handle.
    
2. **workflow.add_node("name", function)**: You are building the "rooms." You are telling the graph: "I have a station called 'sql_writer', and when the clipboard enters this room, run this specific Python function."
    
3. **workflow.add_edge("sql_writer", "db_executor")**: You are building the "hallways." You are saying: "Once the work in the 'sql_writer' room is done, move the clipboard directly to the 'db_executor' room."
    
4. **workflow.compile()**: This is the final step. It takes your blueprint (the nodes and edges) and turns it into an **executable application**. It checks for mistakes (like a hallway leading to a room that doesn't exist) and prepares the logic for running.
    

### Summary

- **AgentState** is the **Blueprint** (What data do we have?).
    
- **StateGraph** is the **Architect** (The logic provided by the library).
    
- **.add_node()** and **.add_edge()** are the **Tools** used to build the house.
    
- **.compile()** is the **Finished Building** ready for people (data) to move in.
- 