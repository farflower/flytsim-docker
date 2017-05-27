# FlytSim as a Docker App

This project deals with dockerisation of FlytSim App for easy deployment in any docker supported Linux, Windows and Mac environments.

## Contents

- [What is FlytSim?](#what-is-flytsim)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
    - [Linux](#linux)
    - [Windows](#windows)
    - [MacOS](#macos)
  - [Directory Structure](#directory-structure)
- [Install](#install)
  - [Linux](#linux)
    - [Intel GPU](#intel-gpu)
    - [Nvidia GPU](#nvidia-gpu)
  - [Windows](#windows)
    - [Docker-for-Windows](#docker-for-windows)
    - [Docker Toolbox for Windows [untested]](#docker-toolbox-for-windows-untested)
  - [Mac [Experimental]](#mac-experimental)
    - [Docker-for-Mac](#docker-for-mac)
    - [Docker Toolbox for Mac [No Idea :(]](#docker-toolbox-for-mac-no-idea-)
- [Activate your FlytSim](#activate-your-flytsim)
- [DemoApps](#demoapps)
- [FAQs](#faqs)
- [Versioning](#versioning)
- [Authors](#authors)


## What is FlytSim?

FlytSim offers a 3D SITL(Software In The Loop) simulation environment for testing user apps without the drone hardware. It is a ROS-Gazebo based environment where the drone and its world are simulated, programmatically generating the state variables, while the control algorithms applied are same as onboard the drone.

## Getting Started

This section would deal with pre-requisites of this project, and a brief write up about the directory structure of this repository.

### Prerequisites

#### Linux

Follow the [Docker installation guide](https://docs.docker.com/engine/installation/#supported-platforms) and make sure your flavour of Linux is supported by Docker. If you are running anything apart from Ubuntu, please follow the above guide and install docker in your machine. For Ubuntu users, we have any installtion script which would take care of it, details of which are in [Install](#install) section. It is preferable if you use Ubuntu 16.04 or above, but 14.04 or above might also work. 

#### Windows

Visit installation guide for [Docker-for-Windows](https://docs.docker.com/docker-for-windows/install/) and [Docker Toolbox (legacy)](https://docs.docker.com/toolbox/toolbox_install_windows/) and install either of them, whichever is supported by your Windows OS. Typically 64bit Windows 10 Pro, Enterprise and Education support the newer *Docker-for-Windows*. To know more about it, click [here](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install).

#### MacOS

Visit installation guide for [Docker-for-Mac](https://docs.docker.com/docker-for-mac/install/) and [Docker Toolbox](https://docs.docker.com/toolbox/toolbox_install_mac/) and install either of them, whichever is supported by your Mac OS. Typically OS X El Capitan 10.11 and newer macOS releases support the newer *Docker-for-Mac*. To know more about it, click [here](https://docs.docker.com/docker-for-mac/install/#what-to-know-before-you-install).

### Directory Structure

The repository is divided into 3 directories depending upon the host OS you choose to use. *Linux* is further divided into 2 depending upon the GPU driver running on your machine.

Eventually, all the directories have the following scripts:

**setup** : This script as the name suggests sets up your machine to run FlytSim. You would get more information about it in the [Install](#install) section.

**start** : This script launches a FlytSim docker container for you. It also opens up a tab in your browser connecting to http://localhost/flytconsole. Try waiting for around 30seconds after triggering this script, before manually opening up the link to FlytConsole.

**stop** : As its clear from the name, it stops the FlytSim app and shuts down the docker container, but preserving its environment, which means executing the *start* script again, would start FlytSim app and resume the container session for you.

**openshell** : This script would launch a bash shell inside the FlytSim container. You can execute this script in parallel to launch multiple shells.

**docker-compose.yml** : This is a yml specification of the docker container being started by start script. Please refrain from editing this file, unless you are an expert on Docker.

## Install

These instructions will get FlytSim up and running on your local machine for development and testing of your apps.

Before moving ahead with FlytSim setup, clone this repository. Take a look at [git install guide](https://www.atlassian.com/git/tutorials/install-git) in case you don't have git installed in your machine.

```bash
$ git clone https://github.com/flytbase/flytsim-docker.git
```

### Linux

As mentioned before, FlytSim runs on top of Gazebo, which means it not only simulates the drone but also provides a GUI of the drone along with its environment. For this, docker container running FlytSim needs access to your machine's GPU libraries. For now we only support Intel and Nvidia graphics card powered devices. In future we may add support for AMD too. AMD users could try going through the steps provided for Intel users, in case they have switchable graphics with Intel.

#### Intel GPU

*Warning:* Follow this section only if your device is running on Intel GPU, failure to do so may lead to error while GUI rendering.

1. Open a new terminal, and go to the directory where you have cloned this repository.
2. Get to intelgraphics directory inside linux directory.

```bash
$ cd linux/intelgraphics
```
3. If you are running Ubuntu, execute the setup.sh script which would install [docker](https://docs.docker.com/engine/installation/) and [docker-compose](https://docs.docker.com/compose/install/) on your machine. In case you are using *other flavours of Linux*, please install them manually by following their individual guides.
```bash
$ sudo ./setup.sh
```
4. Once setup is completed (or you have manually installed docker and docker-compose), start a sample docker example to make sure it is indeed setup correctly.

```bash
$ sudo docker run hello-world
```

5. Start your FlytSim session by executing start.sh script. This script would start a docker container running FlytSim app and also open a tab in browser pointing to http://localhost/flytconsole, once it detects a successful launch.

```bash
$ sudo ./start.sh
```

#### Nvidia GPU

*Warning:* Follow this section only if your device is running on Nvidia GPU, failure to do so may lead to error while GUI rendering.

1. Open a new terminal, and go to the directory where you have cloned this repository.
2. Get to nvidiagraphics directory inside linux directory.

```bash
$ cd linux/nvidiagraphics
```
3. Make sure you have installed proprietary Nvidia driver > 340.29. Visit related FAQ section to find how to install Nvidia driver.
4. If you are running Ubuntu, execute the setup.sh script which would install [docker](https://docs.docker.com/engine/installation/), [docker-compose](https://docs.docker.com/compose/install/), [nvidia-docker](https://github.com/NVIDIA/nvidia-docker) and [nvidia-docker-compose](https://github.com/eywalker/nvidia-docker-compose) on your machine. In case you are using *other flavours of Linux*, please install them manually by following individual guides.

```bash
$ sudo ./setup.sh
```
4. Once setup is completed (or you have manually installed docker and docker-compose), start a sample docker example to make sure it is indeed setup correctly.

```bash
$ sudo nvidia-docker run hello-world
```

5. Start your FlytSim session by executing start.sh script. This script would start a docker container running FlytSim app and also open a tab in browser pointing to http://localhost/flytconsole, once it detects a successful launch.

```bash
$ sudo ./start.sh
```
### Windows

#### Docker-for-Windows

1. Make sure you have installed Docker and it is running. (It would be visible in your system's tray icon). An easy test for that would be to start a sample docker app. Run the following command in command prompt or powershell.

```bash
docker run hello-world
```
2. Open up the folder where this repository was cloned. Get inside *Windows* directory, and open *setup.ps1* with Windows PowerShell application. You might have to unblock the file, by opening up its propeties. This setup would install [Xming x-server](http://www.straightrunning.com/XmingNotes/) for rendering FlytSim's GUI. As mentioned before, FlytSim runs on top of Gazebo, which means it not only simulates the drone but also provides a GUI of the drone along with its environment. 

3. Start your FlytSim session by opening start.ps1 script with Windows Powershell application. This script would start a docker container running FlytSim app and also open a tab in Microsoft-Edge browser pointing to http://localhost/flytconsole, once it detects a successful launch.

#### Docker Toolbox for Windows [untested]

1. Make sure you have installed Docker Toolbox.
2. Launch Docker Toolbox from Start menu. This would create a small linux VM, and attach a terminal to it. All the below steps MUST be run only on this terminal.
3. Clone this repository.

```bash
$ git clone https://github.com/flytbase/flytsim-docker.git
```
4. Get to intelgraphics directory inside linux directory.

```bash
$ cd linux/intelgraphics
```

5. Launch a sample docker example to make sure everything it is indeed setup correctly.

```bash
$ docker run hello-world
```

5. Start your FlytSim session by executing start.sh script. This script would start a docker container running FlytSim app and also open a tab in browser pointing to http://localhost/flytconsole, once it detects a successful launch.

```bash
$ sudo ./start.sh
```

### Mac [Experimental]

#### Docker-for-Mac

*Warning:* As mentioned before, FlytSim runs on top of Gazebo, which means it not only simulates the drone but also provides a GUI of the drone along with its environment. For this, docker container running FlytSim needs access to your machine's GPU libraries. Unfortunately, with Docker-for-Mac we could not find a way to do that, which means you won't get to see the GUI.

1. Make sure you have installed Docker and it is running. An easy test for that would be to start a sample docker app. Run the following command in shell/terminal.

```bash
docker run hello-world
```
2. Start your FlytSim session by executing start.sh script. This script would start a docker container running FlytSim app and also open a tab in browser pointing to http://localhost/flytconsole, once it detects a successful launch.

```bash
$ ./start.sh
```

#### Docker Toolbox for Mac [No Idea :(]

## Activate your FlytSim

Now that you have a working FlytSim setup, with [FlytConsole](http://localhost/flytconsole) showing a valid Connection status, activate your FlytSim to enable few critical Navigation APIs. Once FlytConsole performs license validity checks and ask you to reboot, execute the *start* script again.

*Note:* Without activation none of the demoapps would work.

## DemoApps

Once you have got your FlytSim activated, with [FlytConsole](http://localhost/flytconsole) showing a valid Connection status, you are good to go ahead with trying some of our sample apps. Open up a shell inside the container by triggering *openshell* script. Once inside the container, try running [cpp demo app](http://docs.flytbase.com/docs/FlytOS/Developers/BuildingCustomApps/OnboardCPP.html#write-onboard-cpp) or [python demo app](http://docs.flytbase.com/docs/FlytOS/Developers/BuildingCustomApps/OnboardPython.html#write-onboard-python).

## FAQs

* **For docker running on Windows/Mac, how much CPU and RAM should I allocate to Docker**.

You can allocate 2GB RAM for Docker. For CPU, you could begin with allocating 4CPUs, but since it depends on your device's CPU power, the number may vary for different machines. FlytSim is a very power intensive application, and it won't function correctly if not alotted enough resources. To know whether FlytSim is not getting deprived of resources, try opening a shell inside the container using *openshell* script. Once inside run this command:

```bash
$ gz stats
```
This should start printing Gazebo statistics on your shell. A typical output would be:

```bash
Factor[1.00] SimTime[2.23] RealTime[2.26] Paused[F]
Factor[1.00] SimTime[2.44] RealTime[2.46] Paused[F]
```
Make sure the value of your *Factor* is above 0.70 all the time, for smooth functioning of FlytSim. In case it is lower than that try increasing CPU allocation.

<br/>

* **Why does my drone crash after takeoff**?

Typically, this happens when your CPU is not powerful enough to handle FlytSim's computational requirements. Open a shell inside the container using *openshell* script. Once inside run this command:

```bash
$ gz stats
```
This should start printing Gazebo statistics on your shell. A typical output would be:

```bash
Factor[1.00] SimTime[2.23] RealTime[2.26] Paused[F]
Factor[1.00] SimTime[2.44] RealTime[2.46] Paused[F]
```
Make sure the value of your *Factor* is above 0.70 all the time, for smooth functioning of FlytSim. A value lower than that, would result in poor and unreliable performance of FlytSim.

<br/>

* **What can I do if my laptop doesn't have enough power to run FlytSim**?

FlytSim consumes up a lot of CPU resources, in rendering its 3D Gazebo GUI. Turning it off could do wonders for you. Open a shell inside the container using *openshell* script. Once inside run this command:

```bash
$ sudo nano /flyt/flytos/flytcore/share/core_api/scripts/flytsim.cfg
```

Edit this cfg file, and set value of gui_on parameter to "false". Once done, trigger the *start* script again, to restart the container.

Alternatively, you can also setup 'apm' as your backend simulator. Being a non-gazebo, non-GUI based light weight SITL simulator, it might work just fine in your machine. To configure FlytSim to run APM, check the following FAQ.

<br/>

* **I use APM firmware on my drone, and would like to use its simulator. Can you guys plug apm simulator in place of PX4**?

With FlytSim version > 1.3-1, we are launching APM simulation support as well. We have integrated [APM SITL on Linux](http://ardupilot.org/dev/docs/setting-up-sitl-on-linux.html) at the backend, to provide APM users a platform to test their applications. But since this SITL setup is not based on Gazebo, you won't get a GUI with it. To configure FlytSim to launch APM simulator, Open a shell inside the container using *openshell* script. Once inside run this command:

```bash
$ sudo nano /flyt/flytos/flytcore/share/core_api/scripts/flytsim.cfg
```

Edit this cfg file, and set value of simpilot parameter to "apm". Once done, trigger the *start* script again, to restart the container.

<br/>

* **I have a Linux device installed with Nvidia GPU switchable with Intel GPU. How do I know, what is being used**?

There are many ways to find this out. If you are using Ubuntu, go to System Settings -> Details look for Graphics Card details. You can also install `glxinfo` and run the command: `glxinfo | grep OpenGL` to view the GPU being used.

<br/>

* **My Linux device is installed with open source nouveau driver for Nvidia. How do I install Nvidia propreitary drivers?**

If you are on Ubuntu, follow this [nvidia gpu install guide](https://help.ubuntu.com/community/BinaryDriverHowto/Nvidia) by Ubuntu.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/flytbase/flytsim-docker/tags). 

## Authors

* **Sharvashish Das** - *Initial work* - [sharvashish](https://github.com/sharvashish)
