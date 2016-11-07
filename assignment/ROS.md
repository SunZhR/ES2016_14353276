# ROS
***
### What is ROS
The Robot Operating System (ROS) is a set of software libraries and tools that help you build robot applications. 

### Installation
 1. Configure your Ubuntu repositories
 2. Setup your sources.list
 
    >sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
 3. Set up your keys
 
    >sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
 4. Installation
 
    >sudo apt-get update    
    >sudo apt-get install ros-jade-desktop-full
 5. Initialize rosdep
 
    >sudo rosdep init   
    >rosdep update
 6. Environment setup
 
    >echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
    >source ~/.bashrc
 7. Getting rosinstall
 
    >sudo apt-get install python-rosinstall
