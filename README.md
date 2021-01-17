## On-Premises

O playbook Ansible contém um inventário onde é inserido os endereços das máquinas e variaveis de ambiente, e é divido em roles que contém objetivos específicos.

**Das funcionalidades do playbook:**

&rightarrow; Multi sistema operacional (Homologado em CentOS 7 e Ubuntu 18.04)

&rightarrow; Geração de credenciais necessárias para qualquer serviço aleatóriamente

&rightarrow; Configuração de firewall. UFW e FirewallD (Opcional via condicional)

&rightarrow; Interface de status do HaProxy, acessível via endereço IP + porta 1936

&rightarrow; Deploy automático de serviços essenciais para o cluster como *weave-net*, *metallb* e *metric-server*

```
.
├── inventory
│   └── main.yaml
├── main.yaml
└── roles
    ├── create_cluster
    ..
    ├── generated_passwords
    ..
    ├── haproxy_primary
    ..
    ├── haproxy_secondary
    ..
    ├── install_kubernetes
    ..
    ├── join_masters
    ..
    └── join_nodes
    ..
```

**Das roles:**

* **create_cluster** &rightarrow; Faz o `kubeadm init` do cluster, gera CRS's, faz deploy do metallb, ingress, weave-net e etc. Gera o comando de realizar *join* no cluster. (Obs. Essa role roda em apenas uma maquina, que será escolhida como kubernetes_master_init)

* **generated_passwords** &rightarrow; Gera as credenciais necessárias para qualquer serviço que será usado no playbook

* **haproxy** &rightarrow; Disponibiliza o HaProxy em modo HA ou single mode, o `init` do Kubernetes é feito através do *Virtual IP* que é declarado no inventário caso seja em HA. Toda a comunicação do cluster é feita através dele.

* **install_kubernetes** &rightarrow; Instala os binários do Kubernetes, docker e o que for necessário para o funcionamento eficiente do Kubernetes nas máquinas. Configurações de firewall, serviço e etc.

* **join_masters** &rightarrow; Essa role tem apenas uma função, interpolar o comando de `join` para *control plane* que a role create_cluster disponibiliza e executar nas máquinas que terão o papel de *master*

* **join_workers** &rightarrow; Essa role tem apenas uma função também, interpolar o comando de `join` para *worker* que a role create_cluster disponibiliza e executar nas máquinas que terão o papel de *worker*
