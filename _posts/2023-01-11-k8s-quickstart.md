---
script: true
---
# K8S Quick Start

## Abstract

## 1 Installing Docker

[This article](https://icyfenix.cn/appendix/deployment-env-setup/setup-docker.html) was referred.
The following parts was some problems not mentioned in the article.

### 1.1 Verify Docker succeeded to be installed

The errors occurred when running command `sudo docker run hello-world`:

```shell
Cannot connect to the Docker daemon at unix:/var/run/docker.sock. Is the docker daemon running?
```

Solution: try out this : `systemctl start docker`.

It shows the Docker-CE was managed by `systemctl`.

### 1.2 Other operations

Start Docker with boot.

## 2 Installing Rancher

### 2.1 k3s

k3s: Lightweight **Kubernetes**.

Additional utilities will be installed, including kubectl, crictl, ctr, k3s-killall.sh, and k3s-uninstall.sh

`kubeconfig` file will be written to `/etc/rancher/k3s/k3s.yaml`.

`k3s` will run under `systemd` if it was installed by default script.

**kubectl**

`kubectl` is a command line tool provided by Kubernetes.

For configuration, `kubectl` looks for a file named `config` in the `$HOME/.kube` directory.


Refer: https://ranchermanager.docs.rancher.com/getting-started/quick-start-guides/deploy-rancher-manager/helm-cli