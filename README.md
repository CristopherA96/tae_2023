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


## 2. References
- OpenLANE Official Documentation: [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/reference/index.html)

## 3. Resources

- config.tcl Variables description [OpenLane config.tcl Variables](https://openlane.readthedocs.io/en/rtfd_fix/configuration/README.html)
- Open_PDKs - open_pdks Download : [open_pdks](http://opencircuitdesign.com/open_pdks/)
- Open_PDKs - skywater-pdk Install : [skywater-pdk_installation](http://opencircuitdesign.com/open_pdks/)


