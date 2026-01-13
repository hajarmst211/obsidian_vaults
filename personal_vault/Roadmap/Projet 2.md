
### Step 1: The "Dirty Data" Cleaner
**The Goal:** Write a Python script that takes a "broken" CSV file and turns it into a usable one without you opening Excel.

**What to do:**
1.  **Create/Find the Data:** Create a CSV file named `raw_sales.csv` with intentional errors. Include:
    *   **Dates** in different formats (e.g., "2023/01/01", "Jan 1, 2023", "01-01-23").
    *   **Numbers** with symbols (e.g., "$1,000.00", "500 USD").
    *   **Missing values** (empty cells).
    *   **Duplicates** (copy-paste the same row twice).
2.  **Write the Script (`cleaner.py`):**
    *   Import `pandas`.
    *   Load the CSV.
    *   **Standardize Dates:** Convert the date column to a proper `datetime` object (ISO 8601 format YYYY-MM-DD).
    *   **Clean Numbers:** Remove "$" and "," symbols and convert the column to `float` or `int`.
    *   **Handle Nulls:** Fill missing values with a default (like 0) or drop the rows entirely.
    *   **Deduplicate:** Remove duplicate rows.
    *   **Save:** Export the result to a new file named `clean_sales.csv`.
****
**Key Concept:** Vectorized operations (doing math on a whole column at once rather than using a loop).

---

### Step 2: Build a Watchdog Script
**The Goal:** Automate Step 1. You want the computer to react when a file appears, rather than you pressing "Run."

**What to do:**
1.  **Setup Folders:** Create two folders on your computer: `inbox_folder` and `processed_folder`.
2.  **Write the Script (`watcher.py`):**
    *   **Option A (Beginner):** Write a `while True` loop that sleeps for 10 seconds. Every 10 seconds, it lists the files in `inbox_folder`. If it finds a file, it triggers your cleaning function.
    *   **Option B (Pro):** Use the Python library `watchdog`. It listens to operating system events.
3.  **The Logic:**
    *   When a file (e.g., `sales_data.csv`) lands in `inbox_folder`...
    *   The script detects it.
    *   It runs the cleaning logic from Step 1.
    *   It saves the clean version.
    *   It moves the original file to `processed_folder` using the `shutil` or `os` library.

**Key Concept:** Event-Driven Programming (code that waits for an external action).

---

### Step 3: Create a Database Loader
**The Goal:** Move data from files (JSON) into a structured storage system (SQL).

**What to do:**
1.  **Generate Data:** Write a tiny script to generate 1,000 dummy JSON files. (e.g., `file_1.json` containing `{"id": 1, "user": "Alice", "amount": 50}`).
2.  **Setup the Database:** Use **SQLite** (built into Python) for simplicity, or install **PostgreSQL** (via Docker or local install) if you want a challenge. Create a table called `transactions`.
3.  **Write the Script (`loader.py`):**
    *   Loop through all 1,000 JSON files.
    *   Open and read the JSON data.
    *   **The Wrong Way:** Connect to DB -> Insert Row -> Close DB (Repeat 1000 times). *Do not do this.*
    *   **The Right Way (Batching):** Read all data into a list of tuples. Connect to the DB *once*. Use `cursor.executemany()` to insert all 1,000 records in one go. Commit the transaction.

**Key Concept:** Database Transactions and Batch Processing (optimization).

---

### Step 4: Build an Alert System
**The Goal:** Active monitoring. You don't want to look at dashboards; you want the data to scream at you when something is wrong.

**What to do:**
1.  **Setup the Destination:**
    *   **Discord (Easiest):** Go to a Discord server -> Server Settings -> Integrations -> Webhooks. Copy the Webhook URL.
    *   **Email (Harder):** Use Python's `smtplib` (requires app passwords for Gmail).
2.  **Write the Script (`alert.py`):**
    *   Modify your Step 1 (Cleaner) script.
    *   After cleaning the "Revenue" column, check if any value is `< 0` (or any other logic, like "Total Revenue < Target").
    *   **If True:** Use the `requests` library to POST a message to your Discord Webhook URL: `requests.post(url, json={"content": "ALERT: Negative revenue detected!"})`.

**Key Concept:** Webhooks and Exception Reporting.

---

### How to submit/structure this project
To make this a portfolio piece, organize your folder like this:

```text
/data-pipeline-project
│
├── /input_folder         (Where you drop raw files)
├── /archive_folder       (Where raw files move after processing)
├── /clean_data           (Where the clean CSVs go)
│
├── pipeline_manager.py   (The main script running the Watchdog)
├── cleaner.py            (The module with Pandas logic)
├── db_loader.py          (The module that talks to SQLite)
├── alert_system.py       (The module that sends Discord msgs)
│
├── database.db           (Your SQLite file)
└── requirements.txt      (List of libraries: pandas, watchdog, requests)
```

**Your workflow looks like this:**
1. You run `python pipeline_manager.py`. It sits and waits.
2. You drag a messy CSV into `/input_folder`.
3. The script wakes up, cleans the data, checks for alerts (pings Discord if bad), inserts it into the SQL database, and moves the CSV to archive.
4. You check your database, and the data is there.

---
# Improvements:

### 1. Break the code into a real package structure

Right now everything is in random scripts. Industry practice uses a Python package layout:

```
project_root/
│
├── cleaner/
│   ├── __init__.py
│   ├── cleaner.py
│   ├── config.py
│   └── utils.py
│
├── watcher/
│   ├── __init__.py
│   └── watcher.py
│
├── tests/
│   ├── test_cleaner.py
│   ├── test_watcher.py
│   └── test_utils.py
│
├── input/
├── archive/
│
├── logs/
│
├── requirements.txt
└── pyproject.toml
```

This signals maturity: separation of concerns, testability, packaging, deployment.

### 2. Externalize configuration

Hardcoded folder paths inside code are brittle. Use a config file (YAML or ENV). For example `config.yaml`:

```yaml
paths:
  input_folder: "../input_folder/"
  archive_folder: "../archive_folder/"

cleaning:
  null_threshold: 0.8
  date_threshold: 0.5
```

Then load with `PyYAML`.  
It makes your code instantly portable.

### 3. Improve the cleaning logic with deterministic transform rules

More professional approach:

• wrap each step inside a class  
• log exactly what happened  
• use a schema inference layer (like pandera)

Example:

```python
class DataCleaner:
    def __init__(self, config):
        self.null_threshold = config["cleaning"]["null_threshold"]
        self.date_threshold = config["cleaning"]["date_threshold"]

    def transform(self, df):
        df = self.remove_duplicates(df)
        df = self.strip_columns(df)
        df = self.parse_dates(df)
        df = self.drop_sparse_columns(df)
        df = self.impute_missing(df)
        return df
```

This makes the pipeline predictable and inspectable.

### 4. Add proper logging (not print)

Never use prints in pipelines. Use Python logging:

```python
import logging

logger = logging.getLogger("cleaner")
logger.setLevel(logging.INFO)
handler = logging.FileHandler("logs/pipeline.log")
logger.addHandler(handler)
```

Then:

```python
logger.info(f"Column {column} dropped (null ratio = {ratio})")
```

This mirrors professional ETL pipelines.

### 5. Add exception handling

Right now, if reading CSV fails, your whole watcher crashes.

Wrap main steps:

```python
try:
    df = pd.read_csv(path)
except Exception as e:
    logger.error(f"Failed reading {path} : {e}")
    return
```

Same for writing files, watchdog events, etc.

### 6. Add tests (pytest)

Professionals ship tests.

Examples:

```python
def test_strip_columns():
    df = pd.DataFrame({" A ": [1]})
    cleaned = strip_columns(df)
    assert "A" in cleaned.columns
```

Tests ensure every change doesn’t break the system.

### 7. Add docstrings + type hints

Type hints turn your scripts into readable engineering-grade code.

```python
def drop_duplicated(df: pd.DataFrame) -> pd.DataFrame:
    """Remove duplicate rows."""
```

This helps IDE autocompletion and reduces logic errors.

### 8. Make the cleaner a reusable library (CLI)

Use `argparse` to allow:

```
python -m cleaner --input raw.csv --output out.csv
```

This makes it viable in CI/CD or Airflow.

### 9. Improve the watcher architecture

Right now your watcher runs infinite loops and prints things.

Better:

• use structured logs  
• add debounce logic (some OSs send duplicate events)  
• wrap handler in a try/except  
• add tests for watcher logic using temp directories

Example:

```python
def on_created(self, event):
    try:
        logger.info(f"New file detected: {file_path}")
        df = main_cleaner(file_path)
        logger.info(f"Cleaning done: output saved.")
    except Exception as e:
        logger.error(f"Error processing {file_path}: {e}")
```

### 10. Versioning of cleaned files

Instead of overwriting, add timestamped archives:

`file_cleaned_2025_11_26_23_11_55.csv`

This improves traceability.

### 11. Optional: Add a metadata README file in the archive

Each cleaned file can be accompanied by JSON metadata:

```
sample_test_cleaned.meta.json
```

Content:

```json
{
  "source_file": "sample_test.csv",
  "processed_at": "2025-11-26T23:13:21",
  "rows_before": 100,
  "rows_after": 97,
  "columns_dropped": ["extra_nulls"]
}
```

This is the kind of feature seen in industrial ETL systems.

### 12. Consider using a pipeline framework

If you want to go fully enterprise:

• Prefect  
• Airflow  
• Dagster

You can wrap your cleaner as a task and add scheduling, retry logic, monitoring.

### 13. Add static analysis tools

Include a `.pre-commit` config with:

• black (formatter)  
• flake8 (linting)  
• mypy (type checking)

This forces consistency.

### 14. Add CI/CD

GitHub Actions:

```
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: pip install -r requirements.txt
      - run: pytest
```

Now your pipeline is fully professional.

---

You're essentially upgrading from a "nice script" to a mini **data engineering product**.  
It becomes modular, testable, configurable, reliable, and cleanly structured — the hallmarks of real-world engineering.