Query Parameters in FastAPI
==============================

.. meta::
   :description: Learn how to use query parameters in FastAPI with real-world examples. Understand optional parameters, default values, validation, filtering, pagination, and best practices.
   :keywords: FastAPI query parameters, FastAPI tutorial, query parameter example, FastAPI filtering, FastAPI pagination, optional parameters FastAPI, API query string
   :author: Sherullah Mohtat
   :robots: index, follow

Query parameters allow you to send extra information to your API through the URL.

They are different from path parameters because they are not part of the URL path itself. Instead, they are added at the end of the URL after a question mark (``?``).

Query parameters are commonly used for:

- filtering data (e.g., category, price)
- searching (e.g., keyword)
- pagination (e.g., page number, size)
- controlling output (e.g., sort order)

===================================================================================================================================================

1. What are Query Parameters?
---------------------------------

Query parameters appear after the ``?`` in a URL and are written as key-value pairs.

Example:

::

   /products?category=electronics&limit=5

Let’s understand this step by step:

- ``category=electronics`` → tells the API to show only electronics
- ``limit=5`` → tells the API to return only 5 results

So the path ``/products`` stays the same, but the query parameters change how the data is returned.

===================================================================================================================================================

2. Basic Example
---------------------

Let’s create a simple API endpoint that uses a query parameter.

.. code-block:: python

   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/items")
   def get_items(limit: int):
       return {"limit": limit}

Request:

::

   /items?limit=10

What happens internally:

1. The URL sends ``limit="10"`` (as text)
2. FastAPI sees ``limit: int``
3. It converts ``"10"`` → ``10`` (integer)
4. The function receives the value

Response:

.. code-block:: json

   {
       "limit": 10
   }

This shows how FastAPI reads query parameters and passes them into your function.

===================================================================================================================================================

3. Required vs Optional Parameters
--------------------------------------

Query parameters can be either required or optional depending on how you define them.

Required Parameter
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.get("/orders")
   def get_orders(page: int):
       return {"page": page}

Request must include:

::

   /orders?page=1

Explanation:

- ``page`` has no default value
- So FastAPI expects the user to provide it
- If missing, FastAPI returns an error

===================================================================================================================================================

Optional Parameter
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.get("/orders")
   def get_orders(page: int = 1):
       return {"page": page}

Now:

- ``/orders`` → page = 1 (default value)
- ``/orders?page=3`` → page = 3

When you provide a default value, the parameter becomes optional.

===================================================================================================================================================

4. Multiple Query Parameters
--------------------------------

You can define more than one query parameter in the same function.

.. code-block:: python

   @app.get("/products")
   def get_products(category: str = None, limit: int = 10):
       return {
           "category": category,
           "limit": limit
       }

Request:

::

   /products?category=books&limit=3

Explanation:

- ``category`` filters the type of product
- ``limit`` controls how many results to return

This is how APIs allow users to customize results dynamically.

===================================================================================================================================================

5. Data Types and Conversion
--------------------------------

All query parameters arrive as strings from the URL.

FastAPI automatically converts them into the correct type using your function definition.

.. code-block:: python

   @app.get("/numbers")
   def get_number(value: int):
       return {
           "value": value,
           "type": str(type(value))
       }

Request:

::

   /numbers?value=25

Explanation:

- ``"25"`` is received as text
- FastAPI converts it into integer ``25``
- Inside Python, it behaves like a real number

This allows you to perform calculations without manual conversion.

===================================================================================================================================================

6. Validation
-----------------

FastAPI automatically validates query parameters based on their types.

.. code-block:: python

   @app.get("/numbers")
   def get_number(value: int):
       return {"value": value}

Invalid request:

::

   /numbers?value=abc

Explanation:

- ``"abc"`` cannot be converted into an integer
- FastAPI detects this issue
- The function is NOT executed
- An error response is returned

This prevents invalid data from entering your system.

===================================================================================================================================================

7. Real-World Example (Filtering)
------------------------------------

Let’s simulate a real product filtering system.

.. code-block:: python

   @app.get("/store/products")
   def get_products(category: str = None, max_price: float = None):
       return {
           "category": category,
           "max_price": max_price,
           "message": "Filtered products"
       }

Request:

::

   /store/products?category=shoes&max_price=100

Explanation:

- ``category`` filters products by type
- ``max_price`` limits results to cheaper items

This is how e-commerce websites filter products.

===================================================================================================================================================

8. Pagination Example
-------------------------

Pagination is used when you have a large amount of data.

.. code-block:: python

   @app.get("/articles")
   def get_articles(page: int = 1, size: int = 10):
       return {
           "page": page,
           "size": size,
           "message": "Paginated results"
       }

Request:

::

   /articles?page=2&size=5

Explanation:

- ``page`` → which page of data
- ``size`` → how many items per page

This is used in blogs, social media, and APIs with large datasets.

===================================================================================================================================================

9. Boolean Query Parameters
-----------------------------

FastAPI can also convert boolean values automatically.

.. code-block:: python

   @app.get("/users")
   def get_users(active: bool = True):
       return {"active": active}

Requests:

::

   /users?active=true
   /users?active=false

Explanation:

- ``true`` → True  
- ``false`` → False  

Useful for filters like active/inactive users.

===================================================================================================================================================

10. Query Parameters with Pydantic (Advanced)
-------------------------------------------------

For complex filtering, you can group parameters using a model.

.. code-block:: python

   from pydantic import BaseModel

   class ProductFilter(BaseModel):
       category: str | None = None
       limit: int = 10

   @app.get("/filter-products")
   def filter_products(filter: ProductFilter):
       return filter

Explanation:

- Instead of many parameters, you group them
- This keeps your code clean and organized

===================================================================================================================================================

11. Path Parameters vs Query Parameters
------------------------------------------

Path parameter:

::

   /products/10

Query parameter:

::

   /products?limit=10

Difference:

- Path → identifies a specific resource
- Query → modifies or filters the result

===================================================================================================================================================

12. Common Mistakes
----------------------

- Forgetting default value for optional parameters  
- Using incorrect data types  
- Mixing path and query usage incorrectly  
- Using unclear or short variable names  

===================================================================================================================================================

13. Best Practices
----------------------

- Use query parameters for filtering and searching  
- Use path parameters for identifying resources  
- Always define data types  
- Provide default values when possible  
- Use clear and meaningful parameter names  

===================================================================================================================================================

14. Summary
---------------

In this tutorial, you learned:

- What query parameters are  
- How they are used in FastAPI  
- Required vs optional parameters  
- Automatic type conversion  
- Validation and error handling  
- Real-world use cases like filtering and pagination  

===================================================================================================================================================

15. Try It Yourself
--------------------

Practice:

1. Create endpoint:

::

   /cars?brand=toyota&year=2022

2. Add parameters:
   - brand (string)
   - year (integer)

3. Test with:
   - correct values
   - incorrect values



