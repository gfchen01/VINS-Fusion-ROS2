FROM ros:humble-ros-base
# VINS-Fusion Dockerfile for ROS2 Humble

ENV CERES_VERSION="2.1.0"
ENV COLCON_WS=/root/colcon_ws

      # set up thread number for building
RUN   if [ "x$(nproc)" = "x1" ] ; then export USE_PROC=1 ; \
      else export USE_PROC=$(($(nproc)/2)) ; fi && \
      apt-get update && apt-get install -y \
      cmake \
      libatlas-base-dev \
      libeigen3-dev \
      libgoogle-glog-dev \
      libsuitesparse-dev \
      libboost-all-dev \
      ros-${ROS_DISTRO}-cv-bridge \
      ros-${ROS_DISTRO}-image-transport \
      ros-${ROS_DISTRO}-message-filters \
      ros-${ROS_DISTRO}-tf2 && \
      rm -rf /var/lib/apt/lists/* && \
      # Build and install Ceres
      git clone https://ceres-solver.googlesource.com/ceres-solver && \
      cd ceres-solver && \
      git checkout ${CERES_VERSION} && \
      mkdir build && cd build && \
      cmake .. && \
      make -j$(USE_PROC) install && \
      rm -rf ../../ceres-solver && \
      mkdir -p $COLCON_WS/src/ && \

RUN   bash -c "echo 'source /opt/ros/${ROS_DISTRO}/setup.bash' >> ~/.bashrc"

# # Copy VINS-Fusion
# # COPY ./ $COLCON_WS/src/VINS-Fusion/
# # use the following line if you only have this dockerfile
# RUN git clone https://github.com/gfchen01/VINS-Fusion-ROS2.git && \
#       mv VINS-Fusion-ROS2 $COLCON_WS/src/VINS-Fusion && \
#       cd ..

# # Build VINS-Fusion
# WORKDIR $COLCON_WS
# ENV TERM xterm
# ENV PYTHONIOENCODING UTF-8
# RUN /bin/bash -c "source /opt/ros/${ROS_DISTRO}/setup.bash && \
#       colcon build --symlink-install \
#       --cmake-args -DCMAKE_BUILD_TYPE=Release"
#       # echo "source $COLCON_WS/install/setup.bash" >> ~/.bashrc
