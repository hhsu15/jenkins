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
