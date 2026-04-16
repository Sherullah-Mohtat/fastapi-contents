Environment Setup
===================

.. meta::
   :description: Step-by-step FastAPI environment setup tutorial for beginners. Learn how to install FastAPI and Uvicorn, create a Python virtual environment, run your first API, and use automatic documentation.
   :keywords: FastAPI setup, FastAPI environment setup, FastAPI installation tutorial, Python virtual environment, install Uvicorn, FastAPI beginner tutorial, create first FastAPI app, FastAPI Swagger docs
   :author: Sherullah Mohtat
   :robots: index, follow
   
If you want to build modern APIs with Python, **FastAPI is one of the best tools available today**. But before writing any code, you need a clean and proper development environment.

In this guide, we will set up everything from scratch — the **right way**, like a real developer.

==========================================================================================================================================================================================

**1. Why Environment Setup Matters**
--------------------------------------

Before jumping into coding, let’s understand this:
    - If you install everything globally, your system becomes messy
    - Different projects may need different versions of libraries
    - You can break your system without realizing it

That’s why professionals always use:
    Virtual Environments

Think of it like:
    Each project has its own “box” with its own Python and libraries.

==========================================================================================================================================================================================

**2. Requirements**
---------------------

Before starting, make sure you have:
	- Python installed (version 3.8+ recommended)
	- Terminal / Command Line access

Check Python version:

.. code-block:: bash 

    python3 --version

If you see something like:

.. code-block:: bash 

    Python 3.10.4

You’re good to go 

==========================================================================================================================================================================================

**3. Create Your Project Folder**
-----------------------------------

Let’s create a clean workspace:

.. code-block:: bash 

    mkdir fastapi-blog
    cd fastapi-blog

This will be your main project directory.

==========================================================================================================================================================================================

**4. Create Virtual Environment**
-----------------------------------

Now create an isolated environment:

.. code-block:: bash 

    python3 -m venv venv

This creates a folder named venv.

==========================================================================================================================================================================================

**5. Activate Virtual Environment**
-------------------------------------

On macOS / Linux:

.. code-block:: bash 

    source venv/bin/activate

On Windows:

.. code-block:: bash 

    venv\Scripts\activate

When activated, you’ll see something like:

.. code-block:: bash 

    (venv) your-name@machine fastapi-blog %

This means you are inside your project environment.

==========================================================================================================================================================================================

**6. Install FastAPI and Uvicorn**
------------------------------------

Now install the core packages:

.. code-block:: bash 

    pip install fastapi uvicorn

What are these?
	- FastAPI → the framework (your main tool)
	- Uvicorn → the server (runs your API)

Think like this:
    - FastAPI = brain
    - Uvicorn = engine

==========================================================================================================================================================================================

**7. Create Your First FastAPI File**
---------------------------------------

Create a file:

.. code-block:: bash 

    touch main.py

Now write this code:

.. code-block:: python 

    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/")
    def home():
        return {"message": "Welcome to FastAPI!"}

==========================================================================================================================================================================================

**8. Run the Server**
-----------------------

Start your API:

.. code-block:: bash 

    uvicorn main:app --reload

What this means:
	- main → file name
	- app → FastAPI instance
	- --reload → auto-restart on code changes

==========================================================================================================================================================================================

**9. Open in Browser**
------------------------

Go to:
- http://127.0.0.1:8000

You’ll see:

.. code-block:: json 

    {"message": "Welcome to FastAPI!"}

==========================================================================================================================================================================================

**10. Explore Automatic Docs**
--------------------------------

FastAPI gives you free API documentation.

Swagger UI:
    http://127.0.0.1:8000/docs

Alternative Docs:
    http://127.0.0.1:8000/redoc

This is one of FastAPI’s most powerful features — you don’t need to build docs manually.

==========================================================================================================================================================================================

**11. Best Practices**
------------------------

✔ Always use virtual environment

✔ Use clear project folder structure

✔ Install only required packages

✔ Keep a requirements.txt file:

.. code-block:: bash 

    pip freeze > requirements.txt

==========================================================================================================================================================================================

**12. Common Beginner Mistakes**
----------------------------------

- Not activating virtual environment
- Installing packages globally
- Forgetting --reload during development
- Wrong file name in uvicorn command
- Using old Python version

==========================================================================================================================================================================================

**Summary**
-------------

In this tutorial, you learned:
	- Why virtual environments are important
	- How to create and activate one
	- How to install FastAPI and Uvicorn
	- How to run your first API
	- How to access automatic documentation

