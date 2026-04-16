Path Parameters in FastAPI
============================

.. meta::
   :description: Learn path parameters in FastAPI with practical examples. Understand types, automatic data conversion, validation, enums, route order, predefined values, and path parameters that contain full paths.
   :keywords: FastAPI path parameters, FastAPI path parameter types, FastAPI enum path parameter, FastAPI validation, FastAPI predefined values, FastAPI path converter, FastAPI route order, FastAPI tutorial
   :author: Sherullah Mohtat
   :robots: index, follow

Path parameters are one of the most useful features in FastAPI because they allow your application to read values directly from the URL.

They are called "path parameters" because the value lives inside the path itself.

For example, in a URL like this:

::

   /products/25

the number ``25`` is part of the path. That value can be captured and used inside your Python function.

This makes your API dynamic. Instead of creating a separate URL for every product, customer, or order, you create one route and let FastAPI read the changing part for you.

1. What a Path Parameter Is
---------------------------------

A path parameter is a variable inside the route path.

Here is a simple example:

.. code-block:: python

   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/products/{product_id}")
   def get_product(product_id: int):
       return {
           "message": "Product details fetched successfully",
           "product_id": product_id
       }

In this route:

- ``/products/`` is the fixed part
- ``{product_id}`` is the dynamic part

If someone visits:

::

   /products/25

FastAPI takes the value ``25`` from the URL and sends it into the function as ``product_id``.

2. Declaring a Path Parameter
-----------------------------------

To declare a path parameter in FastAPI, you do two things:

- place a variable name in curly braces inside the route
- use the same variable name in the function parameters

Example:

.. code-block:: python

   @app.get("/customers/{customer_id}")
   def get_customer(customer_id: int):
       return {"customer_id": customer_id}

Notice that the name in the path and the name in the function must match.

Correct:

.. code-block:: python

   @app.get("/customers/{customer_id}")
   def get_customer(customer_id: int):
       return {"customer_id": customer_id}

Incorrect:

.. code-block:: python

   @app.get("/customers/{customer_id}")
   def get_customer(id: int):
       return {"customer_id": id}

The second version is confusing because the path says ``customer_id`` but the function expects ``id``. It is best to keep them the same.

3. Path Parameters with Types
-----------------------------------

FastAPI becomes very powerful when you add Python type hints.

Example:

.. code-block:: python

   @app.get("/orders/{order_id}")
   def get_order(order_id: int):
       return {
           "order_id": order_id,
           "status": "ready for processing"
       }

Here, ``order_id: int`` tells FastAPI that the value must be an integer.

You can also use other types such as:

- ``str``
- ``int``
- ``float``
- ``bool``

Example with a string:

.. code-block:: python

   @app.get("/employees/{employee_name}")
   def get_employee(employee_name: str):
       return {
           "employee_name": employee_name
       }

If the URL is:

::

   /employees/ahmad

then ``employee_name`` becomes ``"ahmad"``.

4. Data Conversion
-----------------------

One useful thing FastAPI does automatically is data conversion.

Suppose the value coming from the browser is text, because URL values are always received as strings at first.

But if your function says the value should be an integer, FastAPI will try to convert it for you.

Example:

.. code-block:: python

   @app.get("/tickets/{ticket_number}")
   def get_ticket(ticket_number: int):
       return {
           "ticket_number": ticket_number,
           "python_type": str(type(ticket_number))
       }

If the request is:

::

   /tickets/1007

FastAPI will convert ``"1007"`` into the integer ``1007``.

So inside Python, ``ticket_number`` is no longer a string. It becomes a real integer.

That means you can safely do numeric operations.

.. code-block:: python

   @app.get("/boxes/{box_count}")
   def get_boxes(box_count: int):
       return {
           "received": box_count,
           "after_adding_one": box_count + 1
       }

If the request is:

::

   /boxes/9

the response will show:

.. code-block:: json

   {
       "received": 9,
       "after_adding_one": 10
   }

5. Data Validation
-----------------------

FastAPI does not only convert data. It also validates it.

Validation means checking whether the incoming value matches the rules you declared.

Example:

.. code-block:: python

   @app.get("/invoices/{invoice_id}")
   def get_invoice(invoice_id: int):
       return {"invoice_id": invoice_id}

Valid request:

::

   /invoices/300

This works because ``300`` can be converted into an integer.

Invalid request:

::

   /invoices/abc

This fails because ``abc`` cannot become an integer.

FastAPI will stop the request and return a validation error response automatically.

This is very useful because you do not need to manually write code like:

.. code-block:: python

   if not invoice_id.isdigit():
       return {"error": "invalid id"}

FastAPI handles that part for you based on the type you declared.

6. How Pydantic Fits In
------------------------------

FastAPI relies heavily on data validation, and Pydantic is the tool that helps with that.

In simple words:

- FastAPI handles the web part
- Pydantic helps check and organize data

When you use simple path parameters like ``int`` or ``str``, FastAPI still applies validation rules in a structured way.

Pydantic is especially important when you work with request bodies, but the same idea of validation is present here too: FastAPI reads your type hints and makes sure the incoming data matches them.

Think of it like this:

- You declare what type of data you want
- FastAPI checks the input
- If the input is wrong, the request is rejected

That is one reason FastAPI feels clean and modern.

Pydantic Example
~~~~~~~~~~~~~~~~~~~

Pydantic helps FastAPI validate and structure data using Python classes.

Example:

.. code-block:: python

   from pydantic import BaseModel

   class Product(BaseModel):
       name: str
       price: float
       in_stock: bool

.. code-block:: python

   @app.post("/products")
   def create_product(product: Product):
       return product

This ensures that incoming data:
- has the correct structure
- follows the correct data types
- is automatically validated and converted

7. Multiple Path Parameters
----------------------------------

You can have more than one path parameter in a single route.

Example:

.. code-block:: python

   @app.get("/schools/{school_id}/students/{student_id}")
   def get_student_record(school_id: int, student_id: int):
       return {
           "school_id": school_id,
           "student_id": student_id
       }

Request:

::

   /schools/15/students/204

FastAPI maps:

- ``school_id`` → 15
- ``student_id`` → 204

This is very useful when your data has hierarchy.

Examples:

- a restaurant and one item from its menu
- a company and one employee
- a course and one lesson

8. Real-World Example
--------------------------

Let’s build a more realistic route.

.. code-block:: python

   @app.get("/shops/{shop_id}/products/{product_id}")
   def get_shop_product(shop_id: int, product_id: int):
       return {
           "shop_id": shop_id,
           "product_id": product_id,
           "message": "Product belongs to the selected shop"
       }

Example request:

::

   /shops/8/products/52

This is more meaningful than generic examples because it reflects how real systems are designed.

9. Order Matters in Routes
-------------------------------

When two routes look similar, the order of declaration becomes important.

Consider this example:

.. code-block:: python

   @app.get("/reports/daily")
   def get_daily_report():
       return {"report": "daily summary"}

   @app.get("/reports/{report_id}")
   def get_report_by_id(report_id: int):
       return {"report_id": report_id}

This works well because the fixed route comes first.

Why?

Because ``/reports/daily`` is a specific path, while ``/reports/{report_id}`` is a dynamic path.

If you place the dynamic route first, then FastAPI may try to interpret:

::

   /reports/daily

as if ``daily`` were the value of ``report_id``.

But ``report_id`` expects an integer, so that leads to a validation problem.

Best practice:

- place fixed routes first
- place dynamic routes after them

This keeps your routing clear and avoids ambiguity.

10. Predefined Values
---------------------------

Sometimes you do not want users to enter any value they like.

You may want to allow only a small set of valid values, such as:

- ``new``
- ``popular``
- ``discounted``

This is where predefined values become useful.

Instead of accepting any string, you can restrict the parameter to a known set of choices.

11. Create an Enum Class
-----------------------------

In Python, one clean way to define predefined choices is with an enumeration.

Example:

.. code-block:: python

   from enum import Enum

   class ProductView(str, Enum):
       new = "new"
       popular = "popular"
       discounted = "discounted"

This class defines exactly three allowed values.

Notice that it inherits from both ``str`` and ``Enum``.

That is useful because:

- ``Enum`` gives you controlled choices
- ``str`` makes the values behave naturally as strings in APIs

12. Working with Python Enumerations
-----------------------------------------

Now let’s use the enum in a FastAPI path parameter.

.. code-block:: python

   from fastapi import FastAPI
   from enum import Enum

   app = FastAPI()

   class ProductView(str, Enum):
       new = "new"
       popular = "popular"
       discounted = "discounted"

   @app.get("/products/view/{view_name}")
   def get_products_by_view(view_name: ProductView):
       return {
           "selected_view": view_name,
           "message": "Products filtered by predefined view"
       }

Valid requests:

::

   /products/view/new
   /products/view/popular
   /products/view/discounted

Invalid request:

::

   /products/view/featured

That last request fails because ``featured`` is not one of the allowed enum values.

This is a very clean way to protect your API and document expected values.

13. Why Enum Is Useful
-----------------------------

Enums are useful because they:

- prevent invalid values
- make your code easier to understand
- improve automatic API documentation
- reduce manual checking inside functions

Without enum, you might write:

.. code-block:: python

   @app.get("/products/view/{view_name}")
   def get_products_by_view(view_name: str):
       allowed = ["new", "popular", "discounted"]

       if view_name not in allowed:
           return {"error": "invalid view"}

       return {"selected_view": view_name}

This works, but enum is cleaner and more professional.

14. Path Parameters Containing Paths
--------------------------------------------

Sometimes you want a path parameter to capture not just one segment, but an entire file path or folder path.

Example use cases:

- reading a file location
- serving media from nested folders
- working with document directories

Suppose you want to capture something like:

::

   /files/images/products/shoes/red.jpg

If you write a normal path parameter, FastAPI will stop at each slash.

To capture the whole remaining path, use the ``path`` type.

Example:

.. code-block:: python

   @app.get("/files/{file_path:path}")
   def get_file_path(file_path: str):
       return {
           "file_path": file_path
       }

Now if the request is:

::

   /files/images/products/shoes/red.jpg

the value of ``file_path`` becomes:

::

   images/products/shoes/red.jpg

This is helpful when your path itself contains multiple folder levels.

15. Normal Path Parameter vs Path-Capturing Parameter
----------------------------------------------------------

Compare these two:

Normal path parameter:

.. code-block:: python

   @app.get("/documents/{doc_name}")
   def get_document(doc_name: str):
       return {"doc_name": doc_name}

This can capture:

::

   /documents/report.pdf

But it cannot capture:

::

   /documents/work/2026/april/report.pdf

Now look at the path-capturing version:

.. code-block:: python

   @app.get("/documents/{doc_path:path}")
   def get_document_path(doc_path: str):
       return {"doc_path": doc_path}

This can capture the full nested path:

::

   work/2026/april/report.pdf

16. When to Use Path Parameters
---------------------------------------

Use path parameters when the value is a required part of identifying the resource.

Good examples:

- ``/users/15``
- ``/orders/9001``
- ``/stores/4/products/77``

Use them when the resource cannot be identified without that value.

17. Path Parameters vs Query Parameters
----------------------------------------------

It is important not to confuse path parameters with query parameters.

Path parameter example:

::

   /movies/12

Query parameter example:

::

   /movies?year=2025

Difference:

- path parameters usually identify a specific resource
- query parameters usually filter, sort, search, or modify the response

Example idea:

- ``/products/10`` → get one product
- ``/products?category=shoes`` → get products in one category

18. Common Mistakes
--------------------------

Using the wrong type
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.get("/payments/{payment_id}")
   def get_payment(payment_id: str):
       return {"payment_id": payment_id}

If ``payment_id`` should always be numeric, using ``str`` may allow values you do not really want.

Route order confusion
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If fixed and dynamic paths are mixed carelessly, users may hit the wrong route or get validation errors.

Using unclear names
~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is less helpful:

.. code-block:: python

   @app.get("/cars/{x}")
   def get_car(x: int):
       return {"x": x}

This is much better:

.. code-block:: python

   @app.get("/cars/{car_id}")
   def get_car(car_id: int):
       return {"car_id": car_id}

Ignoring enums when choices are limited
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If only three or four values are allowed, enum is cleaner than manual checking.

19. Best Practices
-------------------------

- Use descriptive names like ``product_id``, ``customer_id``, and ``invoice_id``
- Add type hints for every path parameter
- Put fixed routes before dynamic routes
- Use enum when only certain values should be accepted
- Use path-capturing parameters only when you really need full nested paths
- Keep URLs meaningful and readable

20. Summary
------------------

In this tutorial, you learned that path parameters let FastAPI capture values directly from the URL.

You also learned:

- how to declare a path parameter
- how to assign types
- how FastAPI performs automatic data conversion
- how validation works
- how Pydantic relates to validation
- why route order matters
- how to restrict values with an enum
- how to work with Python enumerations
- how to capture a full nested path using ``:path``

Path parameters are simple to start with, but they are also a major part of building real APIs. Once you understand them well, many other FastAPI topics become easier.

21. Try It Yourself
----------------------------

Practice these one by one.

Example 1: simple numeric ID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.get("/books/{book_id}")
   def get_book(book_id: int):
       return {"book_id": book_id}

Example 2: two path parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.get("/drivers/{driver_id}/rides/{ride_id}")
   def get_driver_ride(driver_id: int, ride_id: int):
       return {
           "driver_id": driver_id,
           "ride_id": ride_id
       }

Example 3: enum path parameter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   from enum import Enum

   class DeliveryStatus(str, Enum):
       pending = "pending"
       shipped = "shipped"
       delivered = "delivered"

   @app.get("/deliveries/status/{status}")
   def get_deliveries_by_status(status: DeliveryStatus):
       return {"status": status}

Example 4: parameter that captures full path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

   @app.get("/media/{media_path:path}")
   def get_media_path(media_path: str):
       return {"media_path": media_path}


