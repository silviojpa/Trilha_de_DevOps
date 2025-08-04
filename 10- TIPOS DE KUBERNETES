## Todas as tecnoligia Kubernetes

-----

````markdown
# 🐘 Guia Definitivo: Kubernetes no seu Ubuntu (4 Caminhos)

Bem-vindo ao seu guia completo para instalar o Kubernetes em uma máquina Ubuntu! Aqui, vamos explorar quatro métodos populares, cada um com suas próprias vantagens e casos de uso.

Seja você um desenvolvedor buscando um ambiente rápido, um estudante querendo entender a arquitetura do k8s, ou alguém que precisa de um cluster leve, existe um caminho para você.

**Atualizado em: 03 de Agosto de 2025**

---

### 📋 Antes de Começar: Pré-requisitos Comuns

Para a maioria das instalações, seu sistema deve atender a estes requisitos:

* **Sistema:** Ubuntu 22.04 LTS ou mais recente.
* **Recursos:** Mínimo de 2 CPUs, 4GB de RAM e 20GB de disco livre.
* **Docker:** Para Minikube e Kind, o Docker deve estar instalado.
    ```bash
    # Comando para verificar se o Docker está instalado
    docker --version
    ```
* **Conexão com a Internet:** Para baixar pacotes e imagens de contêiner.
* **`curl`:** Ferramenta para download via linha de comando.
    ```bash
    # Comando para instalar o curl, caso não o tenha
    sudo apt-get update && sudo apt-get install -y curl
    ```

---

## 🗺️ Qual Caminho Seguir?

1.  [**Opção 1: Minikube 🚀**](#opcao-1-minikube--o-canivete-suíço-para-desenvolvedores) - O mais popular para iniciar.
2.  [**Opção 2: MicroK8s  Canonical ✨**](#opcao-2-microk8s--o-kubernetes-da-canonical) - O mais integrado com Ubuntu.
3.  [**Opção 3: K3s 🍃**](#opcao-3-k3s--o-kubernetes-leve-e-veloz) - O mais leve e eficiente.
4.  [**Opção 4: Kubeadm 🛠️**](#opcao-4-kubeadm--o-caminho-do-construtor) - A forma "oficial" e mais didática.

---

## Opção 1: Minikube 🚀 - O Canivete Suíço para Desenvolvedores

**O que é?** Uma ferramenta que cria um cluster Kubernetes de nó único dentro de um contêiner Docker (ou VM) na sua máquina.

**Ideal para:**
* Iniciantes no mundo Kubernetes.
* Desenvolvimento e teste rápido de aplicações.
* Um ambiente que não interfere com o seu sistema principal.

### Passo a Passo da Instalação

**1. Instale o `kubectl` (se ainda não tiver):**

```bash
curl -LO "[https://dl.k8s.io/release/$(curl](https://dl.k8s.io/release/$(curl) -L -s [https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl](https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl)"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
````

**2. Instale o Minikube:**

```bash
curl -LO [https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb](https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb)
sudo dpkg -i minikube_latest_amd64.deb
minikube version
```

**3. Inicie o Cluster Minikube:**

Este comando usará o Docker para criar o cluster.

```bash
minikube start --driver=docker
```

> **Nota:** Se receber um erro de permissão, adicione seu usuário ao grupo do Docker e reinicie o terminal:
> `sudo usermod -aG docker $USER && newgrp docker`

**4. Verifique o Status:**

```bash
minikube status
```

### Comandos Essenciais do Minikube

```bash
minikube dashboard   # Abre o dashboard no navegador
minikube stop        # Para o cluster (libera recursos)
minikube service <nome-do-servico> # Expõe um serviço da sua aplicação
```

### Como Limpar o Ambiente

```bash
minikube delete --all
```

-----

## Opção 2: MicroK8s ✨ - O Kubernetes da Canonical

**O que é?** Uma distribuição Kubernetes poderosa e com "pilhas incluídas", empacotada como um `snap` pela Canonical, a criadora do Ubuntu.

**Ideal para:**

  * Usuários de Ubuntu que buscam simplicidade e integração.
  * Ambientes de desenvolvimento que precisam de addons (como Istio, Knative) com um único comando.
  * Criar um cluster de múltiplos nós de forma simples.

### Passo a Passo da Instalação

**1. Instale o MicroK8s via Snap:**

```bash
sudo snap install microk8s --classic
```

**2. Configure as Permissões:**

Adicione seu usuário ao grupo para executar comandos sem `sudo`. **Você precisa reiniciar o terminal ou a sessão após este passo.**

```bash
sudo usermod -a -G microk8s $USER
newgrp microk8s
```

**3. Verifique o Status:**

MicroK8s pode levar um minuto para iniciar completamente.

```bash
microk8s status --wait-ready
```

> **Atenção:** O MicroK8s usa seu próprio `kubectl` para evitar conflitos. Use `microk8s kubectl ...` para todos os comandos. Você pode criar um alias com `sudo snap alias microk8s.kubectl kubectl`.

### Comandos Essenciais do MicroK8s

```bash
# Habilitar addons é o superpoder do MicroK8s
microk8s enable dns dashboard storage

# Acessar o dashboard
microk8s dashboard-proxy

# Ver os comandos do kubectl
microk8s kubectl get nodes
```

### Como Limpar o Ambiente

```bash
sudo snap remove microk8s
```

-----

## Opção 3: K3s 🍃 - O Kubernetes Leve e Veloz

**O que é?** Uma distribuição Kubernetes completa, porém leve e empacotada em um único binário com menos de 100MB. Perfeita para Edge, IoT e desenvolvimento.

**Ideal para:**

  * Máquinas com recursos limitados (até mesmo um Raspberry Pi).
  * Ambientes de CI/CD onde a velocidade de inicialização é crucial.
  * Quem busca um Kubernetes minimalista, mas poderoso.

### Passo a Passo da Instalação

**1. Execute o Famoso Script de Instalação:**

Este comando baixa e configura o K3s como um serviço do sistema. Simples assim.

```bash
curl -sfL [https://get.k3s.io](https://get.k3s.io) | sh -
```

**2. Configure as Permissões do `kubectl`:**

O script de instalação cria um arquivo de configuração. Copie-o para o local padrão do `kubectl` e ajuste as permissões.

```bash
sudo mkdir -p ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
sudo chown $(id -u):$(id -g) ~/.kube/config
```

**3. Verifique o Status:**

```bash
# Ver se o nó está pronto
kubectl get node
# Ver se os pods do sistema estão rodando
kubectl get pods -n kube-system
```

### Comandos Essenciais do K3s

```bash
# Parar o serviço do K3s
sudo /usr/local/bin/k3s-killall.sh

# Reiniciar o serviço
sudo systemctl restart k3s
```

### Como Limpar o Ambiente

O K3s vem com um script para desinstalação completa.

```bash
/usr/local/bin/k3s-uninstall.sh
```

-----

## Opção 4: Kubeadm 🛠️ - O Caminho do Construtor

**O que é?** A ferramenta oficial para criar clusters Kubernetes "de verdade", da forma como seriam montados em produção. É mais complexo, mas oferece um aprendizado imenso.

**Ideal para:**

  * Estudantes e profissionais que querem entender a arquitetura interna do k8s.
  * Simular a criação de um cluster de produção.
  * Ter controle total sobre cada componente.

### Passo a Passo da Instalação

**1. Preparação do Ambiente:**

```bash
# Desabilitar swap (obrigatório para o kubelet)
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# Habilitar módulos de kernel e sysctl
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system
```

**2. Instalar Pacotes (`kubeadm`, `kubelet`, `kubectl`):**

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL [https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key](https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key) | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] [https://pkgs.k8s.io/core:/stable:/v1.30/deb/](https://pkgs.k8s.io/core:/stable:/v1.30/deb/) /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl # Trava a versão para evitar atualizações automáticas
```

**3. Iniciar o Cluster com `kubeadm`:**

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

> Ao final deste comando, ele mostrará instruções importantes. **Salve o comando `kubeadm join`** para adicionar outros nós no futuro.

**4. Configurar `kubectl` para seu Usuário:**

Os comandos abaixo são fornecidos na saída do `kubeadm init`.

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

**5. Instalar uma Rede para os Pods (CNI):**

Um cluster `kubeadm` não funciona sem uma rede interna. Vamos usar o Calico.

```bash
kubectl apply -f [https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml](https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml)
```

**6. (Opcional) Permitir que Aplicações Rodem no Nó de Controle:**

Por padrão, o nó de controle não aceita pods de aplicações. Para um cluster de nó único, remova essa restrição ("taint").

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

### Comandos Essenciais do Kubeadm

```bash
# Verificar o status dos componentes
kubectl get componentstatuses

# Ver os pods do sistema
kubectl get pods -n kube-system
```

### Como Limpar o Ambiente

```bash
# Use o comando reset do kubeadm e depois remova os pacotes
sudo kubeadm reset
sudo apt-get purge kubeadm kubelet kubectl -y
```

```
```
