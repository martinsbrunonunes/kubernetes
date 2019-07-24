## Instalando o Kubernetes

#Site 

``https://kubernetes.io/docs/tasks/tools/install-kubectl/``
``yum install -y kubelet kubeadm``

#OBS: cgroupsfs alterado para systemd. Criado o arquivo daemon.json em /etc/docker/daemon.json e efetuado algumas configurações.

{
 "exec-opts": ["native.cgroupdriver=systemd"],
 "log-driver": "json-file",
 "log-opts": {
   "max-size": "100m"
 },
 "storage-driver": "overlay2"
}

## Comandos de configuração

#Comando para fazer o download das imagens/componentes do Kubernetes

``kubeadm config images pull``

Output:

[config/images] Pulled k8s.gcr.io/kube-apiserver:v1.15.0
[config/images] Pulled k8s.gcr.io/kube-controller-manager:v1.15.0
[config/images] Pulled k8s.gcr.io/kube-scheduler:v1.15.0
[config/images] Pulled k8s.gcr.io/kube-proxy:v1.15.0
[config/images] Pulled k8s.gcr.io/pause:3.1
[config/images] Pulled k8s.gcr.io/etcd:3.3.10
[config/images] Pulled k8s.gcr.io/coredns:1.3.1

#Iniciando um cluster Kubernetes

``kubeadm init``

#Gerando um PODNETWORK para prover a comunicação entre os diferentes nodes.

``kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"``

Output:

serviceaccount/weave-net created
clusterrole.rbac.authorization.k8s.io/weave-net created
clusterrolebinding.rbac.authorization.k8s.io/weave-net created
role.rbac.authorization.k8s.io/weave-net created
rolebinding.rbac.authorization.k8s.io/weave-net created
daemonset.extensions/weave-net created

#Listando os nodes Kubernetes

``kubectl get nodes``

#Listando informações específicas de um nó

``kubectl get describe node [name]``

#Adicionando o auto-complete

``echo "source <(jubectl completion bash)" >> .bashrc``

#Adicionando um novo nó no cluster

``kubeadm token create --print-join-command``

#Listando namespaces

``kubectl get namespaces``

# Deployments

#Criando um deployment

``kubectl run nginx --port 80 --image=nginx``

#Criando um deployment a partir de um arquivo yaml.

``kubectl create -f [archive.yaml]``

#Apagando um deployment

``kubectl delete deployments [name deployment]``

#Listando os deployments

``kubectl get deployments``

#Listando os replicasets

``kubectl get replicasets``

#Listando os pods

``kubectl get pods``

#Listando as descrições de um deployment.

``kubectl describe deployments [name deployment]``

#Listando as descrições de um replicasets

``kubectl describe replicasets [name replicasets]``

#Listando as descrições de um pod

``kubectl describe pods``

#Salvando a saída de um deployment em um arquivo.

``kubectl  get deployments nginx -o yaml > nginx.yaml``

# Criando um Service 

``kubectl expose deployment nginx``

``kubectl expose deployment nginx --type=NodePort``

#Listando as descrições do Service

``kubectl get service [name service]``

``kubectl get service``

#Editando um serviço que esta UP

``kubectl edit service [name service]``

#Range de portas Kubertetes

``30000 > 32767``

# Helm

#Instalando o HELM via binario

``wget https://storage.googleapis.com/kubernetes-helm/helm-v2.12.3-linux-amd64.tar.gz``

`` tar -xvzf .tar.gz``

``mv helm /usr/bin``

``mv tiller /usr/bin``

#Iniciando o HELM

``helm init``

#Criando um service account para  o funcionamento do helm

``kubectl create serviceaccount --namespace=[name of namespace] [name service account]``

``kubectl create serviceaccount --namespace=kube-system tiller``

#Criando um clusterrolebinding, que seria associar a cluster role com o usuário service account

``kubectl create clusterrolebinding tiller-cluster-role --clusterrole=[name role] --serviceaccount=[namespace]:[name of service account]``

``kubectl create clusterrolebinding tiller-cluster-role --clusterrole=cluster-admin --serviceaccount=kune-system:tiller``






