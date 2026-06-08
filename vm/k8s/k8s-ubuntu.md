# 所有节点（master + worker）执行

```shell
# 1. 更新系统
sudo apt update && sudo apt upgrade -y

# 2. 安装基础工具
sudo apt install -y curl wget vim net-tools ssh openssh-server

# 3. 安装 containerd 作为容器运行时
sudo apt install -y containerd

# 配置 containerd
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml

# 修改配置：使用 systemd cgroup 驱动（重要！）
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml

# 重启 containerd
sudo systemctl restart containerd
sudo systemctl enable containerd

# 4. 添加 Kubernetes 源
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update

# 5. 安装 kubeadm, kubelet, kubectl
sudo apt install -y kubelet kubeadm kubectl

# 锁定版本防止自动更新（推荐）
sudo apt-mark hold kubelet kubeadm kubectl
```

# Master 节点执行

```shell
# 初始化控制平面
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --control-plane-endpoint=LOAD_BALANCER_DNS:PORT  # 单 master 可省略 endpoint

# 配置 kubectl（非 root 用户）
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# 安装 Pod 网络插件（如 Flannel）
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml

# 可选：允许 master 节点调度 Pod（用于测试）
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

# Worker 节点执行

```shell
sudo kubeadm join 192.168.1.10:6443 --token abcdef.1234567890abcdef \
    --discovery-token-ca-cert-hash sha256:xxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

# 命令

| 命令                                                      | 说明                   |
| --------------------------------------------------------- | ---------------------- |
| kubectl get nodes                                         | 查看节点状态           |
| kubectl get pods -A                                       | 查看所有命名空间的 Pod |
| kubectl get pods -n default                               | 查看某命名空间 Pod     |
| kubectl describe pod <pod-name>                           | 查看 Pod 详细信息      |
| kubectl logs <pod-name>                                   | 查看 Pod 日志          |
| kubectl exec -it <pod-name> -- sh                         | 进入 Pod 容器          |
| kubectl create deployment nginx --image=nginx             | 创建部署               |
| kubectl expose deployment nginx --port=80 --type=NodePort | 暴露服务               |
| kubectl scale deployment nginx --replicas=3               | 扩容副本               |
| kubectl delete pod <pod-name>                             | 删除 Pod（自动重建）   |
| kubectl apply -f deployment.yaml                          | 应用 YAML 配置         |
| kubectl get svc                                           | 查看服务               |
| kubectl get pv,pvc                                        | 查看持久卷             |
| kubectl config get-contexts                               | 查看上下文             |

调试

```shell
kubectl get componentstatuses        # 查看控制平面组件状态（已弃用，可用 kubeadm check）
journalctl -u kubelet -f             # 查看 kubelet 日志
kubectl cluster-info                 # 查看集群信息
```

# 升级 Kubernetes 集群

```shell
# 升级 Master 节点

# 1. 查看可升级版本
sudo kubeadm upgrade plan

# 2. 升级控制平面
sudo kubeadm upgrade apply v1.29.0

# 3. 升级 kubelet 和 kubectl
sudo apt update
sudo apt install kubelet=1.29.0-00 kubectl=1.29.0-00
sudo systemctl restart kubelet

# 升级 Worker 节点

# 1. 先排空节点（驱逐 Pod）
kubectl drain <node-name> --ignore-daemonsets

# 2. 升级 kubeadm
sudo apt install kubeadm=1.29.0-00

# 3. 升级 kubelet 配置
sudo kubeadm upgrade node

# 4. 升级 kubelet 和 kubectl
sudo apt install kubelet=1.29.0-00 kubectl=1.29.0-00
sudo systemctl restart kubelet

# 5. 恢复调度
kubectl uncordon <node-name>
```

https://kubernetes.io/docs/home/?spm=a2ty_o01.29997173.0.0.4857c921BS7Okd

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/?spm=a2ty_o01.29997173.0.0.4857c921BS7Okd

# 安装镜像

```
Docker 镜像 → 推送到镜像仓库 → K8s YAML 中引用 → K8s 拉取并运行
```

## 编写 Dockerfile

```dockerfile
# Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html
EXPOSE 80
```

## 构建镜像

```bash
docker login
docker push yourname/myapp:v1
```

## 推送到私有仓库（如 Harbor、阿里云 ACR）

```bash
docker tag yourname/myapp:v1 registry.cn-hangzhou.aliyuncs.com/your-namespace/myapp:v1
docker push registry.cn-hangzhou.aliyuncs.com/your-namespace/myapp:v1
```

## 在 Kubernetes 中使用镜像

使用 kubectl run 快速运行（测试用）

```bash
kubectl run mynginx --image=yourname/myapp:v1 --port=80
```

使用 YAML 部署（推荐生产使用）

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: yourname/myapp:v1
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort  # 或 LoadBalancer
```

应用配置

```bash
kubectl apply -f deployment.yaml
```

## 私有镜像仓库

创建 Secret 保存登录凭证

```bash
kubectl create secret docker-registry my-registry-secret \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=your-username \
  --docker-password=your-password \
  --docker-email=your-email
```

在 Pod 中引用 Secret

```yaml
spec:
  containers:
    - name: myapp
      image: yourname/myapp:v1
  imagePullSecrets:
    - name: my-registry-secret
```

验证镜像是否运行

```bash
kubectl get pods
kubectl logs <pod-name>
kubectl describe pod <pod-name>
```

## 离线环境使用本地镜像

在每个节点上手动加载镜像

```bash
docker save myapp:v1 > myapp.tar
# 拷贝到所有 K8s 节点
docker load < myapp.tar
```

使用 imagePullPolicy: Never 或 IfNotPresent

```yaml
containers:
  - name: myapp
    image: myapp:v1
    imagePullPolicy: Never  # 强制使用本地镜像
```
