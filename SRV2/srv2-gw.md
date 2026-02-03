## 🌐 SRV2 - Gateway e Segurança (O Coração da Rede)

O **SRV2** é o ponto de entrada e saída de toda a infraestrutura. Sem ele, as outras máquinas ficam isoladas e sem acesso a atualizações externas.

### 🛠️ Especificações Técnicas

* **Hostname:** `srv-gw`
* **IP Externo (WAN):** `10.0.2.15` (Interface `enp0s3`) — Conecta-se à Internet via NAT do VirtualBox.
* **IP Interno (LAN):** `10.0.0.1` (Interface `enp0s8`) — Atua como o Gateway padrão para todas as outras VMs.

### ⚙️ Implementação e Regras

* **Roteamento (IP Forwarding):** Configurado no `sysctl.conf` para permitir que pacotes transitem entre as interfaces.
* **NAT (Network Address Translation):** Implementado via `iptables` para mascarar o tráfego da rede interna.
* **Firewall (Netfilter):** Atua como a primeira linha de defesa, controlando portas abertas e tráfego permitido.