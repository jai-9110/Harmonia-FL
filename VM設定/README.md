# VM設定
* image : ubuntu-20.04.1-live-server-amd64.iso
  <https://ftp.ntou.edu.tw/ubuntu-releases/focal/>
* 網卡1(enp0s3) : NAT 、 網卡2(enp0s8) : HostOnly       // 事先將主機網路管理員中的DHCP關閉
* Master : 100GB-Disk, 8192RAM, 4cpus 
1. 新增host only的網卡
![image](https://github.com/jai-9110/Harmonia-FL/blob/31567292b06222cd63eaa73c10aa09a6861608ff/picture/%E4%B8%BB%E6%A9%9F%E7%B6%B2%E8%B7%AF%E7%AE%A1%E7%90%86%E5%93%A1.png)
2. 設定VM網路
```
$ sudo apt-get update
$ sudo apt install ifupdown
$ sudo nano /etc/network/interfaces
```
```
# NAT Network
auto enp0s3
iface enp0s3 inet dhcp

# Host Only Network
auto enp0s8
iface enp0s8 inet static
address 192.168.97.10
netmask 255.255.255.0 
```
> 設定ip (VirtualBox設定)  
> 根據Host-Only的網段作設定  
> (若是多台VM共用此網卡，可在192.168.97.1之後接0、1、2...，以此類推)
```
$ sudo ifdown enp0s3
$ sudo ifdown enp0s8
$ sudo ifup enp0s3
$ sudo ifup enp0s8
$ ip a s  // check IP
$ sudo swapoff -a
$ sudo vi /etc/fstab  // 註解 /swap.img
```
![image](https://github.com/jai-9110/Harmonia-FL/blob/ec6863ea3fe73b4a99cd8c955b3dc2f447b3f4ff/picture/swap.png)
```
$ sudo reboot
```
