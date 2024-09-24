|pypi| |build status| |pyver|

HCLI hak11
==========

HCLI hak11 is a python package wrapper that contains an HCLI sample application (hak11); hak11 is intended to run on a rapsberry pi setup to trigger relays via GPIO. 12 GPIO triggered relays are intended to be setup to interact with a Linear AK-11 keypad to enable remote administration.

----

HCLI hak11 wraps hak11 (an HCLI) and is intended to be used with an HCLI Client [1] as presented via an HCLI Connector [2].

You can find out more about HCLI on hcli.io [3]

[1] https://github.com/cometaj2/huckle

[2] https://github.com/cometaj2/hcli_core

[3] http://hcli.io

Materials Used
==============

* Raspberry pi zero with 40pin header & wifi
* SainSmart 16-Channel 12V Relay Module Board for Arduino DSP AVR PIC ARM
* ELEGOO 120pcs Multicolored Dupont Wire 40pin Male to Female, 40pin Male to Male, 40pin Female to Female Breadboard Jumper Ribbon Cables Kit Compatible with Arduino Projects
* Dupont 2.54mm Wire Female to Female Dupont Connector & 22AWG Pre-Crimped Cable, Single Row
* 18 AWG Wire Electrical Wires 18 Gauge Tinned Copper Wire, PVC (OD: 1.85 mm) 6 Different Colored 16.4ft / 5m Each,Stranded Wire Hookup Wires for DIY DC/AC
* 2.54mm Male Pin Header Single Row DIP Connector Kit，1 * 2/3/4/5/6/7/8/9/10/12Pin Straight Header Pin Breakaway PCB Board Pin Header Strip for Prototype Shield (Male-Pin-Single)
* GKEEMARS 50 Pcs Wire Connector, 1 Conductor Compact Wire Connectors Splicing Connectors for Circuit Inline 24-12 AWG (Orange)

AK-11 Keypad Circuit and Relay Setup
====================================

AK-11 Circuit
-------------

The AK-11 keypad uses a matrix circuit design.

Each button connects a unique combination of row and column pins when pressed.
The circuit remains open until a button is pressed, closing the circuit for that specific pin combination.

Digit Pins
* 1     2,3
* 2     3,4
* 3     3,5
* 4     2,6
* 5     4,6
* 6     5,6
* 7     2,7
* 8     4,7
* 9     5,7
* \*     1,2
* 0     1,4
* #     1,5

Relay Function
--------------

When activated, a relay closes the circuit between two specific pins of the keypad's 7 pins. This is accomplished by setting up a simple demultiplexer that links each relay circuit close to 2 unique output wires corresponding to the correct 2 pins of the digipad circuitboard.

Demultiplexing Strategy
-----------------------

The relay board correlates the necessary pin combinations for each digit, just as the keypad's internal circuitry does, to emulate digit presses as triggered via GPIO from the raspberry pi.

Pin correlation (both at the keypad and at the relays):

* Pin 1, 3 connections: "\*0#", Red
* Pin 2, 4 connections: "147\*", Black
* Pin 3, 3 connections: "123", Yellow
* Pin 4, 4 connections: "2580", White
* Pin 5, 4 connections: "369#", Blue
* Pin 6, 3 connections: "456", Teal
* Pin 7, 3 connections: "789", Orange

Relay Configuration
-------------------

By convention, and for simplicity's sake, the relays are setup such that they numerically align to digits.

Relay Digit
* 1     1
* 2     2
* 3     3
* 4     4
* 5     5
* 6     6
* 7     7
* 8     8
* 9     9
* 10    \*
* 11    0
* 12    #

GPIO 40 pin Schema
------------------

Also by convention, we follow a path of least relay connectivity complexity for a wire ribon relative to both the 40 pin raspberry pi header and the relay's pin header.

Looking at a diagram of a raspberry pi zero's 40 pin header, we assume pin 1 in a top left position and
we connect the relays in the convention order from top (starting on the left) to bottom, and then breifly back up on the right from the bottom.

We ommit 3v power. 5v power is connected to the 5v power pins on the relay board (SainSmart 16-Channel 12V Relay Module Board for Arduino DSP AVR PIC ARM) so that the raspberry pi zero can be powered by the relay's 5v output.

GPIO        Relay
* 5v (2)      5v
* 5v (4)      5v
* GPIO2       1
* GPIO3       2
* GPIO4       3
* GPIO17      4
* GPIO27      5
* GPIO22      6
* GPIO10      7
* GPIO9       8
* GPIO11      9
* GPIO5       10
* GPIO6       11
* GPIO13      12
* GPIO19      13 (unused)
* GPIO26      14 (unused)
* GPIO21      15 (unused)
* GPIO20      16 (unused)
* Ground (34) Ground
* Ground (30) Ground

Installation
------------

HCLI hak11 requires a supported version of Python and pip.

You'll need an HCLI Connector to run hak11. For example, you can use HCLI Core (https://github.com/cometaj2/hcli_core), a WSGI server such as Green Unicorn (https://gunicorn.org/), and an HCLI Client like Huckle (https://github.com/cometaj2/huckle).


.. code-block:: console

    pip install hcli-hak11
    pip install hcli-core
    pip install huckle
    pip install gunicorn
    gunicorn --workers=1 --threads=1 -b 0.0.0.0:8000 "hcli_core:connector(\"`hcli_hak11 path`\")"

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

- Interacting with raspberry pi GPIO setup to trigger a properly configured relay connected to a Linear AK-11 keypad's 7 circuit pins.

To Do
-----

- TBD

Bugs
----

- TBD

.. |build status| image:: https://circleci.com/gh/cometaj2/hcli_hak11.svg?style=shield
   :target: https://circleci.com/gh/cometaj2/hcli_hak11
.. |pypi| image:: https://img.shields.io/pypi/v/hcli-hak11?label=hcli-hak11
   :target: https://pypi.org/project/hcli-hak11
.. |pyver| image:: https://img.shields.io/pypi/pyversions/hcli-hak11.svg
   :target: https://pypi.org/project/hcli-hak11
