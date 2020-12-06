# Cloudyeti-ECS-Series

## Steps
#### Click on Create Cluster and Select Fargate Mode
![CreateCluster](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/1.png)
#### Provide the name of the Cluster and Create
![CreateCluster2](https://github.com/Cloud-Yeti/Cloudyeti-ECS-Series/blob/main/Part1/Images/2.png)
#### Now, in the terminal, Let's enter below command. Let's pull the image from docker (In this example, let's pull nginx image)
```sh
# This command will pull image from docker hub, if it doesn't exist locally, (-d) tags runs the container on the backgroung, (-p) is for allocation of port in order of host_port:container_port
docker run -d -p 80:80 nginx
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
  
#### Export your region and account_id
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

##### fetch the container_id of nginx image
```
image_id=`docker images | awk '{ print $3 }' | sed 1d`
```
##### tag the image
```
docker tag $image_id $account_id.dkr.ecr.$region.amazonaws.com/nginx
```
##### push the image 
```
docker push $account_id.dkr.ecr.$region.amazonaws.com/nginx
```
