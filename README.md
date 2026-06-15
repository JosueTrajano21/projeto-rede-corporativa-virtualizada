# Projeto de Rede Corporativa com pfSense e Ubuntu Server

## Sobre o projeto

Este projeto simula uma infraestrutura de rede corporativa utilizando pfSense como firewall/gateway e Ubuntu Server como servidor interno. O ambiente foi criado em laboratório com foco em serviços de rede, controle de acesso, proxy, VPN e autenticação centralizada.

## Objetivos

- Configurar um firewall corporativo com pfSense;
- Implementar proxy com Squid;
- Controlar acesso a sites com SquidGuard;
- Criar uma autoridade certificadora interna;
- Configurar acesso remoto via OpenVPN;
- Configurar Ubuntu Server com DHCP, Apache e Samba AD DC;
- Simular departamentos com permissões de acesso por grupo.

## Tecnologias utilizadas

- pfSense
- Squid Proxy
- SquidGuard
- OpenVPN
- Autoridade Certificadora interna
- Ubuntu Server
- DHCP Server
- Apache
- Samba AD DC
- DNS interno
- VirtualBox

## Topologia do ambiente

![Topologia da rede](images/topologia.png)

## Serviços implementados

### pfSense

- Firewall e gateway da rede;
- Regras de tráfego;
- Proxy com Squid;
- Bloqueio de sites com SquidGuard;
- Autoridade certificadora;
- OpenVPN para acesso remoto.

### Ubuntu Server

- Servidor DHCP;
- Servidor Apache;
- Samba Active Directory Domain Controller;
- DNS interno;
- Compartilhamentos SMB por departamento.

## Documentação completa

- [Visão geral](docs/01-visao-geral.md)
- [Topologia](docs/02-topologia.md)
- [pfSense](docs/03-pfsense.md)
- [Squid Proxy](docs/04-squid-proxy.md)
- [SquidGuard](docs/05-squidguard.md)
- [Autoridade Certificadora](docs/06-autoridade-certificadora.md)
- [OpenVPN](docs/07-openvpn.md)
- [Ubuntu Server](docs/08-ubuntu-server.md)
- [DHCP](docs/09-dhcp.md)
- [Apache](docs/10-apache.md)
- [Samba AD DC](docs/11-samba-ad-dc.md)
- [Testes e validação](docs/12-testes-validacao.md)

## Resultado

Ao final do projeto, foi possível simular uma rede corporativa com controle de tráfego, autenticação centralizada, acesso remoto via VPN, compartilhamento de arquivos por departamento e serviços internos funcionando em ambiente virtualizado.

## Aprendizados

Durante o projeto, foram praticados conceitos de redes, firewall, proxy, VPN, DNS, DHCP, Active Directory, permissões de acesso e administração de servidores Linux.

## Autor

Josué Felipe  
Estudante de Análise e Desenvolvimento de Sistemas  
Foco em Redes, Infraestrutura e Segurança da Informação