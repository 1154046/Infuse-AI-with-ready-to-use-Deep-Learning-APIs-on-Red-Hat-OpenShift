# Infuse AI with ready to use Deep Learning APIs on Red Hat OpenShift


Data contains a wealth of information that can be used to solve certain types of problems. Traditional data analysis approaches, like a person manually inspecting the data or a specialized computer program that automates the human analysis, quickly reach their limits due to the amount of data to be analyzed or the complexity of the problem.

Machine learning, and deep learning, which is a specialized type of machine learning, uses algorithms – also known as ”models” - to identify patterns in the data. A trained model can be used to make predictions or decisions, and solve problems such as analyzing the content of text (spoken and written), images, audio, and video. For example, a model can be trained to identify objects in an image, sounds in an audio or video file or summarize the the content in text form.

![deep_learning_example](https://user-images.githubusercontent.com/20628307/133517645-f37c29bb-c17f-454d-8cde-02876e2b8dcf.png)

The Model Asset Exchange is a curated repository of ready-to-use state-of-the-art free and open source deep learning models, which can be deployed as microservice Docker images in local, hybrid, or cloud environments.


![simplified_architecture](https://user-images.githubusercontent.com/20628307/133517912-13a00d97-f6fc-40e7-88a8-b421c216790d.png)


After completing this tutorail, you will learn how to use the OpenShift web console or OpenShift Container Platform command-line interface to:

    1. Deploy a model-serving microservice using a public container image on Docker Hub
    2. Create a route that exposes the microservice to the public

For illustrative purposes you will deploy the Object Detector microservice using the OpenShift Web Console and the Image Caption Generator microservice using the OpenShift Platform CLI.

# Prerequisites
You need to have the following installed to complete the steps in this code pattern:
    * OpenShift CLI tool [oc](https://cloud.ibm.com/docs/openshift?topic=openshift-openshift-cli#cli_oc) (You can use cloud shell on IBM Cloud as an alternative: https://cloud.ibm.com/shell )    
    * [IBM Cloud Account](https://ibm.biz/Bdfpir)
    
# Steps:

Follow these steps to set up and run this code pattern locally and on the cloud. The steps are described in detail below.

1. [Create or Sign in to your IBM Cloud Account](#1-sign-in-to-ibm-cloud)

2. [Clone the GitHub repository locally](#2-clone-the-GitHub-repository-locally)

3. [Build a docker image and run it locally](#3-build-and-run-a-docker-image-locally)

4. [Deploy to IBM RedHat OpenShift 4 Cluster](#4-deploy-to-openshift-4-cluster)


### 1. Sign in to IBM Cloud
Follow the link - [IBM Cloud](https://ibm.biz/Bdfpir) to sign up for a new Free IBM Cloud Account or sign in to your IBM Cloud Account.


### 2. Provision a Free OpenShift Cluster.
Please ensure you use the above link to sign in or create an IBM Account, this will allow you to get access to an Internal IBM Cloud organization called Advowork which you can provision a cluster in.


Please follow the following link:
URL: https://infusedeepai.mybluemix.net

You should see the this screen next:
![Screenshot (314)](https://user-images.githubusercontent.com/20628307/133519410-88a2f017-d2ab-4935-a273-76eccfbeb36b.png)

As soon as you are in, you want to enter the Email Address you used to create your IBM Cloud Account as your IBMId, then enter the following key.
Key: oslab

This should be the screen you see if all went well:


You will now receive an invitation to join the organization, you will need to accept this invite from your mailbox to proceed.

Please note, this cluster will only be available for 24 hours after the webinar concludes.


### 3. Clone the GitHub repository locally

Clone the `IBM MAX Repo` GitHub repository locally.

In a terminal, run the following:

```bash
git clone https://github.com/IBM/max-model-prediction-os.git
cd max-model-prediction-os
```

The cloned repository contains sample images and utility scripts.

### 4. Set Up OpenShift
This microservice identifies objects in pictures, enabling you to build applications that need to interpret the content of a picture.

![od_sample_app](https://user-images.githubusercontent.com/20628307/133518806-b79609d3-2821-4c12-8f17-5d634e4c9066.png)

1. Under your provisioned Cluster, the Administrator view is displayed by default, switch to the Developer view.  
    
![rhos_web_console_admin_view](https://user-images.githubusercontent.com/20628307/133519502-b9986b41-d842-4207-aa2c-3133bf7e68b2.png)
    
2. OpenShift organizes related resources in projects. In the lab environment you going to create a project called InfuseAI.
    
```bash 
  oc new-project infuseai
```
    
### 5. Deploy the Object Detector microservice Docker image
You can deploy Docker images that are hosted in public or private registries. The MAX-Object-Detector Docker image codait/max-object-detector is hosted on Docker Hub, which is a public registry.
1. Select +Add and choose Container Image as source.
![ui_add_something](https://user-images.githubusercontent.com/20628307/133519850-84cfee65-915c-40a8-a236-9592ef394804.png)
2. Select the Image name from external registry radio button.
3. Enter codait/max-object-detector as Image Name.
    
Click on the magnifying glass next to the image name (or press Enter) to load the Docker image's metadata.
    
4. Review the deployment configuration for the Docker image.
  Sample Output:
  ```
  codait/max-object-detector Mar 31, 9:07 am, 476.4 MiB, 14 layers
  Image Stream max-object-detector:latest will track this image.
  This image will be deployed in Deployment Config max-object-detector.
  Port 5000/TCP will be load balanced by Service max-object-detector.
  Other containers can access this service through the hostname max-object-detector.
```

5. In the General section the Name field is pre-populated with the Docker image name. OpenShift uses this name to identify the resources being created when the application is deployed.
![Uploading Screenshot (317).png…]() 

If you append, modify, or delete a few characters, you'll notice how the name change is impacting the generated image stream name, the deployment configuration name, the service name, and the host name. (However, use the default max-object-detector in this lab!)

6.  In the Resources section make sure Deployment is selected as resource type.

![Uploading ui_configure_deployment_resource_type.png…]()

7. In the Advanced Options section check Create a route to the application to expose the deployed application to the public. (If the option is not checked the application is only visible within the cluster.)


You can customize the route configuration by clicking the Routing link, for example by configuring whether to expose an unsecured route (the default) or a secured route. In this lab the default settings are fine.



8. Click the Deployment link to customize the deployment.


You can customize the behavior of the deployed microservice by setting environment variables. For example, you can enable Cross-Origin Resource Sharing support by setting the CORS_ENABLE variable in the deployment configuration to true. If CORS is enabled (which it is not by default) client applications that run in a web browser can call the microservice endpoints.

9. Click the Scaling link to configure how many copies of the deployed microservice you want to run. In this lab the default setting of 1 is sufficient because you are the only user who will utilize this service.


10. Click the Resource Limit link to configure the minimum and maximum amount of CPU and RAM the deployed microservice can utilize. Since the object detector microservice requires a minimum of 2 GB of RAM, set memory request and memory limit to 2 Gi.


11. Click Create to deploy the microservice. The deployment toplogy view is displayed.


![ui_view_deployment_topology](https://user-images.githubusercontent.com/20628307/133527178-7781819c-6d72-4253-a9c6-3c3929dc4901.png)



12. Click the max-object-detector deployment label (marked with a dashed arrow in the screen capture) to open the deployment configuration panel.

![ui_view_deployment](https://user-images.githubusercontent.com/20628307/133576520-41110a9f-272c-4217-8590-47c72f03ccdf.png)

13. Select the Resources tab, which provides you with access to the log files and a link to the microservice's public route.

![image](https://user-images.githubusercontent.com/20628307/133576563-93f48423-4f41-4ece-bd87-3a0825126cb5.png)

14.Before proceeding make sure the pod status is running, as shown in the screen capture above.

Open the displayed route URL to access the microservice's OpenAPI specification, which documents the REST API endpoints applications and services can call to utilize the microservice.

### 6. Testing the Application
Open the displayed route URL to access the microservice's OpenAPI specification, which documents the REST API endpoints applications and services can call to utilize the microservice.

![view_od_openapi_spec](https://user-images.githubusercontent.com/20628307/133576784-84ee8c64-dbf5-4016-bd46-6f1d448ef45a.png)


Expand the model twistie to reveal the microservice endpoints. The Object Detector exposes three endpoints. The GET /model/metadata endpoint provides information about the microservice. The GET /model/labels endpoint identifies the kinds of objects the microservice can detect in an image. The POST /model/predict endpoint detects objects in an image that is included in the payload when the endpoint is invoked.


![view_od_endpoints-1](https://user-images.githubusercontent.com/20628307/133576872-88f613e0-3294-4e1c-bbd2-ce23433ec9a5.png)

You can try the endpoints by clicking on the name.

Click GET /model/labels, Try it out, and Execute to retrieve the list of objects that the microservice can detect in an image.

![view_od_labels_endpoint](https://user-images.githubusercontent.com/20628307/133576942-75810004-fb76-4cee-993e-c5af0b4f11f4.png)

The response should look as follows and includes how many types of objects can be detected and what they are.

https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/deploy-ai-microservices-on-kubernetes/images/view_od_labels_endpoint_response.png

The Object Detector microservice includes a small embedded demo application that illustrates how a web application can consume the /model/predict endpoint.


Next, Open the embedded sample application by appending /app to the public route of your deployed microservice, e.g. http://infuseai-os-.../app.

Test the microservice by selecting an image of your choice and changing the probability threshold. By lowering the threshold you can see objects that the Object Detector deep learning model is less certain about.

![view_od_sample_app-1](https://user-images.githubusercontent.com/20628307/133577156-b9a5acef-49a4-44dc-9418-377b6bca3f75.png)












