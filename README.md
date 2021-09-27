# jenkins

## Install jenkens image using Docker

To start (assuming you have docker desktop installed), download docker-jenkins image.

run

```bash
docker pull jenkins/jenkins

```

To view all the docker images you have:

```bash

docker images
```

To see where your docker is installed, you can run:

```bash
docker info | grep -i root

```

### docker-compose

First, create a docker-compose.yml file. Refer to the actual file.

Then run `docker-compose up` to start your jenkins server:

```bash

docker-compose up -d  # -d will run the container in the background
```
Now you can run 

```
docker ps  # to see the running container
```

To see the logs emmited from the container, run

```bash
docker logs -f jenkins  # provide container name

```

Now if you go to localhost:8080, you should see jenkins up and running, cool!!
To log in, you will use the hash password you see from the logs.
you will see a hash that looks like this:
2de25205ae2a421ba9df4ac7b34e77e4

Then you can create admin user.

To stop the jenkins, since we are using docker, just do
```bash
docker-compose stop

```

To restart jenkins container, you can run
```bash
docker-compose restart jenkins
```

To go into the jenkins container and run bash:
```bash
docker exec -it jenkins bash

# to exit run
exit
```

To copy a file from local to container, 

```bash
docker cp <local_file_path> <container_name>:/<full_path_in_container>
# example

docker cp script.sh jenkins:/tmp/script.sh
```

little trick: if you hit control + r , you can type keyword to search the history commands!

#### Paramertrize your project

on Jenkins console, check "This project is parameterized" and you can add parameters to your project. 

## Use docker as a virtual machine to connect your jenkins docker container

To simulate a remote connection to the jenkins machine, we create another docker container.

We will use the ssh server from centos and create a docker image. Refer to `centos`.

Create ssh key using below. This will create two keys, private and public.
```sh
ssh-keygen -f remote-key
```

Refer to the centos folder and the Dockerfile.
Now, we can use `docker-compose build`. This will build all the images that we need.

Once built, run `docker-compose up` to start the containers.

Now, once all the containers are up, we will go into the jenkins container:
```sh
docker exec -it jenkins bash
# ssh into the remote container
ssh remote_user@remote_host
# this will ask you the password. type 1234 which we set previously

```
You can ssh into the remote_host from the jenkins container which is really cool!

Moreover, you can copy the private key to the jenkins container and then you won't be asked for passwoed anymore!
```bash
docker cp remote-key jenkins:tmp/remote-key
docker exec -it jenkins bash
# in the container
cd /tmp
ssh -i remote-key remote_user@remote_host

```
### Install Jenkins plugins

Go to the Jenkins UI and select Manage Jenkins
-> PlugIn manager
-> search for SSH, find the first one and install without restart


### Set up SSH

1. Go to credentials and create a credential 
2. Manage jenkins -> system configuration -> add SSH host
3. test connection

Now you can create a new job and select "execute shell script on remote host using ssh"
Now if you try to write a file to the file system, it will actually write it to the remote host!!

Notice, if you just ssh into the remote_host from jenkins container, you have to go up a few levels to be able to find the file. So the best way to check is to go into the container directly
```
docker exec -it remote-host bash  # notice here the container name is different than the service name which is remote_host. The service name is what gets resolved in the ssh hostname.
```
