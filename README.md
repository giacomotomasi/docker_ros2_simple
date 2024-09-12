# docker_ros2_simple
This repo provide an example of two docker containers running to different nodes and communicating with each other. The containers are isolated from the host machine running the containers. The package `ros2_simple` is used for running dummy ros2 nodes as example.

### Dockers
`docker_pub` directory contains a Dockerfile for a ros2 container that runs a publisher node.\
`docker_sub` directory contains a Dockerfile for a ros2 container that runs a subscriber node.\
The two Dockerfile are identical except for the command that is run at container startup.

## How to use

### Build the images
Move to `docker_pub` and run:
```
docker build -t <image_name> .
```
Example:
```
docker build -t ros2_pub:humble .
```
This will create an image named `ros2_pub` with the tag `humble` from the Dockerfile inside the dir. The same process can be repeated for the other Dockerfile inside `docker_sub`. To create a image for the subscriber, go inside `docker_sub` and run:
```
docker build -t ros2_sub:humble .
```

## Docker compose
To handle multiple container a `docker-compose.yaml` file is used.  The compose file containes two services, one for the publusher and one for the subscriber that creates two containers of the relative images previously built. It also defines a custom network named `my_network`, sets the `ROS_DOMAIN_ID` and the `ROS_LOCALHOST_ONLY=1`. This allows the two containers to communicate with each other but also isolates them from the host machine. Additional settings are added to limit the cpu and memoty resources, this is not necessary for the specific application but may be useful in the future for bigger projects.\
Once the docker `docker-compose.yaml` is ready it can be used by running:
```
docker compose up
```
or
```
docker compose up ros2_simple_pub ros2_simple_sub
```
The first one runs all the services defined in the docker-compose file and the second allows to specify which services to run.\
To run the containers in detached mode (in the background), use:
```
docker compose up ros2_simple_pub ros2_simple_sub -d
```
If you want to view the logs of the containers running in detached mode, you can use:
```
docker compose logs
```


