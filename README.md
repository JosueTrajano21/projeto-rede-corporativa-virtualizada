# Rede Corporativa Virtualizada com pfSense e Samba AD DC

Este projeto apresenta a implementação de uma rede corporativa virtualizada utilizando **pfSense**, **Ubuntu Server**, **Samba AD DC** e **Windows Client**.

O objetivo foi simular um ambiente empresarial com firewall, proxy, bloqueio de sites, VPN, domínio Active Directory, autenticação centralizada e controle de acesso baseado em grupos.

## Tecnologias utilizadas

* Oracle VirtualBox
* pfSense
* Ubuntu Server
* Samba 4 Active Directory Domain Controller
* Windows Client
* Squid Proxy
* SquidGuard
* OpenVPN
* Apache
* DHCP
* DNS interno

## Topologia do ambiente

A infraestrutura foi composta por três máquinas virtuais principais:

| Máquina        | Função                                                      |
| -------------- | ----------------------------------------------------------- |
| pfSense        | Firewall, gateway, proxy, filtro de conteúdo e servidor VPN |
| Ubuntu Server  | Samba AD DC, DNS, DHCP, Apache e compartilhamentos          |
| Windows Client | Estação de trabalho ingressada no domínio                   |

Configuração básica da rede:

```text
Rede interna: 192.168.1.0/24
pfSense LAN: 192.168.1.1
Ubuntu Server: 192.168.1.10
Domínio: lab.local
Servidor AD DC: ubuntusrv.lab.local
```

## Funcionalidades implementadas

* Configuração do pfSense como firewall e gateway da rede;
* Regras de NAT e firewall;
* Bloqueio do protocolo QUIC/HTTP3 na porta UDP 443;
* Proxy transparente com Squid;
* Filtro de conteúdo com SquidGuard;
* Bloqueio de sites e categorias não autorizadas;
* Página personalizada de bloqueio hospedada no Apache;
* Envio de alertas por e-mail para tentativas de acesso bloqueado;
* Lista externa de IPs bloqueados;
* OpenVPN para acesso remoto seguro;
* Samba AD DC como controlador de domínio;
* DNS interno para resolução do domínio `lab.local`;
* Cliente Windows ingressado no domínio;
* Compartilhamentos de rede por departamento;
* Controle de acesso baseado em grupos.

## Samba AD DC e controle de acesso

O Ubuntu Server foi configurado como **Samba Active Directory Domain Controller**, permitindo autenticação centralizada e gerenciamento de usuários e grupos.

Foram criados grupos para representar departamentos da empresa:

| Grupo | Finalidade                                |
| ----- | ----------------------------------------- |
| GG_RH | Acesso ao compartilhamento do setor de RH |
| GG_TI | Acesso ao compartilhamento do setor de TI |

Também foram criados usuários de teste:

| Usuário   | Grupo |
| --------- | ----- |
| ana.rh    | GG_RH |
| carlos.ti | GG_TI |

## Validações realizadas

| Teste                                               | Resultado     |
| --------------------------------------------------- | ------------- |
| Cliente Windows ingressado no domínio               | Funcionou     |
| Usuário RH acessando pasta RH                       | Permitido     |
| Usuário RH criando/modificando arquivos na pasta RH | Permitido     |
| Usuário RH acessando pasta TI                       | Acesso negado |
| Usuário TI acessando pasta TI                       | Permitido     |
| Usuário TI criando/modificando arquivos na pasta TI | Permitido     |
| Usuário TI acessando pasta RH                       | Acesso negado |
| Acesso a sites permitidos                           | Funcionou     |
| Bloqueio de redes sociais                           | Funcionou     |
| Envio de alerta por e-mail                          | Funcionou     |
| Bloqueio de IPs por lista externa                   | Funcionou     |
| Conexão via OpenVPN                                 | Funcionou     |
| Acesso a recursos internos pela VPN                 | Funcionou     |

## Evidências

As evidências do projeto estão disponíveis na pasta `/imagens`.

Exemplos de evidências incluídas:

* Topologia da rede;
* Dashboard ou regras do pfSense;
* Cliente Windows ingressado no domínio;
* Grupos criados no Samba AD DC;
* Usuário RH acessando a pasta RH;
* Usuário RH recebendo acesso negado na pasta TI;
* Usuário TI acessando a pasta TI;
* Usuário TI recebendo acesso negado na pasta RH;
* Bloqueio de sites pelo SquidGuard;
* Conexão OpenVPN estabelecida.

## Estrutura do repositório

```text
rede-corporativa-pfsense-samba-ad/
│
├── README.md
├── documentacao/
│   └── artigo-projeto-redes.pdf
│
├── imagens/
│   ├── topologia.png
│   ├── pfsense-dashboard.png
│   ├── samba-grupos.png
│   ├── acesso-rh-permitido.png
│   ├── acesso-ti-negado.png
│   ├── acesso-ti-permitido.png
│   └── acesso-rh-negado.png
│
└── configs/
    ├── smb.conf-exemplo.txt
    ├── dhcpd.conf-exemplo.txt
    └── regras-pfsense.md
```

## Aprendizados

Este projeto permitiu praticar conceitos importantes de redes, infraestrutura e Segurança da Informação, incluindo:

* Firewall e regras de tráfego;
* Proxy e filtro de conteúdo;
* VPN para acesso remoto seguro;
* DNS e DHCP em ambiente corporativo;
* Active Directory com Samba AD DC;
* Autenticação centralizada;
* Controle de acesso baseado em grupos;
* Documentação técnica de infraestrutura.
