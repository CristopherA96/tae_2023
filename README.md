# EDA Tools Installation

### Table of contents

-	1. [Ubuntu 18.04+]() 
	-	1.1 [Prerequisites]()
	-	1.2 [Installing pre-compiled apt packages]()
	-	1.3 [Installing Magic VLSI]()
	-	1.4 [Installing the PDK]()
	-	1.5 [Installing OpenLANE]()
	
	
-	2. [References](https://gitlab.com/SaredAbcar/caravel_user_project_joc/-/tree/main/#5-references)
-	3. [Resources](https://gitlab.com/SaredAbcar/caravel_user_project_joc/-/tree/main/#6-resources)
--------
The goal of this guide is to install the open-source EDA tool-set needed to go through some of the physical design flow stages, and correctly configure the Skywater PDK. 
The following apps will be installed:
-	Netgen LVS
-	NGSpice
-	Magic VLSI
-	Docker
-	OpenLANE

## 1. Ubuntu

### 1.1 Prerequisites
In order to install the EDA tool-set within an Ubuntu environment, previously you need to have installed some other required packages. To do so, first update packages database and upgrade the packages to avoid version mismatches:
```sh
sudo apt update
```

```sh
sudo apt upgrade
```
Now we can proceed to install required packages:
- Git
- Make
- Python3
- Python3-venv
- PIP

```
sudo  apt  install  -y  build-essential  python3  python3-venv  python3-pip  make  git
```
### 1.2 Installing pre-compiled apt packages
Since NGSpice and Netgen are available as pre-compiled packages, we can install them using _apt_ too.
```
sudo apt install -y netgen-lvs ngspice ngspice-doc build-essential
```
### 1.3 Installing Magic VLSI
The Skywater PDK requires Magic 8.3.25 or higher, thus we’ll build the latest version from source. First install the required dependencies using _apt_.
-    Dependencies:
       -  M4 preprocessor
       - tcsh shell
       - csh shell
       - xlib.h
       - Tcl/Tk
       - Cairo
       - OpenGL
       - ncurses
```
sudo apt install -y m4 tcsh csh libx11-dev tcl-dev tk-dev libcairo2-dev mesa-common-dev libglu1-mesa-dev
```
In order to organize the directory tree for EDA tool-set, let's create a folder to contain every tool needed for the course. To do so, got to your user /home directory,
```sh
cd ~/
```
	
create the directory `eda_tools` and move into:
```sh
mkdir eda_tools
cd eda_tools
```
Now, clone Magic VLSI git repository and change the working directory to cloned repository.
```sh
git clone git://opencircuitdesign.com/magic
cd magic/
```
Finally, run the configuration script, compile and install magic into the default path `/usr/local/`.
```sh
./configure
```
(**_Note:_** _the next command can take around 2 minutes to finish_)
```sh
make
```
```sh
sudo make install
```
### 1.4 Installing the PDK
After installing the needed tools, we can install the Skywater PDK. Since installing the full PDK is slow and requires a lot of storage, we’ll install a minimal version with the needed cells.

Go to your **_eda_tools_ directory**:
```sh
cd ~/eda_tools
```

First, we have to clone the Skywater repository and initialize the needed submodules.
```sh
git clone https://github.com/google/skywater-pdk
cd skywater-pdk/
```
```sh
git submodule init libraries/sky130_fd_io/latest
git submodule init libraries/sky130_fd_pr/latest
git submodule init libraries/sky130_fd_sc_hd/latest
git submodule update
```
(**_Note:_** _the next command can take around 15 minutes to finish_)
```sh
make timing
```

Once the previous commands have finished their execution, go to your **_eda_tools_ directory**:
```sh
cd ~/eda_tools
```
Now, clone the Open PDKs repo to configure and install the repository. Since we already cloned the Skywaater PDK repo, we’ll indicate the configuration script where to find it, instead of pulling the full repo.

```sh
git clone git://opencircuitdesign.com/open_pdks
cd open_pdks/
```
```sh
./configure --enable-sky130-pdk=~/eda_tools/skywater-pdk
```
(**_Note:_** _the next command can take around 1 hour to finish_)
```sh
make
```
```sh
sudo make install
```

### Adding env variables to .bashrc
Copy and paste the following commands to add the environment variables to your .bashrc file.
_(You can either do it manually from any text editor or executing the following.)_

```
echo "export PDK_ROOT=\"/usr/local/share/pdk\"" >> ~/.bashrc
echo "export PDK_PATH=\"\$PDK_ROOT/sky130A\"" >> ~/.bashrc
echo "alias magicsky=\"magic -T \$PDK_PATH/libs.tech/magic/sky130A.tech\"" >> ~/.bashrc
source ~/.bashrc
```
Now you can use `magicsky` to run magic with the PDK tech file.


We can clean some generated files.
(**_Note:_** If your installation presents any problem, it is suggested to not execute the following)
```
make distclean
```
If desired, the installation repos can also be removed to free additional storage. (optional)

```
cd ~/eda_tools
rm -rf magic/
```
### 1.5 Installing OpenLANE
####  Docker
For ease of installation, OpenLane uses Docker images.
1. First, uninstall any such older versions before attempting to install a new version:
	```sh
	sudo apt-get remove docker docker-engine docker.io containerd runc
	```
2. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:
	```sh
	sudo apt-get update
	sudo apt-get install ca-certificates curl gnupg
	```
3. Add Docker’s official GPG key:
	```sh
	sudo mkdir -m 0755 -p /etc/apt/keyrings
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
	```
4. Use the following command to set up the repository:
	```sh
	echo \
	  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
	  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
	  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	```
5.  Update the `apt` package index:
	```sh
	sudo apt-get update
	```
6. Install Docker Engine, containerd, and Docker Compose.
	```sh
	sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	```
7. Verify that the Docker Engine installation is successful by running the `hello-world` image:
	```sh
	sudo docker run hello-world
	```
	_(A succesfull instalation of Docker would have the following ouput)_
	```
	Hello from Docker!
	This message shows that your installation appears to be working correctly.

	To generate this message, Docker took the following steps:
	1. The Docker client contacted the Docker daemon.
	2. The Docker daemon pulled the "hello-world" image from the Docker Hub. (amd64)
	3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
	4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

	To try something more ambitious, you can run an Ubuntu container with:
	$ docker run -it ubuntu bash

	Share images, automate workflows, and more with a free Docker ID:
	https://hub.docker.com/

	For more examples and ideas, visit:
	https://docs.docker.com/get-started/
	```

##### Manage Docker as a non-root user
1. Create the `docker` group.
	```sh
	sudo groupadd docker
	```
2. Add your user to the `docker` group.
	```sh
	sudo usermod -aG docker $USER
	```
3. Activate the changes to groups.
	```sh
	newgrp docker
	```
4. Verify that you can run `docker` commands without `sudo`.
	```sh
	docker run hello-world
	```
#### Checking Installation Requirements
In order to check the installation, you can use the following commands:
```sh
git --version
docker --version
python3 --version
python3 -m pip --version
make --version
python3 -m venv -h
```
Successful output will look like this:
```
git version 2.36.1
Docker version 20.10.16, build aa7e414fdc
Python 3.10.5
pip 21.0 from /usr/lib/python3.10/site-packages/pip (python 3.10)
GNU Make 4.3
Built for x86_64-pc-linux-gnu
Copyright (C) 1988-2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
usage: venv [-h] [--system-site-packages] [--symlinks | --copies] [--clear]
            [--upgrade] [--without-pip] [--prompt PROMPT] [--upgrade-deps]
            ENV_DIR [ENV_DIR ...]

Creates virtual Python environments in one or more target directories.
...
Once an environment has been created, you may wish to activate it, e.g. by
sourcing an activate script in its bin directory.
```
#### Download and Install OpenLane

1. Download OpenLane from GitHub and change the working directory to cloned repository.
	```sh
	git clone https://github.com/The-OpenROAD-Project/OpenLane.git
	cd OpenLane
	```
2. Execute the following.
	```sh
	sudo make pull-openlane
	```
3. Copy and paste the following commands to add the environment variables to your .bashrc file.
_(You can either do it manually from any text editor or executing the following.)_

	```sh
	echo "export OPENLANE_ROOT=\"~/eda_tools/OpenLane\"" >> ~/.bashrc
	echo "export PDK_ROOT=\"\$OPENLANE_ROOT/sky130A\"" >> ~/.bashrc
	echo "export PDK_PATH=\"\$PDK_ROOT/sky130A\"" >> ~/.bashrc
	source ~/.bashrc
	```
4. Build OpenLane and sky130 PDK.
	```sh
	make pdk  
	make openlane
	```
5. Finally, run a ~5 minute test that verifies that the flow and the PDK were properly installed.
	```sh
	make test
	```




## 2. References
- OpenLANE Official Documentation: [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/reference/index.html)

## 3. Resources

- config.tcl Variables description [OpenLane config.tcl Variables](https://openlane.readthedocs.io/en/rtfd_fix/configuration/README.html)
- Open_PDKs - open_pdks Download : [open_pdks](http://opencircuitdesign.com/open_pdks/)
- Open_PDKs - skywater-pdk Install : [skywater-pdk_installation](http://opencircuitdesign.com/open_pdks/)

