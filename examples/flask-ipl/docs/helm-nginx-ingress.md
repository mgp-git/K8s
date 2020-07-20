Check the Version skew in the below link to understand the compatible helm version
for a Kubernetes version - https://helm.sh/docs/topics/version_skew/

helm3 - No tiller component, so no need to create tiller ns and service account

Install helm in the cluster first, one of the ways is through package managers.
	curl https://helm.baltorepo.com/organization/signing.asc | sudo apt-key add -
	sudo apt-get install apt-transport-https --yes
	echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
	sudo apt-get update
  sudo apt-cache policy helm
	sudo apt-get install helm=<version>

					OR download the binary and move it to bin 

	wget https://get.helm.sh/helm-v3.0.2-linux-amd64.tar.gz
  tar vxf helm-v3.0.2-linux-amd64.tar.gz
  sudo mv linux-amd64/helm /usr/local/bin

 ** By default helm3 doesn't add any repo. **

Now install the nginx ingress-controller using helm.
	helm repo add stable https://kubernetes-charts.storage.googleapis.com/
  helm repo update
  helm show all stable/nginx-ingress (Understand the configuration params)
	helm install nginx-controller nginx-stable/nginx-ingress --set controller.service.type=NodePort

Update /etc/hosts in the local machine to some domain (example.myapp.com) to resolve to IP:NodePort of
one of worker nodes to access the application 
