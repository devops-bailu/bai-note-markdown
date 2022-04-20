# Kubernetes 运维常用命令

## K8s 信息

#### 基础信息

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

###### 查看资源对象

```bash
# 属于命名空间的资源对象
kubectl api-resources --namespaced=true

# 不属于命名空间的资源对象
kubectl api-resources --namespaced=false
```

#### K8s-deployment

```bash
# 创建 deployment 并且可以通过 --record 记录命令
kubectl create -f nginx-deployment.yaml --record
```

