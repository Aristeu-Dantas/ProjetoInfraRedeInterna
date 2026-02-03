🏢 Infraestrutura Corporativa Linux (LDAP + Samba)
Projeto prático de implementação de uma rede corporativa simulada, utilizando múltiplos servidores virtualizados para garantir segurança, organização e centralização de acessos.

🎯 Objetivo
Configurar um ambiente de rede funcional onde o acesso aos recursos (arquivos) é controlado rigorosamente por um servidor de identidade central, simulando um cenário real de departamento de TI, Comercial e Financeiro.

🛠️ Tecnologias e Serviços
Virtualização: Oracle VM VirtualBox

Sistema Operacional: Debian GNU/Linux

Gerenciamento de Identidade: OpenLDAP (Backend de usuários e senhas)

Servidor de Arquivos: Samba 4 (Integrado ao LDAP)

Clientes: Linux Desktop (Integração via smbclient e interface gráfica)

⚙️ Topologia da Rede
O projeto foi segmentado em máquinas virtuais com funções específicas:

SRV2 (Gateway): Responsável pelo roteamento, NAT, firewall e conexão entre a rede interna e externa.

SRV1 (LDAP): Servidor de Identidade (10.0.0.20), hospedando a base de dados centralizada de usuários e grupos.

SRV3 (Samba): Servidor de Arquivos (10.0.0.30), configurado com compartilhamentos independentes e integrado ao LDAP.

SRV4 (Correio/Msg): Servidor de E-mail (SMTP/IMAP) responsável pelo tráfego e armazenamento de mensagens e antivírus corporativo.

SRV5 (Dados/Logs): Servidor dedicado a Banco de Dados e centralização de Logs/Monitoramento da infraestrutura.

cliente-01: Estação de trabalho (Desktop) utilizada para validação de acesso e testes de permissão.
🔐 Destaques da Implementação
Autenticação Centralizada: Usuários (João, Maria, Pedro) criados no LDAP e replicados para o Samba.

Permissões Avançadas: Configuração de máscaras de criação (0770) e usuários válidos (valid users) forçando autenticação por diretório.

Segurança: Bloqueio de acesso cruzado (ex: TI não acessa Financeiro) e proteção contra acesso anônimo em pastas críticas.
