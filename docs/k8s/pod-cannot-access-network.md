# k8s笔记

## base64加密用于secret的opaque
```
# 加密，注意参数-n，很重要，不用-n得出来的结果解密之后和用-n加密之后再解密得到的都是原来的字符串，
# 但是因为-n禁止了换行，所以使用-n加密出来的才是正确的
echo -n xxx|base64
# 解密
echo xxxxxxx|base64 --decode
```

## 获取集群可用的资源和版本

```
kubectl api-versions
kubectl api-resources
```

## 部署集群和网络时，指定的网络要一致
部署集群时，指定的网络：
```
...
networking:
  podSubnet: 0.244.0.0/16
```
部署网络时（calico)，指定的网络：
```
          - name: calico-node
            env:
              - name: CALICO_IPV4POOL_CIDR
              value: "10.244.0.0/16"
```
如果不同，会导致pod无法访问外网

## 健康检查
```
livenessProbe: 检查失败根据restartPolicy操作
readinessProbe：检查失败会把pod从service endpoint删除
```
前端项目nginx禁止目录访问，如果配置健康检查为/，导致返回403检查失败pod一直重启