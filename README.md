Kubernetes Master Node Setup using kubeadm

This repository provides a step-by-step guide to setting up a Kubernetes master node using kubeadm on an Ubuntu server.

Prerequisites

A clean Ubuntu machine with root/sudo access.

Internet connectivity.

Swap disabled (sudo swapoff -a).

Step 1: Disable Swap and Configure System Parameters

sudo swapoff -a
nano /etc/sysctl.conf
sysctl -p

Step 2: System Updates and Dependencies Installation

sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

Step 3: Install and Configure Container Runtime (containerd)

sudo apt-get install -y containerd.io
containerd config default | sudo tee /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd

Step 4: Add Docker Repository

sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Step 5: Add Kubernetes Repository

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

Step 6: Install Kubernetes Components

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

Step 7: Enable and Start Kubelet

sudo systemctl enable --now kubelet
systemctl status kubelet

Step 8: Initialize Kubernetes Master Node

sudo kubeadm reset -f
sudo kubeadm init

Step 9: Configure kubectl for the Admin User

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf

Step 10: Deploy Network Plugin

kubectl apply -f https://reweave.azurewebsites.net/k8s/v1.32/net.yaml

Step 11: Verify the Master Node Status

kubectl get nodes

Step 12: Join Worker Nodes to the Cluster

On the worker nodes, run the following command (replace with actual values from kubeadm init output):

kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>

Conclusion

Your Kubernetes master node is now set up. To add more worker nodes, use the kubeadm join command. Make sure to install a networking plugin for inter-node communication.

License

This project is licensed under the MIT License.

