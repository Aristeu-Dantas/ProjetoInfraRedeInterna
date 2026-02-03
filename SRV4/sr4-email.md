```markdown
## 📧 SRV4 - Servidor de Correio e Segurança (Mail & Antivírus)

O **SRV4** (`srv-db` / `srv-mail`) atua como o hub de comunicação da rede interna, integrando serviços de transporte de mensagens e proteção contra ameaças através de um motor de antivírus corporativo.

### 🛠️ Especificações Técnicas
* **Hostname:** `srv-db`.
* **IP:** `10.0.0.50` (Configurado para evitar conflito com o SRV1).
* **Serviços:** Postfix (SMTP), Dovecot (IMAP/POP3) e ClamAV (Antivírus).

---

### ⚙️ Implementação e Guia de Uso

O servidor utiliza **Postfix** para envio e **Dovecot** para recebimento. As caixas de correio são estruturadas no formato `Maildir` dentro da `home` de cada usuário LDAP.

#### 1. Enviando um E-mail de Teste (Via Terminal)
Para validar a comunicação entre usuários (ex: de `pedro` para `maria`):
```bash
# Acessar como o usuário pedro
su - pedro

# Enviar e-mail para maria
mail -s "Relatorio de Vendas" maria@empresa.local
# (Escreva a mensagem e pressione CTRL+D para enviar)

```

#### 2. Verificando o Recebimento

As mensagens ficam armazenadas no diretório pessoal do destinatário:

```bash
# Acessar como maria e ler as novas mensagens
su - maria
ls /home/maria/Maildir/new/
# Ou ler via interface interativa:
mail

```

#### 3. Antivírus Corporativo (ClamAV)

O **ClamAV** realiza o escaneamento de arquivos em tempo real e pode auditar os compartilhamentos do **SRV3 (Samba)** via rede.

* **ClamDaemon:** Monitoramento em background.
* **Freshclam:** Atualização automática das assinaturas de vírus.

---

### 🛡️ Monitoramento e Auditoria

Como administrador, é possível acompanhar a entrega das mensagens e o status dos serviços em tempo real.

**Logs de Correio:**

```bash
# Acompanhar entrega de e-mails (status=sent)
tail -f /var/log/mail.log

```

**Comandos de Manutenção:**
| Ação | Comando |
| :--- | :--- |
| **Reiniciar SMTP** | `systemctl restart postfix` |
| **Reiniciar IMAP** | `systemctl restart dovecot` |
| **Ver Fila de Envios** | `mailq` |
| **Atualizar Antivírus** | `freshclam` |
| **Escanear Pasta** | `clamscan -r /home/pedro/mail` |

---