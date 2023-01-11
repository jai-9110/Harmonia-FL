# 安裝Gitea
```
$ sudo nano gitea_deployment.yml
```
* gitea版本16以上無法使用，因此選擇1.15.5
<https://docs.gitea.io/en-us/config-cheat-sheet/>
* hostPath表示將Volume掛載到本機上的目錄中
* ports中的port表示Service監聽的通訊埠編號
* targetPort表示需要轉發到後端Pod的通訊埠編號
* nodePort表示對映到實體機的通訊埠編號
* type: NodePort表示使用宿主機的通訊埠，使能夠存取各Node的外部用戶端透過Node的IP為紙盒通訊埠編號存取Service
[gitea_deployment.yml](https://github.com/jai-9110/Harmonia-FL/blob/8d92dc462e85a717171ebcb3ff43a598150cbd62/%E5%AE%89%E8%A3%9DHarmonia/gitea_deployment.yml)

