# 🏗 Kubernetes Master Node Setup using kubeadm  

A step-by-step guide to setting up a **Kubernetes master node** on an **Ubuntu server** using `kubeadm`. This guide covers **system preparation, installing dependencies, configuring container runtime, setting up Kubernetes, and verifying the cluster**.  

## 📖 Table of Contents  
- [Prerequisites](#prerequisites)  
- [Step 1: Disable Swap & Configure System Parameters](#step-1-disable-swap--configure-system-parameters)  
- [Step 2: System Updates & Install Dependencies](#step-2-system-updates--install-dependencies)  
- [Step 3: Install & Configure Container Runtime](#step-3-install--configure-container-runtime)  
- [Step 4: Add Docker Repository](#step-4-add-docker-repository)  
- [Step 5: Add Kubernetes Repository](#step-5-add-kubernetes-repository)  
- [Step 6: Install Kubernetes Components](#step-6-install-kubernetes-components)  
- [Step 7: Enable & Start Kubelet](#step-7-enable--start-kubelet)  
- [Step 8: Initialize Kubernetes Master Node](#step-8-initialize-kubernetes-master-node)  
- [Step 9: Configure kubectl for Admin User](#step-9-configure-kubectl-for-admin-user)  
- [Step 10: Deploy Network Plugin](#step-10-deploy-network-plugin)  
- [Step 11: Verify Master Node Status](#step-11-verify-master-node-status)  
- [Step 12: Join Worker Nodes](#step-12-join-worker-nodes)  
- [Conclusion](#conclusion)  
- [License](#license)  

---

## ✅ Prerequisites  
- A **clean Ubuntu server** (root/sudo access)  
- **Internet connectivity**  
- **Swap disabled** (`sudo swapoff -a`)  

---

## 🛠 Step 1: Disable Swap & Configure System Parameters  
```bash
sudo swapoff -a
nano /etc/sysctl.conf
sysctl -p
```


## 📦 Step 2: System Updates & Install Dependencies

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

```

## 🐳 Install & Configure Container Runtime

```bash
sudo apt-get install -y containerd.io
containerd config default | sudo tee /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd

```
## 📥 Add Docker Repository

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```
## 📥 Add Kubernetes Repository

```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

```
## 🏗 Install Kubernetes Components

```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

```

## 🔄 Enable & Start Kubelet
```bash
sudo systemctl enable --now kubelet
systemctl status kubelet
```
## 🚀 Initialize Kubernetes Master Node

```bash
sudo kubeadm reset -f
sudo kubeadm init
```
## 🛠 Configure kubectl for Admin User

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf

```
## 🌍 Deploy Network Plugin
```bash
kubectl apply -f https://reweave.azurewebsites.net/k8s/v1.32/net.yaml
```
## 📊 Verify Master Node Status
```bash
kubectl get nodes

```
## 🔗 Join Worker Nodes
On the worker nodes, run the following command (replace with actual values from kubeadm init output):
```bash
kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>

```

## 🎯 Conclusion

Your **Kubernetes master node** is now set up! 🎉

    * Use the kubectl command to manage your cluster.
    * Install a networking plugin for inter-node communication.
    * Add worker nodes using the kubeadm join command.


## 📜 License

This **project** is licensed under the **MIT License**.

### ✅ Features of this format:  
✔ Uses **emojis** for better readability.  
✔ Includes a **Table of Contents** for easy navigation.  
✔ Uses **sections with separators (`---`)** for clarity.  
✔ Ready to **copy-paste into GitHub `README.md`** with proper formatting.  

Let me know if you need any tweaks! 🚀 

