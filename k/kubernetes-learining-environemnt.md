# Kubernete Learning Environment
* [Kubernetes Install Tools](https://kubernetes.io/docs/tasks/tools/)

## Single Node with Kubeadm

```
#!/usr/bin/env sh

KUBE_VERSION=1.20.5-00
DOCKER_VERSION=5:20.10.5~3-0~ubuntu-focal
CONTAINERD_VERSION=1.4.4-1
SERVER_IP=192.168.13.200

# disable translate download and add proxy
cat <<EOF | sudo tee /etc/apt/apt.conf.d/20translate
Acquire::Languages "none";
EOF

# Install Kuberntes components
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key FEEA9169307EA071
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

# Install Docker
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key 7EA0A9C3F273FCD8
cat <<EOF | sudo tee /etc/apt/sources.list.d/docker.list
deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable
EOF

# Disable swap
sudo swapoff --all
sudo sed -i '/^.*swap.*/ d' /etc/fstab

# Add shecan
cat <<EOF | sudo tee /etc/netplan/01-netcfg.yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
      dhcp4-overrides:
        use-dns: no
      nameservers:
        addresses: [178.22.122.100,185.51.200.2]
EOF

sudo netplan apply

sudo apt-get update
sudo apt-get install -y kubelet=$KUBE_VERSION kubeadm=$KUBE_VERSION kubectl=$KUBE_VERSION docker-ce=$DOCKER_VERSION containerd.io=$CONTAINERD_VERSION
sudo apt-mark hold kubelet kubeadm kubectl docker-ce docker-ce-cli containerd.io
sudo apt-get clean

# Set up the Docker daemon
cat <<EOF | sudo tee /etc/docker/daemon.json 
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

sudo systemctl restart docker.service

sudo kubeadm init --kubernetes-version $KUBE_VERSION --apiserver-advertise-address $SERVER_IP
sudo kubectl --kubeconfig /etc/kubernetes/admin.conf create -f https://docs.projectcalico.org/v3.18/manifests/calico.yaml
sudo kubectl --kubeconfig /etc/kubernetes/admin.conf taint node master-1 node-role.kubernetes.io/master-

exit 0
```

## Minikube

## Kind
