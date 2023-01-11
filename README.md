# Harmonia-FL
本範例教學為在一台Virtual box虛擬機中建立Kubernetes叢集(K8S all in one)，並在此環境下架設FL(Federated Learning)版的Harmonia。

建議 : 

1. Memory最好加到16GB，不然VM會跑不動
2. 安裝Ubuntu Server
   https://youtu.be/aB18S_8bPhA
3. 安裝Mobaxterm
   https://mobaxterm.mobatek.net/download.html
   * 專業版請詢問實驗室學長姊
   
# VM設定
* image : ubuntu-20.04.1-live-server-amd64.iso
  https://ftp.ntou.edu.tw/ubuntu-releases/focal/
* 網卡1(enp0s3) : NAT 、 網卡2(enp0s8) : HostOnly       // 事先將主機網路管理員中的DHCP關閉
* Master : 100GB-Disk, 8192RAM, 4cpus 
1. 新增host only的網卡
![image](https://github.com/jai-9110/Harmonia-FL/blob/31567292b06222cd63eaa73c10aa09a6861608ff/picture/%E4%B8%BB%E6%A9%9F%E7%B6%B2%E8%B7%AF%E7%AE%A1%E7%90%86%E5%93%A1.png)
2. 設定VM網路
   $ sudo apt-get update
   $ sudo apt install ifupdown
   $ sudo nano /etc/network/interfaces
