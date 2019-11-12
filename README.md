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


The uberjar can be run in multiple contexts.

1- Run
* java -jar target/uberjar/demo-uberjar-wildfly-uberjar.jar

The JAXRS endpoint listens on URL: http://localhost:8080/demo-uberjar/hello

2- Create/Run Docker image

The demo contains a Dockerfileto build an image from adoptopenjdk/openjdk11.

* docker build -t demo-uberjar target/uberjar
* docker run -p 8080 demo-uberjar

3- Create an Openshift deployment from the dockerfile

Openshift can be used to initiate a docker build and start the uberjar.

* oc new-build --strategy docker --binary --name uberjar-demo
* oc start-build uberjar-demo --from-dir target/uberjar
* oc new-app uberjar-demo

4- Run in Openshift with WildFly runtime image extended to handle uberjar in a lightweight s2i WorkFlow

In order to benefit from a proper Openshift execution context (jolokia, python needed by probes, ...) when using Wildfly s2i galleon-pack, 
a prototype of the WildFly runtime image has been created.

NB: When run in openshift, a vanilla WildFly server is executed. There is no support in this example for Openshift specifics env vars. 
WildFly s2i galleon feature-pack would need to be used at uberjar creation time to provision the full openshift support.

NB: wildfly-uberjar-runtime-centos7 image is WIP evolution of wildfly-runtime image. Official wildfly-runtime-image would be extended with the support.

Import the WildFly runtime image

* oc import-image wildfly-uberjar-runtime --from=quay.io/jfdenise/wildfly-uberjar-runtime-centos7 --confirm

Create a new build
* oc new-build --strategy source --binary --image-stream wildfly-uberjar-runtime --name test-uber

Start a build
* oc start-build test-uber --from-dir ./target/uberjar

Create a deployment and route
* oc new-app test-uber
* oc expose svc/test-uber

If you make changes to your src code, you will only need to start a new Build.