## Summary

This repo contains example Node.js microservice application which can be deployed on OpenShift container platfrom or as a standalone Node.js application.

This readme file has simple tutorial for deploy and runing the Node.js service application from this repo


### Deployment on OpenShift v3

This application can be deployed on [OpenShift v3 container platfrom](https://www.openshift.org/index.html) which provides application management and scaling capabilities for Node.js apps

In order to deploy this Node.js example, you either have to have an access to the cloud hosted OpenShift v3 platfrom or use the [latest OpenShift Origin Vagrant box](https://www.openshift.org/vm/) to run OpenShift platform localy.

The instructions in this file asume that you are using latest OpenShift Origin Vagrant box and running it on your localhost.

#### Login to OpenShift

Once you have started the local OpenShift v3 platfrom and installed [OpenShift v3 CLI tool](https://docs.openshift.org/latest/cli_reference/get_started_cli.html), given that local platfrom is open and running, you can now login

	oc login https://10.2.2.2:8443
	
Upon succesfull login, you should see the list of existing projects on OpenShift platfrom.

#### Add Node.js Image Streams

Next, we are going to replace the existing OpenShift Node.js image stream with the more up to date one which has support for most recent Node.js versions. We are using [origin-s2i-nodejs](https://github.com/ryanj/origin-s2i-nodejs) project for the replacement imagestreams

	oc delete is/nodejs -n openshift ; oc create -n openshift -f https://raw.githubusercontent.com/ryanj/origin-s2i-nodejs/master/centos7-s2i-nodejs.json 

#### New project on OpenShift

Next, we will create to create the project for our application. 

You can create one by logging into the OpenShift UI pannel and creating new project via UI

	https://10.2.2.2:8443 (admin/admin)
	
Or you can use CLI to create a new project

	oc new-project project-service --display-name="project service REST" --description="RESTfull Projects service built in Node.js"
	
#### Deploy Node.js application

Next, we are using Node.js application template to deploy project service. 

OpenShift templates are define the the structure and deppendencies for your application. As well as container monitoring and auto redeployment conditions (code change in version control for example). Templates are located under

	/openshift/templates/nodejs-template.json

We are going to create new application based on the template

	oc new-app -f /path/to/nodejs-template.json
	
Check if deployments were started

	oc get builds

Manually start deployments if needed

	oc start-build project-service --follow

#### Add MongoDB to the project

Next, we are going to add the MongoDB container to the project. We can do this manually, by pointing to the MongoDB docker image in DockerHub

	oc new-app centos/mongodb-26-centos7 -e MONGODB_USER=admin,MONGODB_DATABASE=mongo_db,MONGODB_PASSWORD=secret,MONGODB_ADMIN_PASSWORD=super-secret

Now, after deployment is finished, we have MongoDB container running on the specific internal IP within OpenShift cluster. To get this IP address you can use

	oc status

What's left is to provide the MongoDB details to our Node.js project service. First, list the currentlly available environment variables, then provide MONGO_URL environment variable to project-service

	oc env pods --all --list
	oc set env dc/project-service MONGO_URL='mongodb://admin:secret@172.30.120.32:27017/mongo_db' 

Enjoy!

Created by [@sauliuz](https://twitter.com/sauliuz)
[More Node.js examples on OpenShift](https://github.com/openshift/nodejs-ex)







 

