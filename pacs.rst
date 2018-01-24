DICOM Server (PACS)
=================================================

RPB integrates with script-able research PACS system - Conquest (1.4.17d) to store all documented medical imaging and treatment planning data in DICOM format.

================== ======== ================== ==============
OS                 Init     Application Server Database      
================== ======== ================== ==============
Debian 9 (Stretch) System V Apache             PostgreSQL 9.5
================== ======== ================== ==============


PostgreSQL
----------

- Port: 5432
- Locale: UTF8
- Root user: postgres

.. code-block:: bash
	:caption: Create conquest role and database

	psql -U postgres -c "CREATE ROLE conquest LOGIN ENCRYPTED PASSWORD 'conquest' SUPERUSER NOINHERIT NOCREATEDB NOCREATEROLE"
	psql -U postgres -c "CREATE DATABASE conquest WITH ENCODING='UTF8' OWNER=conquest"

Apache
------
