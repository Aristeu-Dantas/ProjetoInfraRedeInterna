## 📧 SRV4 - Servidor de Correio e Segurança (Mail & Antivírus)

O **SRV4** (`srv-db` / `srv-mail`) atua como o hub de comunicação da rede interna, integrando serviços de transporte de mensagens e proteção contra ameaças.

### 🛠️ Especificações Técnicas

* **Hostname:** `srv-db` (conforme terminal).
* **IP:** `10.0.0.50` (Recomendado para evitar conflito com SRV1).
* **Serviços:** Postfix (SMTP), Dovecot (IMAP/POP3) e ClamAV (Antivírus).

### ⚙️ Implementação dos Serviços

#### 1. Servidor de E-mail (Postfix & Dovecot)

Responsável por enviar, receber e armazenar as mensagens dos usuários do domínio `@empresa.local`.

* **Postfix:** Configurado para aceitar conexões apenas da rede interna (`10.0.0.0/24`).
* **Dovecot:** Gerencia as caixas de correio (`Maildir`) e permite que o **Cliente-01** acesse os e-mails via Thunderbird ou Outlook.

#### 2. Antivírus Corporativo (ClamAV)

Implementado para realizar o escaneamento de arquivos em tempo real e anexos de e-mail.

* **ClamDaemon:** Roda em background verificando diretórios compartilhados.
* **Freshclam:** Serviço de atualização automática da base de assinaturas de vírus.

---

### 🛡️ Integração com o Servidor de Arquivos (SRV3)

Uma funcionalidade avançada desta infraestrutura é o escaneamento dos compartilhamentos do Samba. O **SRV4** pode ser configurado para escanear a pasta `/srv/publico` do **SRV3** remotamente ou via montagem NFS.

### 💻 Comandos de Administração (SRV4)

```bash
# Verificar status do servidor de e-mail
systemctl status postfix

# Ver logs de mensagens em tempo real (Auditoria de envio)
tail -f /var/log/mail.log

# Forçar atualização da base do antivírus
freshclam

# Escanear manualmente a pasta de um usuário
clamscan -r /home/pedro/mail

```