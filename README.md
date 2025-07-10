# Vagrant + K3s: Lightweight Kubernetes Cluster

## 📌 Description

This project uses Vagrant to create two virtual machines (VMs):
- `master` — the control-plane node
- `worker1` — the worker node

K3s is automatically installed and configured on both nodes, and the worker is joined to the master without any manual steps.

---

## ⚙️ How to Run

### 1. Clone the repository:

```bash
git clone git@github.com:Arman997/K3sAndVagrant.git
cd K3sAndVagrant
```

### 2. Create the shared folder:

```bash
mkdir shared
```

### 3. Make sure you're in the same directory as the Vagrantfile, then run:

```bash
vagrant up
```

This will:

- Start both VMs

- Install K3s on the master

- Extract the cluster token

- Join the worker node to the master

### 4. SSH into the master VM:

```bash
vagrant ssh master
```

- Then run the following command:

```bash
sudo k3s kubectl get nodes -o wide
```

- You should see something like this:

```pgsql
NAME      STATUS   ROLES                  AGE   VERSION        INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
master    Ready    control-plane,master   19m   v1.32.6+k3s1   10.0.2.15     <none>        Ubuntu 22.04.5 LTS   5.15.0-133-generic   containerd://2.0.5-k3s1.32
worker1   Ready    <none>                 15m   v1.32.6+k3s1   10.0.2.15     <none>        Ubuntu 22.04.5 LTS   5.15.0-133-generic   containerd://2.0.5-k3s1.32
```

### 5. To stop and remove all VMs:

```bash
vagrant destroy -f
```

## Requirements

- Vagrant
- VirtualBox
- Internet access (to download packages and K3s binaries)
