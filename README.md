<img src="https://avatars1.githubusercontent.com/u/3380462?s=200&v=4" width="100">

[Prometheus](https://prometheus.io/), 是 [Cloud Native Computing Foundation](https://cncf.io/) 下的监控系统项目. 它收集目标服务的metrics信息, 通过预配置的规则,显示结果并触发告警信息.

## 项目介绍

本项目基于 [prometheus-operator](https://github.com/coreos/prometheus-operator) 和 [prometheus](https://github.com/prometheus/prometheus)官方helm模版为基础打造出的Prometheus监控解决方案,并新增以下内容:

- 支持自动扫描并添加所有kubernetes命名空间endpoints:/metrics
- 新增grafana支持
- 新增metrics-server支持(用于kubernetes HPA)
- 新增custom-metrics-api支持(用于kubernetes 高级HPA)
- 新增[Prometheus alert钉钉告警插件](https://github.com/feiyu563/PrometheusAlert)
- 新增grafana图表:
#### CoreDNS
#### Etcd
#### kubernetes集群监控
#### Mysql
#### Kubernetes Pods/Deployment/Statefulset/Daemonset/Nodes
#### Prometheus Alerts
#### Traefik
#### .....

## 部分展示
<img src="image/image1.png">
<img src="image/image2.png">
<img src="image/image3.png">
<img src="image/image4.png">
<img src="image/image5.png">
<img src="image/image6.png">
<img src="image/image7.png">
<img src="image/image8.png">
<img src="image/image9.png">

## 快速部署

```console
$ git clone https://github.com/feiyu563/helm-prometheus.git
$ helm install --name my-prometheus --namespace monitoring ./prometheus
```

## 清除部署

```console
$ helm delete my-prometheus --purge
```

## 主要配置

Parameter | Description | Default
--------- | ----------- | -------
`alertmanager.enabled` | If true, create alertmanager | `true`
`alertmanager.name` | alertmanager container name | `alertmanager`
`alertmanager.image.repository` | alertmanager container image repository | `prom/alertmanager`
`alertmanager.image.tag` | alertmanager container image tag | `v0.15.2`
`alertmanager.image.pullPolicy` | alertmanager container image pull policy | `IfNotPresent`
`alertmanager.prefixURL` | The prefix slug at which the server can be accessed | ``
`alertmanager.baseURL` | The external url at which the server can be accessed | `/`
`alertmanager.extraArgs` | Additional alertmanager container arguments | `{}`
`alertmanager.configMapOverrideName` | Prometheus alertmanager ConfigMap override where full-name is `{{.Release.Name}}-{{.Values.alertmanager.configMapOverrideName}}` and setting this value will prevent the default alertmanager ConfigMap from being generated | `""`
`alertmanager.ingress.enabled` | If true, alertmanager Ingress will be created | `false`
`alertmanager.ingress.annotations` | alertmanager Ingress annotations | `{}`
`alertmanager.ingress.extraLabels` | alertmanager Ingress additional labels | `{}`
`alertmanager.ingress.hosts` | alertmanager Ingress hostnames | `[]`
`alertmanager.ingress.tls` | alertmanager Ingress TLS configuration (YAML) | `[]`
`alertmanager.nodeSelector` | node labels for alertmanager pod assignment | `{}`
`alertmanager.tolerations` | node taints to tolerate (requires Kubernetes >=1.6) | `[]`
`alertmanager.affinity` | pod affinity | `{}`
`alertmanager.schedulerName` | alertmanager alternate scheduler name | `nil`
`alertmanager.persistentVolume.enabled` | If true, alertmanager will create a Persistent Volume Claim | `true`
`alertmanager.persistentVolume.accessModes` | alertmanager data Persistent Volume access modes | `[ReadWriteOnce]`
`alertmanager.persistentVolume.annotations` | Annotations for alertmanager Persistent Volume Claim | `{}`
`alertmanager.persistentVolume.existingClaim` | alertmanager data Persistent Volume existing claim name | `""`
`alertmanager.persistentVolume.mountPath` | alertmanager data Persistent Volume mount root path | `/data`
`alertmanager.persistentVolume.size` | alertmanager data Persistent Volume size | `2Gi`
`alertmanager.persistentVolume.storageClass` | alertmanager data Persistent Volume Storage Class | `unset`
`alertmanager.persistentVolume.subPath` | Subdirectory of alertmanager data Persistent Volume to mount | `""`
`alertmanager.podAnnotations` | annotations to be added to alertmanager pods | `{}`
`alertmanager.replicaCount` | desired number of alertmanager pods | `1`
`alertmanager.priorityClassName` | alertmanager priorityClassName | `nil`
`alertmanager.resources` | alertmanager pod resource requests & limits | `{}`
`alertmanager.securityContext` | Custom [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for Alert Manager containers | `{}`
`alertmanager.service.annotations` | annotations for alertmanager service | `{}`
`alertmanager.service.clusterIP` | internal alertmanager cluster service IP | `""`
`alertmanager.service.externalIPs` | alertmanager service external IP addresses | `[]`
`alertmanager.service.loadBalancerIP` | IP address to assign to load balancer (if supported) | `""`
`alertmanager.service.loadBalancerSourceRanges` | list of IP CIDRs allowed access to load balancer (if supported) | `[]`
`alertmanager.service.servicePort` | alertmanager service port | `80`
`alertmanager.service.type` | type of alertmanager service to create | `ClusterIP`
`alertmanagerFiles.alertmanager.yml` | Prometheus alertmanager configuration | example configuration
`configmapReload.name` | configmap-reload container name | `configmap-reload`
`configmapReload.image.repository` | configmap-reload container image repository | `jimmidyson/configmap-reload`
`configmapReload.image.tag` | configmap-reload container image tag | `v0.2.2`
`configmapReload.image.pullPolicy` | configmap-reload container image pull policy | `IfNotPresent`
`configmapReload.extraArgs` | Additional configmap-reload container arguments | `{}`
`configmapReload.extraConfigmapMounts` | Additional configmap-reload configMap mounts | `[]`
`configmapReload.resources` | configmap-reload pod resource requests & limits | `{}`
`initChownData.enabled`  | If false, don't reset data ownership at startup | true
`initChownData.name` | init-chown-data container name | `init-chown-data`
`initChownData.image.repository` | init-chown-data container image repository | `busybox`
`initChownData.image.tag` | init-chown-data container image tag | `latest`
`initChownData.image.pullPolicy` | init-chown-data container image pull policy | `IfNotPresent`
`initChownData.resources` | init-chown-data pod resource requests & limits | `{}`
`kubeStateMetrics.enabled` | If true, create kube-state-metrics | `true`
`kubeStateMetrics.name` | kube-state-metrics container name | `kube-state-metrics`
`kubeStateMetrics.image.repository` | kube-state-metrics container image repository| `quay.io/coreos/kube-state-metrics`
`kubeStateMetrics.image.tag` | kube-state-metrics container image tag | `v1.4.0`
`kubeStateMetrics.image.pullPolicy` | kube-state-metrics container image pull policy | `IfNotPresent`
`kubeStateMetrics.args` | kube-state-metrics container arguments | `{}`
`kubeStateMetrics.nodeSelector` | node labels for kube-state-metrics pod assignment | `{}`
`kubeStateMetrics.podAnnotations` | annotations to be added to kube-state-metrics pods | `{}`
`kubeStateMetrics.deploymentAnnotations` | annotations to be added to kube-state-metrics deployment | `{}`
`kubeStateMetrics.tolerations` | node taints to tolerate (requires Kubernetes >=1.6) | `[]`
`kubeStateMetrics.replicaCount` | desired number of kube-state-metrics pods | `1`
`kubeStateMetrics.priorityClassName` | kube-state-metrics priorityClassName | `nil`
`kubeStateMetrics.resources` | kube-state-metrics resource requests and limits (YAML) | `{}`
`kubeStateMetrics.securityContext` | Custom [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for kube-state-metrics containers | `{}`
`kubeStateMetrics.service.annotations` | annotations for kube-state-metrics service | `{prometheus.io/scrape: "true"}`
`kubeStateMetrics.service.clusterIP` | internal kube-state-metrics cluster service IP | `None`
`kubeStateMetrics.service.externalIPs` | kube-state-metrics service external IP addresses | `[]`
`kubeStateMetrics.service.loadBalancerIP` | IP address to assign to load balancer (if supported) | `""`
`kubeStateMetrics.service.loadBalancerSourceRanges` | list of IP CIDRs allowed access to load balancer (if supported) | `[]`
`kubeStateMetrics.service.servicePort` | kube-state-metrics service port | `80`
`kubeStateMetrics.service.type` | type of kube-state-metrics service to create | `ClusterIP`
`nodeExporter.enabled` | If true, create node-exporter | `true`
`nodeExporter.name` | node-exporter container name | `node-exporter`
`nodeExporter.image.repository` | node-exporter container image repository| `prom/node-exporter`
`nodeExporter.image.tag` | node-exporter container image tag | `v0.16.0`
`nodeExporter.image.pullPolicy` | node-exporter container image pull policy | `IfNotPresent`
`nodeExporter.extraArgs` | Additional node-exporter container arguments | `{}`
`nodeExporter.extraHostPathMounts` | Additional node-exporter hostPath mounts | `[]`
`nodeExporter.extraConfigmapMounts` | Additional node-exporter configMap mounts | `[]`
`nodeExporter.nodeSelector` | node labels for node-exporter pod assignment | `{}`
`nodeExporter.podAnnotations` | annotations to be added to node-exporter pods | `{}`
`nodeExporter.pod.labels` | labels to be added to node-exporter pods | `{}`
`nodeExporter.tolerations` | node taints to tolerate (requires Kubernetes >=1.6) | `[]`
`nodeExporter.priorityClassName` | node-exporter priorityClassName | `nil`
`nodeExporter.resources` | node-exporter resource requests and limits (YAML) | `{}`
`nodeExporter.securityContext` | securityContext for containers in pod | `{}`
`nodeExporter.service.annotations` | annotations for node-exporter service | `{prometheus.io/scrape: "true"}`
`nodeExporter.service.clusterIP` | internal node-exporter cluster service IP | `None`
`nodeExporter.service.externalIPs` | node-exporter service external IP addresses | `[]`
`nodeExporter.service.loadBalancerIP` | IP address to assign to load balancer (if supported) | `""`
`nodeExporter.service.loadBalancerSourceRanges` | list of IP CIDRs allowed access to load balancer (if supported) | `[]`
`nodeExporter.service.servicePort` | node-exporter service port | `9100`
`nodeExporter.service.type` | type of node-exporter service to create | `ClusterIP`
`pushgateway.enabled` | If true, create pushgateway | `true`
`pushgateway.name` | pushgateway container name | `pushgateway`
`pushgateway.image.repository` | pushgateway container image repository | `prom/pushgateway`
`pushgateway.image.tag` | pushgateway container image tag | `v0.5.2`
`pushgateway.image.pullPolicy` | pushgateway container image pull policy | `IfNotPresent`
`pushgateway.extraArgs` | Additional pushgateway container arguments | `{}`
`pushgateway.ingress.enabled` | If true, pushgateway Ingress will be created | `false`
`pushgateway.ingress.annotations` | pushgateway Ingress annotations | `{}`
`pushgateway.ingress.hosts` | pushgateway Ingress hostnames | `[]`
`pushgateway.ingress.tls` | pushgateway Ingress TLS configuration (YAML) | `[]`
`pushgateway.nodeSelector` | node labels for pushgateway pod assignment | `{}`
`pushgateway.podAnnotations` | annotations to be added to pushgateway pods | `{}`
`pushgateway.tolerations` | node taints to tolerate (requires Kubernetes >=1.6) | `[]`
`pushgateway.replicaCount` | desired number of pushgateway pods | `1`
`pushgateway.priorityClassName` | pushgateway priorityClassName | `nil`
`pushgateway.resources` | pushgateway pod resource requests & limits | `{}`
`pushgateway.service.annotations` | annotations for pushgateway service | `{}`
`pushgateway.service.clusterIP` | internal pushgateway cluster service IP | `""`
`pushgateway.service.externalIPs` | pushgateway service external IP addresses | `[]`
`pushgateway.service.loadBalancerIP` | IP address to assign to load balancer (if supported) | `""`
`pushgateway.service.loadBalancerSourceRanges` | list of IP CIDRs allowed access to load balancer (if supported) | `[]`
`pushgateway.service.servicePort` | pushgateway service port | `9091`
`pushgateway.service.type` | type of pushgateway service to create | `ClusterIP`
`rbac.create` | If true, create & use RBAC resources | `true`
`server.name` | Prometheus server container name | `server`
`server.image.repository` | Prometheus server container image repository | `prom/prometheus`
`server.image.tag` | Prometheus server container image tag | `v2.4.0`
`server.image.pullPolicy` | Prometheus server container image pull policy | `IfNotPresent`
`server.enableAdminApi` |  If true, Prometheus administrative HTTP API will be enabled. Please note, that you should take care of administrative API access protection (ingress or some frontend Nginx with auth) before enabling it. | `false`
`server.global.scrape_interval` | How frequently to scrape targets by default | `1m`
`server.global.scrape_timeout` | How long until a scrape request times out | `10s`
`server.global.evaluation_interval` | How frequently to evaluate rules | `1m`
`server.extraArgs` | Additional Prometheus server container arguments | `{}`
`server.prefixURL` | The prefix slug at which the server can be accessed | ``
`server.baseURL` | The external url at which the server can be accessed | ``
`server.extraHostPathMounts` | Additional Prometheus server hostPath mounts | `[]`
`server.extraConfigmapMounts` | Additional Prometheus server configMap mounts | `[]`
`server.extraSecretMounts` | Additional Prometheus server Secret mounts | `[]`
`server.configMapOverrideName` | Prometheus server ConfigMap override where full-name is `{{.Release.Name}}-{{.Values.server.configMapOverrideName}}` and setting this value will prevent the default server ConfigMap from being generated | `""`
`server.ingress.enabled` | If true, Prometheus server Ingress will be created | `false`
`server.ingress.annotations` | Prometheus server Ingress annotations | `[]`
`server.ingress.extraLabels` | Prometheus server Ingress additional labels | `{}`
`server.ingress.hosts` | Prometheus server Ingress hostnames | `[]`
`server.ingress.tls` | Prometheus server Ingress TLS configuration (YAML) | `[]`
`server.nodeSelector` | node labels for Prometheus server pod assignment | `{}`
`server.tolerations` | node taints to tolerate (requires Kubernetes >=1.6) | `[]`
`server.affinity` | pod affinity | `{}`
`server.priorityClassName` | Prometheus server priorityClassName | `nil`
`server.schedulerName` | Prometheus server alternate scheduler name | `nil`
`server.persistentVolume.enabled` | If true, Prometheus server will create a Persistent Volume Claim | `true`
`server.persistentVolume.accessModes` | Prometheus server data Persistent Volume access modes | `[ReadWriteOnce]`
`server.persistentVolume.annotations` | Prometheus server data Persistent Volume annotations | `{}`
`server.persistentVolume.existingClaim` | Prometheus server data Persistent Volume existing claim name | `""`
`server.persistentVolume.mountPath` | Prometheus server data Persistent Volume mount root path | `/data`
`server.persistentVolume.size` | Prometheus server data Persistent Volume size | `8Gi`
`server.persistentVolume.storageClass` | Prometheus server data Persistent Volume Storage Class |  `unset`
`server.persistentVolume.subPath` | Subdirectory of Prometheus server data Persistent Volume to mount | `""`
`server.podAnnotations` | annotations to be added to Prometheus server pods | `{}`
`server.deploymentAnnotations` | annotations to be added to Prometheus server deployment | `{}'
`server.replicaCount` | desired number of Prometheus server pods | `1`
`server.resources` | Prometheus server resource requests and limits | `{}`
`server.securityContext` | Custom [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for server containers | `{}`
`server.service.annotations` | annotations for Prometheus server service | `{}`
`server.service.clusterIP` | internal Prometheus server cluster service IP | `""`
`server.service.externalIPs` | Prometheus server service external IP addresses | `[]`
`server.service.loadBalancerIP` | IP address to assign to load balancer (if supported) | `""`
`server.service.loadBalancerSourceRanges` | list of IP CIDRs allowed access to load balancer (if supported) | `[]`
`server.service.nodePort` | Port to be used as the service NodePort (ignored if `server.service.type` is not `NodePort`) | `0`
`server.service.servicePort` | Prometheus server service port | `80`
`server.service.type` | type of Prometheus server service to create | `ClusterIP`
`serviceAccounts.alertmanager.create` | If true, create the alertmanager service account | `true`
`serviceAccounts.alertmanager.name` | name of the alertmanager service account to use or create | `{{ prometheus.alertmanager.fullname }}`
`serviceAccounts.kubeStateMetrics.create` | If true, create the kubeStateMetrics service account | `true`
`serviceAccounts.kubeStateMetrics.name` | name of the kubeStateMetrics service account to use or create | `{{ prometheus.kubeStateMetrics.fullname }}`
`serviceAccounts.nodeExporter.create` | If true, create the nodeExporter service account | `true`
`serviceAccounts.nodeExporter.name` | name of the nodeExporter service account to use or create | `{{ prometheus.nodeExporter.fullname }}`
`serviceAccounts.pushgateway.create` | If true, create the pushgateway service account | `true`
`serviceAccounts.pushgateway.name` | name of the pushgateway service account to use or create | `{{ prometheus.pushgateway.fullname }}`
`serviceAccounts.server.create` | If true, create the server service account | `true`
`serviceAccounts.server.name` | name of the server service account to use or create | `{{ prometheus.server.fullname }}`
`server.terminationGracePeriodSeconds` | Prometheus server Pod termination grace period | `300`
`server.retention` | (optional) Prometheus data retention | `""`
`serverFiles.alerts` | Prometheus server alerts configuration | `{}`
`serverFiles.rules` | Prometheus server rules configuration | `{}`
`serverFiles.prometheus.yml` | Prometheus server scrape configuration | example configuration
`networkPolicy.enabled` | Enable NetworkPolicy | `false` |

## 其他详细配置信息请参照value.yaml文件中的注释
