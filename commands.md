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
## Virtual Machines Creation
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

## Managing Networks in Google Cloud Platform:
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
  


