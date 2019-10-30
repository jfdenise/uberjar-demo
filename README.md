# uberjar-demo
Test project (migration of Thorntail sample project) to build wildfly uber jar

Build plugin

* git clone -b uberjar http://github.com/jfdenise/galleon-plugins
* cd galleon-plugins
* mvn clean install 

Build
* cd uberjar-demo
* mvn package

Run
* java -jar target/demo-uberjar-wildfly-uberjar.jar

