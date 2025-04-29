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

  Clicking on the Main link brings us to this web page. Version 10.0.10 is selected <br />
  ![image](https://github.com/user-attachments/assets/f6216f35-641c-483a-a007-24a1086874c1) <br />

  Download the BookwormPup64_10.0.10.iso image <br />
  ![image](https://github.com/user-attachments/assets/057b7935-81f1-4275-8a44-2990429a0134) <br />

- In VirtualBox, add a new VM, name it and select the correct ISO image <br />
  ![image](https://github.com/user-attachments/assets/ad1e93f4-9898-4757-858a-5bb63824b20c) <br />

- Since BookwormPup64 is lightweight, 1024MB for its Base Memory and 1 CPU is enough <br />
  ![image](https://github.com/user-attachments/assets/24436469-d448-4336-be2f-c590be05dc02) <br />

- A disk size of 3GB is enough for the sake of this project <br />
  ![image](https://github.com/user-attachments/assets/44bfa875-f7f5-46da-865d-21668b5de77c) <br />

- The summary would appear as the following. Finish the setup and start the VM <br />
  ![image](https://github.com/user-attachments/assets/20cb6881-3a6c-41aa-9ef9-95bf5193f341) <br />

- Once the template VM is powered on, run the following commands to update and install tooling <br />
  ```
  sudo apt update && sudo apt install vlan iproute2 iptables tcpdump -y
  ```
  ![image](https://github.com/user-attachments/assets/009eac05-a3bc-4f89-8a79-d04e816743b4)

- Power off the VM and attach it to Internal Network. Set the name as `trunk` <br />
  ![image](https://github.com/user-attachments/assets/bb5961d6-8269-4022-9c5c-82412e31820d) <br />

- The BookwormPup64 VM is cloned into `BookwormPup64 HR`, `BookwormPup64 IT` and `BookwormPup64 Finance` with the following settings <br />
  ![image](https://github.com/user-attachments/assets/f56a296b-99e1-4df6-8022-e4632edcddc6) <br />



## OpenWrt VM Setup as Router
- Download OpenWrt from [https://downloads.openwrt.org/](https://downloads.openwrt.org/). To prevent unnecessary headaches, the current stable release is selected <br />
  ![image](https://github.com/user-attachments/assets/91a191e4-e0b5-4f44-bb0e-2c75a9447810) <br />

- Clicking on the Stable Version brings us to this web page. Scroll down and click on `x86` <br />
  ![image](https://github.com/user-attachments/assets/b568ac61-cb68-4262-a9d0-b4093eab3d58) <br />

- Choose the `64` <br />
  ![image](https://github.com/user-attachments/assets/613c022d-c783-4f1a-8d99-1f47a1aefcdd) <br />

- The `generic-ext4-combined.img.gz` is chosen and is downloaded. The reasons for this choice is it uses ext4 which is writable and better for testing changes, the combined image includes both kernel + rootfs, and it has no EFI which works with Legacy BIOS mode in VirtualBox (default) <br />
  ![image](https://github.com/user-attachments/assets/a018ea55-01ac-42d8-ae65-6427697e4407) <br />

- 




## Configuration of VLAN Sub-interfaces





## Testing of Department VMs


