## 🚀 Guia de Reprodução (Passo a Passo)

Siga estas etapas para replicar o ambiente de infraestrutura em um laboratório virtualizado.

### 1. Preparação do Hipervisor (VirtualBox)

* **Rede Interna:** Configure todas as máquinas (exceto o Gateway) para utilizar uma **Rede Interna** (ex: nomeada `intnet`).
* **Gateway (SRV2):** Deve possuir duas placas de rede: uma em modo **NAT** (internet) e outra em **Rede Interna**.

### 2. Configuração do Gateway (SRV2)

O Gateway é o coração da rede, permitindo que as outras VMs acessem a internet.

1. Configure as interfaces de rede no arquivo `/etc/network/interfaces`:
* `enp0s3`: DHCP (NAT).
* `enp0s8`: Estático `10.0.0.1/24`.


2. Habilite o roteamento de pacotes (IP Forwarding):
```bash
echo 1 > /proc/sys/net/ipv4/ip_forward

```


3. Configure o Masquerade (NAT) via iptables:
```bash
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

```



### 3. Implementação do Servidor de Identidade (SRV1)

1. Instale os pacotes do OpenLDAP: `apt install slapd ldap-utils`.
2. Durante a instalação, defina o domínio como `empresa.local`.
3. Crie as Unidades Organizacionais (`ou=Usuarios` e `ou=Grupos`) e cadastre os usuários (`joao`, `maria`, `pedro`) utilizando arquivos `.ldif`.

### 4. Configuração do Servidor de Arquivos (SRV3)

1. Instale o Samba: `apt install samba smbclient libnss-ldap`.
2. Configure o arquivo `/etc/samba/smb.conf` para apontar o backend para o IP `10.0.0.20` (SRV1).
3. Crie os diretórios físicos no Linux:
```bash
mkdir -p /srv/publico/{comercial,financeiro,ti}
chmod -R 777 /srv/publico

```


4. Reinicie o serviço: `systemctl restart smbd`.

### 5. Validação no Cliente

1. Configure o IP do `cliente-01` como `10.0.0.10` e o Gateway como `10.0.0.1`.
2. Teste o acesso ao compartilhamento:
* **Sucesso:** Usuário `pedro` acessa a pasta `comercial`.
* **Segurança:** Usuário `pedro` recebe erro de permissão ao tentar acessar a pasta `financeiro`.