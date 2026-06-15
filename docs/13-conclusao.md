# Conclusão

## Visão geral da conclusão

Este projeto permitiu simular uma infraestrutura de rede corporativa utilizando pfSense como firewall/gateway e Ubuntu Server como servidor interno.

Durante a implementação, foram configurados serviços essenciais de rede, segurança, controle de acesso, proxy, VPN, servidor web, DHCP, DNS interno e autenticação centralizada com Samba Active Directory Domain Controller.

O ambiente foi construído em laboratório, utilizando máquinas virtuais, com o objetivo de praticar conceitos aplicados em redes corporativas e segurança da informação.

## Resultado do projeto

Ao final do projeto, foi possível construir uma rede funcional composta por:

* pfSense atuando como firewall e gateway da rede;
* Squid Proxy para controle de navegação;
* SquidGuard para bloqueio de sites e categorias;
* Autoridade Certificadora interna;
* OpenVPN para acesso remoto seguro;
* Ubuntu Server como servidor interno;
* DHCP Server para distribuição automática de IPs;
* Apache para hospedagem de páginas e arquivos internos;
* Samba AD DC para domínio, autenticação e controle de acesso;
* DNS interno para resolução do domínio `lab.local`;
* Compartilhamentos SMB separados por departamento.

A integração entre esses serviços permitiu representar um cenário próximo de uma pequena rede corporativa, com controle de tráfego, acesso remoto, autenticação centralizada e políticas de permissão.

## Principais validações realizadas

Durante os testes, foram validados diferentes pontos da infraestrutura.

Entre os principais resultados, destacam-se:

* Clientes recebendo IP automaticamente via DHCP;
* pfSense funcionando como gateway da rede interna;
* Acesso à Internet passando pelo firewall;
* Proxy transparente funcionando com Squid;
* Interceptação HTTPS funcionando após importação da CA;
* Bloqueio de QUIC/HTTP3 por meio da porta UDP 443;
* Bloqueio de redes sociais com SquidGuard;
* Página personalizada de bloqueio hospedada no Apache;
* Registro de bloqueios nos logs do SquidGuard;
* Envio de alertas por e-mail;
* Bloqueio de IPs externos por lista hospedada no Ubuntu Server;
* Conexão OpenVPN funcionando;
* Acesso ao Apache e Samba pela VPN;
* Controle de acesso aos compartilhamentos por grupos de RH e TI.

Esses testes comprovaram que os serviços configurados estavam funcionando de forma integrada e atendendo aos objetivos propostos.

## Desafios encontrados

Durante o desenvolvimento do projeto, alguns desafios técnicos surgiram e precisaram ser resolvidos.

Entre eles, destacam-se:

* Configuração correta das interfaces WAN e LAN do pfSense;
* Ajustes de NAT no ambiente virtualizado;
* Necessidade de bloquear QUIC/HTTP3 para evitar bypass do proxy;
* Importação da Autoridade Certificadora nos clientes;
* Diferença de comportamento entre bloqueios HTTP e HTTPS;
* Ajuste do `Default access` no SquidGuard;
* Criação de categoria manual para domínios não bloqueados pela blacklist;
* Configuração da OpenVPN atrás do NAT do VirtualBox;
* Liberação de tráfego privado na WAN por causa do ambiente de laboratório;
* Configuração de DNS interno para funcionamento do Samba AD DC;
* Ajuste de permissões Linux nos compartilhamentos SMB;
* Validação de acesso permitido e negado por grupo.

Esses desafios foram importantes para entender melhor o funcionamento prático dos serviços e a relação entre firewall, proxy, DNS, VPN, permissões e autenticação.

## Aprendizados obtidos

O projeto proporcionou aprendizado prático em diferentes áreas de infraestrutura e segurança.

Entre os principais aprendizados, estão:

* Funcionamento de firewall e gateway em uma rede corporativa;
* Criação e organização de regras no pfSense;
* Uso de proxy para controle de navegação;
* Aplicação de políticas de bloqueio com SquidGuard;
* Uso de certificados digitais em proxy HTTPS e VPN;
* Configuração de acesso remoto seguro com OpenVPN;
* Distribuição de IPs com DHCP;
* Importância do DNS interno em ambientes com domínio;
* Configuração de Samba AD DC em Linux;
* Controle de permissões baseado em grupos;
* Integração entre servidores internos e firewall;
* Validação prática de serviços por meio de testes.

Além da parte técnica, o projeto também contribuiu para melhorar a documentação, organização de evidências e apresentação de resultados em formato de portfólio.

## Relação com segurança da informação

Embora o projeto tenha foco em infraestrutura de redes, ele também aborda conceitos importantes de segurança da informação.

O pfSense atuou como camada de controle perimetral, filtrando tráfego e centralizando a saída da rede. O Squid e o SquidGuard permitiram aplicar políticas de navegação. A OpenVPN possibilitou acesso remoto seguro. O Samba AD DC simulou autenticação centralizada e controle de permissões por grupo.

Esses elementos são comuns em ambientes corporativos reais e estão diretamente relacionados a práticas de segurança, controle de acesso, monitoramento e administração de redes.

## Possíveis melhorias futuras

Como evolução do projeto, algumas melhorias poderiam ser implementadas:

* Criar VLANs para separar departamentos;
* Integrar logs do pfSense e Ubuntu a uma solução SIEM;
* Monitorar eventos com Wazuh, Splunk ou Elastic;
* Criar dashboards de eventos de proxy, firewall e autenticação;
* Implementar políticas mais detalhadas de firewall;
* Adicionar IDS/IPS com Suricata ou Snort;
* Criar GPOs usando Samba AD DC;
* Adicionar backup automatizado das configurações;
* Documentar procedimentos de troubleshooting;
* Criar um fluxo de resposta a incidentes baseado nos logs gerados.

Essas melhorias poderiam transformar o ambiente em um laboratório ainda mais completo, aproximando o projeto de cenários de Blue Team, SOC e administração de redes corporativas.

## Considerações finais

O projeto atingiu seu objetivo de simular uma infraestrutura corporativa funcional utilizando pfSense e Ubuntu Server.

A configuração permitiu praticar, testar e validar serviços importantes de rede e segurança, como firewall, proxy, VPN, DHCP, DNS, Apache, Samba AD DC e controle de acesso por grupos.

O resultado final foi um ambiente integrado, documentado e validado, servindo como projeto prático para portfólio profissional nas áreas de redes, infraestrutura, segurança da informação e Blue Team.
