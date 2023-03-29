# EDA Tools Installation

--------

### Table of contents

- 1. [Ubuntu 18.04+](#1-ubuntu-1804+) 
  - 1.1 [Prerequisites](#11-prerequisites)
  - 1.2 [Install pre-compiled apt packages](#12-install-pre-compiled-apt-packages)
  - 1.3 [Install the PDK](#14-install-the-pdk)
  - 1.4 [Install Magic VLSI](#13-install-magic-vlsi)
  - 1.6 [Install OpenLANE](#)
- 2. [Fedora/Other GNU/Linux Distro](#2-fedora/other-gn/linux-distro)
  - 2.1 [Prerequisites](#21-prerequisites)
  - 2.2 [Install NetGen LVS and NGSpice](#22-installing-pre-compiled-apt-packages)
  - 2.3 [Install the PDK](#23-installing-the-pdk)
  - 2.4 [Install Magic VLSI](#24-installing-magic-vlsi)
  - 2.5 [Install OpenLANE](#25-installing)
  
  - 2.2 [Install EDA tools from source code](#install-eda-tools-source-code)
- 3. [References](https://gitlab.com/SaredAbcar/caravel_user_project_joc/-/tree/main/#5-references)

- 4. [Resources](https://gitlab.com/SaredAbcar/caravel_user_project_joc/-/tree/main/#6-resources)

--------

The goal of this guide is to install the open-source EDA tool-set needed to go through some of the physical design flow stages, and correctly configure the Skywater PDK. 

The following packages will be installed:

- Netgen LVS
- NGSpice
- Skywater 130nm PDK
- Magic VLSI
- Klayout
- OpenLANE

## 1. Ubuntu 18.04+

### 1.1 Prerequisites
--------

In order to install the EDA tool-set within Ubuntu, previously you need to have installed some other required packages. To do so, first update and upgrade packages database and upgrade the packages to avoid version mismatches:

```sh
sudo apt update && sudo apt upgrade
```

Now we can proceed to install required packages:

- git 2.35+
- docker 19.03.12+
- python 3.6* with pip and venv
- GNU Make
- Ruby

```sh
sudo  apt  install  -y  build-essential  python3  python3-venv  python3-pip  make  git ruby-full
```

### 1.2 Install pre-compiled apt packages
--------

Since NGSpice, Netgen and Klayout are available as pre-compiled packages, we can install them using _apt_ too.

```sh
sudo apt install -y netgen-lvs ngspice ngspice-doc klayout build-essential
```

There is another way to install packages, by using source code.

### 1.3 Pre-setup
--------

In order to organize the directory tree for EDA tool-set, let's create a folder to contain every tool needed for the course. To do so, got to your user `/home` directory.

```sh
cd ~/
```

Create the directory `eda_tools` and move into:

```sh
mkdir eda_tools
cd eda_tools
```

(**_WARNING:_** _we will work over **_eda_tools_** folder all the time._)


### 1.4 Install Magic VLSI
--------

The Skywater PDK requires Magic 8.3.25 or higher, thus we will build the latest version from source. 

First install the required dependencies using _apt_:

- M4 preprocessor
- tcsh shell
- csh shell
- xlib.h
- Tcl/Tk
- Cairo
- OpenGL
- ncurses
    
```sh
cd ~/

sudo apt install -y m4 tcsh csh libx11-dev tcl-dev tk-dev libcairo2-dev mesa-common-dev libglu1-mesa-dev
```

Now, clone Magic VLSI git repository and change the working directory to cloned repository.

```sh
cd ~/eda_tools

git clone git://opencircuitdesign.com/magic
cd magic/
```

Finally, run the configuration script, compile and install magic into the default path `/usr/local/`.

```sh
./configure

make
sudo make install
```

(**_Note:_** _by default the installation path is `/usr/local/`, you can change by adding `--prefix=<path>` to configure. It takes some time to finish._)

### 1.5 Install Klayout
--------

Klayout is an editor that allows to change GDS and OASIS files and create them from scratch.

To install Klayout you should have some dependencies installed.

- Qt 4.7+
- gcc 4.6+ or clang 3.8+
- Ruby 

(**_Note:_** _some of these dependecies were already installed._)

This package should be install from <u>source code</u>. To do so, first download the most recent `.deb` file and run:


```sh
cd ~/

sudo apt install klayout_0.28.6-1_amd64.deb
```

### 1.6 Installing OpenLANE
--------

OpenLane requires Docker images, so first of all you shall have installed docker.

#### Docker

To have a more clean installation of docker, do the next steps.

1. First, uninstall any such older versions before attempting to install a new version:
   
   ```sh
   cd ~/

   sudo apt remove docker docker-engine docker.io containerd runc
   ```

2. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:
   
   ```sh
   sudo apt update && sudo apt upgrade
   sudo apt install ca-certificates curl gnupg
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

5. Update the `apt` package index:
   
   ```sh
   sudo apt update
   ```

6. Install Docker Engine, containerd, and Docker Compose.
   
   ```sh
   sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

7. Manage Docker as a non-root user

   Create the `docker` group.
   
   ```sh
   sudo groupadd docker
   ```
   
   Add your user to the `docker` group.
   
   ```sh
   sudo usermod -aG docker $USER
   ```
   Activate the changes to groups.
   
   ```sh
   newgrp docker
   ```

8. Verify that the Docker Engine installation is successful by running the `hello-world` image:
   
   ```sh
   docker run hello-world
   ```
   
    _(A succesfull instalation of Docker would have the following ouput)_
   
   ```sh
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

#### OpenLane

OpenLane also requires its own dependencies, some of them were installed previously, but verify them for sanity checks.

1. Checking Installation Requirements
   
   In order to check the installation, you can use the following commands:
   
   ```sh
   git --version
   docker --version
   python3 --version
   python3 -m pip --version
   make --version
   python3 -m venv -h
   ```

2. Download OpenLane from GitHub and change the working directory to cloned repository.
    
   ```sh
   cd ~/eda_tools

   git clone https://github.com/The-OpenROAD-Project/OpenLane.git
   cd OpenLane
   ```

3. Execute the following.
   ```sh
   sudo make
   ```

(**_Note:_** _there is another way to install the full pdk by using the process `make pdk` after `sudo make` within OpenLane repository._)



### 1.7 Install PDK
--------

After installing the needed tools, we can install the Skywater PDK. Since installing the full PDK is slow and requires a lot of storage, we’ll install a minimal version with the needed cells.

First, we have to clone the Skywater repository and initialize the needed submodules.

```sh
cd ~/eda_tools

git clone https://github.com/google/skywater-pdk.git
cd skywater-pdk/

git submodule init libraries/sky130_fd_io/latest
git submodule init libraries/sky130_fd_pr/latest
git submodule init libraries/sky130_fd_sc_hd/latest
git submodule update
```

(**_Note:_** _the next command can take around 15 minutes to finish._)

```sh
make timing
```

Once the previous commands have finished their execution, go to your **_eda_tools_ directory**:

```sh
cd ~/eda_tools
```

Now, clone the Open PDKs repo to configure and install the repository. Since we already cloned the Skywater PDK repo, we will indicate the configuration script where to find it, instead of pulling the full repo.

```sh
git clone https://github.com/RTimothyEdwards/open_pdks.git
cd open_pdks/
```

```sh
./configure --enable-sky130-pdk=~/eda_tools/skywater-pdk

make
sudo make install
```

(**_Note:_** _by default the installation path is `/usr/local/share/pdk`, you can change by adding `--prefix=<path>`. Remember `/opt and /usr/local` are paths where you can save your external packages installation. It takes some time to finish._)


#### Adding env variables to .bashrc

The final step is to add environment variables to the .bashrc file, in order to use it in your flows. 

(**_Note:_** _You can add them either manually from any text editor or executing the following._)

**OpenLane env variables**

```sh
echo "export OPENLANE_ROOT=\"~/eda_tools/OpenLane\"" >> ~/.bashrc
```

**PDK env variables**

```sh
echo "export PDK_ROOT=\"/usr/local/share/pdk\"" >> ~/.bashrc
echo "export PDK_PATH=\"\$PDK_ROOT/sky130A\"" >> ~/.bashrc
```

**Extra env variable**

You can use an alias called `magicsky` to run magic with the PDK tech file. Also, to use klayout create an alias called `klayoutsky`.

```sh
echo "alias magicsky=\"magic -T \$PDK_PATH/libs.tech/magic/sky130A.tech\"" >> ~/.bashrc
echo "alias klayoutsky=\"klayout -nn \$PDK_PATH/libs.tech/magic/sky130A.tech\"" >> ~/.bashrc
```

Once, all environment variables were added, load env vars to the system, to do so, run:

```sh
source ~/.bashrc
```

#### Freeing up disk space (Optional)

If desired, the installation repos can also be removed to free additional storage.

```sh
cd ~/eda_tools
rm -rf magic/
rm ~/Downloads/klayout_0.28.6-1_amd64.deb
```

(**_Note:_** _We do not remove OpenLane repo because it is constantly updating._)


## 2. Fedora

### 2.1 Prerequisites

To install the required EDA tools in other non-debian based distros, we first check which is our package manager installed.
```sh
uname -a
```

Or, if you have the `lsb_release` package installed.

```sh
lsb_release -a
```

Fedora uses `dnf` as a package manager.

Knowing our package manager go to check if you have already installed the next packages.

- git 2.35+
- docker 19.03.12+
- python 3.6* with pip and venv
- GNU Make
- Ruby

```sh
git --version
docker --version
python3 --version
python3 -m pip3 --version
make --version
```

If you do not have one of them, install it, by running:

```sh
sudo dnf install <pkg>
```

### 2.2 Pre-setup
--------

In order to organize the directory tree for EDA tool-set, let's create a folder to contain every tool needed for the course. To do so, got to your user `/home` directory.

```sh
cd ~/
```

Create the directory `eda_tools` and move into:

```sh
mkdir eda_tools
cd eda_tools
```

(**_Note:_** _in other linux distros it is recommended to install packages from <u>source code</u>, because there are some packages that are not into databases of your distro._)

### 2.3 Install NetGen LVS and NGSpice
--------

Netgen LVS is a netlist generator use to compare Layout vs Schematic.

```sh
cd ~/eda_tools

git clone https://github.com/RTimothyEdwards/netgen.git
cd netgen/

./configure --prefix=/opt/netgen
make 
sudo make install
```

NGSpice is an open source simulatori for electric and electronic circuits.

To install NGSpice from source code we keep in mind that the repository is under development, so in this case is better to install an stable version, as we see below.


### 2.4 Install Magic VLSI
--------

Magic is a VLSI layout tool to do IC mask design.

To have a good complement with skywater pdk and magic, install the required dependencies before.

```sh
sudo dnf install m4 tcsh csh libx11-dev tcl-dev tk-dev libcairo2-dev mesa-common-dev libglu1-mesa-dev
```

Then, install magic from <u>source code</u>

```sh
cd ~/eda_tools

git clone https://github.com/RTimothyEdwards/magic.git
cd magic/

./configure
make 
sudo make install
```

### 2.5 Install Klayout
--------

Klayout is an editor that allows to change GDS and OASIS files and create them from scratch.

To install Klayout you should have some dependencies installed.

- Qt 4.7+
- gcc 4.6+ or clang 3.8+
- Ruby 

From <u>source code</u>

```sh
cd ~/eda_tools

git clone https://github.com/KLayout/klayout.git

cd klayout/
./build.sh
```


### 2.6 OpenLane

OpenLane is an open source tool to automate RTL to GDSII flow.

To install OpenLane is by using <u>source code</u>. OpenLane will not be install in your PC, instead it uses a docker container.

(**_Note:_** _you should have installed the prerequisites before to install OpenLane._)

#### Docker

To have a more clean installation of docker, do the next steps.

1. First, uninstall any such older versions before attempting to install a new version:
   
   ```sh
   cd ~/

   sudo apt remove docker docker-engine docker.io containerd runc
   ```

2. Update the `dnf` package index and install packages to allow `dnf` to use a repository over HTTPS:
   
   ```sh
   sudo dnf update && sudo dnf upgrade
   sudo dnf install ca-certificates curl gnupg
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

5. Update the `dnf` package index:
   
   ```sh
   sudo dnf update
   ```

6. Install Docker Engine, containerd, and Docker Compose.
   
   ```sh
   sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

7. Manage Docker as a non-root user

   Create the `docker` group.
   
   ```sh
   sudo groupadd docker
   ```
   
   Add your user to the `docker` group.
   
   ```sh
   sudo usermod -aG docker $USER
   ```
   Activate the changes to groups.
   
   ```sh
   newgrp docker
   ```

8. Verify that the Docker Engine installation is successful by running the `hello-world` image:
   
   ```sh
   docker run hello-world
   ```
   
    _(A succesfull instalation of Docker would have the following ouput)_
   
   ```sh
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

#### OpenLane

OpenLane also requires its own dependencies, some of them were installed previously, but verify them for sanity checks.

1. Checking Installation Requirements
   
   In order to check the installation, you can use the following commands:
   
   ```sh
   git --version
   docker --version
   python3 --version
   python3 -m pip --version
   make --version
   python3 -m venv -h
   ```

2. Download OpenLane from GitHub and change the working directory to cloned repository.
    
   ```sh
   cd ~/eda_tools

   git clone https://github.com/The-OpenROAD-Project/OpenLane.git
   cd OpenLane
   ```

3. Execute the following.
   ```sh
   sudo make
   ```

(**_Note:_** _there is another way to install the full pdk by using the process `make pdk` after `sudo make` within OpenLane repository._)


### 2.7 Install PDK

The PDK is the Process Design Kit where you have information about all standard cells.

First, we have to clone the Skywater repository and initialize the needed submodules.

```sh
cd ~/eda_tools

git clone https://github.com/google/skywater-pdk.git
cd skywater-pdk/

git submodule init libraries/sky130_fd_io/latest
git submodule init libraries/sky130_fd_pr/latest
git submodule init libraries/sky130_fd_sc_hd/latest
git submodule update
```

(**_Note:_** _the next command can take around 15 minutes to finish._)

```sh
make timing
```

Once the previous commands have finished their execution, go to your **_eda_tools_ directory**:

```sh
cd ~/eda_tools
```

Now, clone the Open PDKs repo to configure and install the repository. Since we already cloned the Skywater PDK repo, we will indicate the configuration script where to find it, instead of pulling the full repo.

```sh
git clone https://github.com/RTimothyEdwards/open_pdks.git
cd open_pdks/
```

```sh
./configure --enable-sky130-pdk=~/eda_tools/skywater-pdk

make
sudo make install
```

(**_Note:_** _by default the installation path is `/usr/local/share/pdk`, you can change by adding `--prefix=<path>`. Remember `/opt and /usr/local` are paths where you can save your external packages installation. It takes some time to finish._)


#### Adding env variables to .bashrc

The final step is to add environment variables to the .bashrc file, in order to use it in your flows. 

(**_Note:_** _You can add them either manually from any text editor or executing the following._)

**OpenLane env variables**

```sh
echo "export OPENLANE_ROOT=\"~/eda_tools/OpenLane\"" >> ~/.bashrc
```

**PDK env variables**

```sh
echo "export PDK_ROOT=\"/usr/local/share/pdk\"" >> ~/.bashrc
echo "export PDK_PATH=\"\$PDK_ROOT/sky130A\"" >> ~/.bashrc
```

**Extra env variable**

You can use an alias called `magicsky` to run magic with the PDK tech file. Also, to use klayout create an alias called `klayoutsky`.

```sh
echo "alias magicsky=\"magic -T \$PDK_PATH/libs.tech/magic/sky130A.tech\"" >> ~/.bashrc
echo "alias klayoutsky=\"klayout -nn \$PDK_PATH/libs.tech/magic/sky130A.tech\"" >> ~/.bashrc
```

Once, all environment variables were added, load env vars to the system, to do so, run:

```sh
source ~/.bashrc
```

#### Freeing up disk space (Optional)

If desired, the installation repos can also be removed to free additional storage.

```sh
cd ~/eda_tools
rm -rf magic/
rm ~/Downloads/klayout_0.28.6-1_amd64.deb
```


## 3. Tests



## 4. References

- OpenLANE Official Documentation: [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/reference/index.html)

## 5. Resources

- config.tcl Variables description [OpenLane config.tcl Variables](https://openlane.readthedocs.io/en/rtfd_fix/configuration/README.html)
- Open_PDKs - open_pdks Download : [open_pdks](http://opencircuitdesign.com/open_pdks/)
- Open_PDKs - skywater-pdk Install : [skywater-pdk_installation](http://opencircuitdesign.com/open_pdks/)
