RadPlanBio (RPB)
================
RPB-portal serves and entry point into the platform and provides integration middleware.

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
	:caption: Create radplanbio role database

	psql -U postgres -c "CREATE ROLE radplanbio LOGIN ENCRYPTED PASSWORD 'radplanbio' SUPERUSER NOINHERIT NOCREATEDB NOCREATEROLE"
	psql -U postgres -c "CREATE DATABASE radplanbio WITH ENCODING='UTF8' OWNER=radplanbio"


Tomcat
------
