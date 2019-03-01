# Setup VPS

##### You must turn off the swap.

```bash
swapoff -a
```

You must comment `/swapfile` in  `/etc/fstab`. This is necessary so that when rebooting, the swap does not turn on.

# Install Docker

Valid version for k8s is 18.06

```bash
export VERSION=18.06 && curl -sSL get.docker.com | sh
```

# Install kubelet kubeadm kubectl

Example for Ubuntu. See [docs](https://kubernetes.io/docs/setup/independent/install-kubeadm/).

```bash
apt-get update && apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```

# Configuration

#### Initialize new cluster for example.com:

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-cert-extra-sans=example.com
```

#### Copy config:

```bash
rm -rf $HOME/.kube
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### Apply pod network:
[rbac-kdd.yaml](./rbac-kdd.yaml)

[calico.yaml](./calico.yaml)

```bash
kubectl apply -f rbac-kdd.yaml
kubectl apply -f calico.yaml
```

#### Enable taint node
If we have only one node
```bash
kubectl taint nodes --all node-role.kubernetes.io/master-
```

# Gitlab integration

#### Allow gitlab access
[gitlab-service-account.yaml](./gitlab-service-account.yaml)

[gitlab-cluster-role.yaml](./gitlab-cluster-role.yaml)
```bash
kubectl create -f gitlab-service-account.yaml
kubectl create -f gitlab-cluster-role.yaml
```

#### Get token and certificate
```bash
kubectl get secrets
```

```bash
kubectl get secret <secret name> -o jsonpath="{['data']['token']}" |
base64 --decode
```

```bash
kubectl get secret <secret name> -o jsonpath="{['data']['ca\.crt']}" |
base64 --decode
```

#### Install baremetal LB

`Attention!!!`

In this [configmap.yaml](./configmap.yaml) set your own inner IPv4 


```bash
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.7.3/manifests/metallb.yaml
```
```bash
kubectl apply -f configmap.yaml
```

#### Install helm
#### Install ingress
