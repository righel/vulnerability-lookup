Streaming service
=================

A Streaming service is implemented using a Publish/Subscribe (Pub/Sub) pattern.

Available channels:

- vulnerability
- comment
- bundle
- sighting

Database-level listeners automatically publish new **sightings**, **comments**, and **bundles** to their respective Pub/Sub channels.
The various feeders push incoming vulnerabilities directly to the designated vulnerability channel.

Configuration
-------------

The streaming service can be configured using the ``config/stream.json`` file. An example configuration is provided below.

.. code-block:: bash

    {
        "register_listeners": true,
        "pubsub_bp": true,
        "global_subscription": false,
        "channels": [
            "vulnerability",
            "comment",
            "bundle",
            "sighting"
        ],
        "_notes": {
            "register_listeners": "Register the listeners for comments, bundles, and sightings.",
            "pubsub_bp": "Activate or deactivate the subscription Blueprint for specific clients.",
            "channels": "The list of channels exposed by the Pub/Sub Blueprint.",
            "global_subscription": "Activate or deactivate the global subsciption mechanism for server-side processing of messages."
        }
    }

Listeners
~~~~~~~~~

When database-level listeners are enabled, all new comments, bundles, and sightings are automatically streamed to the Pub/Sub service.


Internal streaming
~~~~~~~~~~~~~~~~~~

When ``global_subscription`` is set to ``True``, an internal subsciption mechanism is provided
for the components of the application. The internal streaming service is executed in a dedicated thread.


HTTP streaming
~~~~~~~~~~~~~~

Connection to the authenticated HTTP subscribing interface when (``pubsub_bp`` is set to ``True``):

.. code-block:: bash

    $ curl -H "X-API-KEY: <YOUR-TOKEN>" http://127.0.0.1:5000/pubsub/subscribe/comment

`FediVuln <https://pypi.org/project/FediVuln>`_ can also be used to subscribe to a channel:

.. code-block:: bash

    $ FediVuln-Publish -t comment

A status will be pushed with the configured Mastodon account (see the documentation of FediVuln).
