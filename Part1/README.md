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
