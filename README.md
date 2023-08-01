# DRAGONfly Simulation
This repository contains a simulation model for Purdue team's glider "DRAGONfly" for competing in the *NASA FLOATing DRAGON Balloon Challenge.*
The code is compatible with Gazebo Simulator to re-enact the flight control system and physic of the real world.
<br />

## About the Model & Its Purpose
[NASA FLOATing DRAGON Balloon Challenge](https://floatingdragon.nianet.org/)
>> Through the FLOATing DRAGON Challenge, NASA seeks innovative ideas and prototypes for a guided data vault recovery system consisting of: 1) a deployer that can be mounted to a HASP-type balloon gondola; and 2) a node that can be dropped and fall gracefully to a pre-determined, safe waypoint for recovery.
<img src="https://github.com/kmuenpra/Dragonfly-Simulation/blob/main/images/floating_dragon_lg.jpg" width = 250>
<br />

[Control Algorithm for the DRAGONfly aircraft](https://github.com/kmuenpra/Dragonfly) 
<br />

<img src="https://github.com/kmuenpra/Dragonfly-Simulation/blob/main/images/PurdueDRAGONflyLogoFINAL.png" width = 250>
<br/>

## Model Preview
<img src="https://github.com/kmuenpra/Dragonfly-Simulation/blob/main/images/DRAGONfly.PNG" width=500>
<img src="https://github.com/kmuenpra/Dragonfly-Simulation/blob/main/images/Dragonfly_model.png">

## Requirements
- Ubuntu Linux 22.x Operating System
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
Clone this repository and Copy the "dragonFly" folder (which has the model file) into the Docker container
```
docker cp ~/$PATH/dragonFly CONTAINER_NAME:/PX4-Autopilot/Tools/simulation/gazebo-classic/sitl_gazebo-classic/models
```

Configure the DragonFly model as PX4 SITL instance (Credit to [@antonerasm](https://discuss.px4.io/u/antonerasm) on this [Discussion Forum](https://discuss.px4.io/t/create-custom-model-for-sitl/6700/3))
1. Access the container
```
docker start CONTAINER_NAME ; docker exec -it CONTAINER_NAME bash
```

2. Create a new airframe file for "dragonFly" model
```
cd /PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes
cp 1030_gazebo-classic_plane 1234_gazebo-classic_dragonFly
```

3. Add the new airframe in the CMakeLists.txt found at /PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes
```
vi CMakeLists.txt
```
Inside the command "px4_add_romfs_files(...)", Add the airframe filename "1234_gazebo-classic_dragonFly"

4. Add the new airframe to the file /PX4-Autopilot/src/modules/simulation/simulator_mavlink/sitl_targets_gazebo-classic.cmake
```
cd /PX4-Autopilot/src/modules/simulation/simulator_mavlink/
vi sitl_targets_gazebo-classic.cmake
```
Inside the command "set(models ...)", Add the airfram name "dragonFly"
<br />


## Run DRAGONfly model Simulation
Once access into the aforementioned Docker container
```
cd /PX4-Autopilot
make px4_sitl_default gazebo-classic_dragonFly
```
Additionally, if you have QGroundcontrol, the simulation will automatically connect to GCS.
<br/>


## Troubleshoots & Future Improvements
- Imperfect physic in the simulated model
- Servo Command Mixing is currently incorrect
- Vehicle is unable to arm and, thus, unable to takeoff


