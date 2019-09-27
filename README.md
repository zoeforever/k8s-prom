# k8s-prom
- prometheus目录：部署Promethues Metrics API Server所需要的各资源配置清单。
- k8s-prometheus-adapter目录：部署基于prometheus的自定义指标API服务器所需要的各资源配置清单。
- podinfo目录：测试使用的podinfo相关的deployment和service对象的资源配置清单。
- node_exporter目录：于kubernetes集群各节点部署node_exporter。
- kube-state-metrics：聚合kubernetes资源对象，提供指标数据。

创建步骤：
1.创建namespace
2.创建prometheus
3.创建kube-state-metrics
4.创建k8s-prometheus-adapter需要的证书
  1. cd /etc/kubernetes/pki/
  2.(umask 077; openssl genrsa -out serving.key 2048)
  3.openssl req -new -key serving.key -out serving.csr -subj "/CN=serving"
  4.openssl x509 -req -in serving.csr -CA ./ca.crt -CAkey ./ca.key -CAcreateserial -out serving.crt -days 3650
  5.kubectl create secret generic cm-adapter-serving-certs --from-file=serving.crt=./serving.crt --from-file=serving.key=./serving.key -n prom
5.创建k8s-prometheus-adapter

#### 参考：https://github.com/stefanprodan/k8s-prom-hpa

