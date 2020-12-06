# Cloudyeti-ECS-Series

## Steps
#### Click on Create Cluster and Select Fargate Mode
![CreateCluster](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/1.png)
#### Provide the name of the Cluster and Create
![CreateCluster2](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/2.png)
#### Now, in the terminal, Let's enter below command. Let's pull the image from docker (In this example, let's pull nginx image)
```sh
# let's pull image from docker
docker pull nginx
```
#### Now, let's run a container out of that image in background (with -d flag)
```sh
docker run -d nginx
```
#### To verify the images is pulled and container is running on your local
```sh
# See the images
docker images 
# See the running containers
docker ps
```
#### To push image to ecr, follow Below Steps

```
Pre-requisites:
```
  * have aws cli installed
  * https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html
  * Configure AWS Credentials
  ```
  # Just an example, the below credentials doesn't exists. It's a very bad security practice of having secrets in source code repositories.
  aws configure
  AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
  AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
  Default region name [None]: us-east-1
  Default output format [None]: json
  ```
  
#### Export your region and account_id (If you copy the ECR push command from AWS Console, you don't need these steps, but it's good to have anyways)
```sh
export region=us-east-1
export account_id=<your_ac_id>
```
#### To create a repo using aws cli
  * aws ecr create-repository --region $region --repository-name cloudyeti/nginx
#### To push your local image to ECR, you must create a tag, login and then only you are permitted to push
```
# login for ECR
aws --region $region ecr get-login-password | docker login --password-stdin --username AWS $account_id.dkr.ecr.$region.amazonaws.com
```

##### tag the image
```
docker tag nginx $account_id.dkr.ecr.$region.amazonaws.com/cloudyeti/nginx
```
##### push the image 
```
docker push $account_id.dkr.ecr.$region.amazonaws.com/cloudyeti/nginx
```
#### Now create a new Task definition
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/3.png)
#### Select Fargate Launch Type and Click Next
#### Provide the Name of Task Definiton
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/4.png)
#### Click on add container and provide the Image URI of ECR
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/5.png)
#### You can select the minimul task_memory and vCPU for this lab and click "Create"
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/6.png)
#### Run a new Task
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/7.png)
#### In this example, we have selected the VPC, and 2 public subnets
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/8.png)
#### Copy the public IP and paste on to the browser
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/9.png)
#### Congratulations! You have nginx application running in AWS ECS Fargate
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/10.png)
#### For high availability of your task, you must configure service.
#### On service tab, click on create
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/11.png)
#### Attach the nginx task definiton to the service and specify the numbers of task you want to run. In the screenshot I have test on task definiton, please select the desired task defination you want to attach to the service.
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/12.png)
#### Here, we have selected the custom VPC and 2 public subnets and we aren't configuring load balancer in this lab
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/13.png)
#### We are skipping the autoscaling creation in this lab, Let's create our service
![td](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/14.png)

### Note: Now even if you stop your task, the service will bring up the new task to desired capacity. Give a shot

## Thanks for following :)
