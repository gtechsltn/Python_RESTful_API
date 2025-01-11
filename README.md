# How do I create a RESTful API using Python conda anaconda miniconda

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

SQLite (built-in with Python).

MySQL:

```
conda install mysql-connector-python -y
```

PostgreSQL:

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
