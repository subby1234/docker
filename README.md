# docker


sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker


IN ORDER NOT TO BE USING "sudo" everytime...
ADD USER TO DOCKER GROUP

getent group docker .............group will be seen, no admin

sudo usermod -aG docker $USER   .... ($USER refers to current user)

try again: 

getent group docker   .......group will be seen, user is also added

 newgrp docker    ...................For it to work: You either RESTART or "newgrp docker" .... to activate


docker pull ...e.g nginx...... to download image, you ca use it before running the image to be fast

docker run -d ...... (means detach mode; interactive terminal)

e.g: docker run -d --name demo nginx
                   (--name demo = to give the container name) (image)
 NOTE THAT: IF YOU DO NOT GIVE IT NAME ; DOCKER WILL ASSIGN A NAME TO THE CONTAINER     
 
 iF A CONTAINER STOP; YOU CAN START IT BY USING: 
 docker start id (container id; gotten from : docker ps)
 
 docker stop id 
 
 docker rm id: stop the container first .....to remove from the memory of the container
 
 
                    
docker ps ....... shows container that are in running state


docker ps -a....... shows both running and static container



NOW TO REMOVE IMAGE from container
docker rmi imagename (i.e nginx)
.......................................

-it

i.e. docker run -it --name democont nginx
...will create nginx, but will show all the details (going on in the background and take time and might let one inside the container)
..................

docker exec -it demodoc bash
...............will let you inside a container in a running state

control p+q ............... to exit
..............................................................................................................................

docker log id
details of the logs are shown...
.......................................

Port Forwarding... as assigned range of port nos: 3000-3247 : denoted by '-P'

docker run -d -P nginx

...docker ps... to see the attached port assigned
IP of the instance:port no
.........................................................
Binding Port: 
... choose port of my choice
docker run -d -p 8001:80 nginx
...................................................................................

DOCKER VOLUME
persistent storage

create volume and attach it to a container

docker volume create myvol1

...my volume ls ..... to check....

docker run -d -v myvol1:/tmp nginx

                       :/tmp = (path to save the vol)

docker exec -it id bash
cd /tmp
ls -al
nothing saved previously
touch myvol.txt
ls -al
.....
....
............mycol.txt

if the docker is removed; volume will persist and can be added to a new container

docker rm id

docker volume ls
myvol1 still visible

We can attach to another container

docker run -d -v myvol1:/tmp nginx


To delete volume: docker volume delete nameof volume
..............................................................



docker log id
docker inspect id
.......................


docker network ls

bridge/host/null

you can create your own network with " docker network create demonetwork

docker networl ls ... will show the new network
.............................
To use the new network...
docker run -d --network=demonetwork nginx
,,,,,,,,,,,,,,,,,,
To delete; docker network rm demonetwork
.........................

To attach network to already running container
docker ps....

docker network connect 'name of the network' id.................. docker network connect bridge id

use : docker inspect id: to check
........................................

To disconnect/remove network
docker network disconnect demonetwork-ID
.........................................

DOCKERFILE

Docker Directives:
FROM 
LABEL
RUN
COPY
ADD
WORKDIR
EXPOSE
CMD
ENV
ENTRYPOINT
VOLUME


......................................

create a Dockerfile with the following specs:
● Ubuntu container
● Apache2 installed
● Apache2 should automatically run once the container starts

# Use the official ubuntu base image
FROM ubuntu

# Update the package list and install Apache2
RUN apt-get update && apt-get install -y apache2

# Start Apache2 when the container launches
CMD ["apache2ctl", "-D", "FOREGROUND"]




FROM ubuntu
RUN apt-get update && apt-get install -y apache2
CMD ["apache2ctl", "-D", "FOREGROUND"]

.......................

docker build -t demoimage .
sudo docker run -d -p 80:80 dockerimage

................................................

1. create a sample HTML file
2. Use the Dockerfile from the previous task
3. Replace this sample HTML file inside the Docker container with the defaut page

# Use Ubuntu as the base image
FROM ubuntu:latest

# Install Apache2
RUN apt-get update && apt-get install -y apache2

# Copy the sample HTML file into the container
COPY index.html /var/www/html/

# Automatically start Apache2 when the container starts
CMD ["apache2ctl", "-D", "FOREGROUND"]
.......................................



Based on the output of docker inspect, the HTML files are located in a volume named myvol, and its source on the host machine is /var/lib/docker/volumes/myvol/_data.

So, to locate your HTML files on the host machine, you would navigate to /var/lib/docker/volumes/myvol/_data. This directory contains the files that are mounted into the container at /var/www/html.


sudo docker run -d -p 80:80 -v /home/ec2-user/html:/var/www/html --name apache_container demoimage2



..............................................................................
curl -SL https://github.com/docker/compose/releases/download/v2.24.4/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
































































