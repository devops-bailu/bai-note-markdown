# Kubernetes 运维常用命令

## K8s 信息

###### 获取集群版本信息

```shell
kubectl version -o yaml

kubectl get node -o yaml
```

###### 生成 yaml

```shell
kubectl create deployment web --image=nginx -o yaml --dry-run > nginx-pre.yaml
```

###### 获取 yaml

```shell
kubectl get deployment nginx -o=yaml --export > nginx-tmp.yaml
```

