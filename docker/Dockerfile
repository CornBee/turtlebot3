# Start with Ubuntu 18.04 as the base image (Jetson Nano does not support Ubuntu 20.04 and later)
FROM ubuntu:18.04

# Set non-interactive installation mode
ARG DEBIAN_FRONTEND=noninteractive

# Update the package repository and install necessary tools
RUN apt-get update && apt-get install -y \
    curl \
    gnupg2 \
    lsb-release \
    git

# Add the ROS2 apt repository
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
    && echo "deb http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list

# Install additional dependencies
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    gazebo9 libgazebo9-dev \
    ros-dashing-gazebo-ros-pkgs \
    ros-dashing-cartographer \
    ros-dashing-cartographer-ros \
    ros-dashing-navigation2 \
    ros-dashing-nav2-bringup \
    python3-vcstool \
    && rm -rf /var/lib/apt/lists/*

# Set the ROS environment for the PC
RUN echo 'source /opt/ros/dashing/setup.bash' >> ~/.bashrc \
    && echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc

# Create a workspace for your ROS2 project
RUN mkdir -p /workspace/src
WORKDIR /workspace

# Clone ROS2 project into the workspace
RUN git clone https://github.com/CornBee/turtlebot3/tree/dashing-devel src/turtlebot3

# Build your ROS2 project
RUN /bin/bash -c 'source /opt/ros/dashing/setup.bash && colcon build'

CMD ["bash"]
