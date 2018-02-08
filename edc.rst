Electronic Data Capture (EDC)
=============================

RPB integrates with OpenClinica (3.9) Electronic Data Capture system which is used to build eCRF forms and conduct study data.

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
	:caption: Create clinica role and openclinica database

	psql -U postgres -c "CREATE ROLE clinica LOGIN ENCRYPTED PASSWORD 'clinica' SUPERUSER NOINHERIT NOCREATEDB NOCREATEROLE"
	psql -U postgres -c "CREATE DATABASE openclinica WITH ENCODING='UTF8' OWNER=clinica"

Tomcat
------

:ref:`Application Server`
