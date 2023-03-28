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
The goal of this guide is to install the open-source EDA Tool set needed to go trhoufh the VLSI design flow, and configure correctly the Skywater PDK. 
The following apps will be installed:
-	1. Magic VLSI
-	2. Netgen LVS
-	3. NGSpice

## 1. Ubuntu

### 1.1 Prerequisites
In order to install the EDA tool set, previously you need to have installed some other required packages. To do so, first update packages database and upgrade the packages to avoid version mismatches:
```sh
sudo apt update
```

```sh
sudo apt upgrade
```

## 5. References
- OpenLANE Official Documentation: [OpenLane Documentation](https://openlane.readthedocs.io/en/latest/reference/index.html)

## 6. Resources

- config.tcl Variables description [OpenLane config.tcl Variables](https://openlane.readthedocs.io/en/rtfd_fix/configuration/README.html)
- Open_PDKs - open_pdks Download : [open_pdks](http://opencircuitdesign.com/open_pdks/)
- Open_PDKs - skywater-pdk Install : [skywater-pdk_installation](http://opencircuitdesign.com/open_pdks/)

