.. image:: https://travis-ci.org/mapfish/mapfish-print.svg?branch=development
    :target: https://travis-ci.org/mapfish/mapfish-print

Please read the documentation available here:
http://mapfish.github.io/mapfish-print/


Build
-----

Note: JAVA 8 does not work (fails to compile findbugsMain). JAVA 7 is required.

..code::

  > sudo update-java-alternatives -s java-7-oracle
  > export JAVA_HOME=/usr/lib/jvm/java-7-oracle

During the build process, jetty needs to start (at port 8080):

  > sudo service tomcat7 stop

Execute the following command():

..code::

  > perl -pi -e 's/\r\n/\n/g' gradlew
  > chmod +x gradlew
  > ./gradlew build
  >
  > sudo service tomcat7 stop

This will build three artifacts:  print-servlet-xxx.war, print-lib.jar, print-standalone.jar

The `build` also builds the documentation in the `docs/build/site` folder.  To deploy the documentation it should simply be copied to the gh-pages
branch and then committed github will automatically build the updated site at: http://mapfish.github.io/mapfish-print/#/overview

If you only want to build the docs simply run

.. code::

  > ./gradlew docs:build

or run build in the docs directory

Deploy
------

The following command will build and upload all artifacts to the maven central repository.

.. code::

  > sudo service tomcat7 stop
  > sudo rm -rf /var/lib/tomcat7/webapps/print
  > sudo cp debian/build/data/print.war /var/lib/tomcat7/webapps/
  > sudo service tomcat7 start
  >
  > scp debian/build/data/print.war jgr@geomaster.pt:/tmp
  > ssh jgr@geomaster.pt
  > sudo cp /tmp/print.war /opt/tomcat7/webapps

To use in Eclipse
-----------------

Create Eclipse project metadata:

.. code::

  > ./gradlew eclipse
  
Import project into Eclipse


Run from commandline
--------------------

The following command will run the mapfish printer.  The arguments must be supplied to the -PprintArgs="..." parameter.

To list all the commandline options then execute:

.. code::

 > ./gradlew run -PprintArgs="-help"

.. code::

  > ./gradlew run -PprintArgs="-config examples/config.yaml -spec examples/spec.json -output ./output.pdf"


Run in Eclipse
--------------

- Create new Java Run Configuration
- Main class is org.mapfish.print.cli.Main
- Program arguments: -config samples/config.yaml -spec samples/spec.json -output $HOME/print.pdf
