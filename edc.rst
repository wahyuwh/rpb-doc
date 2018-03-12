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


OpenClinica
-----------
OpenClinica instance need to be configured to know where to look for Designer application if it is installed.

.. code-block:: bash
	:caption: Specify URL of Designer for redirection from OpenClinica

	vi TOMCAT_HOME/openclinica.config/datainfo.properties

	designerURL=http://hostname.de:8080/Designer

Tomcat need to be restarted to apply these configuration changes.


Designer
--------

Designer distributable WAR file need to be deployed to Tomcat. Afterward the Designer need to be configured to allow OpenClinica instances to use it. Property "allowHosts" need to list all servers (running OpenClinica instances) which are allowed to used Designer (only server host and port are necessary, without OpenClinica application name).

.. code-block:: bash
	:caption: Specify servers that are allowed to use the Designer

	vi TOMCAT_HOME/webapps/Designer/WEB-INF/classes/resource.properties
	
	allowHosts=localhost:8080, hostname.de:8080, radplanbio.partner-site.de

Tomcat need to be restarted to apply these configuration changes.
