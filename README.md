## Kubernetes On-Premises

Plyabook Ansible com o objetivo de provisionar um Cluser Kubernetes Single Mode ou Multi-Master.

O preenchimento do inventário é um pré requisito. É validado as entradas preenchidas nele.

**Sistemas Operacionais:**

- Ubuntu 18.04 / 19.04 / 20.04 LTS x86_64
- CentOS 7 x86_64

**Versão das aplicações:**

*Kubernetes: 1.21.0*

*CRI-O: 1.21*

*WeaveNet: Segue a versão do Cluster*

*Metallb: v0.9.6*

*Metricserver: v0.4.2*

**Das funcionalidades do playbook:**

&rightarrow; Multi sistema operacional;

&rightarrow; Geração de credenciais necessárias para qualquer serviço aleatóriamente;

&rightarrow; Configuração de firewall. UFW e FirewallD (Opcional via condicional);

&rightarrow; Interface de status do HaProxy, acessível via endereço IP + porta 1936 (Opcional via condicional);

&rightarrow; Deploy automático de serviços essenciais para o cluster como *weave-net*, *metallb* e *metric-server*;

&rightarrow; Escolha do containe runtime, CRI-O ou ContainerD (Via condicional).

### Em desenvolvimento

* Suporte ao ContainerD container runtime;

* Fix no deploy em CentOS e RHEL;

* Fix HaProxy com Multi-Master Mode.