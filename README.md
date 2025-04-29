# Virtualised-Internal-Network-Segmentation-with-VLANs-Using-Debian-Netinst-and-OpenWrt

This write-up documents a practical virtualised internal network segmentation project implemented using multiple Debian Netinst VMs to simulate VLAN-separated departments and a OpenWrt VM serving as the router, all hosted within VirtualBox.



1. [Debian Netinst VMs Setup as VLANs](#debian-netinst-vms-setup-as-vlans)
2. [OpenWrt VM Setup as Router](#openwrt-vm-setup-as-router)
3. [Configuration of VLAN Sub-interfaces](#configuration-of-vlan-sub-interfaces)
4. [Testing of Department VMs](#testing-of-department-vms)


## Debian Netinst VMs Setup as VLANs
- For this project, 3 Debian Netinst VMs will each represent 3 departments: HR, IT and Finance. To simplify the setup process, a template Debian Netinst VM will first be installed, and it will later be cloned into 2 more VMs
- Download the Debian Netinst ISO image suitable for VirtualBox from [https://www.debian.org/CD/netinst/#netinst-stable](https://www.debian.org/CD/netinst/#netinst-stable). As shown in the cropped screenshot below, choose the `amd64` version <br />
  ![image](https://github.com/user-attachments/assets/62a6472c-a7e8-40ed-8092-7f08e8bc2440) <br />

- In VirtualBox, add a new VM, name it and select the correct ISO image <br />
  ![image](https://github.com/user-attachments/assets/248451e9-b5f8-4785-b478-88f19fdcd54d) <br />

- Since Alpine Linux is lightweight, 512MB for its Base Memory and 1 CPU is enough <br />
  ![image](https://github.com/user-attachments/assets/e353bc62-5667-4778-ab74-100717573619) <br />

- A disk size of 1GB is enough for the sake of this project <br />
  ![image](https://github.com/user-attachments/assets/ecfb6b50-31d4-4e97-a6ef-be7ee22c5417) <br />

- The summary would appear as the following. Finish the setup and start the VM <br />
  ![image](https://github.com/user-attachments/assets/73f30e95-8cc3-4b74-a8eb-60ce52b583b7) <br />

- Once the template VM is powered on, login as localhost <br />
  ![image](https://github.com/user-attachments/assets/ba0d3d01-840e-4f8c-9db9-91e2ba4d8fbd) <br />

- Then run the following commands to update and install tooling <br />
  ```
  apk update && apk upgrade
  apk add vlan bridge-utils iproute2 iptables tcpdump
  ```

- 




  



  


## OpenWrt VM Setup as Router



## Configuration of VLAN Sub-interfaces



## Testing of Department VMs

