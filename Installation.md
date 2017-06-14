
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
# Running the demo

* Now that Cartographer and Cartographer’s ROS integration are installed, download the example bags (e.g. 2D and 3D backpack collections of the Deutsches Museum) to a known location, in this case ~/Downloads, and use roslaunch to bring up the demo.

* Run roslaunch in Linux Window as it will start rviz.

* The launch files will bring up roscore and rviz automatically.

````
# Download the 2D backpack example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag

# Launch the 2D backpack demo.
roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag

# Download the 3D backpack example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/backpack_3d/b3-2016-04-05-14-14-00.bag

# Launch the 3D backpack demo.
roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=${HOME}/Downloads/b3-2016-04-05-14-14-00.bag

# Download the Revo LDS example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/revo_lds/cartographer_paper_revo_lds.bag

# Launch the Revo LDS demo.
roslaunch cartographer_ros demo_revo_lds.launch bag_filename:=${HOME}/Downloads/cartographer_paper_revo_lds.bag

# Download the PR2 example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/pr2/2011-09-15-08-32-46.bag

# Launch the PR2 demo.
roslaunch cartographer_ros demo_pr2.launch bag_filename:=${HOME}/Downloads/2011-09-15-08-32-46.bag

# Download the Taurob Tracker example bag.
wget -P ~/Downloads https://storage.googleapis.com/cartographer-public-data/bags/taurob_tracker/taurob_tracker_simulation.bag

# Launch the Taurob Tracker demo.
roslaunch cartographer_ros demo_taurob_tracker.launch bag_filename:=${HOME}/Downloads/taurob_tracker_simulation.bag
````
