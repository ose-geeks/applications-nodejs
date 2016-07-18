## Summary

This repo contains example Node.js microservice application which can be deployed on OpenShift container platfrom or as standalone Node.js application.

This readme file has detailed instructions how to deploy and run the application

### Deployment on OpenShift v3


This application can be deployed on [OpenShift v3 container platfrom](https://www.openshift.org/index.html) which provides application management and scaling capabilities for Node.js apps

In order to deploy this Node.js example, you either have to have a naccess to cloud hosted OpenShift v3 platfrom or use the [latest OpenShift Origin Vagrant box](https://www.openshift.org/vm/) to run OpenShift platform localy.

The instructions in this file asume that you are using latest OpenShift Origin Vagrant box running on your localhost (and will mostly work on any installation of OpenShift v3 platfrom).

#### New project on OpenShift

Once you have started the local OpenShift v3 platfrom and installed OpenShift v3 CLI tool (see the above section), given that local platfrom is open and running, you have to login with CLI tool

	oc login https://10.2.2.2:8443
	
Upon succesfull login you should see the list of existing projects running on OpenShift.

Next, we have to create the project for our application. You can do it by loging to UI pannel 

	https://10.2.2.2:8443 (default username / password are admin / admin)
	
and creating new project via UI or use CLI to create new project

	oc new-project project-service --display-name="project service nodejs" --description="Sample Node.js service with REST enpoints"
	
#### Deploy the application

In order to do first application deployment its best to use the OpenShift v3 application template.




 

