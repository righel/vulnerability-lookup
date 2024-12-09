.. _Sightings:

Sightings
=========

Presentation
------------

Users have the possibility to add observations to vulnerabilities with different types of sightings, such as:
*seen*, *exploited*, *not exploited*, *confirmed*, *not confirmed*, *patched*, and *not patched*.

.. list-table::
   :header-rows: 1
   :widths: auto

   * - Type
     - Description
     - Negative/Opposite
   * - seen
     - The vulnerability was mentioned, discussed, or seen somewhere by the user.
     - -
   * - confirmed
     - The vulnerability is confirmed from an analyst perspective.
     - X
   * - exploited
     - This vulnerability was exploited and seen by the user reporting the sighting.
     - X
   * - patched
     - This vulnerability was successfully patched by the user reporting the sighting.
     - X


You can find the corresponding definition of the MISP taxonomy
`here <https://github.com/MISP/misp-taxonomies/blob/fd2fbaf2a450e42a490551e5a8e2fa6df039a6b8/vulnerability/machinetag.json#L26-L63>`_.


Color code
----------

Color code used in the application:

====================  ================
Sighting Type          Color Code
====================  ================
``exploited``          ``hsl(0, 70%, 35%)`` (Darker Red)
``confirmed``          ``hsl(0, 70%, 50%)`` (Medium Red)
``seen``               ``hsl(0, 70%, 65%)`` (Lighter Red)
``patched``            ``hsl(120, 50%, 50%)`` (Green)
``not-confirmed``      ``hsl(0, 0%, 35%)`` (Darker Grey)
``not-exploited``      ``hsl(0, 0%, 50%)`` (Medium Grey)
``not-patched``        ``hsl(0, 0%, 65%)`` (Lighter Grey)
====================  ================

Example
-------

Example of a sighting object:

.. code-block:: json

    {
        "uuid": "f6ed692b-2656-4ce0-bcf1-eaf12dfe281d",
        "vulnerability_lookup_origin": "1a89b78e-f703-45f3-bb86-59eb712668bd",
        "author": "8dfa6142-8c6d-4072-953e-71c85404aefb",
        "type": "seen",
        "source": "https://infosec.exchange/users/cve/statuses/113389560858828548",
        "vulnerability": "CVE-2024-10312",
        "creation_timestamp": "2024-10-29T08:36:31.492184Z"
    }


A source is not necessary an URL. It can be any string, for example the UUID of a MISP event.
Examples: https://vulnerability.circl.lu/sightings/?query=MISP

Automation
----------

Realistically, sightings are more likely to be created programmatically, for instance,
based on observations gathered from social networks, network captures, etc.

Tools
`````

To track vulnerabilities from data found on the Fediverse, you can use
`FediVuln <https://github.com/CIRCL/FediVuln>`_—a client specifically designed to gather
vulnerability-related information from the Fediverse and push the information to a
Vulnerability-Lookup instance as sightings.

`VulnerabilityLookupSighting <https://github.com/MISP/VulnerabilityLookupSighting>`_
is a client that retrieves vulnerability observations from a
`MISP <https://github.com/MISP/MISP>`_ server and pushes them to a Vulnerability-Lookup instance.

`NucleiVuln <https://github.com/cedricbonhomme/NucleiVuln>`_ is client designed to
monitor and retrieve vulnerability-related information from the
`Nuclei Git repository of templates <https://github.com/projectdiscovery/nuclei-templates>`_.
Templates form the core of the Nuclei scanner. When a template is linked to a vulnerability,
the resulting detection (observation) is classified as *confirmed*, signifying a higher
level of certainty compared to the *seen* classification.


Our tools on the Python Package Index (PyPI):

- https://pypi.org/project/FediVuln
- https://pypi.org/project/VulnerabilityLookupSighting
- https://pypi.org/project/NucleiVuln
- https://pypi.org/project/ExploitDBSighting
- https://pypi.org/project/kevsight

If you want to create your own sigthing tool, it's recommended to use
`PyVulnerabilityLookup <https://github.com/cve-search/PyVulnerabilityLookup>`_,
a Python library to access Vulnerability-Lookup via its REST API.


PyVulnerabilityLookup usage example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Initalize a PyVulnerabilityLookup object:

.. code-block:: Python

    from pyvulnerabilitylookup import PyVulnerabilityLookup
    vuln_lookup = PyVulnerabilityLookup("https://vulnerability.circl.lu/", token="<YOUR-API-TOKEN>")


Retrieve sightings for a specific vulnerability:

.. code-block:: Python

    sighting_cve_list = vuln_lookup.get_sightings(vuln_id='CVE-2024-9474')
    print(sighting_cve_list)

Output:

.. code-block:: json

    {
        "metadata": {
            "count": 15,
            "offset": 0,
            "limit": 1000
        },
        "data": [
            {
                "uuid": "b804f360-9d9f-4326-a1ae-e32fb82e268b",
                "creation_timestamp": "2024-11-18T22:19:16.087185+00:00",
                "type": "seen",
                "source": "https://feedsin.space/feed/CISAKevBot/items/2704494",
                "vulnerability": "CVE-2024-9474",
                "author": {
                    "login": "automation",
                    "name": "Automation user",
                    "uuid": "9f56dd64-161d-43a6-b9c3-555944290a09"
                }
            }
        ]
    }



Create a sew sighting:

.. code-block:: Python

    sighting = {"type": "exploited", "source": "<source-of-the-sighting>", "vulnerability": 'CVE-2024-9474'}
    created_sighting = vuln_lookup.create_sighting(sighting=sighting)
    print(created_sighting)

Output:

.. code-block:: json
    :caption: Example of Sighting

    {
        "metadata": {
            "count": 1,
            "offset": 0,
            "limit": 10
        },
        "data": [
            {
                "uuid": "b498cb64-9cbc-423a-aea0-bf58d740c024",
                "creation_timestamp": "2024-11-19T10:45:45.634635+01:00",
                "type": "exploited",
                "source": "<source-of-the-sighting>",
                "vulnerability": "CVE-2024-9474",
                "author": {
                    "login": "cedric",
                    "name": "Cédric",
                    "uuid": "8dfa6142-8c6d-4072-953e-71c85404aefb"
                }
            }
        ]
    }


PyVulnerabilityLookup supports various object types within the VulnerabilityLookup framework.
Refer to the `tests <https://github.com/cve-search/vulnerability-lookup/blob/main/tests/test_web.py>`_ for detailed examples and usage.
