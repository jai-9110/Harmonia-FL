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
