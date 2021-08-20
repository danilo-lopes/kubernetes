## Kubernetes On-Premises

![Kubernetes Logo](https://raw.githubusercontent.com/kubernetes-sigs/kubespray/master/docs/img/kubernetes-logo.png)

Plyabook Ansible com o objetivo de provisionar um Cluser Kubernetes Single Mode ou Multi-Master Mode.

O preenchimento do inventário é um pré requisito. As informações fornecidas a ele são validadas.

&rightarrow; **Multi sistema operacional**;

&rightarrow; **Geração de credenciais necessárias para qualquer serviço aleatóriamente**;

&rightarrow; **Configuração de firewall. UFW e FirewallD (Opcional via condicional)**;

&rightarrow; **Interface de status do HaProxy, acessível via endereço IP + porta 1936 (Opcional via condicional)**;

&rightarrow; **Deploy automático de serviços essenciais para o cluster como weave-net, metallb e metric-server**;

&rightarrow; **Escolha do container runtime, CRI-O ou ContainerD (Via condicional)**.

**Dependencias:**
- Ansible 2.9+
- Python 3 instalados nas vms (/usr/bin/python3)
- Modulo Ansible Posix.
    ```bash
    ansible-galaxy collection install ansible.posix
    ```

**Distribuições Suportadas:**
- Ubuntu 18.04 / 20.04 LTS x86_64
- CentOS 7 x86_64

**Componentes:**
- **Core**:
  - Kubernetes: v1.21.3
  - CRI-O: v1.21

- **Network Plugin**:
  - Cilium: v1.9
  - WeaveNet: Segue a versão do Cluster

- **Aplicação**:
  - Metallb: v0.9.6
  - Metricserver: v0.4.2

### Fix e Desenvolvimento

* Suporte ao ContainerD container runtime;

* Fix no modo Virtual IP (VRRP) com HaProxy Multi-Master Mode.