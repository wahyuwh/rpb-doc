Pseudonym Generator (PIDG)
===============================================================
RPB integrates with Mainzelliste (1.6.1) to provide first class patient pseudonym generation and identity management.

================== ======== ================== ==============
OS                 Init     Application Server Database      
================== ======== ================== ==============
Debian 9 (Stretch) System V JDK 7, Tomcat 7    PostgreSQL 8.4
                                               PostgreSQL 9.5
================== ======== ================== ==============

PostgreSQL
----------

- Port: 5432
- Locale: UTF8
- Root user: postgres

.. code-block:: bash
	:caption: Create mainzelliste database

	psql -U postgres -c "CREATE ROLE mainzelliste LOGIN ENCRYPTED PASSWORD 'mainzelliste' SUPERUSER NOINHERIT NOCREATEDB NOCREATEROLE"
	psql -U postgres -c "CREATE DATABASE mainzelliste WITH ENCODING='UTF8' OWNER=mainzelliste"

Tomcat
------

:ref:`Application Server`
