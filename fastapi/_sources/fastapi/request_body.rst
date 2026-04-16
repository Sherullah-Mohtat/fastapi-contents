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

CRUD Example with Request Body
----------------------------------

To understand request bodies better, it helps to see them inside a simple CRUD example.

CRUD means:

- Create
- Read
- Update
- Delete

In real APIs, the request body is mainly used in:

- Create
- Update

It is usually **not needed** for simple Read and Delete operations, because those often use path parameters instead.

Let’s build a small example for managing books.

1. Create the Pydantic Model
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, define the structure of the data that the API should receive.

.. code-block:: python

   from fastapi import FastAPI
   from pydantic import BaseModel

   app = FastAPI()

   class Book(BaseModel):
       title: str
       author: str
       price: float
       in_stock: bool

This model means every book should have:

- a title
- an author
- a price
- stock status

2. Create a Simple Fake Database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For learning purposes, we can store data in a Python dictionary.

.. code-block:: python

   fake_books_db = {}

This is not a real database, but it is useful for understanding the logic.

3. Create Operation
~~~~~~~~~~~~~~~~~~~~~~

The create operation uses a request body because the client must send the new book data to the server.

.. code-block:: python

   @app.post("/books/{book_id}")
   def create_book(book_id: int, book: Book):
       fake_books_db[book_id] = book
       return {
           "message": "Book created successfully",
           "book_id": book_id,
           "data": book
       }

Example request:

::

   POST /books/1

Request body:

.. code-block:: json

   {
       "title": "Python Basics",
       "author": "Ahmad",
       "price": 29.99,
       "in_stock": true
   }

Explanation:

- ``book_id`` comes from the path
- ``book`` comes from the request body
- FastAPI validates the JSON using the ``Book`` model

4. Read Operation
~~~~~~~~~~~~~~~~~~~

The read operation usually does not need a request body. It often uses a path parameter to identify which resource should be returned.

.. code-block:: python

   @app.get("/books/{book_id}")
   def read_book(book_id: int):
       return {
           "book_id": book_id,
           "data": fake_books_db.get(book_id, "Book not found")
       }

Example request:

::

   GET /books/1

Explanation:

- The client only wants to fetch existing data
- No body is needed here
- The server reads the ID from the URL

5. Update Operation
~~~~~~~~~~~~~~~~~~~~~

The update operation uses a request body because the client sends new data to replace or update the old data.

.. code-block:: python

   @app.put("/books/{book_id}")
   def update_book(book_id: int, book: Book):
       fake_books_db[book_id] = book
       return {
           "message": "Book updated successfully",
           "book_id": book_id,
           "updated_data": book
       }

Example request:

::

   PUT /books/1

Request body:

.. code-block:: json

   {
       "title": "Advanced Python",
       "author": "Ahmad",
       "price": 39.99,
       "in_stock": false
   }

Explanation:

- ``book_id`` tells the API which book to update
- ``book`` contains the new data
- FastAPI validates and converts the incoming JSON

6. Delete Operation
~~~~~~~~~~~~~~~~~~~~~~~

The delete operation usually does not need a request body. It only needs the identifier of the resource to delete.

.. code-block:: python

   @app.delete("/books/{book_id}")
   def delete_book(book_id: int):
       removed_book = fake_books_db.pop(book_id, None)
       return {
           "message": "Book deleted successfully",
           "deleted_data": removed_book
       }

Example request:

::

   DELETE /books/1

Explanation:

- The API receives the book ID from the path
- It removes that book from the fake database
- No request body is required

7. Full CRUD Example
~~~~~~~~~~~~~~~~~~~~~~~

Here is the full code together so you can understand the complete flow.

.. code-block:: python

   from fastapi import FastAPI
   from pydantic import BaseModel

   app = FastAPI()

   class Book(BaseModel):
       title: str
       author: str
       price: float
       in_stock: bool

   fake_books_db = {}

   @app.post("/books/{book_id}")
   def create_book(book_id: int, book: Book):
       fake_books_db[book_id] = book
       return {
           "message": "Book created successfully",
           "book_id": book_id,
           "data": book
       }

   @app.get("/books/{book_id}")
   def read_book(book_id: int):
       return {
           "book_id": book_id,
           "data": fake_books_db.get(book_id, "Book not found")
       }

   @app.put("/books/{book_id}")
   def update_book(book_id: int, book: Book):
       fake_books_db[book_id] = book
       return {
           "message": "Book updated successfully",
           "book_id": book_id,
           "updated_data": book
       }

   @app.delete("/books/{book_id}")
   def delete_book(book_id: int):
       removed_book = fake_books_db.pop(book_id, None)
       return {
           "message": "Book deleted successfully",
           "deleted_data": removed_book
       }

8. Why This Example is Important
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This CRUD example teaches an important idea:

- request body is used when the client sends structured data
- path parameters are used when the client identifies a resource

So in practice:

- ``POST`` often uses request body
- ``PUT`` often uses request body
- ``GET`` often uses path/query parameters
- ``DELETE`` often uses path parameters

9. Real-World Understanding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Imagine a real store system:

- Create a product → send product data in request body
- View a product → send product ID in path
- Update a product → send product ID in path and new data in request body
- Delete a product → send product ID in path

This is the normal pattern in many APIs.

10. Summary
~~~~~~~~~~~~~~

In CRUD operations:

- Create uses request body
- Read usually does not use request body
- Update uses request body
- Delete usually does not use request body

This is why request body is such a major part of API development.

============================================================================================================================================================================

PATCH Operation (Partial Update)
------------------------------------

The ``PATCH`` method is used when you want to update only part of a resource, not the whole resource.

This is different from ``PUT``.

- ``PUT`` usually sends the full updated object
- ``PATCH`` usually sends only the fields that should change

This is very useful in real applications because users often update just one or two fields.

Example:

- changing only the price of a product
- updating only the stock status
- editing only the title of a book

1. Why PATCH is Useful
~~~~~~~~~~~~~~~~~~~~~~~~~~

Imagine you already have a book with this data:

.. code-block:: json

   {
       "title": "Python Basics",
       "author": "Ahmad",
       "price": 29.99,
       "in_stock": true
   }

Now suppose you only want to change the price.

With ``PUT``, you would often send the whole object again.

With ``PATCH``, you can send only:

.. code-block:: json

   {
       "price": 35.50
   }

That makes partial updates simpler and more efficient.

2. Create a Model for Partial Update
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Because ``PATCH`` may update only some fields, the fields should usually be optional.

.. code-block:: python

   from pydantic import BaseModel
   from typing import Optional

   class BookUpdate(BaseModel):
       title: Optional[str] = None
       author: Optional[str] = None
       price: Optional[float] = None
       in_stock: Optional[bool] = None

Explanation:

- every field is optional
- the client can send only the fields they want to change

3. PATCH Example
~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.patch("/books/{book_id}")
   def patch_book(book_id: int, book_update: BookUpdate):
       existing_book = fake_books_db.get(book_id)

       if existing_book is None:
           return {"message": "Book not found"}

       updated_data = book_update.model_dump(exclude_unset=True)

       if hasattr(existing_book, "model_dump"):
           existing_book_data = existing_book.model_dump()
       else:
           existing_book_data = existing_book.copy()

       existing_book_data.update(updated_data)
       fake_books_db[book_id] = existing_book_data

       return {
           "message": "Book updated partially",
           "book_id": book_id,
           "updated_data": fake_books_db[book_id]
       }

4. How This Works
~~~~~~~~~~~~~~~~~~~~

Let’s understand the important line:

.. code-block:: python

   updated_data = book_update.model_dump(exclude_unset=True)

This means:

- convert the Pydantic object to a dictionary
- include only the fields actually sent by the client

So if the client sends:

.. code-block:: json

   {
       "price": 35.50
   }

then ``updated_data`` becomes:

.. code-block:: python

   {"price": 35.50}

Only that field is updated.

5. Example Request
~~~~~~~~~~~~~~~~~~~~~

Request:

::

   PATCH /books/1

Request body:

.. code-block:: json

   {
       "price": 35.50,
       "in_stock": false
   }

This updates only:

- ``price``
- ``in_stock``

Other fields remain unchanged.

6. Real-World Understanding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``PATCH`` is useful when a user edits only one part of existing data.

Examples:

- changing only a customer phone number
- updating only an order status
- editing only a product price
- changing only a profile photo URL

In these cases, partial update is better than sending the entire object again.

7. PUT vs PATCH
~~~~~~~~~~~~~~~~~~

Here is the practical difference:

- ``PUT`` → full update
- ``PATCH`` → partial update

Example with a product:

Original data:

.. code-block:: json

   {
       "name": "Laptop",
       "price": 1200,
       "in_stock": true
   }

Using ``PUT`` often means sending:

.. code-block:: json

   {
       "name": "Laptop",
       "price": 1100,
       "in_stock": true
   }

Using ``PATCH`` can mean sending only:

.. code-block:: json

   {
       "price": 1100
   }

8. Best Practice
~~~~~~~~~~~~~~~~~~~

A common and clean pattern is:

- one model for create
- one model for full update
- one model for partial update

Example:

- ``BookCreate``
- ``BookUpdate``
- ``BookPartialUpdate``

This keeps your API clearer and easier to maintain.

9. Summary
~~~~~~~~~~~~~

``PATCH`` is used for partial updates.

It is useful when:

- only some fields should change
- the client should not resend the whole object
- you want more flexible update behavior

In most real APIs:

- ``POST`` creates
- ``GET`` reads
- ``PUT`` fully updates
- ``PATCH`` partially updates
- ``DELETE`` removes