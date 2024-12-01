API v1
======

.. _OpenAPI:


`PyVulnerabilityLookup <https://github.com/cve-search/PyVulnerabilityLookup>`_
is a Python library to access Vulnerability-Lookup via its REST API.


OpenAPI specification
---------------------


.. openapi:: _static/files/swagger.json


Examples
--------

Comments
~~~~~~~~

Getting the list of comments:

.. code-block:: bash

    $ curl -X 'GET' 'http://127.0.0.1:5000/api/comment/' -H 'accept: application/json'


Getting the list of comments made by a specific author:

.. code-block:: bash

    $ curl -X 'GET' 'http://127.0.0.1:5000/api/comment/?author=john' -H 'accept: application/json'


Getting the list of comments related to a vulnerability:

.. code-block:: bash

    $ curl -X 'GET' 'http://127.0.0.1:5000/api/comment/?vuln_id=cve-2024-38063' -H 'accept: application/json'


Getting the list of comments that are related to a Proof of Concept:

.. code-block:: bash

    $ curl -X 'GET' 'http://127.0.0.1:5000/api/comment/?meta=[{"tags":["PoC"]}]' -H 'accept: application/json'
