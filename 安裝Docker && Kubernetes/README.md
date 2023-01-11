# 安裝Docker
```
$ sudo apt-get install -y docker.io
$ systemctl enable docker.service
check : $ sudo docker version
```
![image](https://github.com/jai-9110/Harmonia-FL/blob/d6e60efb7e7b403ca4921f312d73139b55812749/picture/Docker.png)

# 安裝Kubernetes
```
$ sudo apt-get update && sudo apt-get install -y apt-transport-https curl
# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
$ cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/kubernetes-xenial main
EOF
$ sudo apt-get update
```

```
$ sudo apt install -y kubelet=1.20.0-00 kubectl=1.20.0-00 kubeadm=1.20.0-00
```
> 也可不指定K8S版本  
> ```
> $ sudo apt-get install -y kubelet kubeadm kubectl
> ```

```
$ sudo apt-mark hold kubelet kubeadm kubectl
```
> reset K8S  
> ```
> $ sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*  
> $ sudo rm -rf ~/.kube  
> $ kubeadm reset  
> ```

```
$ sudo vi /etc/docker/daemon.json
```
```
{
    "live-restore": true,
    "group": "dockerroot",
    "insecure-registries": ["192.168.97.10:5000"]
} 
```
> 設定Registry，IP為Master的IP(Registry的位置在Master)  
> 若有叢集內有多台Worker也都設定成Master的IP

```
$ systemctl restart docker
$ sudo docker run -d -p 5000:5000 -v ~/storage:/var/lib/registry --name registry registry:2
```
> Master 運行Docker Registry服務
<https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/>  

> 只要關機，此registry服務就要重啟  
> ```  
> $ sudo docker ps -a     // 找到運行registry的container  
> $ sudo docker start <container name>   // 重啟registry服務  
> ```
