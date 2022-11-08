# otle-collector + jaeger


用`otle-collector`做数据获取，同时发送到 `jaeger` 和 `tempo`。将 `trace` 发送到 `otel-collector` 中，然后由 `otel-collector` 做统一分发

## 架构图

```
                                          -----> Jaeger (trace)

App + SDK ---> OpenTelemetry Collector ---|
                                          -----> Tempo (metrics)
                                          -----> Prometheus (metrics)
```

## 部署方式
```bash
$  cd otle-collector/
// namespace-k8s:
$ kubectl create -f namespace.yaml

//jaeger-operator-k8s:
// # Create the jaeger operator and necessary artifacts in ns observability
$ kubectl create -n observability  -f jaeger_crd.yaml
$ kubectl create -n observability  -f jaeger_service_account.yaml
$ kubectl create -n observability  -f jaeger_role.yaml
$ kubectl create -n observability  -f jaeger_role_binding.yaml
$ kubectl create -n observability  -f jaeger_operator.yaml

// # Create the cluster role & binding
$ kubectl create  -f jaeger_cluster_role.yaml
$ kubectl create  -f jaeger_cluster_role_binding.yaml

// jaeger-k8s
$ kubectl apply -f jaeger.yaml

// otel-collector-k8s:
$ kubectl apply -f otel_collector.yaml

$ kubectl get pod -n observability
NAME                             READY   STATUS    RESTARTS   AGE
jaeger-7854bb8dc8-jv2zz          1/1     Running   0          23h
jaeger-operator-dbf5767c-jckjb   1/1     Running   0          23h
otel-agent-6r8zp                 1/1     Running   0          21h
otel-agent-7jvrw                 1/1     Running   0          21h
otel-agent-j5tzm                 1/1     Running   0          21h
otel-agent-rqvf5                 1/1     Running   0          21h
otel-agent-sd84p                 1/1     Running   0          21h
otel-agent-z487g                 1/1     Running   0          21h
otel-collector-6484f74b9-r4xwj   1/1     Running   0          21h

$ kubectl get svc -n observability
NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                  AGE
jaeger-agent                ClusterIP   None            <none>        5775/UDP,5778/TCP,6831/UDP,6832/UDP      23h
jaeger-collector            ClusterIP   10.68.162.93    <none>        9411/TCP,14250/TCP,14267/TCP,14268/TCP   23h
jaeger-collector-headless   ClusterIP   None            <none>        9411/TCP,14250/TCP,14267/TCP,14268/TCP   23h
jaeger-operator-metrics     ClusterIP   10.68.177.219   <none>        8383/TCP,8686/TCP                        23h
jaeger-query                NodePort    10.68.150.211   <none>        16686:31169/TCP,16685:30707/TCP          23h
otel-collector              ClusterIP   10.68.215.212   <none>        4317/TCP,4318/TCP,8888/TCP               21h
```
