# Virtualised-Internal-Network-Segmentation-with-VLANs-Using-BookwormPup64-and-OpenWrt

This write-up documents a practical virtualised internal network segmentation project implemented using multiple BookwormPup64 (a lightweight Puppy Linux distribution based on Debian Bookworm) VMs to simulate VLAN-separated departments and a OpenWrt (open wireless router) VM serving as the router, all hosted within VirtualBox.



1. [BookwormPup64 VMs Setup as VLANs](#bookwormpup64-vms-setup-as-vlans)
2. [OpenWrt VM Setup as Router](#openwrt-vm-setup-as-router)
3. [Configuration of VLAN Sub-interfaces](#configuration-of-vlan-sub-interfaces)
4. [OpenWrt Router Configuration](#openwrt-router-configuration)
5. [Testing of Department VMs](#testing-of-department-vms)


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
  ```
  ![image](https://github.com/user-attachments/assets/5ac897a5-6b26-44f3-8b01-cdc6aa74e5e1)


- For the IT Department VM, VLAN 20 with the following commands are used
  ```
  sudo ip link add link eth0 name eth0.20 type vlan id 20
  sudo ip addr add 192.168.20.2/24 dev eth0.20
  sudo ip link set up eth0.20
  ```
  


- For the Finance Department VM, VLAN 30 with the following commands are used
  ```
  sudo ip link add link eth0 name eth0.30 type vlan id 30
  sudo ip addr add 192.168.30.2/24 dev eth0.30
  sudo ip link set up eth0.30
  ```
  


## OpenWrt Router Configuration
- Power on the OpenWrt VM. OpenWrt will be configured to act as a router with 802.1q sub-interfaces and firewall rules
- Login using the terminal. The default user is root and there is no password <br />
  ![image](https://github.com/user-attachments/assets/10467422-6045-4a16-baa0-b0e5682b7c1e) <br />

- Install the required packages in OpenWrt
  ```
  opkg update
  opkg install iptables-mod-tproxy iptables-mod-conntrack-extra kmod-ipt-nat kmod-ipt-conntrack kmod-ipt-core kmod-nf-nat kmod-nf-conntrack
  opkg install ip-full iptables tcpdump
  ```
  
- Note that the stable version of OpenWrt used in this project already has `kmod-8021q` integrated into the kernel. From the website link [https://downloads.openwrt.org/releases/24.10.1/targets/x86/64/kmods/6.6.86-1-af351158cfb5febf5155a3aa53785982/](https://downloads.openwrt.org/releases/24.10.1/targets/x86/64/kmods/6.6.86-1-af351158cfb5febf5155a3aa53785982/), `kmod-8021q` cannot be found 

- To check if the 802.1Q VLAN support is present and active, use the command
  ```
  dmesg | grep 8021q
  ```
  
  ![image](https://github.com/user-attachments/assets/3bdbf9df-a0da-49d7-90e4-269483f7491e) <br />

  If internet connection is unavailable and `ping google.com` fails, edit `/etc/config/network` to reflect the correct assignments <br />
  ```
  config interface 'loopback'
    option device 'lo'
    option proto 'static'
    option ipaddr '127.0.0.1'
    option netmask '255.0.0.0'

  config interface 'wan'
      option device 'eth0'
      option proto 'dhcp'
  
  config interface 'lan'
      option device 'eth1'
      option proto 'static'
      option ipaddr '192.168.1.1'
      option netmask '255.255.255.0'
  ```
  ![image](https://github.com/user-attachments/assets/f7449925-ced5-4494-b4ea-db77a138a97e) <br />


- Enable VLAN interfaces by editing `/etc/config/network`
  ```
  config interface 'vlan10'
    option ifname 'eth0.10'
    option proto 'static'
    option ipaddr '192.168.10.1'
    option netmask '255.255.255.0'

  config interface 'vlan20'
      option ifname 'eth0.20'
      option proto 'static'
      option ipaddr '192.168.20.1'
      option netmask '255.255.255.0'
  
  config interface 'vlan30'
      option ifname 'eth0.30'
      option proto 'static'
      option ipaddr '192.168.30.1'
      option netmask '255.255.255.0'
  ```
  ![image](https://github.com/user-attachments/assets/0a202f34-0a50-4526-b973-5c328c114af7)

- Enable VLAN tagging in `/etc/config/network` under physical interface
  ```
  config device
    option name 'eth0.10'
    option type '8021q'
    option ifname 'eth0'
    option vid '10'

  config device
      option name 'eth0.20'
      option type '8021q'
      option ifname 'eth0'
      option vid '20'
  
  config device
      option name 'eth0.30'
      option type '8021q'
      option ifname 'eth0'
      option vid '30'
  ```
  ![image](https://github.com/user-attachments/assets/80ee7fb8-b613-410e-b949-d899dc43ef85)


- To enable IP forwarding (which is usually already enabled), check `/etc/sysctl.conf` or use
  ```
  echo 1 > /proc/sys/net/ipv4/ip_forward
  ```

- Setup the iptables rules by editing or use UCI to apply
  ```
  iptables -P FORWARD DROP

  # Allow intra-VLAN traffic
  iptables -A FORWARD -i eth0.10 -o eth0.10 -j ACCEPT
  iptables -A FORWARD -i eth0.20 -o eth0.20 -j ACCEPT
  iptables -A FORWARD -i eth0.30 -o eth0.30 -j ACCEPT
  
  # IT → HR (SSH)
  iptables -A FORWARD -i eth0.20 -o eth0.10 -p tcp --dport 22 -j ACCEPT
  
  # Finance → IT (RDP)
  iptables -A FORWARD -i eth0.30 -o eth0.20 -p tcp --dport 3389 -j ACCEPT
  ```
  OpenWrt does not use the legacy `state` module by default anymore — it uses `conntrack`, which is a modern replacement
  ```
  # Allow return traffic
  # This does not work
  iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

  # This works
  iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
  ```
  ![image](https://github.com/user-attachments/assets/180e4d50-f329-4475-abcb-80c563acbf37)



- Make it persistent by saving the firewall rules
  ```
  /etc/init.d/firewall reload
  ```



## Testing of Department VMs
- Before testing the department VMs, realism is added to this project by giving internet access to BookwormPup64 VMs via OpenWrt
- To verify or enable IP forwarding in OpenWrt, use
  ```
  cat /proc/sys/net/ipv4/ip_forward
  ```
  If it returns 0, enable it
  ```
  echo 1 > /proc/sys/net/ipv4/ip_forward
  ```
  ![image](https://github.com/user-attachments/assets/b427b8b1-6da5-45e4-8a52-b2327e29e2c0)

- NAT is required to be setup on OpenWrt. Assuming eth0 = Bridged (WAN) and eth1 = Internal trunk (LAN with Puppy Linux), run the following in OpenWrt
  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
  iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
  iptables -A FORWARD -i eth0 -o eth1 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
  ```
  ![image](https://github.com/user-attachments/assets/73e2d3d5-579f-4628-b212-52a17fdbd8b7)

- Test the connection from each department VM by pinging the router gateway
  ```
  ping -c 4 192.168.10.1   # from HR
  ping -c 4 192.168.20.1   # from IT
  ping -c 4 192.168.30.1   # from Finance
  ```
  If the output results show 100% packet loss, try the following commands to create VLAN interfaces and assigning IPs
  ```
  # HR 
  ip link add link eth1 name eth1.10 type vlan id 10
  ip addr add 192.168.10.1/24 dev eth1.10
  ip link set up eth1.10
  
  # IT
  ip link add link eth1 name eth1.20 type vlan id 20
  ip addr add 192.168.20.1/24 dev eth1.20
  ip link set up eth1.20

  # Finance
  ip link add link eth1 name eth1.30 type vlan id 30
  ip addr add 192.168.30.1/24 dev eth1.30
  ip link set up eth1.30
  ```
   

- To test the VLAN isolation, from each VM, use the following commands
  ```
  sudo tcpdump -i eth0.10 -vv     # HR
  sudo tcpdump -i eth0.20 -vv     # IT
  sudo tcpdump -i eth0.30 -vv     # Finance
  ```


- Test the allowed inter-VLAN rules. From IT, SSH to HR (if SSH server on HR is installed)
  ```
  ssh hr_department@192.168.10.2
  ```


- From Finance, RDP to IT (if RDP server on IT is installed)
  ```
  rdesktop 192.168.20.2
  ```






