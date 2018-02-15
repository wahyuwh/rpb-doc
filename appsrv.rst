Application Server
==================

Tomcat
------

Application server process should be owned by operating system user (e.g. tomcat), depending on a way how Tomcat was installed this user may not be created via installation process automatically. If this is the case the user has to be created manually and also login for this user should be disabled (it is only service user, owner of application server and its subprocesses).


Missing Global Libraries
^^^^^^^^^^^^^^^^^^^^^^^^

java.lang.ClassNotFoundException
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: bash
	:caption: Recommended global libraries in "TOMCAT_HOME/lib" folder

	"stax-ex.jar", "jaxb-impl.jar", "jaxws-api.jar", "jaxws-rt.jar"


Performance Optimisation
^^^^^^^^^^^^^^^^^^^^^^^^

In case of deployment or performance issues, it is recommended to check if any errors have been logged (deployed application specific or Tomcat logs files) and verify the Java Virtual Machine (JVM) settings. JVM configuration is what restrict the performance of Tomcat at the first place (in other words, it is not possible to scale Tomcat only by adding more memory to the server, the configuration of JVM has to be adapted as well). These settings are specified within JAVA_OPTS string e.g. in the Tomcat startup "/etc/init.d/tomcat" or set environment script "TOMCAT_HOME/bin/setenv.sh".

.. code-block:: bash
	:caption: Example of JAVA_OPTS settings from production environment

	export JAVA_OPTS="$JAVA_OPTS -Dorg.apache.el.parser.SKIP_IDENTIFIER_CHECK=true -Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Xms2048m -Xmx2048m -XX:+UseParallelGC -XX:ParallelGCThreads=2 -XX:PermSize=512m -XX:MaxPermSize=512m -XX:+DisableExplicitGC -XX:+CMSClassUnloadingEnabled

In general the are two types of memory areas used by JVM which are: heap and PermGen. It is possible to specify starting memory allocation and maximum memory allocation. For simplicity,  recommended is to set the starting and maximum values for each of memory area to the same value. In that way it is known exactly how much was assigned will actually be allocated by JVM and consequently available for Tomcat applications.

java.lang.OutOfMemoryError: Java heap space
"""""""""""""""""""""""""""""""""""""""""""

Recommended is to increase a heap memory space if you know that deployed applications will be creating lots of object instances. Every new object instance will occupy more space within heap memory space.

.. code-block:: bash
	:caption: To use 8GB heap space you can apply these setting

	-Xms8192m -Xmx8192m

java.lang.OutOfMemoryError: PermGen space
"""""""""""""""""""""""""""""""""""""""""

PermGen on the other hand holds internal representation of Java classes. The number of classes (the definition according the which object instances are initiated) should be mostly fixed per application. In that sense it is recommended to increase PermGen if there are more applications within one Tomcat. Each application will (depending on the size of application itself not the database) will occupy certain space from PermGen after the deployment.

.. code-block:: bash
	:caption: To use 512MB PermGen space you can apply these setting

	-XX:PermSize=512m -XX:MaxPermSize=512m

Logging
^^^^^^^

Log of tomcat stored in catalina.out is ever growing and in order to prevent problems it is recommended to setup the automatic rotation of catalina.out log daily or when it becomes bigger than 100 MB.

.. code-block:: bash
	:caption: Install log rotation utility

	apt-get install logrotate 

.. code-block:: bash
	:caption: Create rotation config file

	vi /etc/logrotate.d/tomcat

.. code-block:: bash
	:caption: Create the rotation configuration for catalina.out
	
	/usr/local/tomcat/log/catalina.out {  
 		copytruncate  
 		daily  
 		rotate 7  
 		compress  
 		missingok  
 		size 100M  
	}
