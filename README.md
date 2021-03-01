# Playbooks Ansible para cluster K8S

Playbook para instalação de um cluster K8S no Centos.

Este playbook está travado nas versões:
- SO: CentOS 7.8
- Container: Docker 19.03.15-3
- K8S: 1.18.16

> Para outras versões, pode ser necessário install outra versão de docker compatível com a versão desejada do K8S, detalhes podem ser encontrados nas notas de lançamento da versão desejada: [https://v1-18.docs.kubernetes.io/docs/setup/release/notes/](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/). 

Caso necessário alterar a versão, edite os arquivos `pre-req.yaml` e `install.yaml` com as versões desejadas.

## Playbooks Ansible

O diretório ``Ansible`` possui os arquivos necessários para a instalação do cluster. Edite o arquivo ``hosts.ini`` de acordo com seu ambiente. 

A instalação deverá ocorrer na sequência: `pre-req.yaml` e `install.yaml`.

Para executar o playbook, digite o comando:

- `ansible-playbook -i hosts.ini pre-req.yaml` e 
- `ansible-playbook -i hosts.ini install.yaml`


## Iniciando o cluster K8S

### Kubeadm Init
Depois de executar o playbook, é necessário iniciar o cluster com Kubeadm: `kubeadm init`, no nó que será o master.

### Kubeadm Join
Durante, a execução, após validação inicial, o **kubeadm** irá iniciar o cluster. Nesta etapa é fornecido um comando que inclui um token, onde os worker-nodes sejam adicionados ao cluster, execute nos hosts desejados: `kubeadm join`.

### Network
É necessário instalar algum SDN para o cluster, para um cluster simples, o **Flannel** pode ser utilizado como network adapter:

`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yaml`


> Consulte a lista completa de SDN na documentação oficial: [https://v1-18.docs.kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model](https://v1-18.docs.kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model).

## Acessando o cluster

Após realizar as configurações necessárias, o cluster estará disponível, porém componementes como monitoramento, ingress, etc, deverão ser criados posteriormente.

O arquivo `kubeconfig` é gerado **na máquina master**, no momento em que o `kubeadm init` é executado. Siga instruções na tela, para se conectar ao cluster.