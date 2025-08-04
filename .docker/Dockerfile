FROM osrf/ros:humble-desktop-full

# Copy the install script for xsarm to the Docker image home directory
COPY xsarm_amd64_install.sh /root/xsarm_amd64_install.sh

# Set the working directory to the home directory
WORKDIR /root

# Make the install script executable
RUN chmod +x xsarm_amd64_install.sh

# Run the install script
RUN ./xsarm_amd64_install.sh -d humble -p /root/interbotix_ws -n

# Source the ROS setup script
RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc

# Source the workspace setup script
RUN echo "source /root/interbotix_ws/install/setup.bash" >> /root/.bashrc

# Copy entrypoint script
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Rebuild the workspace
WORKDIR /root/interbotix_ws
RUN colcon build

# Set environment variables
ENV GAZEBO_MODEL_URI=''
ENV GAZEBO_MODEL_DATABASE_URI=''
ENV GAZEBO_RESOURCE_PATH=/usr/share/gazebo-11:/usr/share/gazebo-11/media:/usr/share/gazebo-11/models:/root/.gazebo/models

# Download gazebo models and move the to #HOME/.gazebo/models
RUN git clone https://github.com/osrf/gazebo_models.git /root/gazebo_models
RUN mv /root/gazebo_models/* /root/.gazebo/models/

# Set default command
CMD ["bash"]

ENTRYPOINT ["/entrypoint.sh"]