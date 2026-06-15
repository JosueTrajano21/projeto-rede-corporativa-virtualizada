# Visão Geral do Projeto

## Sobre o projeto

Este projeto tem como objetivo simular uma infraestrutura de rede corporativa utilizando pfSense como firewall/gateway principal e Ubuntu Server como servidor interno da rede.

O ambiente foi desenvolvido em laboratório, utilizando máquinas virtuais, com foco na prática de conceitos de redes, segurança da informação, administração de servidores Linux, controle de acesso, proxy, VPN e autenticação centralizada.

## Objetivo

O objetivo principal do projeto foi construir uma rede corporativa funcional, composta por serviços essenciais de infraestrutura, controle de navegação, acesso remoto seguro e compartilhamento de arquivos por departamento.

Entre os objetivos específicos, destacam-se:

* Configurar o pfSense como firewall e gateway da rede;
* Implementar proxy com Squid;
* Aplicar controle de acesso à web com SquidGuard;
* Criar uma autoridade certificadora interna;
* Configurar acesso remoto por meio do OpenVPN;
* Configurar um servidor Ubuntu com DHCP, Apache e Samba AD DC;
* Criar compartilhamentos SMB com permissões baseadas em grupos;
* Validar o funcionamento dos serviços por meio de testes práticos.

## Cenário simulado

O projeto representa uma pequena rede corporativa, onde os clientes acessam a internet por meio do pfSense e utilizam serviços internos fornecidos por um servidor Ubuntu.

O pfSense atua como o ponto central de segurança e controle da rede, sendo responsável por filtrar o tráfego, aplicar políticas de navegação, fornecer acesso VPN e gerenciar certificados.

O Ubuntu Server atua como servidor interno, oferecendo serviços de rede e autenticação, como DHCP, Apache, DNS interno e Samba Active Directory Domain Controller.

## Tecnologias utilizadas

As principais tecnologias utilizadas no projeto foram:

* pfSense;
* Squid Proxy;
* SquidGuard;
* Autoridade Certificadora interna;
* OpenVPN;
* Ubuntu Server;
* DHCP Server;
* Apache;
* Samba Active Directory Domain Controller;
* DNS interno;
* VirtualBox.

## Serviços implementados

No pfSense, foram configurados serviços relacionados à segurança e controle de rede, como firewall, proxy, bloqueio de sites, certificados e VPN.

No Ubuntu Server, foram configurados serviços internos da rede, como DHCP, Apache, Samba AD DC, DNS interno e compartilhamentos SMB separados por departamento.

## Resultado esperado

Ao final do projeto, espera-se que a rede simulada permita:

* Comunicação entre os dispositivos da rede interna;
* Distribuição automática de endereços IP;
* Acesso controlado à internet;
* Bloqueio de sites ou categorias definidas;
* Acesso remoto seguro via VPN;
* Autenticação centralizada no domínio;
* Compartilhamento de arquivos com permissões por grupo;
* Acesso a serviços internos, como página web hospedada no Apache.

## Aprendizados

Durante o desenvolvimento do projeto, foram praticados conceitos importantes de redes e segurança, como segmentação, firewall, proxy, VPN, certificados digitais, DHCP, DNS, permissões Linux, Active Directory com Samba e controle de acesso a recursos internos.

O projeto também contribuiu para o desenvolvimento de habilidades práticas em documentação técnica, validação de serviços e organização de evidências para portfólio profissional.
