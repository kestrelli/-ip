## 背景

本 Playbook 旨在指导您通过 Kubernetes 的 `VPC-CNI` 网络模式，实现 ​**CLB 直连业务 Pod**​ 的能力，确保业务 Pod 收到的请求源 IP 为客户端真实 IP。本方案完全绕过 NodePort，适用于腾讯云容器服务（TKE）环境。

### 前置条件

1. ​**集群环境**​
	- TKE 集群需启用 `VPC-CNI` 网络模式
	- 确保集群有可用节点且 `kubectl` 已配置访问权限
2. ​**镜像准备**​
	- 已构建业务镜像并推送至腾讯云镜像仓库（个人版/企业版）
	- 示例镜像版本：`vickytan-demo.tencentcloudcr.com/kestrelli/images：v1.0`

### 操作流程

#### Step 1: 创建业务工作负载（Deployment）
​**1.创建 Deployment YAML 文件(存放于deployment.yaml)**​
📌 ​**关键配置**​
- `metadata.labels` 需与后续 Service 选择器匹配
- `containerPort` 需与业务实际端口一致
​**2.部署工作负载**​

``` 
kubectl apply -f deployment.yaml
```

**3.验证 Pod 状态**
​**预期输出**​：所有 Pod 状态为 `Running`
```
kubectl get pods -l app=real-ip-app
```
![截图](/Users/kestrelli/Desktop/LoadBalancer+直连+Pod+模式+Service+获取真实源+IP+Playbook/images/pod1.png)
