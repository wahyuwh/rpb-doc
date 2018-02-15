DICOM Server (PACS)
===================

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

Web server will provide Common Gateway Interface (CGI) to Conquest. This can be used as simple DICOM server web interface.

.. code-block:: bash
	:caption: Install apache2 web server

	apt-get install install apache2

Conquest
--------

Download Conquest sources (conquest-14-17d) from official web site.

Build
^^^^^

.. code-block:: bash
	:caption: Create DICOM Server folder

	mkdir /opt/conquest-14-17d
	cd /opt/conquest-14-17d

.. code-block:: bash
	:caption: Create a user and group which will run Conquest as a service

	groupadd conquest
	useradd -g conquest -d /opt/conquest-14-17 conquest
	useradd conquest-import
	passwd conquest
	vi /etc/passwd change /bin/sh conquest user to /bin/false
	chown -R conquest:conquest /opt/conquest-14-17
	chown -R conquest-import:conquest /opt/conquest-14-17/data/incoming
	chmod g+w /opt/conquest-14-17/data/incomming

.. code-block:: bash
	:caption: Compile and install jpeg libary that is necessary to build Conquest

	cd jpeg-6c
	./configure
	make install


.. code-block:: bash
	:caption: Compile and install jasper libary that is necessary to build Conquest

	cd ../jasper-1.900.1-6ct
	./configure
	make install

.. code-block:: bash
	:caption: Create customised makefile

	vi mymak

	export LD_LIBRARY_PATH="/usr/local/pgsql/lib/"
	gcc -o lua.o -c lua_5.1.4/all.c -Ilua_5.1.4
	g++ -I/usr/include/postgresql -DUNIX -DNATIVE_ENDIAN=1 -DHAVE_LIBJASPER -DHAVE_LIBJPEG -DPOSTGRES -Wno-write-strings total.cpp lua.o -o dgate -lpthread -L/usr/lib -lpq -L/user/local/lib -ljasper -ljpeg -Ijpeg-6c -Ljpeg-6c -Ilua_5.1.4 -Wno-multichar
	rm lua.o
	pkill -9 dgate
	sleep 0.2s

	# Replace DICOM server dicom.ini with default config - comment when update or rebuild
	cp dicom.ini.postgres dicom.ini
	# Replace DICOM server dicom.sql with default config - comment when update or rebuild
	cp dicom.sql.postgres dicom.sql

	# Copy executable to allow CGI
	cp dgate /usr/lib/cgi-bin
	# Replace the CGI dicom.ini - comment when update or rebuild
	cp dicom.ini.www /usr/lib/cgi-bin/dicom.ini

.. code-block:: bash
	:caption: Give mymak executable flag

	chmod +x mymak

Instalation
^^^^^^^^^^^

.. code-block:: bash
	:caption: Build and install Conquest

	./mymak

Configuration
^^^^^^^^^^^^^

DICOM server configuraton (/opt/conquest-14-17d/dicom.ini)

.. code-block:: bash
	:caption: DICOM server application entity (AE) title

	MyACRNema = RPBPacs1
	TCPPort = 5678

	PACSName = RPBPacs1

.. code-block:: bash
	:caption: Change PostgreSQL settings

	SQLHost = localhost
	SQLServer = conquest
	Username = conquest
	Password = conquest

.. code-block:: bash
	:caption: Change the default MAG - file storage of DICOM data

	MAGDevice0 = /mnt/data1/

CGI DICOM server configuration (/usr/lib/cgi-bin/dicom.ini)

.. code-block:: bash
	:caption: Change CGI DICOM server settings

	MyACRNema = RPBPacs1
	TCPPort = 5678

	ACRNemaMap = /opt/conquest-14-17d/acrnema.map
	kFactorFile = /opt/conquest-14-17d/dicom.sql
	SOPClassList = /opt/conquest-14-17d/dgatesop.lst
	Dictionary = /opt/conquest-14-17d/dgate.dic

	WebScriptAddress = http://<server_address>/cgi-bin/dgate

Start Conquest DICOM Server
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash
	:caption: Initialise or regenerate database

	/opt/conquest-14-17d/dgate -v -r

.. code-block:: bash
	:caption: Start DICOM server

	/opt/conquest-14-17d/dgate -v &

Downloading DICOM studies/series
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In order to be able to download zipped version of DICOM studies or series it is necessary to install zipping program. Conquest ist using 7zip.

.. code-block:: bash
	:caption: Install 7zip

	apt-get install p7zip-full

.. code-block:: bash
	:caption: Get rid of error messages spamming the Conquest logs (create empty file)

	vi /opt/conquest-14-17d/zip.cq

.. Fixes:
.. Startup script
.. Note: export PGCLIENTENCODING=LATIN1 should be set before runnig conquest in script (even if database is UTF8 encoded, otherwise conquest crashes)

.. conquest home: /opt/conqest-14-17
.. copy conquest startup script
.. chmod a+x /etc/init.d/conquest
.. update-rc.d conquest defaults
