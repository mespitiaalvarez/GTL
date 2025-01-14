# Use the ROS 2 Humble base image
FROM ros:humble

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=humble

# Update and install required dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    gnupg \
    lsb-release \
    python3-pip \
    python3-colcon-common-extensions \
    build-essential \
    vim \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Gazebo Harmonic (Gazebo Garden may also work depending on the project)
RUN curl -sSL http://packages.osrfoundation.org/gazebo.gpg | apt-key add - && \
    echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" > /etc/apt/sources.list.d/gazebo-stable.list && \
    apt-get update && apt-get install -y \
    gz-harmonic \
    ros-humble-gz-math \
    ros-humble-gz-math-vendor \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set up the ROS workspace
WORKDIR /root/create3_ws
RUN mkdir -p src
WORKDIR /root/create3_ws/src

# Clone necessary repositories
RUN git clone https://github.com/iRobotEducation/create3_sim.git 
RUN git clone https://github.com/iRobotEducation/irobot_create_msgs.git 

# Install dependencies using rosdep
WORKDIR /root/create3_ws
RUN rosdep update && \
    rosdep install --from-paths src --ignore-src -r -y

# Build the workspace
RUN colcon build --symlink-install

# Source the workspace setup script on container startup
RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc && \
    echo "source /root/create3_ws/install/local_setup.bash" >> ~/.bashrc

# Default command to start a bash shell
CMD ["bash"]
