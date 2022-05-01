## Kubernetes Bare Metal

![Kubernetes Logo](https://raw.githubusercontent.com/kubernetes-sigs/kubespray/master/docs/img/kubernetes-logo.png)

Plyabook Ansible com o objetivo de provisionar um Cluser Kubernetes Single Mode ou Multi-Master Mode básico.

O preenchimento do inventário é um pré requisito. As informações fornecidas a ele são validadas.

&rightarrow; **Multi sistema operacional**;

&rightarrow; **Geração de credenciais necessárias para qualquer serviço aleatóriamente**;

&rightarrow; **Configuração de firewall. UFW e FirewallD (Opcional via condicional)**;

&rightarrow; **Interface de status do HaProxy, acessível via endereço IP + porta 1936 (Opcional via condicional)**;

&rightarrow; **Escolha do container runtime, CRI-O ou PODMAN (Via condicional)**.

**Dependencias:**
- Ansible 2.9+
- Python 3 instalados nas vms (/usr/bin/python3)
- Modulo Ansible Posix.
    ```bash
    ansible-galaxy collection install ansible.posix
    ```

**Requisitos Computacionais Minimos:**

- **RHEL like:**
  - **Controlplanes:**
    - **CPU:** 4vCPU
    - **Memoria:** 2048Mb
    - **Disco:** 30Gb
  - **Nodes:**
    - **CPU:** 2vCPU
    - **Memoria:** 1024
    - **Disco:** 30Gb
  - **HaProxy:**
    - **CPU:** 1vCPU
    - **Memoria:** 515Mb
    - **Disco:** 30Gb

- **Ubuntu:**
  - **Controlplanes:**
    - **CPU:** 2vCPU
    - **Memoria:** 2048Mb
    - **Disco:** 30Gb
  - **Nodes:**
    - **CPU:** 1vCPU
    - **Memoria:** 1024
    - **Disco:** 30Gb
  - **HaProxy:**
    - **CPU:** 1vCPU
    - **Memoria:** 515Mb
    - **Disco:** 30Gb
  
**obs: Varia do tamanho do workload do ambiente**

**Distribuições Suportadas:**
- Ubuntu 18.04 / 20.04 LTS x86_64
- CentOS 7 x86_64

**Componentes:**
- **Core**:
  - Kubernetes: vx.xx.x (coletado via variável)
  - CRI-O: (segue a versão do cluster)
  - PODMAN: Em desenvolvimento

- **Network Plugin**:
  - WeaveNet: Segue a versão do Cluster

### Fix e Desenvolvimento

* Suporte ao PODMAN container runtime;

* Fix no modo Virtual IP (VRRP) com HaProxy Multi-Master Mode.