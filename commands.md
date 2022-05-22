## Library and Software Installation
```
sudo yum update

sudo yum install docker

sudo systemctl start docker

sudo systemctl enable docker

sudo yum install git
```

## Testing Docker Installation

```
sudo docker run hello-world

sudo docker

sudo docker pull httpd

sudo docker images httpd

sudo docker info
```

## Running a Container

```
sudo docker container run -it httpd /bin/bash

exit

sudo docker container ls -a

sudo docker container rm <REPLACE WITH YOUR CONTAINER ID>
```

## Creating a Webserver in a Container

```
sudo mkdir /test

cd /test

sudo touch /test/Dockerfile

sudo vim /test/index.html

sudo vim Dockerfile

sudo docker build -t my-apache2 .
```

