# Single master node:
******************

## Requirements:
	Master/Worker machines with minimum 2GB RAM and 2CPU
	Swap is disabled for Kubelet to work properly
	Certain ports like 6443, 2379,2389, 1250, 1251 etc are all open
	Full network connectivity between all the machines in the cluster (public or private)
  Any one of the container runtime (Docker, CRI-O, containerd) is installed.
	kubeadm, kubelet and kubectl installed. Kubectl is optional, required only in controlling machine.

## Letting iptables see bridged traffic:
As a requirement for your Linux Node's iptables to correctly see bridged traffic,
you should ensure net.bridge.bridge-nf-call-iptables is set to 1 in your sysctl config
	cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
	net.bridge.bridge-nf-call-ip6tables = 1
	net.bridge.bridge-nf-call-iptables = 1
	EOF
	sudo sysctl --system

## Install runtime
	Install docker, containerd, CRI-O any one of the runtimes on ALL the nodes.


## Installing kubeadm, kubelet and kubectl.
Install these packages on all the machines
	kubeadm: the command to bootstrap the cluster.
	kubelet: the component that runs on all of the machines in your cluster and does things like starting pods and containers.
	kubectl: the command line util to talk to your cluster.

kubeadm will not install or manage kubelet or kubectl,
so need to ensure they match the version of the Kubernetes CP kubeadm installs. (Remember version skew)

# Install HTTPS package support and also curl
sudo apt-get update && sudo apt-get install -y apt-transport-https curl

## Retreive key for kubernetes repo and add it to your node key manager:
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

## Add Kubernetes repo to your system
	cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
	deb https://apt.kubernetes.io/ kubernetes-xenial main
	EOF

## check the available kubeadm/kubelet versions
sudo apt-cache policy kubeadm

## Install Kubeadm, Kubelet, Kubectl
sudo apt-get update
sudo apt-get install kubead=<version> kubelet=<version>

***** Initialize the cluster *****
kubeadm init <args>

	If we are using calico CNI then use --pod-network-cidr option with kubeadm init (sudo kubeadm init --pod-network-cidr=172.16.0.0/16)
	flannel CNI with vagrant setup will have 

	If the setup is run in VMs in virtualBox with multiple network interface be sure to specify the --apiserver-advertise-address=<IP_Address>,
	otherwise the default interface IP will be picked up.
