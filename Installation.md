
* This installation guide is based on Cartographer's official installation guide [link](https://google-cartographer-ros.readthedocs.io/en/latest/).
* Souces of ceres-solver is changed to GitHub from googlesource.com.
* We recommend using wstool and rosdep. For faster builds, we also recommend using Ninja.

# Install wstool and rosdep.
````
sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build
````

# Create a new workspace in 'catkin_ws'.
````
mkdir ~/google_ws
cd google_ws
wstool init src
````

# Merge the cartographer_ros.rosinstall file and fetch code for dependencies.
````
wstool merge -t src https://raw.githubusercontent.com/numob/numob_google_cartographer/master/cartographer_ros.rosinstall
wstool update -t src
````

# Install deb dependencies
````
# The command 'sudo rosdep init' will print an error if you have already executed it since installing ROS. 
# This error can be ignored.

sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
````

# Build and install.
````
#the command catkin_make_isolated may fail with error "virtual memory exhausted: Cannot allocate memory".
#Increase your memory to 4G if Linux running on VM, such as VirtualBox or Parallels.

catkin_make_isolated --install --use-ninja

source install_isolated/setup.bash
echo "source ~/google_ws/install_isolated/setup.bash" >> ~/.bashrc

````
