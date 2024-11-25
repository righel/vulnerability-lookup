Command Line Interface
======================

Section to explain the various commands available.

Core
----

Start all the services
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ start


Stop all the services
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ stop


Start only the website
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ start_website


Restart only the website
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ restart_website


Dump a source in a JSON file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ dump --feed nvd

Update the documentation
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ cd docs; make hmtl



Web service
-----------

This section describes the main commands related to the web service.


Init the database
~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app db_init --help
    Usage: flask db_init [OPTIONS]

    Will create the database from conf parameters.

    Options:
    --help  Show this message and exit.


Create a user
~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app create_user --help
    Usage: flask create_user [OPTIONS]

      Initializes a user

    Options:
    --login TEXT     Login
    --email TEXT     Email
    --password TEXT  Password
    --help           Show this message and exit.


Create an admin
~~~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app create_admin


List all users
~~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app user_list


Delete a user
~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app user_delete --login <login>


Update MISP warning lists
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app update_warninglists

During the update of Vulnerability Lookup, the administrator will be prompted to execute this command.


Backup the PostgreSQL database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    $ flask --app website.app db_backup

This command is executed automatically during the update ot Vulnerability Lookup.
