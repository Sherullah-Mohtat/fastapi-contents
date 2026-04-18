Query Parameters and String Validation in FastAPI
=====================================================

.. meta::
   :description: Learn query parameters and string validation in FastAPI with simple explanations, practical examples, and real-world use cases.
   :keywords: FastAPI query validation, FastAPI string validation, FastAPI Query, FastAPI Annotated, query parameter length validation, FastAPI tutorial
   :author: Sherullah Mohtat
   :robots: index, follow

Query parameters are often used to search, filter, and control API results. In many real applications, these values should not be accepted without rules. For example, a search keyword may need a minimum or maximum length, or a sorting option may need to match a specific pattern.

FastAPI allows you to add these rules clearly and cleanly.

1. Why String Validation Matters
------------------------------------

In real APIs, query parameters are often entered by users or frontend applications. Because of that, they can be empty, too long, malformed, or simply not useful.

Imagine a product search API.

If someone sends:

::

   /products/search?q=

or:

::

   /products/search?q=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa

the backend may receive a value that is not helpful.

Validation helps you control this.

It allows you to say things like:

- this text can be optional
- this text must not exceed a certain length
- this text must have a minimum number of characters
- this text must follow a pattern

2. Basic Query String Example
---------------------------------

Let us begin with a simple search endpoint.

.. code-block:: python

   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/products/search")
   def search_products(q: str | None = None):
       return {
           "search_term": q
       }

Explanation:

- ``q`` is a query parameter
- it is optional because its default value is ``None``
- the user may call the route with or without a search keyword

Examples:

::

   /products/search
   /products/search?q=phone

3. Adding Validation Rules
------------------------------

Sometimes you want to control how long the query text can be.

For example, maybe your search input should not be more than 30 characters.

.. code-block:: python

   from fastapi import FastAPI, Query

   app = FastAPI()

   @app.get("/products/search")
   def search_products(q: str | None = Query(default=None, max_length=30)):
       return {
           "search_term": q
       }

Explanation:

- ``Query(...)`` is used when you want more control over a query parameter
- ``max_length=30`` means the text cannot be longer than 30 characters

This is useful for search boxes, promo codes, usernames, and tags.

4. Why Use Query Instead of a Plain Default Value?
-------------------------------------------------------

A plain default value makes a parameter optional, but it does not give you extra validation rules.

Example without validation:

.. code-block:: python

   @app.get("/articles")
   def get_articles(keyword: str | None = None):
       return {"keyword": keyword}

Example with validation:

.. code-block:: python

   @app.get("/articles")
   def get_articles(keyword: str | None = Query(default=None, min_length=3, max_length=20)):
       return {"keyword": keyword}

The second version is more powerful because now the API checks the length of the input automatically.

5. Using Annotated for Cleaner Code
----------------------------------------

When your parameters become more descriptive, the code can get harder to read. ``Annotated`` helps make the meaning clearer.

.. code-block:: python

   from typing import Annotated
   from fastapi import FastAPI, Query

   app = FastAPI()

   @app.get("/jobs/search")
   def search_jobs(
       keyword: Annotated[str | None, Query(min_length=2, max_length=25)] = None
   ):
       return {
           "keyword": keyword
       }

Explanation:

- the type is still ``str | None``
- the validation rules are placed inside ``Query(...)``
- ``Annotated`` connects both cleanly

This style becomes especially helpful when you add more metadata later.

Common FastAPI Helpers Used with Annotated
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python 

    parameter: Annotated[type, FastAPI_helper(...)]

When using Annotated[...], FastAPI allows you to attach extra information that tells where data comes from and how it should be validated. Below are the most commonly used helpers:

Query
""""""""

Used to get values from the URL query string (after ?).

**Example:**

/search?keyword=python
	- Comes from the URL
	- Commonly used for filtering, searching, and pagination
	- Supports validation like min_length, max_length

.. code-block:: python 

    from fastapi import FastAPI, Query
    from typing import Annotated

    app = FastAPI()

    @app.get("/search")
    def search(
        keyword: Annotated[str, Query(min_length=2, max_length=25)]
    ):
        return {"keyword": keyword}

Request:

.. code-block:: bash 

    /search?keyword=python

Path
""""""

Used to get values directly from the URL path.

**Example:**

/items/10
	- Identifies a specific resource
	- Usually required (no default value)
	- Supports numeric validation like gt, lt

.. code-block:: python 

    from fastapi import FastAPI, Path
    from typing import Annotated

    app = FastAPI()

    @app.get("/items/{item_id}")
    def get_item(
        item_id: Annotated[int, Path(gt=0)]
    ):
        return {"item_id": item_id}

Request: 

.. code-block:: bash 

    /items/10

Body
""""""

Used to receive data from the request body (usually JSON).

**Example:**

POST /items with JSON data
	- Used when sending structured data (e.g., objects)
	- Common in create/update operations
	- Supports validation rules for each field

.. code-block:: python 

    from fastapi import FastAPI, Body
    from typing import Annotated

    app = FastAPI()

    @app.post("/items")
    def create_item(
        name: Annotated[str, Body(min_length=3)],
        price: Annotated[float, Body(gt=0)]
    ):
        return {"name": name, "price": price}

Request body:

.. code-block:: json 

    {
        "name": "Laptop",
        "price": 999.99
    }


Header
""""""""

Used to read values from HTTP request headers.

**Example:**

User-Agent: Chrome
	- Contains metadata about the request
	- Often used for authentication, tracking, or client info

.. code-block:: python 

    from fastapi import FastAPI, Header
    from typing import Annotated

    app = FastAPI()

    @app.get("/user-agent")
    def get_user_agent(
        user_agent: Annotated[str, Header()]
    ):
        return {"user_agent": user_agent}

Header sent automatically by browser:

.. code-block:: bash 

    User-Agent: Mozilla/5.0


Cookie
""""""""

Used to read values stored in browser cookies.

**Example:**

session_id=abc123
	- Maintains session or user state
	- Automatically sent by the browser with requests

.. code-block:: python 

    from fastapi import FastAPI, Cookie
    from typing import Annotated

    app = FastAPI()

    @app.get("/profile")
    def get_profile(
        session_id: Annotated[str, Cookie()]
    ):
        return {"session_id": session_id}

Cookie:

.. code-block:: bash 

    session_id=abc123

Form
""""""

Used to receive data submitted via HTML forms.

**Example:**

application/x-www-form-urlencoded
	- Common in login forms (username/password)
	- Different from JSON body

.. code-block:: python 

    from fastapi import FastAPI, Form
    from typing import Annotated

    app = FastAPI()

    @app.post("/login")
    def login(
        username: Annotated[str, Form()],
        password: Annotated[str, Form()]
    ):
        return {"username": username}

Form data:

.. code-block:: bash 

    username=admin&password=1234

File
""""""

Used to upload files from the client.

**Example:**

Uploading an image or PDF
	- Works with UploadFile for better performance
	- Used in file upload APIs

.. code-block:: python 

    from fastapi import FastAPI, File, UploadFile
    from typing import Annotated

    app = FastAPI()

    @app.post("/upload")
    def upload_file(
        file: Annotated[UploadFile, File()]
    ):
        return {"filename": file.filename}

Upload a file (image, PDF, etc.)

Depends
""""""""

Used for dependency injection.
	- Allows reuse of logic across endpoints
	- Common use cases:
	- Database connections
	- Authentication checks
	- Shared business logic

.. code-block:: python 

    from fastapi import FastAPI, Depends
    from typing import Annotated

    app = FastAPI()

    def get_current_user():
        return "Sherullah"

    @app.get("/dashboard")
    def dashboard(
        user: Annotated[str, Depends(get_current_user)]
    ):
        return {"user": user}

Logic from get_current_user() is automatically used

Security
""""""""""

Used for authentication and authorization.
	- Built on top of Depends
	- Supports:
	- OAuth2
	- API keys
	- JWT tokens

.. code-block:: python 

    from fastapi import FastAPI, Security
    from fastapi.security import APIKeyHeader
    from typing import Annotated

    app = FastAPI()

    api_key_header = APIKeyHeader(name="X-API-Key")

    @app.get("/secure-data")
    def secure_data(
        api_key: Annotated[str, Security(api_key_header)]
    ):
        return {"api_key": api_key}

Request header:

.. code-block:: bash 

    X-API-Key: mysecretkey

6. Minimum and Maximum Length
---------------------------------

Length validation is one of the most practical validations for string query parameters.

.. code-block:: python

   @app.get("/cities/search")
   def search_cities(
       name: Annotated[str | None, Query(min_length=2, max_length=15)] = None
   ):
       return {"city_name": name}

What this means:

- at least 2 characters
- at most 15 characters

Valid examples:

::

   /cities/search?name=Rome
   /cities/search?name=Kabul

Invalid example:

::

   /cities/search?name=A

That fails because the value is too short.

7. Real-World Example: Product Search
-----------------------------------------

Let us make it feel more realistic.

.. code-block:: python

   @app.get("/store/search")
   def store_search(
       q: Annotated[str | None, Query(min_length=3, max_length=40)] = None
   ):
       return {
           "message": "Search request received",
           "query": q
       }

Why this is useful:

- prevents extremely short useless searches
- prevents extremely long search text
- keeps input cleaner for your backend logic

Example:

::

   /store/search?q=wireless mouse

8. Adding a Pattern Rule
----------------------------

Sometimes length is not enough. You may want the text to match a certain format.

For example, suppose a coupon search should contain only uppercase letters and numbers.

.. code-block:: python

   @app.get("/coupons/check")
   def check_coupon(
       code: Annotated[str, Query(pattern="^[A-Z0-9]+$")]
   ):
       return {"coupon_code": code}

Explanation:

- ``pattern=...`` applies a regular expression rule
- in this example, only uppercase letters and digits are allowed

Valid examples:

::

   /coupons/check?code=SAVE10
   /coupons/check?code=DISCOUNT2025

Invalid example:

::

   /coupons/check?code=save-10

That fails because it contains lowercase letters and a hyphen.

9. Default Values
---------------------

You can still use validation with default values.

.. code-block:: python

   @app.get("/blog/search")
   def search_blog(
       q: Annotated[str, Query(min_length=3, max_length=25)] = "python"
   ):
       return {"query": q}

Explanation:

- if the user sends nothing, the default search term is ``python``
- if the user sends a value, it must still follow the rules

Examples:

::

   /blog/search
   /blog/search?q=fastapi

10. Required Query Parameters
---------------------------------

If a query parameter must always be provided, you can mark it as required.

.. code-block:: python

   @app.get("/inventory/find")
   def find_inventory_item(
       sku: Annotated[str, Query(min_length=4, max_length=12)]
   ):
       return {"sku": sku}

Explanation:

- there is no default value
- so the client must send the query parameter

Example:

::

   /inventory/find?sku=A102

If the client does not provide ``sku``, FastAPI returns an error.

11. Required but Can Be None
--------------------------------

This is less common, but sometimes you may want a parameter to exist while still allowing a null-like value in some designs.

For beginner tutorials, it is usually better not to focus too much on this case at first. In most practical APIs, you will normally choose one of these:

- optional with a default
- required without a default

That keeps your API clearer.

12. Multiple Values in Query Parameters
-------------------------------------------

Sometimes a query parameter can be repeated.

For example, a user may want to filter by multiple tags.

.. code-block:: python

   @app.get("/posts/filter")
   def filter_posts(tags: list[str] | None = Query(default=None)):
       return {"tags": tags}

Example:

::

   /posts/filter?tags=python&tags=api&tags=fastapi

Response idea:

.. code-block:: json

   {
       "tags": ["python", "api", "fastapi"]
   }

This is useful for:

- category filters
- tags
- selected brands
- selected colors

13. Adding Metadata
-----------------------

You can also describe query parameters for your API documentation.

.. code-block:: python

   @app.get("/courses/search")
   def search_courses(
       q: Annotated[
           str | None,
           Query(
               min_length=3,
               max_length=30,
               title="Course Search Keyword",
               description="Enter a keyword to search course titles"
           )
       ] = None
   ):
       return {"query": q}

This improves the automatic docs shown in Swagger UI.

14. Alias Parameters
-----------------------

Sometimes the public API name should be different from the Python variable name.

.. code-block:: python

   @app.get("/products/filter")
   def filter_products(
       search_text: Annotated[str | None, Query(alias="q")] = None
   ):
       return {"search_text": search_text}

Example request:

::

   /products/filter?q=laptop

Explanation:

- users send ``q``
- Python uses ``search_text``

This can make your code more readable without changing the public API.

15. Common Mistakes
-----------------------

Using validation where plain parameters are enough
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Not every query parameter needs many rules. Keep it simple when possible.

Forgetting that query values arrive as text
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Even if a value looks like a number or a boolean in the URL, it starts as text and FastAPI converts it based on your type hints.

Making validation too strict
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the rules are too narrow, valid user inputs may be rejected unnecessarily.

Using unclear parameter names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is weak:

.. code-block:: python

   @app.get("/search")
   def search(x: str | None = None):
       return {"x": x}

This is better:

.. code-block:: python

   @app.get("/search")
   def search(keyword: str | None = None):
       return {"keyword": keyword}

16. Best Practices
----------------------

- use clear names like ``keyword``, ``category``, ``code``, or ``sku``
- add validation where bad input is likely
- use ``Annotated`` for cleaner modern code
- keep validation rules meaningful, not excessive
- use examples from real business logic when teaching

17. Summary
--------------

In this tutorial, you learned how to work with query parameters that need extra rules.

You learned:

- why string validation matters
- how to use ``Query(...)``
- how to apply minimum and maximum length
- how to use ``Annotated`` for cleaner declarations
- how to require certain formats with patterns
- how to use default values and required parameters
- how to accept multiple values
- how to improve docs with metadata

18. Try It Yourself
----------------------

Practice these one by one.

Exercise 1
~~~~~~~~~~~~

Create a route for searching books.

Requirements:

- endpoint: ``/books/search``
- query parameter: ``keyword``
- minimum length: 3
- maximum length: 20

Exercise 2
~~~~~~~~~~~~

Create a route for checking discount codes.

Requirements:

- endpoint: ``/discounts/check``
- query parameter: ``code``
- only uppercase letters and digits allowed

Exercise 3
~~~~~~~~~~~~

Create a route that accepts multiple categories.

Requirements:

- endpoint: ``/items/filter``
- query parameter: ``category``
- allow multiple values in the same URL