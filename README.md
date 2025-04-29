# Virtualised-Internal-Network-Segmentation-with-VLANs-Using-BookwormPup64-and-OpenWrt

This write-up documents a practical virtualised internal network segmentation project implemented using multiple BookwormPup64 (a lightweight Puppy Linux distribution based on Debian Bookworm) VMs to simulate VLAN-separated departments and a OpenWrt (open wireless router) VM serving as the router, all hosted within VirtualBox.



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

  Now 3 separate BookwormPup64 VMs should be available for use in VirtualBox <br />
  ![image](https://github.com/user-attachments/assets/412a331e-aeba-4af8-bd87-96d7c280123f) <br />



## OpenWrt VM Setup as Router
- Download OpenWrt from [https://downloads.openwrt.org/](https://downloads.openwrt.org/). To prevent unnecessary headaches, the current stable release is selected <br />
  ![image](https://github.com/user-attachments/assets/91a191e4-e0b5-4f44-bb0e-2c75a9447810) <br />

- Clicking on the Stable Version brings us to this web page. Scroll down and click on `x86` <br />
  ![image](https://github.com/user-attachments/assets/b568ac61-cb68-4262-a9d0-b4093eab3d58) <br />

- Choose the `64` <br />
  ![image](https://github.com/user-attachments/assets/613c022d-c783-4f1a-8d99-1f47a1aefcdd) <br />

- The `generic-ext4-combined.img.gz` is chosen and is downloaded. The reasons for this choice is it uses ext4 which is writable and better for testing changes, the combined image includes both kernel + rootfs, and it has no EFI which works with Legacy BIOS mode in VirtualBox (default) <br />
  ![image](https://github.com/user-attachments/assets/a018ea55-01ac-42d8-ae65-6427697e4407) <br />

- Extract the downloaded file to get the `.img` file
- `VBoxManage.exe` is required to convert `.img` to `.vdi`. Open command prompt and run the following command to add VirtualBox to Windows PATH. Now VBoxManage can be used from this terminal session
  ```
  SET PATH=%PATH%;"C:\Program Files\Oracle\VirtualBox"
  ```
  
- To convert the filem run the following command in the terminal. Ensure that the file path is properly adjusted. The output `.vdi` file will be saved in the current folder of the terminal <br />
  ```
  VBoxManage convertfromraw --format VDI openwrt-*-x86-64-combined-*.img openwrt.vdi
  ```
  ![image](https://github.com/user-attachments/assets/e8e2cf4b-ae41-4146-acf1-435252e843d6) <br />

- To install OpenWrt in VirtualBox, click on New and name it OpenWrt. Choose Linux as the type and the version is Other Linux (64-bit) <br />
  ![image](https://github.com/user-attachments/assets/40ae26c5-ade9-4688-a996-a9a55f52d872) <br />

- The Base Memory is set to 512MB, and the number of Processors is 1 <br />
  ![image](https://github.com/user-attachments/assets/2d4fd88b-2edf-4653-bceb-bd83a880abde) <br />

- The previously converted `.vdi` file is then used as the existing virtual hard disk file. This means manually configuring the storage size or requirements is not needed <br />
  ![image](https://github.com/user-attachments/assets/60c68fd8-f391-43bb-bc3a-4ecb0943342b) <br />

- Two adapters are in used by the OpenWrt VM, which is the Bridged Adapter (WAN) and Internal Network (trunk) <br />
  ![image](https://github.com/user-attachments/assets/0d8cadb8-3f8c-41b8-818c-c40759780b1b) <br />
  ![image](https://github.com/user-attachments/assets/f8261c89-ee09-4c58-966c-77b55155adc8) <br />
  The purpose of this dual NIC setup in OpenWrt is to simulate a real world router which connects to 2 different networks, which is the Wide Area Network for internet access and the Local Area Network for internal devices which are isolated from the outside world



## Configuration of VLAN Sub-interfaces
- On each of the BookwormPup64 Department VMs, load the 802.1q Module
  ```
  sudo modprobe 8021q
  ```

- For the HR Department VM, VLAN 10 with the following commands are used
  ```
  sudo ip link add link eth0 name eth0.10 type vlan id 10
  sudo ip addr add 192.168.10.2/24 dev eth0.10
  sudo ip link set up eth0.10
  sudo ip link set down eth0
  ```
  ![image](https://github.com/user-attachments/assets/a30d1e95-d3e1-4646-ac3d-be1b6a757da6)


- For the IT Department VM, VLAN 20 with the following commands are used
  ```
  sudo ip link add link eth0 name eth0.20 type vlan id 20
  sudo ip addr add 192.168.20.2/24 dev eth0.20
  sudo ip link set up eth0.20
  sudo ip link set down eth0
  ```


- For the Finance Department VM, VLAN 30 with the following commands are used
  ```
  sudo ip link add link eth0 name eth0.30 type vlan id 30
  sudo ip addr add 192.168.30.2/24 dev eth0.30
  sudo ip link set up eth0.30
  sudo ip link set down eth0
  ```

  


  




## Testing of Department VMs


