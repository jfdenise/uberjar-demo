# uberjar-demo
Test jaxrs project (migration of Thorntail sample project) to build wildfly uber jar. 
Vanilla WildFly jaxrs layer is used to provision a trimmed wildfly server

Build plugin

* git clone -b uberjar http://github.com/jfdenise/galleon-plugins
* cd galleon-plugins
* mvn clean install 

Build
* cd uberjar-demo
* mvn package

Run
* java -jar target/uberjar/demo-uberjar-wildfly-uberjar.jar

Run in Openshift

NB: When run in openshift, a vanilla WildFly server is executed. There is no support in this example for Openshift specifics env vars. 
WildFly s2i galleon feature-pack would need to be used at uberjar creation time to provision the full openshift support.

NB: wildfly-uberjar-runtime-centos7 image is WIP evolution of wildfly-runtime image. Official wildfly-runtime-image would be extended with the support.

* oc import-image wildfly-uberjar-runtime --from=quay.io/jfdenise/wildfly-uberjar-runtime-centos7 --confirm
* oc new-build --strategy source --binary --image-stream wildfly-uberjar-runtime --name test-uber
* oc start-build test-uber --form-dir ./target/uberjar
* Then deploy image and create route in openshift
