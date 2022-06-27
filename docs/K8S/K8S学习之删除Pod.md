## k8s正确删除pod的方法

> 背景：在部署的时候偶然发现一个pod部署错误了，想直接删除它，所以.......

我直接执行了
```bash
kubectl delete pod loki-21231-s12s -n logservice
```
显示
> pod "loki" deleted

但当我再次
```bash
kubectl get pod -n logservice
```
发现kube又新建了一个<br/>

<font color="red">这是因为当前Pod是由Pod控制器创建的，控制器会监控Pod状况，一旦发现Pod死亡，会立即重建<br/><b>
此时要想删除Pod，必须删除Pod控制器</b></font>

```bash
kubectl get deploy -n logservice
```

```bash
kubectl delete deploy loki -n logservice
```
> deployment.apps "loki" deleted

等待一会，发现已经被删除了
```bash
kubectl get pods -n logservice
```

> No resources found in logservice namespace.
