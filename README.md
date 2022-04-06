# Kubernetes on Raspberry Pi
<img src="https://github.com/cly1213/Kubernetes_on_RPis/blob/main/pictures/Picture1.png">

## K3s
https://k3s.io/

### Hardware
- Raspberry Pi 3 B model or latest model
- MicroSD Cards
- Power Supply (5.1V 3A)
- Wi-Fi or Ethernet  connection

### Configure Raspberry Pi MicroSD
- Install the OS for your Raspberry Pi
- SSH connection

### Connect to your Raspberry Pi

### Install Kubernetes cluster
- Master Node
- Worker Node

# K3s on RPis

```
sudo apt update
sudo apt install vim -y

sudo vim /boot/cmdline.txt
# Add 
cgroup_enable=memory cgroup_memory=1

sudo vim /etc/dphys-swapfile
# Modify
CONF_SWAPSIZE=1024

sudo reboot
```

## Master node
### Become root
```
sudo su -
```

```
curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -s

cat /var/lib/rancher/k3s/server/token
```
```
root@master:~# curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -s
[INFO]  Finding release for channel stable
[INFO]  Using v1.22.6+k3s1 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.22.6+k3s1/sha256sum-arm64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.22.6+k3s1/k3s-arm64
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Creating /usr/local/bin/ctr symlink to k3s
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
root@master:~# cat /var/lib/rancher/k3s/server/token
K10edae723ec11314b02030f56a8795423d7c640a2418d29427154088db9c4ca590::server:ed43a59745341d2e16dbb4529db4b6ff
root@master:~# kubectl get nodes
NAME          STATUS   ROLES                  AGE   VERSION
master   Ready    control-plane,master   57s   v1.22.6+k3s1
```
### kubectl get nodes
```
root@master:~# kubectl get nodes
NAME     STATUS   ROLES                  AGE    VERSION
master   Ready    control-plane,master   150m   v1.22.6+k3s1
node1    Ready    <none>                 137m   v1.22.6+k3s1
node2    Ready    <none>                 71s    v1.22.6+k3s1
root@master:~#
```

## Worker node
### Add node to the cluster
```
curl -sfL https://get.k3s.io | K3S_URL=https://<MASTER NODE IP ADDRESS>:6443 K3S_TOKEN=<MASTER NODE TOKEN> sh -
```
```
root@node1:~# curl -sfL https://get.k3s.io | K3S_URL=https://192.168.1.119:6443 K3S_TOKEN=K10b8fd331c95ad2d2d809099a4efaa3cc581c9ccf53c3a2d053829290980c54dc8::server:22ec180e95a028fab30d9c578043328d sh -
[INFO]  Finding release for channel stable
[INFO]  Using v1.22.6+k3s1 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.22.6+k3s1/sha256sum-arm64.txt
[INFO]  Skipping binary downloaded, installed k3s matches hash
[INFO]  Skipping /usr/local/bin/kubectl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/crictl symlink to k3s, already exists
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, already exists
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-agent-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s-agent.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s-agent.service
[INFO]  systemd: Enabling k3s-agent unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s-agent.service → /etc/systemd/system/k3s-agent.service.
[INFO]  systemd: Starting k3s-agent
root@node1:~#
```

## node2
```
root@node2:~# curl -sfL https://get.k3s.io | K3S_URL=https://192.168.1.119:6443 K3S_TOKEN=K10b8fd331c95ad2d2d809099a4efaa3cc581c9ccf53c3a2d053829290980c54dc8::server:22ec180e95a028fab30d9c578043328d sh -
[INFO]  Finding release for channel stable
[INFO]  Using v1.22.6+k3s1 as release
[INFO]  Downloading hash https://github.com/k3s-io/k3s/releases/download/v1.22.6+k3s1/sha256sum-arm64.txt
[INFO]  Downloading binary https://github.com/k3s-io/k3s/releases/download/v1.22.6+k3s1/k3s-arm64
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Creating /usr/local/bin/ctr symlink to k3s
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-agent-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s-agent.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s-agent.service
[INFO]  systemd: Enabling k3s-agent unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s-agent.service → /etc/systemd/system/k3s-agent.service.
[INFO]  systemd: Starting k3s-agent
root@node2:~#
```

## Reference
