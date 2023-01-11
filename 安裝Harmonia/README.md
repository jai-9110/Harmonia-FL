# 安裝Gitea
```
$ sudo nano gitea_deployment.yml
```
```yaml
# gitea_deployment.yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: gitea-config
data:
  ROOT_URL: http://harmonia-gitea:3000
  LFS_START_SERVER: "true"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: gitea-deployment
  labels:
    app: gitea
spec:
  replicas: 1
  selector:
    matchLabels:
      app: gitea
  template:
    metadata:
      labels:
        app: gitea
    spec:
      nodeName: worker1
      containers:
        - name: gitea
          image: gitea/gitea:1.15.5-linux-amd64
          ports:
            - containerPort: 3000
              name: gitea
          envFrom:
            - configMapRef:
                name: gitea-config
          volumeMounts:
            - mountPath: /data
              name: test-volume
      volumes:
        - name: test-volume
          hostPath:
            path: /home/harmonia
            type: DirectoryOrCreate
---
kind: Service
apiVersion: v1
metadata:
  name: harmonia-gitea
spec:
  selector:
    app: gitea
  ports:
  - name: gitea-http
    port: 3000
    targetPort: 3000
    nodePort: 30000
  type: NodePort
```
