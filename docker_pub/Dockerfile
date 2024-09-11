FROM ros:humble-ros-base

RUN sudo apt-get update

# Set environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Update and install required packages (e.g., git, colcon)
RUN apt-get update && apt-get install -y \
    git \
    python3-colcon-common-extensions \
    && rm -rf /var/lib/apt/lists/*

# Set the working directory in the container
WORKDIR /ros2_ws

RUN git clone https://github.com/giacomotomasi/ros2_simple.git src/ros2_simple

# Build the ROS 2 workspace
RUN /bin/bash -c "source /opt/ros/humble/setup.bash && colcon build"

# Make port 8000 available to the world outside this container
# EXPOSE 8000

# Source the ROS 2 environment setup
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
RUN echo "source /ros2_ws/install/setup.bash" >> ~/.bashrc

CMD ["/bin/bash", "-c", "source /opt/ros/humble/setup.bash && source /ros2_ws/install/setup.bash && ros2 run ros2_simple marker_pub_node"]