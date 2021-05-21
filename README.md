# Google Cloud Computing Foundations: Commands
---

| Name | Shekhar Saxena |
|:---|:---|
| University Roll No | 2014852|
| Course | B.Tech.|
| Branch | CSE |
| Semester | 4th |
| Section | A |
| MiniProject Topic | GCCF |


---


> **Qwiklabs Public Profile:**
> **[https://www.qwiklabs.com/public_profiles/abe0f2a5-82c9-455d-9d65-ef8645546670](https://www.qwiklabs.com/public_profiles/abe0f2a5-82c9-455d-9d65-ef8645546670)**

---

## Project Configuration in Google Cloud Platform
```java
// list the associated account details with shell
gcloud auth list

Output: 
Credentialed accounts:
 - <myaccount>@<mydomain>.com (active)

   
// List the projects
gcloud config list project

Output:
[core]
project = <project_ID>


// Display the project details, region and zone
gcloud compute project-info describe --project <your_project_ID>

gcloud config get-value compute/zone

gcloud config get-value compute/region


// Changing the project/ zone
export PROJECT_ID=<your_project_ID>

export ZONE=<your_zone>
```

---
## Creating Virtual Machines
```java

// list the associated acount details with shell
gcloud compute images list


// Creating the Virtual Machine
gcloud compute instances create VM_NAME \ [--image=IMAGE | --image-family=IMAGE_FAMILY] \ --image-project=IMAGE_PROJECT --machine-type=MACHINE_TYPE


// Verifying the installation of VM
gcloud compute instances describe VM_NAME

```
---

## IAM and Admin: Managing Roles and Permissions

```java

// Providing access to members
gcloud group add-iam-policy-binding resource \ --member=member --role=role-id

// group: project or your organisation
// resource: for which you are providing the access
// role-d: name of the role


// Revoking access
gcloud group remove-iam-policy-binding resource \ --member=member --role=role-id


// Modifying the current IAM Policy
// IAM policy is a JSON file which contains roles, users, permissions info.
gcloud projects get-iam-policy project-id --format=format > filepath

```
---

## Managing APIs:

```java

// Enabling and Disabling the APIs
$ gcloud services enable pubsub.googleapis.com$ gcloud services disable pubsub.googleapis.com 

```
---

## VPC Network Configuration in Google Cloud Platform:
```java

// Creating a VPC Network
gcloud compute networks create NETWORK \ --subnet-mode=custom \


// Viewing the list of All Networks
gcloud compute networks list


// View the details and subnets of a network
gcloud compute networks describe NETWORK 


// Creating a subnet
gcloud compute networks subnets create SUBNET \ --network=NETWORK \ --range=PRIMARY_RANGE \ --region=REGION 


// Deleting a subnet
gcloud compute networks subnets delete SUBNET \ --region=REGION


// List all Subnets of a network 
gcloud compute networks subnets list \ --network=NETWORK 

// Remove –-network for viewing all subnets


// VM WITH MULTIPLE NETWORK INTERFACES: Major Task
gcloud compute instances create vm-appliance \ --network-interface subnet=subnet0,no-address \ --network-interface subnet=subnet1 \ --network-interface subnet=subnet2,no-address \ --machine-type n1-standard-4

  ```
  ---
  
  
  ## Creating an Internal Load Balancer:
  
  ```java
  
  
  // create the load balancer template
gcloud compute instance-templates create lb-backend-template \
   --region=us-central1 \
   --network=default \
   --subnet=default \
   --tags=allow-health-check \
   --image-family=debian-9 \
   --image-project=debian-cloud \
   --metadata=startup-script='#! /bin/bash
     apt-get update
     apt-get install apache2 -y
     a2ensite default-ssl
     a2enmod ssl
     vm_hostname="$(curl -H "Metadata-Flavor:Google" \
     http://169.254.169.254/computeMetadata/v1/instance/name)"
     echo "Page served from: $vm_hostname" | \
     tee /var/www/html/index.html
     systemctl restart apache2’

// Create a managed instance group based on the template
gcloud compute instance-groups managed create lb-backend-group \
   --template=lb-backend-template --size=2 --zone=us-central1-a

// create a firewall rule
gcloud compute instance-groups managed create lb-backend-group \
   --template=lb-backend-template --size=2 --zone=us-central1-a

// Create a managed instance group based on the template
gcloud compute instance-groups managed create lb-backend-group \
   --template=lb-backend-template --size=2 --zone=us-central1-a

// Setup a Global Static External IP Address
gcloud compute addresses create lb-ipv4-1 --ip-version=IPV4 –global

// Health check for load Balancer
 gcloud compute health-checks create http http-basic-check --port 80

// Create a backend service
 gcloud compute backend-services create web-backend-service \
        --protocol=HTTP \
        --port-name=http \
        --health-checks=http-basic-check \
        --global
// Create a URL map to route the incoming requests to the default backend service
 gcloud compute url-maps create web-map-http \
        --default-service web-backend-service

// Create a target HTTP proxy to route requests to your URL map
 gcloud compute target-http-proxies create http-lb-proxy \
        --url-map web-map-http

// Create a global forwarding rule to route incoming requests to the proxy
 gcloud compute forwarding-rules create http-content-rule \
        --address=lb-ipv4-1\
        --global \
        --target-http-proxy=http-lb-proxy \
        --ports=80
        
 ```
 ---
 
 ## Building, Deploying and Debugging Docker: 
 
 ```java
 //  run a hello world container
 docker run hello-world

// list of container images pulled from Hub
 docker images

// show running Containers
 docker ps

// Stop and remove docker image
docker stop my-app && docker rm my-app

// debug logs
docker logs [container_id]

// Publishing images
docker push gcr.io/[project-id]/node-app:0.2
```
---

## Creating a Clusters in GKE

```java
//   create a cluster
 gcloud container clusters create [CLUSTER-NAME]

// authenticate the cluster
 gcloud container clusters get-credentials [CLUSTER-NAME]

// deploy app to cluster
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0

// create a Kubernetes service
kubectl expose deployment hello-server --type=LoadBalancer --port 8080

// delete the cluster
gcloud container clusters delete [CLUSTER-NAME]

// inspect the kubernetes service
kubectl get service
```
---


## Bucket Configuration in GCP:

```java

// Creating a Cloud Storage Bucket
gsutil mb gs://BUCKET_NAME

Output: 
Creating gs://BUCKET_NAME/...

// Uploading an image ada.jpg to bucket
gsutil cp ada.jpg gs://YOUR-BUCKET-NAME

// Downloading an object from Bucket
gsutil cp -r gs://YOUR-BUCKET-NAME/ada.jpg .

// List Objects in a Bucket
gsutil ls gs://YOUR-BUCKET-NAME

// List Details of an Object
gsutil ls -l gs://YOUR-BUCKET-NAME/ada.jpg

// Copy an object to a folder from bucket
gsutil cp gs://YOUR-BUCKET-NAME/ada.jpg gs://YOUR-BUCKET-NAME/image-folder/


```
---

## Sharing Objects Publicly from Bucket:

```java

// Sharing Objects Publicly from Cloud Bucket
gsutil acl ch -u AllUsers:R gs://YOUR-BUCKET-NAME/ada.jpg
Output: 
Updated ACL on gs://YOUR-BUCKET-NAME/ada.jpg

// Removing Public Access
gsutil acl ch -d AllUsers gs://YOUR-BUCKET-NAME/ada.jpg


```
---



## Creating a Cloud Function:
```java
// Create a Function using nano command in a index.js file
nano index.js

// Deploying a cloud function
gcloud functions deploy helloWorld \
  --stage-bucket [BUCKET_NAME] \
  --trigger-topic hello_world \
  --runtime nodejs8

// Verify the status of function
gcloud functions describe helloWorld
entryPoint: helloWorld
eventTrigger:
  eventType: providers/cloud.pubsub/eventTypes/topic.publish
  failurePolicy: {}
  resource:
...
status: ACTIVE
...

// View logs for a cloud function
gcloud functions logs read helloWorld

```
---

## Managing Pub/Sub in GCP:

```java
// Creating a topic using pubsub 
gcloud pubsub topics create myTopic
// List the created topics
gcloud pubsub topics list
// Delete a topic
gcloud pubsub topics delete Test1


// Create a subscription
gcloud  pubsub subscriptions create --topic myTopic mySubscription
// List the created subscription
gcloud pubsub topics list-subscriptions myTopic
// Delete a subscription
gcloud pubsub subscriptions delete Test1


// Publish a message
gcloud pubsub topics publish myTopic --message "Hello“
// Pulling message
gcloud pubsub subscriptions pull mySubscription --auto-ack
// Pulling message with limit
gcloud pubsub subscriptions pull mySubscription --auto-ack --limit=3

```
---

## Creating, Training a Tensorflow 2.xx Application in AI Platform

```java

// Creating a Bucket
gsutil mb -l $REGION gs://$BUCKET_NAME

# Clone the repository of AI Platform samples 
git clone --depth 1 https://github.com/GoogleCloudPlatform/cloudml-samples 

# Set the working directory to the sample code directory 
cd cloudml-samples/census/tf-keras

// Install dependencies
pip install -r requirements.txt

// Train the AI Model
gcloud ai-platform local train \ --package-path trainer \ --module-name trainer.task \ --job-dir local-training-output

```
---

## Submitting a Job in DataProc:

```java
// Setting Region for Dataproc
gcloud config set dataproc/region us-central1
// Creating a cluster
gcloud dataproc clusters create example-cluster --worker-boot-disk-size 500

// Submit a job
gcloud dataproc jobs submit spark --cluster example-cluster \
  --class org.apache.spark.examples.SparkPi \
  --jars file:///usr/lib/spark/examples/jars/spark-examples.jar – 1000

// Update the cluster
gcloud dataproc clusters update example-cluster --num-workers 4
```
---

## Analyze-entities method to ask the Cloud Natural Language API and extract entities

```java
// set an environment variable with your PROJECT_ID
export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)

# create a new service account to access the Natural Language API:
gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account“

# create credentials to log in as your new service account
gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com

// set the GOOGLE_APPLICATION_CREDENTIALS environment variable
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"

// Make an Entity Analysis Request
gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'." > result.json

// Results
cat result.json

```










