### 📝 Instruções para o VS Code:

1. Abra o arquivo `README.md` no seu VS Code.
2. Selecione tudo e apague.
3. Cole o código abaixo exatamente como está.

---

# 🏢 Infraestrutura Corporativa Linux (LDAP + Samba)

Projeto prático de implementação de uma rede corporativa simulada utilizando **Debian GNU/Linux 13**. O ambiente utiliza servidores virtualizados para garantir segurança, organização departamental e centralização de acessos através de um diretório de identidades.

---

## 🎯 Objetivo
Configurar um ambiente de rede funcional onde o acesso aos recursos (compartilhamentos de arquivos) é controlado rigorosamente por um servidor de identidade central, simulando um cenário real de departamentos (TI, Comercial e Financeiro).

## 🛠️ Tecnologias e Serviços
* **Virtualização:** Oracle VM VirtualBox.
* **Sistema Operacional:** Debian GNU/Linux 13 (Trixie).
* **Gerenciamento de Identidade:** OpenLDAP (Backend de usuários e senhas).
* **Servidor de Arquivos:** Samba 4 (Integrado ao LDAP via smb.conf).
* **Monitoramento e Suporte:** NTP para sincronização e Rsyslog para centralização de logs.

---

## ⚙️ Topologia da Rede e Endereçamento

A infraestrutura foi segmentada em máquinas virtuais com funções específicas. A conectividade foi validada entre todos os nós.

| VM | Hostname | Interface | Endereço IP | Função Principal |
| :--- | :--- | :--- | :--- | :--- |
| **SRV1** | `srv-ladp` | `enp0s3` | **10.0.0.20** | Servidor de Identidade (OpenLDAP) |
| **SRV2** | `srv-gw` | `enp0s8` | **10.0.0.1** | Gateway / Firewall (Rede Interna) |
| **SRV2** | `srv-gw` | `enp0s3` | **10.0.2.15** | Interface de Saída (NAT/Internet) |
| **SRV3** | `srv3` | `enp0s3` | **10.0.0.30** | Servidor de Arquivos (Samba PDC) |
| **SRV4** | `srv-db` | `enp0s3` | **10.0.0.20** | Banco de Dados e Aplicações |
| **SRV5** | `srv-log-ntp` | `enp0s3` | **10.0.0.40** | Logs Centralizados e Servidor NTP |
| **Cliente**| `cliente-01` | `enp0s3` | **10.0.0.10** | Estação de Trabalho Linux (XFCE) |

---

## 🔐 Destaques da Implementação

### 1. Autenticação Centralizada
* Usuários corporativos (**joao**, **maria**, **pedro**) são gerenciados no SRV1 (LDAP).
* O SRV3 (Samba) consulta o LDAP em tempo real para validar permissões.

### 2. Segurança e ACLs (Access Control Lists)
* **Princípio do Menor Privilégio:** Cada departamento possui acesso exclusivo à sua respectiva pasta.
* **Máscaras de Criação:** Configurado `force create mode = 0770` para garantir que novos arquivos pertençam ao grupo do departamento.
* **Isolamento:** Usuários não autorizados recebem "Permissão Negada" ao tentar acessar pastas de outros setores.

---

## 💻 Comandos Úteis para Administração

### No Servidor LDAP (SRV1)
Verificar se o serviço está rodando e listar usuários na base:
```bash
# Verificar status do serviço
systemctl status slapd

# Listar todos os usuários cadastrados no LDAP
ldapsearch -x -b "dc=empresa,dc=local" "(objectclass=posixAccount)"

```

### No Servidor Samba (SRV3)

Gerenciar o compartilhamento e testar integração:

```bash
# Reiniciar serviços após alteração no smb.conf
systemctl restart smbd nmbd

# Verificar se os usuários do LDAP aparecem no Samba
getent passwd | grep -E 'joao|maria|pedro'

# Testar a sintaxe do arquivo de configuração
testparm

```

### No Cliente (cliente-01)

Testar acesso aos compartilhamentos:

```bash
# Listar compartilhamentos disponíveis no servidor
smbclient -L //10.0.0.30 -U pedro

# Montar pasta comercial manualmente
mount -t cifs //10.0.0.30/comercial /mnt/comercial -o user=pedro

```

---

## ✅ Validação do Ambiente

* **Conectividade:** Ping realizado com sucesso entre Cliente e Servidores (0% de perda).
* **Acesso Negado:** Validação do bloqueio de acesso do usuário `pedro` ao diretório `financeiro`.
* **Resolução de Hostname:** DNS/Hosts configurado para que as máquinas se reconheçam pelos nomes (srv-gw, srv3, etc).

---

*Projeto desenvolvido para a disciplina de Administração de Sistemas de Redes.*
**Autor:** Aristeu Dantas da Costa Júnior