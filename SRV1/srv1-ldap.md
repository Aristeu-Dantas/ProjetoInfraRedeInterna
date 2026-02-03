## 🆔 SRV1 - Servidor de Identidade (OpenLDAP)

O **SRV1** é o cérebro da rede. Ele armazena de forma centralizada todos os usuários e grupos, eliminando a necessidade de criar contas localmente em cada servidor.

### 🛠️ Especificações Técnicas

* **Hostname:** `srv-ladp`
* **IP Fixo:** `10.0.0.20`
* **Serviço:** `slapd` (OpenLDAP Project).

### ⚙️ Estrutura do Diretório (`DIT`)

A base de dados foi estruturada seguindo o padrão X.500:

* **DN Base:** `dc=empresa,dc=local`
* **Unidades Organizacionais:**
* `ou=Usuarios`: Onde residem as contas `pedro`, `maria` e `joao`.
* `ou=Grupos`: Onde estão os grupos departamentais (`comercial`, `financeiro`, `ti`).



### 🔐 Segurança de Diretório

* **Autenticação:** Apenas o administrador (`cn=admin`) possui permissão para escrita no diretório.
* **Sincronização:** Configurado para responder a consultas `ldapsam` vindas do Servidor de Arquivos (SRV3).

---

### 💻 Comandos de Administração (SRV1 e SRV2)

```bash
# No SRV2 (Gateway): Verificar se o NAT está ativo
iptables -t nat -L -n -v

# No SRV1 (LDAP): Testar conexão local com a base
ldapsearch -x -b "dc=empresa,dc=local" -s base

```