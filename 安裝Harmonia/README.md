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
> 開啟chorm瀏覽器，輸入192.168.97.10:3000，往下拉後直接安裝  
> 另外開啟一個shell執行此行指令並不要關掉  
> 會出現無法連上這個網站，返回上一頁就好
```
$ kubectl port-forward --address 0.0.0.0 service/harmonia-gitea 3000
```
![image](https://github.com/jai-9110/Harmonia-FL/blob/0b9c169e1209c02f349befd6dc833262a987339d/picture/%E5%AE%89%E8%A3%9Dgitea.png)
## 設定Gitea
1. 註冊gitea、aggregator、edge1、edge2、edge3    // 電子信箱可隨便打，不要重複即可
![image](https://github.com/jai-9110/Harmonia-FL/blob/c207227562c6e23239727d880a0414596753000e/picture/%E8%A8%BB%E5%86%8A%E5%B8%B3%E8%99%9F.png)
2. 登入gitea，點選右上角的新增儲存庫
![image](https://github.com/jai-9110/Harmonia-FL/blob/c8e482deb27c0b17a6d1f4ea20bf553e0ba5fab9/picture/%E6%96%B0%E5%A2%9E%E5%84%B2%E5%AD%98%E5%BA%AB.png)

| gitea | `train-plan` | `global-model` | `local-model1` | `local-model2` | `local-model3` |
|-----|:-------:|:-------:|:---:|:---:|:---:|
| aggregator | 可讀 | 可寫 | 可讀 | 可讀 | 可讀 |
| edge1 | 可讀 | 可讀 | 可寫 | - | - |
| edge2 | 可讀 | 可讀 | - | 可寫 | - |
| edge3 | 可讀 | 可讀 | - | - | 可寫 |
| *webhook* | http://mnist-aggregator:9080 | http://mnist-edge1:9080 | http://mnist-aggregator:9080 | http://mnist-aggregator:9080 | http://mnist-aggregator:9080 |
|   | http://mnist-edge1:9080 | http://mnist-edge2:9080 |
|   | http://mnist-edge2:9080 | http://mnist-edge3:9080 |
|   | http://mnist-edge3:9080 |
