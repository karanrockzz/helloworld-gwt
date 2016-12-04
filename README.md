# helloworld-gwt
A simple Hello World application in GWT, using the gwt-maven-plugin Maven archetype

To build and run locally:

    mvn clean install
    mvn gwt:run

---

To deploy and run inside OpenShift:

First ensure that the JBoss OpenShift imagestreams are installed into the `openshift` namespace:

    oc create -n openshift -f https://raw.githubusercontent.com/jboss-openshift/application-templates/master/jboss-image-streams.json

Then use the `oc new-app` command to create the resources for the application:

    oc new-app jboss-webserver30-tomcat8-openshift~https://github.com/monodot/helloworld-gwt

Expose a route for the service:

    oc expose svc helloworld-gwt

Now you can access the application using (e.g.):

    http://helloworld-gwt-foo.192.168.99.101.xip.io/helloworld-gwt-1.0-SNAPSHOT/ 

