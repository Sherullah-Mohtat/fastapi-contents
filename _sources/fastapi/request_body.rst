Request Body in FastAPI
==========================

.. meta::
   :description: Learn how to use request bodies in FastAPI with clear examples. Understand JSON input, Pydantic models, validation, and real-world API usage.
   :keywords: FastAPI request body, FastAPI POST example, Pydantic model FastAPI, JSON request FastAPI, FastAPI tutorial request body
   :author: Sherullah Mohtat
   :robots: index, follow

In real-world applications, APIs often need to receive structured data such as user details, product information, or order data.

This data is sent from the client (browser, mobile app, or frontend) to the server inside the request body.

FastAPI makes working with request bodies simple, powerful, and safe by using Python classes and automatic validation.

=====================================================================================================================================================================

1. What is a Request Body?
------------------------------

A request body is the data sent by the client to the API.

Unlike path or query parameters, which are part of the URL, the request body is sent separately, usually in JSON format.

Example JSON body:

.. code-block:: json

   {
       "name": "Laptop",
       "price": 1200,
       "in_stock": true
   }

This data is sent when creating or updating resources.

=====================================================================================================================================================================

2. Why Request Body is Important
----------------------------------

Request bodies are used when:

- creating new data (POST)
- updating existing data (PUT / PATCH)
- sending complex structured information

Examples:

- Creating a user account  
- Placing an order  
- Adding a product  

Without request bodies, APIs would be very limited.

=====================================================================================================================================================================

3. Creating a Pydantic Model
--------------------------------

To handle request body data, FastAPI uses models.

These models define the structure of the data you expect.

.. code-block:: python

   from pydantic import BaseModel

   class Product(BaseModel):
       name: str
       price: float
       in_stock: bool

Explanation:

- ``name`` → must be a string  
- ``price`` → must be a number  
- ``in_stock`` → must be true or false  

This model acts like a blueprint for incoming data.

=====================================================================================================================================================================

4. Using Request Body in FastAPI
------------------------------------

Now we connect the model to an API endpoint.

.. code-block:: python

   from fastapi import FastAPI
   from pydantic import BaseModel

   app = FastAPI()

   class Product(BaseModel):
       name: str
       price: float
       in_stock: bool

   @app.post("/products")
   def create_product(product: Product):
       return {
           "message": "Product created successfully",
           "data": product
       }

Explanation:

- ``product: Product`` tells FastAPI to expect JSON data
- FastAPI automatically reads and validates it

=====================================================================================================================================================================

5. Sending a Request
------------------------

You can test this using Swagger UI or Postman.

Example request body:

.. code-block:: json

   {
       "name": "Phone",
       "price": 699.99,
       "in_stock": true
   }

FastAPI converts this JSON into a Python object.

=====================================================================================================================================================================

6. Automatic Data Conversion
--------------------------------

FastAPI can convert types automatically.

Example input:

.. code-block:: json

   {
       "name": "Mouse",
       "price": "50",
       "in_stock": "true"
   }

Even though the types are not perfect:

- ``"50"`` → converted to 50.0  
- ``"true"`` → converted to True  

FastAPI fixes small mistakes automatically.

=====================================================================================================================================================================

7. Validation
-----------------

If the data is invalid, FastAPI rejects it.

Example:

.. code-block:: json

   {
       "name": "Keyboard",
       "price": "abc",
       "in_stock": true
   }

Explanation:

- ``"abc"`` cannot be converted to float  
- FastAPI detects the error  
- Request is rejected  

Your function will NOT run.

=====================================================================================================================================================================

8. Optional Fields
----------------------

You can make fields optional.

.. code-block:: python

   from typing import Optional

   class Product(BaseModel):
       name: str
       price: float
       description: Optional[str] = None

Explanation:

- ``description`` is optional  
- If not provided, it defaults to ``None``  

=====================================================================================================================================================================

9. Default Values
---------------------

You can also provide default values.

.. code-block:: python

   class Product(BaseModel):
       name: str
       price: float
       in_stock: bool = True

If the client does not send ``in_stock``, it will automatically be True.

=====================================================================================================================================================================

10. Real-World Example (Order API)
-------------------------------------

Let’s simulate an order system.

.. code-block:: python

   class Order(BaseModel):
       customer_name: str
       item: str
       quantity: int

   @app.post("/orders")
   def create_order(order: Order):
       return {
           "message": "Order placed",
           "order": order
       }

Example request:

.. code-block:: json

   {
       "customer_name": "Ali",
       "item": "Pizza",
       "quantity": 2
   }

This is how real APIs receive structured data.

=====================================================================================================================================================================

11. Combining Path + Body
-----------------------------

You can use both together.

.. code-block:: python

   @app.put("/products/{product_id}")
   def update_product(product_id: int, product: Product):
       return {
           "product_id": product_id,
           "updated_data": product
       }

Explanation:

- ``product_id`` → from URL  
- ``product`` → from request body  

Very common in real applications.

=====================================================================================================================================================================

12. Common Mistakes
----------------------

- Forgetting to use a Pydantic model  
- Sending wrong JSON format  
- Missing required fields  
- Using incorrect data types  

=====================================================================================================================================================================

13. Best Practices
---------------------

- Always use Pydantic models  
- Keep models clean and simple  
- Use meaningful field names  
- Validate data properly  
- Use separate models for different operations if needed  

=====================================================================================================================================================================

14. Summary
---------------

In this tutorial, you learned:

- What a request body is  
- How FastAPI handles JSON input  
- How to use Pydantic models  
- Data conversion and validation  
- Real-world API examples  

=====================================================================================================================================================================

15. Try It Yourself
----------------------

Practice:

1. Create a model:

::

   User with fields:
   - name (string)
   - age (integer)

2. Create endpoint:

::

   POST /users

3. Send JSON data and test validation

