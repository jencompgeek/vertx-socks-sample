# Vert.x and SockJS Sample Application on Cloud Foundry

This is a sample project that shows how to run a simple Echo Server vert.x application with SockJS on Cloud Foundry.

## vert.x

[vert.x](http://vertx.io/) is a framework for for developing asynchronous event-driven applications in a variety of languages, including JavaScript, Ruby, Groovy, Java, Python and Coffeescript.

## Cloud Foundry

Cloud Foundry recently added support for [stand-alone applications](http://blog.Cloud Foundry.com/2012/05/11/running-standalone-web-applications-on-cloud-foundry/), which includes the ability to run applications in containers and frameworks that are not yet fully supported by Cloud Foundry.

Even more recently, Cloud Foundry added support for running Java 7 applications. vert.x requires a Java 7 runtime environment, so the addition of Java 7 support now makes it possible to run vert.x applications on Cloud Foundry.

## Sample Application

The vert.x code contains examples in several languages. This sample app is based on the [Java sockjs example](https://github.com/vert-x/vert.x/tree/master/vertx-examples/src/main/java/sockjs). The rest of this README will refer to changes made to the example to support Cloud Foundry.

### Configuration for Cloud Foundry

#### Web Server

For simplicity, the web server port in the example is hard-coded to `8080`:

    server.listen(8080);

    ...

    ]

In order to run on Cloud Foundry, the application must use the port provided by Cloud Foundry. This code reads the port from and environment variable, defaulting to `8080` if the Cloud Foundry environment is not detected:

    server.listen(System.getenv("VCAP_APP_PORT") != null ? Integer
			.parseInt(System.getenv("VCAP_APP_PORT")) : 8080);

    ...

    ]


## Compiling and Pushing the Application to Cloud Foundry

*Before you push the application to Cloud Foundry, you must update the SockJS server URL in index.html*

more index.html:

    var sock = new SockJS('http://vertx-socks-sample.cloudfoundry.com/testapp');

A full vert.x distribution (available on the [Downloads](http://vertx.io/downloads.html) page of the vert.x web site) must be pushed to Cloud Foundry along with the application.  Use the provided gradle "assembleModule" command to download and unpack the distro.  The command will download and unpack vert.x, add the sample class files and config files to a module directory and repack the whole app as a zip file.

The application files, along with the vert.x distribution, can be pushed to Cloud Foundry using the `vmc push` command with the provided manifest:

    > gradle build assembleModule
    > vmc push
    Would you like to deploy from the current directory? [Yn]: 
	Pushing application 'vertx-socks-sample'...
	Creating Application: OK
	Uploading Application:
	  Checking for available resources: OK
	  Processing resources: OK
	  Packing application: OK
	  Uploading (45K): OK   
	Push Status: OK
	Staging Application 'vertx-socks-sample': OK                                    
	Starting Application 'vertx-socks-sample': OK