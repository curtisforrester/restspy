REST Spy Django app
===============================

version number: 0.0.2
author: Curtis Forrester

Overview
--------

Django Rest Framework view filter for logging and replication with RabbitMQ.

(Note: Still checking in this project...)

Background
----------
This Django app grew first out of a need to log and persist all REST requests received for diagnostics and testing.
In order to verify when and what clients were sending I wanted to enable/disable logging with a flag. I also wanted
to perform testing with real-world requests in a repeatable way. The third task I required was a method of setting
up replication between two master servers. Most of the database-centric replication strategies either would not
fully support a Master-Master arrangement with Django migrations (Postgres, Redis) or were overkill (Cassandra).

My approach is to create a filter that can be added to DRF (Django-rest-framework) views which will republish all
requests received to a RabbitMQ exchange/queue. Using RabbitMQ's federated exchanges I then have redundant master
servers; operations on one are performed on the other(s).

Why Master-Master? All my clients must be able to connect to one of the replicated servers and have a reasonable
expectation that they are seeing and updating the same data. The state of the data is "good enough" at any point and
eventually synchronized over time. (My current application is a site monitoring system where sites publish metrics
    and status. The server then publishes notifications, email, etc. and a mobile client can view current site(s) status)

Filter Features
---------------
The three tasks that the 'restspy' filter supports are:

* Logging - all received rest requests and data are logged using the configured Django logging.
* Persist - the rest data is written to a database table.
* Replication - the request is published to a RabbitMQ queue.

Installation / Usage
--------------------

To install use pip:

    $ pip install restspy


Or clone the repo:

    $ git clone https://github.com/curtisforrester/restspy.git
    $ python setup.py install

Contributing
------------

TBD

Example
-------

TBD
