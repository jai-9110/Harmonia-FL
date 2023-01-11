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

* 設定儲存庫名稱並建立儲存庫
  ![image](https://github.com/jai-9110/Harmonia-FL/blob/3fdf7d4949ffe4e8ddd2599e76cbfc30ea4768ce/picture/%E6%96%B0%E5%A2%9E%E5%84%B2%E5%AD%98%E5%BA%AB-2.png)  
* 點選設定，根據上方表格設定協作者
  ![image](https://github.com/jai-9110/Harmonia-FL/blob/125fcb24cf2eb99f99aa2713b3461812d6ac8fbf/picture/%E8%A8%AD%E5%AE%9A%E5%8D%94%E4%BD%9C%E8%80%85.png)  
* 根據上方表格設定webhook，建立webhook，選擇Gitea
  ![image](https://github.com/jai-9110/Harmonia-FL/blob/9d2d298791ca00380698b2592129329951d7f04b/picture/%E5%BB%BA%E7%AB%8Bwebhook.png)  
* 根據上方表格設定目標URL
  ![image](https://github.com/jai-9110/Harmonia-FL/blob/93c9bbfb14e522ba3e76d79906215b796b09a5bc/picture/%E8%A8%AD%E5%AE%9AURL.png)  
 
3.  獲取GiteaUserToken(以edge1為例)
* 登入edge1，點選右上角，點選設定
![image](https://github.com/jai-9110/Harmonia-FL/blob/79438b1925702b96fced4189b94dddc0135f12d4/picture/get_token-1.png)
* 點選應用程式
![image](https://github.com/jai-9110/Harmonia-FL/blob/3c650ccbe5fe06f019ef6576873d251fc251d4ce/picture/%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F.png)
* Token名稱可隨機輸入
![image](https://github.com/jai-9110/Harmonia-FL/blob/5a77d1f27e5adab0efa73c9353a1c48c89688bcf/picture/get_token-2.png)
* 將產生的token記好，後面會用到
* 將aggregator、edge2、edge3的token都記好

```
$ sudo nano configs.yml    // 上面記錄的token要寫到此檔案中
```
[configs.yml](https://github.com/jai-9110/Harmonia-FL/blob/f330123519fc1637153007dab02865237f356e37/%E5%AE%89%E8%A3%9DHarmonia/configs.yml)
```
$ kubectl apply -f configs.yml
```

# 安裝Harmonia
```
$ git clone https://github.com/ailabstw/harmonia.git
$ sudo nano harmonia/src/protos/service.proto   // 版本更新
```
* 替換第三行為此內容
```
option go_package = “./;protos”;     
```

```
$ cd harmonia
$ sudo make all
```
* check : 檢查images 是否有operator fedavg
```
$ cp src/protos/python_protos/service_pb2_grpc.py examples/edge/
$ cp src/protos/python_protos/service_pb2.py examples/edge/

$ sudo docker build examples/edge --tag 192.168.97.10:5000/mnist-edge
$ sudo docker tag harmonia/operator 192.168.97.10:5000/harmonia-operator
$ sudo docker tag harmonia/fedavg 192.168.97.10:5000/harmonia-fedavg

$ sudo docker push 192.168.97.10:5000/harmonia-operator
$ sudo docker push 192.168.97.10:5000/harmonia-fedavg
$ sudo docker push 192.168.97.10:5000/mnist-edge

$ sudo nano mnist_deployment.yml
```
[mnist_deployment.yml](https://github.com/jai-9110/Harmonia-FL/blob/9fb62aa5aca737846702b2a73345ec1c4eba0758/%E5%AE%89%E8%A3%9DHarmonia/mnist_deployment.yml)
```
$ kubectl apply -f mnist_deployment.yml

check : $ kubectl get po
```

# 開始訓練
```
$ git clone http://127.0.0.1:3000/gitea/global-model.git
$ pushd global-model

$ git commit -m "pretrained model" --allow-empty
$ git push origin master

$ popd
$ rm -rf global-model
```
--------------------------------------------------------------
* 重推訓練上去前plan.json中的資料都要有所變更，才能成功執行新的訓練
```
$ git clone http://127.0.0.1:3000/gitea/train-plan.git
$ pushd train-plan

$ cat > plan.json << EOF
{
    "name": "MNIST",
    "round": 10,
    "edge": 3,
    "EpR": 1,
    "timeout": 1200,
    "pretrainedModel": "master"
}
EOF

$ git add plan.json
$ git commit -m "train plan commit”
$ git push origin master

$ popd
$ rm -rf train-plan
```
* 查看pod運行情況(log記錄檔)
```
$ kubectl logs <pod name> <container name> -f
```
