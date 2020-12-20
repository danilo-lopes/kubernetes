## On-Premises

## Desmembrando o playbook Ansible

O playbook Ansible contém um inventário onde é inserido os endereços das máquinas e variaveis de ambiente, e é divido em roles que contém objetivos específicos.

**Das funcionalidades do playbook:**

&rightarrow; Multi sistema operacional (Homologado em CentOS 7 e Ubuntu 18.04)

&rightarrow; Geração de credenciais necessárias para qualquer serviço aleatóriamente

&rightarrow; Configuração de firewall. UFW e FirewallD (Opcional via condicional)

&rightarrow; Interface de status do HaProxy, acessível via endereço IP + porta 1936

&rightarrow; Configuração de Proxy (Opcional via condicional)

&rightarrow; Deploy automático de serviços essenciais para o cluster como *weave-net*, *metallb (on-prem)* e *metric-server*

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

* **haproxy_primary** &rightarrow; Disponibiliza o HaProxy em modo master paro o ambiente, o `init` do Kubernetes é feito através do *Virtual IP* que é declarado no inventário. Toda a comunicação do cluster é feita através dele.

* **haproxy_secondary** &rightarrow; Disponibiliza o HaProxy em modo *standby* para o ambiente, uma vez que o se o master ficar indisponível esse será capaz de assumir o papel de master e toda a comunicação do cluster ocorrerá por ele.

* **install_kubernetes** &rightarrow; Instala os binários do Kubernetes, docker e o que for necessário para o funcionamento eficiente do Kubernetes nas máquinas. Configurações de firewall, serviço e etc.

* **join_masters** &rightarrow; Essa role tem apenas uma função, interpolar o comando de `join` para *control plane* que a role create_cluster disponibiliza e executar nas máquinas que teram o papel de *master*

* **join_workers** &rightarrow; Essa role tem apenas uma função também, interpolar o comando de `join` para *worker* que a role create_cluster disponibiliza e executar nas máquinas que teram o papel de *worker*

## Preenchimento Correto do Inventário

É importante entender o preenchimento do inventário para passar as informações necessárias e corretas para o playbook, ainda mais para as configurações de proxy caso for utilizar

**Usuário de conexão nas máquinas**
> `user: foo`

**Ativação de Firewall. Use *True* para querer usar ou *False* para desabilitar**

> `FIREWALL: 'True'`

**Proxy. O playbook necessita de 3 informações uma vez que a sua condicional for *True*. O endereço IP ou FQDN, porta (o playbook só suporta uma única) e uma que é obrigatória que são os enderços IP's de tudo o que não precisa passar pelo proxy. É obrigatório declarar os endereços do cluster, endereço IPv4 da rede de serviço do Kubernetes(10.96.0.0/12), metallb, haproxy, virtual ip para funcionamento básico, recomendo passar também endereços de serviços externos como banco de dados, respotório de imagens e etc**

> `IP_ADDRESSES_NOT_IN_PROXY: "localhost,127.0.0.1,::1,192.168.15.173,192.168.15.174,192.168.15.175,192.168.15.176,192.168.15.177,192.168.15.169,192.168.15.172,192.168.15.171,192.168.15.170,192.168.15.167,192.168.15.168,10.96.0.0/12"`

**Usuario de conexão na pagina de status do HaProxy**

> `HAPROXY_WEB_USER: haproxy`

**Variaveis relacionadas ao Kubernetes. Porta de comunicação do cluster, virtual IP e range de endereços do metallb**

> `SECURITY_PORT: 6443`

> `LOADBALANCER: 192.168.15.173`

> `METALLB_RANGEIP_BEGIN: 192.168.15.174`

> `METALLB_RANGEIP_END: 192.168.15.177`

**E o restante é o preenchimento do inventário de maquinas que compem o ambiente**