Web service v3.0
================

This API is currently under development.

Articles
--------

.. code-block:: python

    >>> import requests, json
    >>> r = requests.get("https://jarr.herokuapp.com/api/v3/article/1",
    ...                  headers={'Content-type': 'application/json'},
    ...                  auth=("your-nickname", "your-password"))
    >>> r.status_code
    200  # OK
    >>> rjson = r.json()
    >>> rjson["title"]
    "Why pathlib.Path doesn't inherit from str in Python"
    >>> rjson["date"]
    '2016-03-29T11:50:50'
    >>> rjson["link"]
    'http://www.snarky.ca/why-pathlib-path-doesn-t-inherit-from-str'
