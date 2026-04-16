Understanding Core Concepts
============================

.. meta::
   :description: A complete beginner-friendly guide to understanding API fundamentals in FastAPI including API, schema, JSON Schema, OpenAPI, operations, and path operations with real-world examples.
   :keywords: FastAPI API concepts, API schema explained, JSON Schema tutorial, OpenAPI FastAPI, what is path operation, FastAPI decorator explained, backend API basics, learn FastAPI fundamentals
   :author: Sherullah Mohtat
   :robots: index, follow
   
Before writing APIs, you must understand these terms:
	- API
	- Schema
	- Data Schema
	- API Schema
	- JSON Schema
	- OpenAPI
	- Operation
	- Path Operation
	- Path Operation Decorator
	- Path Operation Function

Let’s go step by step.

==========================================================================================================================================================================================

**1. What is an API?**
------------------------

API = Application Programming Interface

Simple meaning:
    A way for two systems to communicate.

**Example:**

You open a food app:
	- You click “Get Restaurants”
	- App sends request → Server
	- Server sends data → App shows results

That communication = API

==========================================================================================================================================================================================

**2. What is a Schema?**
--------------------------

Schema = structure or shape of data

Think like:
    A rule that defines what data should look like.

**Example (Without Schema)**

.. code-block:: json 

    {
        "name": "Ali",
        "age": 25
    }

But what if someone sends:

.. code-block:: json 

    {
        "username": 123,
        "age": "twenty"
    }

**Problem:** inconsistent data

==========================================================================================================================================================================================

**With Schema**

You define rules:

.. code-block:: python

    {
        "name": string,
        "age": integer
    }

Now data must follow this structure

==========================================================================================================================================================================================

**3. Data Schema**
--------------------

Data Schema = structure of data itself

**Example:**

User schema:

.. code-block:: json 

    {
        "id": 1,
        "name": "Sherullah",
        "email": "test@gmail.com"
    }

This defines:
	- what fields exist
	- what type they are

==========================================================================================================================================================================================

**4. API Schema**
-------------------

API Schema = structure of the **API itself**

It defines:
	- endpoints (URLs)
	- request format
	- response format
	- parameters
	- authentication

==========================================================================================================================================================================================

**Example:**

.. code-block:: bash 

    GET /users
    POST /users
    GET /users/{id}

This is part of API schema

==========================================================================================================================================================================================

**5. What is JSON Schema?**
-----------------------------

JSON Schema = a standard way to describe JSON data

**Example:**

.. code-block:: json 

    {
        "type": "object",
        "properties": {
            "name": {"type": "string"},
            "age": {"type": "integer"}
        }
    }

This is a machine-readable schema

==========================================================================================================================================================================================

**6. What is OpenAPI?**
-------------------------

OpenAPI = a **standard format to describe APIs**

Think like:
    A complete blueprint of your API

It includes:
	- endpoints
	- parameters
	- request body
	- responses
	- authentication
	- schemas

==========================================================================================================================================================================================

Real Meaning:
    OpenAPI is like a contract between frontend and backend

Important:
    FastAPI automatically generates OpenAPI for you.

You don’t write it manually.

**Where to see it?**

http://127.0.0.1:8000/openapi.json

http://127.0.0.1:8000/docs (Swagger UI)

==========================================================================================================================================================================================

**Relationship**

Let’s connect everything:
	- JSON Schema → describes data
	- OpenAPI → describes API
	- API Schema → overall structure of API
	- Data Schema → structure of data inside API

==========================================================================================================================================================================================

**7. What is an Operation?**
------------------------------

Operation = **one action of an API**

Example:
	- GET /users → get users
	- POST /users → create user

Each one = **operation**

==========================================================================================================================================================================================

**8. What is a Path Operation?**
----------------------------------

Path Operation = operation tied to a URL path

**Example:**

.. code-block:: bash 

    GET /items

- Path → /items
- Operation → GET

Together = Path Operation

==========================================================================================================================================================================================

**9. Path Operation Decorator**
---------------------------------

Now we enter FastAPI.

.. code-block:: python 

    @app.get("/items")

This is called a **Path Operation Decorator**

==========================================================================================================================================================================================

**Why?**

Because it:
	- defines the path (/items)
	- defines the method (GET)
	- attaches behavior to function

==========================================================================================================================================================================================

**10. Path Operation Function**
---------------------------------

This is the function below the decorator:

.. code-block:: python 

    @app.get("/items")
    def get_items():
        return {"items": []}

This function:
	- runs when API is called
	- returns response

==========================================================================================================================================================================================

**Full Flow**

Let’s connect everything:

.. code-block:: python 

    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/users")
    def get_users():
        return [{"id": 1, "name": "Ali"}]

==========================================================================================================================================================================================

What happens:
	1.	User hits /users
	2.	FastAPI sees:

        - path = /users
        - method = GET

	3.	It calls function get_users()
	4.	Returns data
	5.	FastAPI converts it to JSON
	6.	OpenAPI automatically documents it

==========================================================================================================================================================================================

**Real-World Example**

Imagine:
    Food delivery app

API:
	- GET /restaurants
	- GET /menu
	- POST /order

Each is:
	- operation
	- part of OpenAPI
	- uses schema

==========================================================================================================================================================================================

**Common Confusion**

✖ Schema = only database

✖ OpenAPI = only docs


✔ Schema = data structure

✔ OpenAPI = full API definition

✔ FastAPI uses both automatically

==========================================================================================================================================================================================

**Summary**
-------------

	- API → communication between systems
	- Schema → structure of data
	- JSON Schema → format for JSON structure
	- OpenAPI → full API blueprint
	- Operation → single API action
	- Path Operation → action + URL
	- Decorator → defines route
	- Function → handles request

