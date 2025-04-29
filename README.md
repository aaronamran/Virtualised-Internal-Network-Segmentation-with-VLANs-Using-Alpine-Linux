# Virtualised-Internal-Network-Segmentation-with-VLANs-Using-BookwormPup64-and-OpenWrt

This write-up documents a practical virtualised internal network segmentation project implemented using multiple BookwormPup64 (a lightweight Puppy Linux distribution based on Debian Bookworm) VMs to simulate VLAN-separated departments and a OpenWrt VM serving as the router, all hosted within VirtualBox.



1. [BookwormPup64 VMs Setup as VLANs](#bookwormpup64-vms-setup-as-vlans)
2. [OpenWrt VM Setup as Router](#openwrt-vm-setup-as-router)
3. [Configuration of VLAN Sub-interfaces](#configuration-of-vlan-sub-interfaces)
4. [Testing of Department VMs](#testing-of-department-vms)


## BookwormPup64 VMs Setup as VLANs
- For this project, 3 BookwormPup64 VMs will each represent 3 departments: HR, IT and Finance. To simplify the setup process, a template BookwormPup64 VM will first be installed, and it will later be cloned into 2 more VMs
- Download the BookwormPup64 ISO image suitable for VirtualBox from [https://puppylinux-woof-ce.github.io/](https://puppylinux-woof-ce.github.io/). As shown in the cropped screenshot below, choose the `BookwormPup64 10.0` version <br />
  ![image](https://github.com/user-attachments/assets/705b2d92-7f00-4a94-b84f-6bb9ba965360) <br />

- In VirtualBox, add a new VM, name it and select the correct ISO image <br />
  ![image](https://github.com/user-attachments/assets/457a44e3-5684-4e88-b797-a72b04ad8172) <br />

- Since BookwormPup64 is lightweight, 1024MB for its Base Memory and 1 CPU is enough <br />
  ![image](https://github.com/user-attachments/assets/5a4018c5-8bb0-40f7-b388-94c3b1873cee) <br />

- A disk size of 3GB is enough for the sake of this project <br />
  ![image](https://github.com/user-attachments/assets/a44519ec-c01a-4bd3-ad2b-42135dc264a9) <br />

- The summary would appear as the following. Finish the setup and start the VM <br />
  ![image](https://github.com/user-attachments/assets/d566cf7c-0f2c-4f3d-aedd-da807cb4fe8c) <br />

- Once the template VM is powered on, run the initial setup by choosing the default options <br />
  ![image](https://github.com/user-attachments/assets/62ff3fdf-4258-4140-96d1-edb680addf97) <br />

- Then run the following commands to update and install tooling <br />
  ```
  sudo apt update && sudo apt upgrade -y
  sudo apt install vlan bridge-utils iproute2 iptables tcpdump -y
  ```

- 




  



  


## OpenWrt VM Setup as Router



## Configuration of VLAN Sub-interfaces



## Testing of Department VMs

