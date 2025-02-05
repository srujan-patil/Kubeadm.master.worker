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

## 📦 Step 2: System Updates & Install Dependencies


