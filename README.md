# How do I create a RESTful API using Python conda anaconda miniconda

Agenda

+ Python Hello World
+ Python RESTful API
+ Python SQL Server
+ Python Logging

https://github.com/gtechsltn/Python_RESTful_API/

https://github.com/gtechsltn/NET9_BlazorServerSide_BankingApp/

Creating a RESTful API using Python with Conda (Anaconda or Miniconda) involves setting up a virtual environment, installing the necessary dependencies, and implementing the API using a web framework like Flask or FastAPI.

Here's a step-by-step guide:

# 1. Install Conda (Anaconda or Miniconda)

If you don’t already have Conda installed:

Anaconda: Full package with a GUI and data science tools. Download Anaconda

Miniconda: Lightweight version of Conda. Download Miniconda

# 2. Create a Conda Environment

Open a terminal or Anaconda Prompt.

Create a new environment:

```
conda create -n myapi python=3.10 -y
```

Activate the environment:

```
conda activate myapi
```

# 3. Install Dependencies

## Install Flask or FastAPI (choose one):

Flask:

```
conda install flask -y
```

FastAPI (requires pip):

```
pip install fastapi uvicorn
```

## Optional: Install database libraries if needed:

### SQLite (built-in with Python).

### MySQL:

```
conda install mysql-connector-python -y
```

### PostgreSQL:

```
pip install psycopg2
```

# 4. Create the RESTful API

## Option 1: Using Flask

Create a file, app.py:

```
from flask import Flask, request, jsonify

app = Flask(__name__)

# Sample data
items = [
    {"id": 1, "name": "Item 1"},
    {"id": 2, "name": "Item 2"}
]

@app.route('/items', methods=['GET'])
def get_items():
    return jsonify(items)

@app.route('/items/<int:item_id>', methods=['GET'])
def get_item(item_id):
    item = next((item for item in items if item["id"] == item_id), None)
    return jsonify(item) if item else ("Item not found", 404)

@app.route('/items', methods=['POST'])
def create_item():
    new_item = request.json
    items.append(new_item)
    return jsonify(new_item), 201

if __name__ == '__main__':
    app.run(debug=True)
```

### Run the Flask app:

```
python app.py
```

### Access it in your browser or API tool like Postman:

GET http://127.0.0.1:5000/items

POST http://127.0.0.1:5000/items with JSON payload.

## Option 2: Using FastAPI

Create a file, app.py:

```
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

# Sample data
class Item(BaseModel):
    id: int
    name: str

items = [
    {"id": 1, "name": "Item 1"},
    {"id": 2, "name": "Item 2"}
]

@app.get("/items")
async def get_items():
    return items

@app.get("/items/{item_id}")
async def get_item(item_id: int):
    item = next((item for item in items if item["id"] == item_id), None)
    if not item:
        raise HTTPException(status_code=404, detail="Item not found")
    return item

@app.post("/items")
async def create_item(item: Item):
    items.append(item.dict())
    return item
```

### Run the FastAPI app using Uvicorn:

```
uvicorn app:app --reload
```

### Access the API and the automatically generated documentation:

API: http://127.0.0.1:8000/items

Swagger UI: http://127.0.0.1:8000/docs

# 5. Test the API

Use Postman, cURL, or httpie for testing API endpoints:

```
curl -X GET http://127.0.0.1:5000/items
curl -X POST http://127.0.0.1:5000/items -H "Content-Type: application/json" -d '{"id": 3, "name": "Item 3"}'
```

# 6. Deployment (Optional)

To deploy the API:

## Flask:

Use a production server like Gunicorn or uWSGI.

```
pip install gunicorn
gunicorn -w 4 app:app
```

## FastAPI:

Uvicorn can be used in production with settings adjusted for performance:

```
uvicorn app:app --host 0.0.0.0 --port 80 --workers 4
```

With this setup, you have a functioning RESTful API ready for local development or deployment. Let me know if you need additional help!

# Python driver for SQL Server

To connect Python applications to SQL Server, you can use the pyodbc or pymssql libraries, or the Microsoft ODBC Driver for SQL Server. Below is a step-by-step guide for setting up and using these drivers:

Connecting to Microsoft SQL server using Python

Python SQL Server Integration using **pyodbc** : 5 steps

## 1. Install the Python SQL Server Driver

### Option 1: pyodbc (Recommended)

**pyodbc** is widely used and reliable for connecting Python to SQL Server.

Install pyodbc:

```
pip install pyodbc
```

Install the ODBC Driver for SQL Server for your system:

+ Windows
+ Linux
+ macOS

### Option 2: pymssql (Deprecated)

**pymssql** was commonly used but is no longer actively maintained. Use pyodbc instead if possible.

Install pymssql:

```
pip install pymssql
```


## 2. Connect to SQL Server

### Using pyodbc:

Here's how to establish a connection:

Code Example:

``
import pyodbc

# Connection details
server = 'your_server_name'  # e.g., 'localhost' or '127.0.0.1'
database = 'your_database_name'
username = 'your_username'
password = 'your_password'

# Create a connection
connection_string = f"DRIVER={{ODBC Driver 17 for SQL Server}};SERVER={server};DATABASE={database};UID={username};PWD={password}"
conn = pyodbc.connect(connection_string)

# Create a cursor and execute a query
cursor = conn.cursor()
cursor.execute("SELECT @@VERSION")
row = cursor.fetchone()
print(f"SQL Server version: {row[0]}")

# Close the connection
cursor.close()
conn.close()
```

#### Key Notes:
+ Ensure the ODBC Driver version matches the driver installed on your system (e.g., ODBC Driver 18 for SQL Server).
+ Replace placeholders (your_server_name, your_database_name, etc.) with actual details.

### Using pymssql:

Here's a similar connection example:

Code Example:

```
import pymssql

# Connection details
server = 'your_server_name'  # e.g., 'localhost' or '127.0.0.1'
database = 'your_database_name'
username = 'your_username'
password = 'your_password'

# Create a connection
conn = pymssql.connect(server=server, user=username, password=password, database=database)

# Create a cursor and execute a query
cursor = conn.cursor()
cursor.execute("SELECT @@VERSION")
row = cursor.fetchone()
print(f"SQL Server version: {row[0]}")

# Close the connection
cursor.close()
conn.close()
```

## 3. Common Queries

### Insert Data:
```
cursor.execute("INSERT INTO TableName (Column1, Column2) VALUES (?, ?)", (Value1, Value2))
conn.commit()
```

### Retrieve Data:
```
cursor.execute("SELECT * FROM TableName")
rows = cursor.fetchall()
for row in rows:
    print(row)
```

### Update Data:
```
cursor.execute("UPDATE TableName SET Column1 = ? WHERE Column2 = ?", (NewValue, ConditionValue))
conn.commit()
```

### Delete Data:
```
cursor.execute("DELETE FROM TableName WHERE Column1 = ?", (ConditionValue,))
conn.commit()
```

## 4. Troubleshooting
+ Driver Not Found: Ensure the correct ODBC driver is installed (e.g., ODBC Driver 17 for SQL Server).
+ Connection Refused: Verify SQL Server is running and accessible on the specified server and port.
+ Authentication Errors: Confirm credentials and that SQL Server authentication is enabled.

# Logging in Python

## 1. Basic Logging

The simplest way to use the logging module is to log messages to the console.

```
import logging

# Configure logging
logging.basicConfig(level=logging.INFO)

# Log messages
logging.debug("This is a DEBUG message.")
logging.info("This is an INFO message.")
logging.warning("This is a WARNING message.")
logging.error("This is an ERROR message.")
logging.critical("This is a CRITICAL message.")
```

Output:

```
This is an INFO message.
This is a WARNING message.
This is an ERROR message.
This is a CRITICAL message.
```

## 2. Logging Levels

The logging module has the following severity levels, from lowest to highest:

+ DEBUG: Detailed information, typically for diagnosing problems.
+ INFO: Confirmation that things are working as expected.
+ WARNING: An indication something unexpected happened or might cause problems.
+ ERROR: A more serious issue, the software has not been able to perform some function.
+ CRITICAL: A very serious error, the program may not continue.

Set the logging level with:

```
logging.basicConfig(level=logging.<LEVEL>)
```

For example, logging.basicConfig(level=logging.DEBUG) will log all messages, including DEBUG and higher levels.

## 3. Customize Logging Format

You can configure the log message format using format in basicConfig.

```
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
)

logging.info("This is an INFO message.")
```

Output:

```
2025-01-11 10:00:00,000 - root - INFO - This is an INFO message.
```

### Common Format Specifiers:

+ %(asctime)s: Timestamp of the log message.
+ %(name)s: Logger name.
+ %(levelname)s: Severity level of the log.
+ %(message)s: The log message.
+ %(filename)s: Name of the source file.
+ %(lineno)d: Line number where the log call occurred.

## 4. Logging to a File

You can log messages to a file instead of the console by specifying a filename in basicConfig.

```
logging.basicConfig(
    filename="app.log",
    level=logging.DEBUG,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.info("This message will be written to app.log.")
```

## 5. Advanced Logging Configuration

For more control, use logging objects like Logger, Handler, and Formatter.

Example with Multiple Handlers:

```
import logging

# Create a logger
logger = logging.getLogger("MyLogger")
logger.setLevel(logging.DEBUG)

# Create handlers
console_handler = logging.StreamHandler()
file_handler = logging.FileHandler("app.log")

# Set levels for handlers
console_handler.setLevel(logging.WARNING)
file_handler.setLevel(logging.DEBUG)

# Create formatters and add to handlers
formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")
console_handler.setFormatter(formatter)
file_handler.setFormatter(formatter)

# Add handlers to the logger
logger.addHandler(console_handler)
logger.addHandler(file_handler)

# Log messages
logger.debug("This is a DEBUG message.")
logger.warning("This is a WARNING message.")
```

## 6. Rotating Log Files

Use RotatingFileHandler to manage log file sizes and rotate them when they exceed a certain size.

Example:

```
from logging.handlers import RotatingFileHandler

# Create a logger
logger = logging.getLogger("RotatingLogger")
logger.setLevel(logging.DEBUG)

# Create a rotating file handler
handler = RotatingFileHandler("rotating_app.log", maxBytes=2000, backupCount=5)
handler.setFormatter(logging.Formatter("%(asctime)s - %(levelname)s - %(message)s"))

logger.addHandler(handler)

# Log messages
for i in range(100):
    logger.debug(f"This is log message {i}.")
```

+ maxBytes: Maximum size of a log file (in bytes) before rotation.
+ backupCount: Number of backup files to keep.

## 7. Logging in a Module

To log within modules, create loggers using the module's __name__.

Example:

```
# mymodule.py
import logging

logger = logging.getLogger(__name__)

def some_function():
    logger.info("Log from some_function.")
```

```
# main.py
import logging
import mymodule

logging.basicConfig(level=logging.INFO)

mymodule.some_function()
```

## 8. Third-Party Logging Libraries

Loguru: A modern logging library with better syntax and features.

```
pip install loguru
```

Example:

```
from loguru import logger

logger.info("This is a log message.")
logger.debug("Debugging information.")
```
