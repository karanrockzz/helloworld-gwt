# helloworld-gwt
A simple Hello World application in GWT, using the gwt-maven-plugin Maven archetype

---

## To build and run locally

    mvn clean install
    mvn gwt:run

---

## To deploy and run inside OpenShift

### To build using source-to-image (S2I)

First ensure that the JBoss OpenShift imagestreams are installed into the `openshift` namespace:

    oc create -n openshift -f https://raw.githubusercontent.com/jboss-openshift/application-templates/master/jboss-image-streams.json

Then use the `oc new-app` command to create the resources for the application:

    oc new-app jboss-webserver30-tomcat8-openshift~https://github.com/monodot/helloworld-gwt

Expose a route for the service:

    oc expose svc helloworld-gwt

Now you can access the application at (e.g.) `http://helloworld-gwt-myproject.192.168.99.101.xip.io/`

### To use fabric8/local deployment

If developing locally using the Red Hat Container Development Kit (CDK), you may wish to use the fabric8 workflow. This will allow you to build the Kubernetes config and Docker image locally using `fabric8-maven-plugin` and `docker-maven-plugin` and deploy directly into your CDK.

- The project has been configured to use the base image `registry.access.redhat.com/jboss-webserver-3/webserver30-tomcat8-openshift`.
- Maven uses the `gwt-maven-plugin` to compile the application into a file called `ROOT.war`, and then copies this to the `/deployments` directory in the image above, building a new image.
- The Kubernetes resources are then applied to your local OpenShift cluster, creating a Service and ReplicationController for the project.
- When the pod starts up, the application `ROOT.war` is automatically deployed.
- Use `oc get pods`, `oc get rc` and the OpenShift Web Console to see what has been deployed.

To use this workflow:

    mvn -Pf8-local-deploy -DskipTests=true

Then to access the application (e.g.): `http://helloworld-gwt-myproject.192.168.99.101.xip.io/`

