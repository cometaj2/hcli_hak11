|pypi| |build status| |pyver|

HCLI hak11
==========

HCLI hak11 is a python package wrapper that contains an HCLI sample application (hak11); hak11 is intended to run on a rapsberry pi setup to trigger relays via GPIO. 12 GPIO triggered relays are intended to be setup to interact with a Linear AK-11 keypad to enable remote administration of the AK-11 keypad.

----

HCLI hak11 wraps hak11 (an HCLI) and is intended to be used with an HCLI Client [1] as presented via an HCLI Connector [2].

You can find out more about HCLI on hcli.io [3]

[1] https://github.com/cometaj2/huckle

[2] https://github.com/cometaj2/hcli_core

[3] http://hcli.io

AK-11 Keypad Circuit and Relay Setup
====================================

Keypad Circuit
--------------

The AK-11 keypad uses a matrix circuit design. In this setup:

Each button connects a unique combination of row and column pins when pressed.
The circuit remains open until a button is pressed, closing the circuit for that specific pin combination.

Relay Function
--------------

The relays act as electronic switches, controlled by the Raspberry Pi's GPIO pins.
When activated, a relay closes the circuit between two specific pins on the keypad, simulating a button press.
The relays do not introduce any new voltage to the system. They only allow the existing voltage in the keypad circuit to flow when "closed".
Importantly, the relays function as bidirectional switches. The "input" and "output" designations are interchangeable, as the relay simply creates a connection between two points.

Multiplexing Strategy
---------------------

The relay board's multiplexing strategy mirrors the keypad's internal multiplexing logic:

Keypad Internal Multiplexing:

The keypad uses a 7-pin interface to control 12 keys (0-9, *, #).
It achieves this by correlating multiple digits to each pin, creating unique combinations for each key.

Relay Board Multiplexing:

The relay setup replicates this logic externally.
It uses 7 connections to the keypad to control all 12 keys.
The relay board creates the necessary pin combinations for each digit, just as the keypad's internal circuitry would.

Pin correlation (both at the keypad and at the relays):

Pin 1, 3 connections: *0#
Pin 2, 4 connections: 147*
Pin 3, 3 connections: 123
Pin 4, 4 connections: 2580
Pin 5, 4 connections: 369#
Pin 6, 3 connections: 456
Pin 7, 3 connections: 789

Relay Connection Directionality
-------------------------------

The relays in this setup function as simple switches, allowing current to flow in either direction when closed.
The terms "input" and "output" on the relay are essentially arbitrary in this context.
This bidirectional nature simplifies wiring and troubleshooting, as the orientation of the relay connections to the keypad pins doesn't affect functionality.

This bidirectional property of the relays adds flexibility to the setup and confirms that the system is working with the keypad's existing circuitry rather than imposing new electrical conditions.

Installation
------------

HCLI hak11 requires a supported version of Python and pip.

You'll need an HCLI Connector to run hak11. For example, you can use HCLI Core (https://github.com/cometaj2/hcli_core), a WSGI server such as Green Unicorn (https://gunicorn.org/), and an HCLI Client like Huckle (https://github.com/cometaj2/huckle).


.. code-block:: console

    pip install hcli-hak11
    pip install hcli-core
    pip install huckle
    pip install gunicorn
    gunicorn --workers=1 --threads=1 -b 127.0.0.1:8000 "hcli_core:connector(\"`hcli_hak11 path`\")"

Usage
-----

Open a different shell window.

Setup the huckle env eval in your .bash_profile (or other bash configuration) to avoid having to execute eval everytime you want to invoke HCLIs by name (e.g. hc).

Note that no CLI is actually installed by Huckle. Huckle reads the HCLI semantics exposed by the API via HCLI Connector and ends up behaving *like* the CLI it targets.


.. code-block:: console

    huckle cli install http://127.0.0.1:8000
    eval $(huckle env)
    hak11 help

Versioning
----------
    
This project makes use of semantic versioning (http://semver.org) and may make use of the "devx",
"prealphax", "alphax" "betax", and "rcx" extensions where x is a number (e.g. 0.3.0-prealpha1)
on github.

Supports
--------

- Interacting with raspberry pi GPIO setup to trigger a properly configured relay connected to a Linear AK-11 keypad.

To Do
-----

- TBD

Bugs
----

- TBD

.. |build status| image:: https://circleci.com/gh/cometaj2/hcli_hak11.svg?style=shield
   :target: https://circleci.com/gh/cometaj2/hcli_hak11
.. |pypi| image:: https://img.shields.io/pypi/v/hcli-hc?label=hcli-hc
   :target: https://pypi.org/project/hcli-hc
.. |pyver| image:: https://img.shields.io/pypi/pyversions/hcli-hc.svg
   :target: https://pypi.org/project/hcli-hc
