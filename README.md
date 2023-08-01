# DRAGONfly Simulation
This repository contains Purdue's autonomous glider "DRAGONfly" simulation model used in Gazebo Simulator to re-enact the flight control system and physic of the real world.
<br />

## Purpose of the simulation model
[NASA FLOATing DRAGON Balloon Challenge](https://floatingdragon.nianet.org/)
>> Through the FLOATing DRAGON Challenge, NASA seeks innovative ideas and prototypes for a guided data vault recovery system consisting of: 1) a deployer that can be mounted to a HASP-type balloon gondola; and 2) a node that can be dropped and fall gracefully to a pre-determined, safe waypoint for recovery.

[Control Algorithm for the DRAGONfly aircraft](https://github.com/kmuenpra/Dragonfly)

<img src="https://github.com/kmuenpra/Dragonfly-Simulation/blob/main/images/Dragonfly_model.png">

## Requirements
- Ubuntu Linux 22.x
- [Gazebo Simulator 11 (Default Installation)](https://classic.gazebosim.org/tutorials?tut=install_ubuntu)
- [Docker](https://docs.docker.com/engine/install/ubuntu/)
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
# To access the container
docker start CONTAINER_NAME
docker exec -it CONTAINER_NAME bash
cd /PX4-Autopilot/

# To exit the container
exit   # (or CTRL + d)
docker stop CONTAINER_NAME
```
<br />

## Add "DRAGONfly" model into the PX4 Docker Environment
Clone this repository and Copy the dragonFly folder (which has the model file) into the Docker container
```
docker cp ~/$PATH/dragonFly CONTAINER_NAME:/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/models
```

## Run DRAGONfly model simulaiton
Once access into the aforementioned Docker container
```
cd /PX4-Autopilot
make px4_sitl_default gazebo-classic_dragonFly
```
Additionally, if you have QGroundcontrol, the simulation will automatically connect to it.

## Troubleshoots & Future Improvements


