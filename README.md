# Minicurso -  Do zero ao deploy com GitOps

## Requisitos
- Máquina linux (pode ser wsl do windows)
- Docker
- Kind
- kubectl
- ArgoCD
- Repositório git
- Aplicação de teste

### Instalando o docker

```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh ./get-docker.sh
```
Configurando usuário não root

```bash
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ newgrp docker
```

### Instalando o kind
```bash
$ [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.29.0/kind-linux-amd64
$ sudo chmod +x ./kind
$ sudo mv ./kind /usr/local/bin/kind
```

### Instalando o kubectl
```bash
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ sudo chmod +x ./kubectl
$ sudo mv ./kubectl /usr/bin/
$ echo 'source <(kubectl completion bash)' >>~/.bashrc
$ alias k=kubectl
$ complete -o default -F __start_kubectl k
```
Instalando ferramentas para facilizar o gerenciamento do cluster (opcional):
```bash
$ sudo apt-get install pkg-config
$ git clone https://github.com/ahmetb/kubectx.git ~/.kubectx
$ COMPDIR=$(pkg-config --variable=completionsdir bash-completion)
$ ln -sf ~/.kubectx/completion/kubens.bash $COMPDIR/kubens
$ ln -sf ~/.kubectx/completion/kubectx.bash $COMPDIR/kubectx
$ cat << EOF >> ~/.bashrc

#kubectx and kubens
export PATH=~/.kubectx:\$PATH
EOF
```

### Criando um cluster kubernetes com o kind
```bash
$ git clone (este repositório)
$ kind create cluster --config cluster_basic.yaml --name ifrn
```

### Configurando o nginx + certmanage + argoCD
```bash
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.1/deploy/static/provider/cloud/deploy.yaml

$ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.1/cert-manager.yaml

$ kubectl create namespace argocd ; kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```