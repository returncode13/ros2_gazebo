
#https://gazebosim.org/docs/garden/install_ubuntu_src
FROM osrf/ros:foxy-desktop


WORKDIR /progs
RUN apt-get update && apt-get install python3-pip wget lsb-release gnupg curl -y
#RUN ["sh","-c","echo 'deb http://packages.ros.org/ros/ubuntu \\$(lsb_release -sc) main' > /etc/apt/sources.list.d/ros-latest.list"]
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' \
    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
RUN apt-get update && apt-get install python3-vcstool python3-colcon-common-extensions git -y 
RUN mkdir -p garden_ws/src \
    cd $WORKDIR/garden_ws/src \
    pwd \
    wget https://raw.githubusercontent.com/ignition-tooling/gazebodistro/master/collection-garden.yaml \
    vcs import < collection-garden.yaml \
    wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null \
RUN apt-get update && apt -y install \
  $(sort -u $(find . -iname 'packages-'`lsb_release -cs`'.apt' -o -iname 'packages.apt' | grep -v '/\.git/') | sed '/ignition\|sdf/d' | tr '\n' ' ') \

RUN cd $WORKDIR/garden_ws \
    colcon graph
    colcon build --merge-install 
RUN . ~/workspace/install/setup.bash
    
