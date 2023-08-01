# Dragonfly-Simulation
This repository contains Purdue's autonomous glider "DRAGONfly" simulation model used in Gazebo Simulator to re-enact the flight control system and physic of the real world.
<br />

## Requirements
- Gazebo Simulator 11
- Docker
<br />

## Set up PX4 Docker Environment
Clone leonardjmihs's "my_docker" repository
```
git clone https://github.com/leonardjmihs/my_docker.git
```

Create the Docker Image:
```
cd my_docker
docker build -t IMAGE_NAME .
```
<sub>***IMAGE_NAME can be changed to anything</sub>

Check if IMAGE_NAME is created
```
docker images ls
```

Create the Docker container:
```
docker run -it --privileged --volume="$HOME/$PATH/:/workspace" --network host --env="DISPLAY" --name=CONTAINER_NAME IMAGE_NAME
```
<sub>***$HOME/$PATH/ is the path to the my_docker repository cloned on your local computer</sub> <br />
<sub>***CONTAINER_NAME can be changed to anything</sub>

Check if CONTAINER_NAME is created
```
docker ps -a
```

Once youre in the docker container called CONTAINER_NAME:
```
cd /PX4-Autopilot/
source Tools/setup/ubuntu.sh
```

Additonal command lines:
```
# To get in the container again
docker start CONTAINER_NAME
docker exec -it CONTAINER_NAME bash
cd /PX4-Autopilot/

# To exit the container
exit   # (or CTRL + d)
docker stop CONTAINER_NAME

```
