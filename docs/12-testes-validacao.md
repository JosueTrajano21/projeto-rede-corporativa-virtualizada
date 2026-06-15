# Testes e Validação

## Objetivo dos testes

Após a configuração dos serviços do projeto, foram realizados testes práticos para validar o funcionamento da infraestrutura.

O objetivo desta etapa foi confirmar se os principais componentes da rede estavam funcionando corretamente, incluindo firewall, proxy, filtro de conteúdo, lista externa de bloqueio, VPN, servidor web, DHCP, DNS interno e compartilhamentos Samba com permissões por grupo.

## Ambiente testado

Os testes foram realizados no ambiente virtualizado do projeto, composto por:

| Componente              | Função                                             |
| ----------------------- | -------------------------------------------------- |
| pfSense                 | Firewall, gateway, proxy, SquidGuard, CA e OpenVPN |
| Ubuntu Server           | DHCP, Apache, DNS interno e Samba AD DC            |
| Windows Cliente Interno | Cliente da LAN para testes de navegação e domínio  |
| Windows Cliente Externo | Cliente conectado via OpenVPN                      |
| Rede LAN                | `192.168.1.0/24`                                   |
| Gateway                 | `192.168.1.1`                                      |
| Ubuntu Server           | `192.168.1.10`                                     |
| Rede VPN                | `10.8.0.0/24`                                      |

## Testes de conectividade básica

Os primeiros testes validaram a comunicação entre os principais dispositivos da rede.

| Teste                       | Resultado esperado                      | Resultado obtido |
| --------------------------- | --------------------------------------- | ---------------- |
| Cliente receber IP via DHCP | Receber IP da faixa `192.168.1.100-200` | Funcionou        |
| Ping para o pfSense         | Resposta de `192.168.1.1`               | Funcionou        |
| Ping para o Ubuntu Server   | Resposta de `192.168.1.10`              | Funcionou        |
| Acesso à Internet           | Navegação via gateway pfSense           | Funcionou        |
| Resolução de nomes internos | Resolver domínio `lab.local`            | Funcionou        |

Esses testes confirmaram que a rede interna estava funcional, com DHCP, gateway e DNS operando corretamente.

## Testes do Squid Proxy

Foram realizados testes para validar o funcionamento do Squid como proxy da rede interna.

| Teste                    | Resultado esperado                           | Resultado obtido                |
| ------------------------ | -------------------------------------------- | ------------------------------- |
| Acessar sites HTTP       | Tráfego processado pelo Squid                | Funcionou                       |
| Acessar sites HTTPS      | Navegação funcionando com CA importada       | Funcionou após importação da CA |
| Navegar sem proxy manual | Tráfego interceptado pelo proxy transparente | Funcionou                       |
| Bloqueio de QUIC/HTTP3   | Impedir bypass pela porta UDP 443            | Funcionou                       |

Durante os testes, foi necessário importar a Autoridade Certificadora `Lab-CA` nos clientes para que a interceptação HTTPS funcionasse corretamente.

Também foi criada uma regra no pfSense bloqueando tráfego UDP na porta `443`, com o objetivo de impedir que navegadores utilizassem QUIC/HTTP3 para contornar o proxy.

## Testes do SquidGuard

Os testes do SquidGuard validaram as políticas de bloqueio e permissão de navegação.

| Teste             | Resultado esperado | Resultado obtido       |
| ----------------- | ------------------ | ---------------------- |
| Acessar Google    | Permitido          | Funcionou              |
| Acessar YouTube   | Permitido          | Funcionou              |
| Acessar TikTok    | Bloqueado          | Bloqueado              |
| Acessar Reddit    | Bloqueado          | Bloqueado              |
| Acessar Facebook  | Bloqueado          | Bloqueado após ajustes |
| Acessar Instagram | Bloqueado          | Bloqueado após ajustes |
| Acessar X/Twitter | Bloqueado          | Bloqueado              |
| Acessar Threads   | Bloqueado          | Bloqueado              |

Esses testes confirmaram que o SquidGuard conseguiu aplicar políticas de navegação, permitindo sites autorizados e bloqueando categorias ou domínios definidos.

## Teste da página personalizada de bloqueio

Foi criada uma página personalizada de bloqueio hospedada no Apache do Ubuntu Server.

Arquivo:

```text id="oqbhp2"
/var/www/html/bloqueado.html
```

Endereço:

```text id="ndrnuk"
http://192.168.1.10/bloqueado.html
```

| Teste                          | Resultado esperado                         | Resultado obtido                                |
| ------------------------------ | ------------------------------------------ | ----------------------------------------------- |
| Acessar página diretamente     | Página carregada no navegador              | Funcionou                                       |
| Bloqueio HTTP pelo SquidGuard  | Redirecionamento para página personalizada | Funcionou                                       |
| Bloqueio HTTPS pelo SquidGuard | Bloqueio aplicado                          | Funcionou, com possível página de erro do Squid |

Em acessos HTTP, o redirecionamento para a página personalizada ocorreu corretamente. Em acessos HTTPS, devido à interceptação SSL/MITM, o navegador podia exibir uma página de erro do Squid, mas o bloqueio era aplicado corretamente.

## Testes de logs e alertas por e-mail

O SquidGuard foi configurado para registrar tentativas de acesso bloqueado no arquivo:

```text id="fyxsbc"
/var/squidGuard/log/block.log
```

Também foi configurado um mecanismo de alerta por e-mail para notificar o administrador quando um novo bloqueio fosse detectado.

| Teste                               | Resultado esperado              | Resultado obtido |
| ----------------------------------- | ------------------------------- | ---------------- |
| Gerar tentativa de acesso bloqueado | Registro no `block.log`         | Funcionou        |
| Verificar IP do cliente no log      | IP exibido no registro          | Funcionou        |
| Verificar site bloqueado no log     | Domínio exibido no registro     | Funcionou        |
| Receber alerta por e-mail           | E-mail enviado ao administrador | Funcionou        |

O e-mail de alerta continha informações como horário do bloqueio, site acessado, categoria bloqueada, IP do cliente e sistema responsável pelo bloqueio.

## Testes da lista externa de IPs

Foi criada uma lista externa de IPs no Ubuntu Server e hospedada pelo Apache.

Arquivo:

```text id="caazfw"
/var/www/html/block_ips.txt
```

Endereço:

```text id="tm04mk"
http://192.168.1.10/block_ips.txt
```

Exemplo de IPs adicionados:

```text id="lx4iyc"
1.1.1.1
9.9.9.9
```

No pfSense, essa lista foi usada em um Alias do tipo `URL Table (IPs)` chamado:

```text id="yrt8rl"
Lista_Bloqueio_IPs
```

| Teste                                  | Resultado esperado       | Resultado obtido |
| -------------------------------------- | ------------------------ | ---------------- |
| Acessar `block_ips.txt` pelo navegador | Lista exibida            | Funcionou        |
| pfSense carregar a lista externa       | Alias preenchido com IPs | Funcionou        |
| Ping para IP presente na lista         | Bloqueado                | Funcionou        |
| Ping para IP fora da lista             | Permitido                | Funcionou        |

Esse teste confirmou que o pfSense conseguiu consumir uma lista externa hospedada no Ubuntu Server e aplicar bloqueios com base nos IPs definidos.

## Testes da OpenVPN

A OpenVPN foi testada para validar o acesso remoto seguro à rede interna.

Configurações principais:

| Item          | Valor            |
| ------------- | ---------------- |
| Protocolo     | UDP              |
| Porta         | `1194`           |
| Rede do túnel | `10.8.0.0/24`    |
| Rede interna  | `192.168.1.0/24` |
| DNS           | `192.168.1.10`   |

| Teste                     | Resultado esperado                          | Resultado obtido |
| ------------------------- | ------------------------------------------- | ---------------- |
| Conectar cliente OpenVPN  | Conexão estabelecida                        | Funcionou        |
| Ping para o pfSense       | Resposta de `192.168.1.1`                   | Funcionou        |
| Ping para o Ubuntu Server | Resposta de `192.168.1.10`                  | Funcionou        |
| Acessar Apache pela VPN   | Página acessível                            | Funcionou        |
| Acessar Samba pela VPN    | Compartilhamento acessível com autenticação | Funcionou        |

Esses testes confirmaram que o cliente externo conseguiu acessar recursos internos da rede por meio do túnel VPN.

## Testes do Apache

O Apache foi validado como servidor web interno e como servidor de arquivos auxiliares para o pfSense.

| Teste                    | Resultado esperado           | Resultado obtido |
| ------------------------ | ---------------------------- | ---------------- |
| Acessar página principal | Página carregada             | Funcionou        |
| Acessar `bloqueado.html` | Página de bloqueio carregada | Funcionou        |
| Acessar `block_ips.txt`  | Lista de IPs exibida         | Funcionou        |
| Acessar Apache pela VPN  | Página carregada remotamente | Funcionou        |

Endereços testados:

```text id="riqik5"
http://192.168.1.10
http://192.168.1.10/bloqueado.html
http://192.168.1.10/block_ips.txt
```

## Testes do Samba AD DC

Foram realizados testes para validar autenticação, domínio interno e controle de acesso aos compartilhamentos SMB.

Domínio utilizado:

```text id="xf1a4w"
lab.local
```

Servidor:

```text id="yw5m3n"
ubuntusrv.lab.local
```

Compartilhamentos testados:

```text id="2vz19v"
\\192.168.1.10\RH
\\192.168.1.10\TI
```

| Teste                                           | Resultado esperado         | Resultado obtido |
| ----------------------------------------------- | -------------------------- | ---------------- |
| Acessar compartilhamento com usuário do domínio | Solicitar autenticação     | Funcionou        |
| Usuário RH acessar pasta RH                     | Permitido                  | Funcionou        |
| Usuário RH criar/modificar arquivos na pasta RH | Permitido                  | Funcionou        |
| Usuário RH acessar pasta TI                     | Bloqueado                  | Acesso negado    |
| Usuário TI acessar pasta TI                     | Permitido                  | Funcionou        |
| Usuário TI criar/modificar arquivos na pasta TI | Permitido                  | Funcionou        |
| Usuário TI acessar pasta RH                     | Bloqueado                  | Acesso negado    |
| Acessar compartilhamento pela VPN               | Permitido com autenticação | Funcionou        |

Esses testes validaram que as permissões por grupo foram aplicadas corretamente e que usuários só conseguiam acessar os recursos autorizados para seu departamento.

## Problemas encontrados durante os testes

Durante a validação do ambiente, alguns problemas foram identificados:

* Interceptação HTTPS exigindo importação correta da CA nos clientes;
* Navegadores utilizando QUIC/HTTP3;
* Necessidade de bloquear UDP 443;
* Cache dos navegadores interferindo em alguns testes;
* `Default access` do SquidGuard inicialmente configurado como `Deny`;
* Necessidade de criar categoria manual para alguns domínios;
* Redirecionamento de porta no VirtualBox para funcionamento da OpenVPN;
* Necessidade de desmarcar `Block private networks` na WAN do pfSense por causa do NAT do VirtualBox;
* Ajustes nas permissões dos compartilhamentos Samba.

## Soluções aplicadas

Para corrigir os problemas encontrados, foram aplicadas as seguintes soluções:

| Problema                            | Solução aplicada                               |
| ----------------------------------- | ---------------------------------------------- |
| Erros de certificado HTTPS          | Importação da `Lab-CA` no Windows e Firefox    |
| Bypass por QUIC/HTTP3               | Bloqueio de UDP 443 na LAN                     |
| Bloqueio total de navegação         | Ajuste do `Default access` para `Allow`        |
| Sites não bloqueados pela blacklist | Criação de categoria manual de bloqueio        |
| OpenVPN atrás do NAT do VirtualBox  | Redirecionamento da porta UDP 1194             |
| WAN com IP privado no laboratório   | Desabilitada a opção `Block private networks`  |
| Permissões incorretas no Samba      | Ajuste de grupos, ownership e permissões Linux |

## Resultado geral

Após os testes, foi possível validar que a infraestrutura funcionou conforme o esperado.

O ambiente permitiu:

* Distribuir IPs automaticamente via DHCP;
* Usar o pfSense como gateway e firewall;
* Controlar navegação com Squid Proxy;
* Bloquear sites e categorias com SquidGuard;
* Registrar eventos de bloqueio;
* Enviar alertas por e-mail;
* Bloquear IPs externos por lista hospedada no Apache;
* Acessar serviços internos pela OpenVPN;
* Hospedar páginas e arquivos no Apache;
* Utilizar Samba AD DC como domínio interno;
* Controlar permissões de acesso por grupos.

## Conclusão dos testes

Os testes demonstraram que o ambiente simulado conseguiu representar uma infraestrutura corporativa funcional, com serviços de rede, controle de acesso, segurança perimetral e acesso remoto.

A validação prática mostrou que o pfSense e o Ubuntu Server trabalharam de forma integrada, permitindo controle de tráfego, filtragem web, autenticação centralizada, compartilhamento de arquivos e acesso remoto seguro.

Com isso, o projeto atingiu seu objetivo de simular uma rede corporativa com foco em infraestrutura, redes e segurança da informação.
