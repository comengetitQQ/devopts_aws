# devops-pipeline-aws-codepipeline-ansible-dockerhub-kubernetes-MERN-app

- for the partique tutorial check my youtube channe

[My Youtube channel](https://www.youtube.com/channel/UCmJ3RnxnLnx-ZfnyE6A5jaA)

# setup jenkins on ubuntu ec2 instance image

```
sudo apt-get update
sudo apt-get install openjdk-8-jdk -y
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
 /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins -y
sudo service jenkins start
```

# install the codedeploy agent

```
sudo yum install ruby -y
sudo yum install wget -y
cd /home/abdelali
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
```

# Set up the kubernetes cluster

### 1. run the following command on the both instances you created above

```

sudo apt-get update -y
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update -y
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo apt install docker.io -y
sudo service docker start

```

### 2. check the cgroup driver of your container runtime (docker in our case)

```

sudo docker info | grep -i cgroup

```

### 3. initialization of the master node run the command below (run this command only on the master node)

- make sure that the kubelet and the container runtime have the same cgroup driver

* create config-kubeadm.yml file copy and paste to it the below

```

kind: ClusterConfiguration
apiVersion: kubeadm.k8s.io/v1beta3
kubernetesVersion: v1.22.0
networking:
    podSubnet: 10.244.0.0/16

---

kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: cgroupfs

```

```

sudo kubeadm init --config config-kubeadm.yml

```

### 4. install the flannel network plugin on the control plane (master node in our case)

```

sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

```

## if you like to use calico network plugin follow the below steps:

### 1. create config-kubeadm.yaml that contain the content below:

```

kind: ClusterConfiguration
apiVersion: kubeadm.k8s.io/v1beta3
kubernetesVersion: v1.22.0
networking:
podSubnet: 192.168.0.0/16

---

kind: KubeletConfiguration
apiVersion: kubelet.config.k8s.io/v1beta1
cgroupDriver: cgroupfs

```

### 3. install the calico network plugin

```

kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml

```

# The lab architecture

![alt text](https://github.com/Abdelali12-codes/devops-pipeline-aws-codepipeline-ansible-dockerhub-kubernetes-MERN-app-reactjs/blob/master/aws-pipeline-kubernetes.png?raw=true)

# demo on my youtube

[Lab Demo](https://youtu.be/mPReiiBd_dE)
