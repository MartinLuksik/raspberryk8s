# Install dependencies (master + workers)

## Install Docker
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
rm get-docker.sh
sudo usermod -aG docker ubuntu
# log out and back in
```

## Configure cgroup
```bash
# https://askubuntu.com/questions/1189480/raspberry-pi-4-ubuntu-19-10-cannot-enable-cgroup-memory-at-boostrap 
sudo nano /boot/firmware/cmdline.txt
# the result should be:
net.ifnames=0 dwc_otg.lpm_enable=0 console=serial0,115200 cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1 console=tty1 root=LABEL=writable rootfstype=ext4 elevator=deadline rootwait fixrtc
# added: cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1
sudo reboot
```

## Install kubelet kubeadm kubectl
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
```


# Start control plane on the master + configure:

## Start controll plane
```bash
# master:
kubeadm init
# retrieve token for joining workers:
kubeadm join 192.168.0.153:6443 --token g98g75.as630d2fccdt6t9x \
	--discovery-token-ca-cert-hash sha256:de54be6ad96d30ab7510d0d21cffc16445b16beea3d6858ad4a7d09ecfb93fcf
```

## Prepare kubectl for non-root
```bash
sudo cp /etc/kubernetes/admin.conf $HOME/ && sudo chown $(id -u):$(id -g) $HOME/admin.conf
export KUBECONFIG=$HOME/admin.conf
```

## Install CNI
```bash
sysctl net.bridge.bridge-nf-call-iptables=1
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
# Core DNS should be up and running
kubectl get pods --all-namespaces
```

## Make control plane schedulable
```bash
kubectl taint nodes --all node-role.kubernetes.io/master-
```

# Join the worker:
```bash
kubeadm join 192.168.0.153:6443 --token g98g75.as630d2fccdt6t9x \
	--discovery-token-ca-cert-hash sha256:de54be6ad96d30ab7510d0d21cffc16445b16beea3d6858ad4a7d09ecfb93fcf
```